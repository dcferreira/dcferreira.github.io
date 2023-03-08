---
title: Making and Deploying an AI Web App in 2023 (Part 7)
subtitle: Build a CI/CD Pipeline for an AI App

# Summary for listings and search engines
summary: How to develop a simple CI/CD pipeline for testing your code with GitHub Actions.

# Link this post with a project
projects: []

# Date published
date: "2023-03-08T00:07:00Z"

# Date updated
lastmod: "2023-03-08T00:07:00Z"

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

This post uses [GitHub Actions](https://docs.github.com/en/actions).
Possible alternatives are: [GitLab Pipelines](https://docs.gitlab.com/ee/ci/pipelines/), [Jenkins](https://www.jenkins.io/),
[pypyr](https://pypyr.io/), and [many others](https://github.com/pditommaso/awesome-pipeline).
{{% /callout %}}

Now that we have a first version of our app working, it's time to setup a CI pipeline.
In this post, we'll make a very simple pipeline which runs linting and unit/integration tests.
We'll use [GitHub Actions](https://docs.github.com/en/actions) for this post.

# Setup CI Pipeline

The first thing we need to do is create a file `.github/workflows/backend.yaml` on the root of our repository:

```yaml
name: Backend CI

on: push

jobs:
  run:
    name: Run on Python ${{ matrix.python-version }} (${{ matrix.os }})
    runs-on: ${{ matrix.os }}
    strategy:
      matrix:
        python-version: ["3.10"] # can use other python versions
        os: ["ubuntu-latest"] # can test in other OS

    steps:
      - uses: actions/checkout@v3

      - name: Check for CRLF endings
        uses: erclu/check-crlf@v1

      - name: Set up Python ${{ matrix.python-version }}
        uses: actions/setup-python@v4
        with:
          python-version: ${{ matrix.python-version }}

      - name: Install hatch
        run: python -m pip install -U pip hatch

      - name: Lint
        run: hatch run lint

      - name: Test
        run: hatch run cov
```

The file above will run a job that clones our repo, checks if all files have
[LF line endings](https://adaptivepatchwork.com/2012/03/01/mind-the-end-of-your-line/)
(if you're using Windows to develop, that can be a common issue),
installs Python and hatch, and runs our linter and unit tests.

Using the hatch scripts (see [Part 3](/post/2023-03-03-ai-web-app) for details) greatly
simplifies the pipeline.
With the scripts, it's easy to make sure that we run the same command locally as on the
CI pipeline, since all the parameters are in the config file.

We also want to run the integration tests.
For that, we will need to download our database (or, in a real world case, a subset of our real database).
This download will take some time, and the integration tests themselves are also heavy.
Therefore, we don't want to run this job for every commit.
In this case, we want to limit to only commits in Pull Requests, or in the `main` branch.

This is our job configuration (just append to `.github/workflows/backend.yaml`):

```yaml
integration:
  runs-on: ubuntu-latest
  # only run on pull requests and main branch
  if: ${{ github.event_name == 'pull_request' || github.ref == 'refs/heads/main' }}
  steps:
    - uses: actions/checkout@v3

    - name: Set up Python 3.10
      uses: actions/setup-python@v4
      with:
        python-version: "3.10"

    - name: Download database
      run: |
        wget https://github.com/neuml/txtai/releases/download/v1.1.0/tests.gz
        mv tests.gz articles.sqlite.gz
        gunzip articles.sqlite

    - name: Install hatch
      run: python -m pip install hatch

    - name: Run integration tests
      run: hatch run integration
```

On your first setup of the CI pipeline, it's very common that something doesn't behave as
you expected, since you're running the code on a different machine for the first time (even if the same code!).
In my case, I had to make [some small changes to my code to get it working](https://github.com/dcferreira/ai-web-app/compare/92099b561bc4e8db3d567244cebf2e7eb1a2df56..b91b29a4baf45969446068c1b2b53913b5f933ee).

Having this pipeline always running is a pretty good start, as it ensures that tests are
run regularly and you're notified if they fail.
For next steps in this pipeline, you can have a look at
[building/pushing docker images](https://github.com/marketplace/actions/build-and-push-docker-images)
(such as the one we built in [Part 6](/post/2023-03-06-ai-web-app)).

{{% callout note %}}
If you've been following these instructions, your code should look like this:
https://github.com/dcferreira/ai-web-app/tree/b91b29a4baf45969446068c1b2b53913b5f933ee
{{% /callout %}}

To continue this tutorial, go to [Part 8](/post/2023-03-08-ai-web-app).
