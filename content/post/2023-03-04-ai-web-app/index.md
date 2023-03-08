---
title: Making and Deploying an AI Web App in 2023 (Part 4)
subtitle: Develop an App with Test-driven Development

# Summary for listings and search engines
summary: How and why we should use pytest and unit tests to facilitate development when writing an AI App.

# Link this post with a project
projects: []

# Date published
date: "2023-03-08T00:04:00Z"

# Date updated
lastmod: "2023-03-08T00:04:00Z"

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
our use case (see [Part 2](/post/2023-03-02-ai-web-app) of this series for details).
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
generates a SQLite file.

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

# Implementing Functions

We now need to implement the functions `index_embeddings` and `search` that pass the tests above.
We take most of the code from [Part 2](/post/2023-03-02-ai-web-app), but change a couple of things to
make it more production-ready.
The biggest changes we do in this case are: (1) moving away from using `print` towards using a proper
logging library ([loguru](https://loguru.readthedocs.io/en/stable/) in this case), and (2) removing the dependency
on the `pandas` library, as it wasn't really necessary and in this case will only add overhead to our functions.

The first thing to do is to add `loguru` to the dependencies section of our `pyproject.toml`.
We can then add the imports we need for `index_embeddings`:

```python
import sqlite3
from pathlib import Path

import regex as re
from loguru import logger
from txtai.embeddings import Embeddings
from txtai.pipeline import Tokenizer
```

and the function itself is copied from [Part 2](/post/2023-03-02-ai-web-app), but using `loguru` instead of `print`

```python
def index_embeddings(database: Path) -> Embeddings:
    def stream():
        # Connection to database file
        db = sqlite3.connect(database)
        cur = db.cursor()

        # Select tagged sentences without a NLP label.
        # NLP labels are set for non-informative sentences.
        cur.execute(
            "SELECT Id, Name, Text FROM sections "
            "WHERE (labels is null or labels NOT IN ('FRAGMENT', 'QUESTION')) "
            "AND tags is not null"
        )

        count = 0
        for row in cur:
            # Unpack row
            uid, name, text = row

            # Only process certain document sections
            if not name or not re.search(
                r"background|(?<!.*?results.*?)discussion|introduction|reference",
                name.lower(),
            ):
                # Tokenize text
                tokens = Tokenizer.tokenize(text)

                document = (uid, tokens, None)

                count += 1
                if count % 1000 == 0:
                    logger.debug(f"Streamed {count} documents")

                # Skip documents with no tokens parsed
                if tokens:
                    yield document

        logger.info(f"Iterated over {count} total rows")

        # Free database resources
        db.close()

    # BM25 + fastText vectors
    embeddings = Embeddings(
        {
            "method": "sentence-transformers",
            "path": "all-MiniLM-L6-v2",
            "scoring": "bm25",
        }
    )

    embeddings.index(stream())

    return embeddings
```

For the `search` function, in the notebook version we're outputting a `pandas` DataFrame.
In the notebook that's a nice way to visualize the output of the function, but actually we
don't really the dataframe and can use something simpler.
Furthermore, as this function will be exposed as an API, it would be nice to have a schema
of what the output will actually look like.

For that we can use Python's [dataclasses](https://docs.python.org/3/library/dataclasses.html)
(an alternative would be [pydantic](https://docs.pydantic.dev/)'s `BaseModel`, with more functionality
but requires one more library to be installed).
We can have a look at what fields we want to fetch from our database and return as a result from search
and make our own `Result` class:

```python
from dataclasses import dataclass
from datetime import datetime


@dataclass
class Result:
    id: str
    title: str
    published: datetime
    reference: str
    text: str
    score: float
```

We then change our original `search` function to

```python
def search(
    embeddings: Embeddings, database: Path, query: str, topn: int = 5
) -> List[Result]:
    db = sqlite3.connect(database)
    cur = db.cursor()

    results: List[Result] = []
    for uid, score in embeddings.search(query, topn):
        cur.execute("SELECT article, text FROM sections WHERE id = ?", [uid])
        uid, text = cur.fetchone()

        cur.execute(
            "SELECT Title, Published, Reference from articles where id = ?", [uid]
        )
        res = cur.fetchone()
        results.append(
            Result(
                id=uid,
                title=res[0],
                published=res[1],
                reference=res[2],
                text=text,
                score=score,
            )
        )

    db.close()
    return results
```

Running `hatch run cov` should now show the tests as passing.

{{% callout note %}}
If you've been following these instructions, your code should look like this:
https://github.com/dcferreira/ai-web-app/tree/afe845ca0a5866d4a2d4ef6bb687c31f3384d473
{{% /callout %}}
