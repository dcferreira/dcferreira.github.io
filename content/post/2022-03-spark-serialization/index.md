---
title: Efficient UDFs on Databricks with unpickleable objects
subtitle: How to avoid `PicklingError` on custom UDFs on Databricks/Spark, while keeping optimal performance.

# Summary for listings and search engines
summary: How to avoid `PicklingError` on custom UDFs on Databricks/Spark, while keeping optimal performance.

# Link this post with a project
projects: []

# Date published
date: "2022-03-21T17:30:00Z"

# Date updated
lastmod: "2022-03-21T17:30:00Z"

# Is this an unpublished draft?
draft: false

# Show this page in the Featured widget?
featured: false

# Featured image
# Place an image named `featured.jpg/png` in this page's folder and customize its options here.
image:
  caption: "Image credit: [**Unsplash**](https://unsplash.com/photos/CpkOjOcXdUY)"
  focal_point: ""
  placement: 2
  preview_only: false

authors:
  - admin

tags:
  - Spark
  - Databricks

categories:
  - Spark
  - Databricks
---

## Overview

I often run into a problem when writing UDFs on Databricks, where I need some to access some object that `pickle` can't serialize.
Often times this is just something that comes from some external library, and so fixing the code is not a practical solution.

An easy solution to this is to initialize the object inside the UDF itself.
This avoids the need for serialization, but it introduces a new problem: the object is initialized for every run of the UDF, hitting performance.

The solution that addresses these 2 problems is to cache the object initialization.
Then, each executor initializes the object only once.

## The Problem

Here is a simple example:

```python
import time
from lxml.etree import HTMLParser

# `spark` is the spark context, on databricks it is a global variable that's always available
df = spark.createDataFrame([{"n": n} for n in range(10000)])

class Slow:
    def __init__(self):
        self.parser = HTMLParser()
        time.sleep(0.01)

    def double(self, x: int) -> int:
        return 2 * x

slow_global = Slow()

@udf("int")
def f_error(n):
    return slow_global.double(n)
```

When actually executing the UDF

```python
df.select("n", f_error("n")).collect()
```

we get the error

```
PicklingError: Could not serialize object: TypeError: can't pickle lxml.etree.HTMLParser objects
```

## Naive Solution

The naive solution is to initialize the object in each run of the UDF:

```python
@udf("int")
def f(n):
    slow = Slow()
    return slow.double(n)
```

This works

```python
df.select("n", f("n")).collect()
```

but it's very inefficient.

On a cluster with 2 `i3.xlarge` workers on AWS, executing this took me around 25 seconds.

## Optimized Solution

The solution is then to cache the object initialization.
For this, we need the [`cachetools`](https://cachetools.readthedocs.io/) library.
On Databricks, you can install it by running the following cell

```
%pip install cachetools
```

{{% callout note %}}
We can't use [`lru_cache`](https://docs.python.org/3/library/functools.html#functools.lru_cache) from the standard library,
because it requires serialization.
Trying it gives us the error: `PicklingError: Could not serialize object: AttributeError: 'functools._lru_cache_wrapper' object has no attribute '__bases__'`
{{% /callout %}}

Usage is very simple:

```python
from cachetools import cached

@cached(cache={})
def get_slow():
    return Slow()

@udf("int")
def f_cached(n):
    slow = get_slow()
    return slow.double(n)
```

Executing it

```python
df.select("n", f_cached("n")).collect()
```

took around 0.5 seconds, in the same cluster as above.
