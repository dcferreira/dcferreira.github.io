---
title: Making and Deploying an AI Web App in 2023 (Part 5)
subtitle: Implement and Test a Python Backend Server

# Summary for listings and search engines
summary: How to use FastAPI+Uvicorn to write and deploy the backend of a simple AI App.

# Link this post with a project
projects: []

# Date published
date: "2023-03-08T00:05:00Z"

# Date updated
lastmod: "2023-03-08T00:05:00Z"

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

This post uses [FastAPI](https://fastapi.tiangolo.com) as the web framework and
[Uvicorn](https://www.uvicorn.org) as the web server.

Some alternatives to FastAPI are [Flask](https://flask.palletsprojects.com/en/2.2.x/), [Starlette](https://www.starlette.io/), and [Django](https://www.djangoproject.com/).
Alternatives to Uvicorn include [Gunicorn](https://gunicorn.org/), [Hypercorn](https://hypercorn.readthedocs.io/en/latest/), [nginx](https://www.nginx.com/).
{{% /callout %}}

By this point we have an AI project that we're happy with, and want to make it easily available.
The goal of this post is to write a simple backend service that calls the Python code from [Part 4](/post/2023-03-04-ai-web-app),
and returns a result in some readable form, such as JSON.

# Backend

For our case, we only want to expose the `search` function (see [Part 4](/post/2023-03-04-ai-web-app) for details).

The first thing to do is to [add our dependencies to `pyproject.toml`](https://hatch.pypa.io/latest/config/metadata/#dependencies).
We want to add `fastapi` and `uvicorn` to our config file.
[FastAPI](https://fastapi.tiangolo.com) allows us to write Python code to define the API with type hints (what goes in and what comes out),
as well as the Python logic to be executed (what happens in between).
[Uvicorn](https://www.uvicorn.org/) will allow us to actually serve our Python code as an HTTP service.

Let's start by making a new file `app.py` in the root of our project.
In this file, we define a function `search_endpoint` which receives a query and calls our `search` function to
make the query in our database.

```python
from pathlib import Path
from typing import List

from fastapi import FastAPI
from loguru import logger

from ai_web_app.main import Result, index_embeddings, search

app = FastAPI()

logger.info("Indexing database...")
database_path = Path("./articles.sqlite")
embeddings = index_embeddings(database_path)


@app.get("/search")
async def search_endpoint(query: str, topn: int = 5) -> List[Result]:
    results = search(embeddings, database_path, query, topn=topn)
    return results

```

To make this easier to run, we should also add a new script to our `pyproject.toml` file
(add the following script to the appropriate section)

```toml
[tool.hatch.envs.default.scripts]
serve = "uvicorn app:app --port 8080"
```

and then we can simply run this command to start the server:

```bash
hatch run serve
```

Running that command should index our database (took ~10s on my computer) and start serving
requests on port `8080`.

We can test how this looks like by running in another terminal window

```bash
curl -X GET "http://127.0.0.1:8080/search?query=symptoms%20of%20covid"
```

If you've been following so far, this is what you should get as a response
![image.png](/assets/ai-web-app/image_1674756005367_0.png)

# Integration Tests

To make sure our backend endpoint behaves as expected even if we change it in the future,
we should set up some simple [integration tests](https://en.wikipedia.org/wiki/Integration_testing).

For this, we will use [pytest-xprocess](https://pytest-xprocess.readthedocs.io), launch our server as a
separate process, make some simple requests, and assert that the response is as expected.

First of all, we need to add `pytest-xprocess` to our dev dependencies in `pyproject.toml`.

After that, we create a file `tests/test_app.py`:

```python
import pytest
import requests
from xprocess import ProcessStarter


class TestPythonServer:
    @pytest.fixture(scope="module")
    def server(self, xprocess):
        class Starter(ProcessStarter):
            timeout = 600
            pattern = "Application startup complete"
            args = ["hatch", "run", "serve"]

        xprocess.ensure("server", Starter)
        url = "http://127.0.0.1:8080"
        yield url

        xprocess.getinfo("server").terminate()

    @pytest.mark.integration
    def test_valid_search(self, server):
        topn = 5
        response = requests.get(
            server + "/search", params={"query": "symptoms of covid", "topn": topn}
        )

        assert response.status_code == 200
        assert len(response.json()) == topn

    @pytest.mark.integration
    def test_empty_search(self, server):
        response = requests.get(server + "/search")

        assert response.status_code == 422

```

This file defines a class with 3 methods:

- `server` starts up our web server, and waits for it to be available.
  It's configured to wait until it sees the words `"Application startup complete"` in the output of
  `hatch run serve`.
- `test_valid_search` makes a simple request and assures that the response is valid, and has the correct
  number of requested results.
  You could spend a lot of time here coming up with better test cases, but for this simple example there's
  not that much that can go wrong.
- `test_empty_search` tests a case where the incoming request doesn't have any query.
  In this case, the server should return an error.
  It is important to verify your assumptions of the behavior for failing cases, so you don't have silent errors
  in your system.

You can also add this as a new script in the `pyproject.toml` file:

```toml
[tool.hatch.envs.default.scripts]
integration = "no-cov -m integration -x"
```

One final thing for the integration tests: typically these tests are quite slow to run, as they require
an often lengthy setup process.
As such, we shouldn't run these tests every time.

In the file above, we marked the tests with `@pytest.mark.integration`.
This allows us to configure pytest to not run those tests by default.
We can do that by adding this to our `pyproject.toml` file:

```toml
[tool.pytest.ini_options]
addopts = "--strict-markers -m \"not integration\""
markers = [
    "integration: slow tests, shouldn't be run so often"
]
```

Now we can differentiate between running unit tests (`hatch run cov`) and integration tests (`hatch run integration`).

{{% callout note %}}
If you've been following these instructions, your code should look like this:
https://github.com/dcferreira/ai-web-app/tree/1ac21850a30e8d5efacba162ba8e7eefec08b8ac
{{% /callout %}}

To continue this tutorial, go to [Part 6](/post/2023-03-06-ai-web-app).
