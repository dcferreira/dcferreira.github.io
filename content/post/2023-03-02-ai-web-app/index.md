---
title: Making and Deploying an AI Web App in 2023 (Part 2)
subtitle: How to do a proof of concept for an AI Web App.

# Summary for listings and search engines
summary: Making and Deploying an AI Web App in 2023 (Part 2) -- How to do a proof of concept for an AI Web App.

# Link this post with a project
projects: []

# Date published
date: "2023-03-03T00:00:00Z"

# Date updated
lastmod: "2023-03-03T00:00:00Z"

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
  - AI
  - Machine Learning
  - Web App
  - txtai
  - Web Development
  - Serverless
  - NLP

categories:
  - AI
  - NLP
---

{{% callout note %}}
This is part of a multi-part blogpost about how to build an AI Web App.
Please refer to [Part 1](/post/2023-03-01-ai-web-app) for more context.

This post uses [txtai](https://neuml.github.io/txtai/) as the AI library.

Alternatives would be: [transformers](https://huggingface.co/docs/transformers/index), [sentence-transformers](https://www.sbert.net/), [PyTorch](https://pytorch.org/), [Keras](https://keras.io/), [scikit-learn](https://scikit-learn.org/stable/), among many others.
{{% /callout %}}

The first step in any project is always to make a proof of concept.
At this stage, we don't care about performance, edge cases, or any other intricacies.
We just want to confirm that what we want to do is viable.

For the sake of example, we will take strong inspiration from [this example from the `txtai` library](https://github.com/neuml/txtai/blob/master/examples/03_Build_an_Embeddings_index_from_a_data_source.ipynb).
We will index a database of documents and then query for them with natural language.
We're implementing our own little Google Search, which only returns results from the files we give it.

But we're not just search for keywords, like classical search engines.
We will use [text embeddings](https://en.wikipedia.org/wiki/Word_embedding) to search for the closest match.
By searching for embeddings we're not literally searching for the words we give it, but for the meaning of the whole query.
This is called [Semantic search](https://en.wikipedia.org/wiki/Semantic_search).

The first step in any project is to create a new directory for it.

```bash
mkdir ai-web-app
```

We should also create a directory for notebooks, to keep things tidy:

```bash
cd ai-web-app
mkdir notebooks
```

For this example, we will use a test dataset that `txtai` made available.
This data is a collection of research documents into COVID-19. The original version of this dataset can be found [on Kaggle](https://www.kaggle.com/datasets/allen-institute-for-ai/CORD-19-research-challenge).
We download the dataset with the following shell commands:

```bash
wget https://github.com/neuml/txtai/releases/download/v1.1.0/tests.gz
gunzip tests.gz
mv tests articles.sqlite
```

and install the needed libraries with

```bash
pip install txtai sentence-transformers pandas jupyterlab
```

We can then start a notebook server with [Jupyter Lab](https://docs.jupyter.org/en/latest/) with

```bash
cd notebooks && jupyter lab
```

Our app needs an index (a searchable database of documents), and a way to query (search) the index.
The following code (mostly stolen from [`txtai`'s example](https://github.com/neuml/txtai/blob/master/examples/03_Build_an_Embeddings_index_from_a_data_source.ipynb)) creates an index with the COVID-19 dataset downloaded above:

```python
import sqlite3

import regex as re

from txtai.embeddings import Embeddings
from txtai.pipeline import Tokenizer


def stream():
    # Connection to database file
    db = sqlite3.connect("../articles.sqlite")
    cur = db.cursor()

    # Select tagged sentences without a NLP label. NLP labels are set for non-informative sentences.
    cur.execute("SELECT Id, Name, Text FROM sections WHERE (labels is null or labels NOT IN ('FRAGMENT', 'QUESTION')) AND tags is not null")

    count = 0
    for row in cur:
        # Unpack row
        uid, name, text = row

        # Only process certain document sections
        if not name or not re.search(r"background|(?<!.*?results.*?)discussion|introduction|reference", name.lower()):
            # Tokenize text
            tokens = Tokenizer.tokenize(text)

            document = (uid, tokens, None)

            count += 1
            if count % 1000 == 0:
                print("Streamed %d documents" % (count), end="\r")

            # Skip documents with no tokens parsed
            if tokens:
                yield document

    print("Iterated over %d total rows" % (count))

    # Free database resources
    db.close()

# BM25 + fastText vectors
embeddings = Embeddings({
    "method": "sentence-transformers",
    "path": "all-MiniLM-L6-v2",
    "scoring": "bm25"
})

embeddings.score(stream())
embeddings.index(stream())
```

In the code above, the text embeddings are generated using the [`all-MiniLM-L6-v2` model](https://huggingface.co/sentence-transformers/all-MiniLM-L6-v2).
We could also train our own model instead of using a pre-trained model, but that can be quite some work.
If we can avoid it, why not?

So at this point, we have a database and want to search it.
In the following cell, we define a Python function to query the index for the closest results, and then query the original data for extra information about the results.
The last thing we do in this cell is to search for `"risk factors"`.

```python
import pandas as pd

pd.set_option("display.max_colwidth", None)

def search(query: str, topn: int = 5) -> pd.DataFrame:
    db = sqlite3.connect("../articles.sqlite")
    cur = db.cursor()

    results = []
    for uid, score in embeddings.search(query, topn):
        cur.execute("SELECT article, text FROM sections WHERE id = ?", [uid])
        uid, text = cur.fetchone()

        cur.execute("SELECT Title, Published, Reference from articles where id = ?", [uid])
        results.append(cur.fetchone() + (text,))

    db.close()
    df = pd.DataFrame(results, columns=["Title", "Published", "Reference", "Match"])
    return df


search("risk factors")
```

The output of that cell will be a pandas dataframe with the results.
![image.png](/assets/ai-web-app/image_1674755971020_0.png)

We can also try other queries:
![image.png](/assets/ai-web-app/image_1674755993542_0.png)
![image.png](/assets/ai-web-app/image_1674756005367_0.png)

As you can see, while the results maybe aren't the best, they are good enough for our proof of concept.
We could spend a lot more time making this part better, but in the spirit of [shipping early and often](https://www.ycombinator.com/library/40-the-art-of-shipping-early-and-often), this is enough for now.

{{% callout note %}}
If you've been following these instructions, your code should look like this:
https://github.com/dcferreira/ai-web-app/tree/19cda18ea099d46716b280694eed2677ef680e5d
{{% /callout %}}
