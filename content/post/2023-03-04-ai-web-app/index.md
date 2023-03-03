---
title: Making and Deploying an AI Web App in 2023 (Part 4)
subtitle: Setup simple test cases for your Python code

# Summary for listings and search engines
summary: Making and Deploying an AI Web App in 2023 (Part 4) -- Setup simple test cases for your Python code.

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
{{% /callout %}}

Often a good practice when programming is to start by setting up some unit tests which your code will have to pass.
This forces you to think about what functions you actually need to write, and if you're part of a team, helps
ensure that everyone is one the same page.
For a smaller project that you do not which to maintain for a long time, this step can be skipped.
But be warned: your project _will_ eventually break if used enough, and unit tests will likely be your only warning.

# Setup

In our example we have a database with articles about COVID-19.
For our unit tests, we want to use the same data, but a lot smaller so tests run fast, and we have control of the outputs.

So the first step is to make some simple examples.
The database has a `articles` table and a `sections` table.
I opened up the database and extracted 2 articles, and a couple of sections from each of them.
It's important here that you choose examples that would help you.
In this case I chose 2 sections from each article: a Title and an Abstract, because the Titles won't be indexed in
our usecase (see [Part 2](/post/2023-03-02-ai-web-app) of this series for details).
I also chose sections that are different enough among themselves that I can make a query for each of them, and
verify that the result is the section I wanted.

Put these 2 files in `tests/assets`:

- `articles.csv`:

  ```csv
  id,source,published,publication,authors,title,tags,design,size,sample,method,reference,entry
  071hvhme,WHO,2020-01-01 00:00:00,MSMR,"Kebisek, Julianna; Forrest, Lanna J; Maule, Alexis L; Steelman, Ryan A; Ambrose, John F","Special report: Prevalence of selected underlying health conditions among active component Army service members with coronavirus disease 2019, 11 February-6 April 2020",COVID-19,0,873,"COVID-19 has been a reportable condition for the Department of Defense since 5 February 2020, and, as of the morning of 6 April, a total of 873 cases were reported to the Disease Reporting System internet from Army installations.",,https://doi.org/,2020-06-07
  08as6hga,WHO,2020-01-01 00:00:00,Int. j. sports med,"Stokes, Keith A; Jones, Ben; Bennett, Mark; Close, Graeme L; Gill, Nicholas; Hull, James H; Kasper, Andreas M; Kemp, Simon P T; Mellalieu, Stephen D; Peirce, Nicholas; Stewart, Bob; Wall, Benjamin T; West, Stephen W; Cross, Matthew",Returning to Play after Prolonged Training Restrictions in Professional Collision Sports,COVID-19,0,,The COVID-19 pandemic in 2020 has resulted in widespread training disruption in many sports.,,https://doi.org/,2020-06-08
  ```

- `sections.csv`:
  ```csv
  id,article,tags,design,name,text,labels
  1749694,071hvhme,COVID-19,0,TITLE,"Special report: Prevalence of selected underlying health conditions among active component Army service members with coronavirus disease 2019, 11 February-6 April 2020",FRAGMENT
  1749697,071hvhme,COVID-19,0,ABSTRACT,"COVID-19 has been a reportable condition for the Department of Defense since 5 February 2020, and, as of the morning of 6 April, a total of 873 cases were reported to the Disease Reporting System internet from Army installations.",SAMPLE_SIZE
  1867855,08as6hga,COVID-19,0,TITLE,Returning to Play after Prolonged Training Restrictions in Professional Collision Sports,FRAGMENT
  1867856,08as6hga,COVID-19,0,ABSTRACT,The COVID-19 pandemic in 2020 has resulted in widespread training disruption in many sports.,SAMPLE_SIZE
  ```

You should also setup some fixtures to access these files.
The original data is a SQLite file with 2 tables, so we need to write a fixture that reads the CSV files and
generates a SQLlite file.

Create a `tests/conftest.py` file:

```python
import sqlite3
from pathlib import Path
from tempfile import NamedTemporaryFile

import pandas as pd
import pytest


@pytest.fixture(scope="module")
def assets_path():
    return Path(__file__).resolve().parent / "assets"


@pytest.fixture(scope="module")
def articles_database(assets_path) -> Path:
    with NamedTemporaryFile() as f:
        conn = sqlite3.connect(f.name)

        # load CSVs
        articles = pd.read_csv(assets_path / "articles.csv", sep=",")
        sections = pd.read_csv(assets_path / "sections.csv", sep=",")

        # write CSVs to DB
        articles.to_sql("articles", conn, index=False)
        sections.to_sql("sections", conn, index=False)

        yield Path(f.name)

```

You can then use these fixtures for all your tests.

# Writing tests

We can already make a skeleton of what we want, before actually starting programming.
Let's then make a `ai_web_app/main.py` file, where we implement the functions that we used in [Part 2](/post/2023-03-02-ai-web-app):

```python
from pathlib import Path

from txtai.embeddings import Embeddings


def index_embeddings(database: Path) -> Embeddings:
    raise NotImplementedError


def search(embeddings: Embeddings, database: Path, query: str, topn: int = 5):
    raise NotImplementedError

```

If you use an IDE, it will start complaining that `txtai` isn't installed.
That's because we haven't yet added it to our package dependencies.
So open up `pyproject.toml` and add the latest version as a dependency:

```toml
dependencies = [
  "txtai[similarity]==5.3.0",
]
```

Now we're ready to implement our tests.
At this stage we just need some very simple tests, to make sure the indexing and
querying are doing what we expect.

Let's create a new file `tests/test_main.py` and add this:

```python
import pytest

from ai_web_app.main import index_embeddings, search


@pytest.fixture
def example_embeddings(articles_database):
    return index_embeddings(articles_database)


def test_index_embeddings_count(example_embeddings):
    assert example_embeddings.count() == 2


def test_search(example_embeddings, articles_database):
    res_sports = search(example_embeddings, articles_database, "sports", 1)
    assert res_sports[0].id == "08as6hga"

    res_military = search(example_embeddings, articles_database, "army", 1)
    assert res_military[0].id == "071hvhme"

```

There are 2 tests there:

- `test_index_embeddings_count` tests the size of the generated index.
  We expect the index to have only 2 entries, as sections of type `TITLE` are not indexed.
- `test_search` tests the correction of the queries.
  We have one example about sports and another about the army, and in this text we just make
  sure that the correct section is returned for each query.

# Testing

To run the tests use

```bash
hatch run cov
```

This command will fail with `NotImplementedError`, as the functions we're testing aren't yet implemented.
If you're following along and got a different error, check the link below for the full code.

{{% callout note %}}
If you've been following these instructions, your code should look like this:
https://github.com/dcferreira/ai-web-app/tree/0d29f2c81a628fb5e856f7da5314995a6bf601bf
{{% /callout %}}
