---
title: Making and Deploying an AI Web App in 2023 (Part 6)
subtitle: Containerize an App with Docker

# Summary for listings and search engines
summary: How to use multi-stage building in Docker to containerize a hatch project with a web app.

# Link this post with a project
projects: []

# Date published
date: "2023-03-08T00:06:00Z"

# Date updated
lastmod: "2023-03-08T00:06:00Z"

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

Now that your web app is working locally, it's time to think about deploying it somewhere.
The easiest way to do that is to make a Docker image, and send it to wherever you want to deploy it.
Having a docker image guarantees that your environment is replicable wherever you take it.

# Build a Docker Image

In our app, we have a 2-stage process: first we build our Python package into a [wheel file](https://pythonwheels.com/),
and then we run the web server.
Therefore, we can use Docker's [multi-stage build feature](https://docs.docker.com/build/building/multi-stage/).
Having this process split into 2 stages enables us to have a minimalist Python environment in the final image.
That is, we install the dev environment in the first stage, but only the minimal needed dependencies in the final image.

This is our `Dockerfile`, which should be in the root of our project:

```Dockerfile
FROM python:3.10 AS builder

# install hatch
RUN pip install --no-cache-dir --upgrade hatch
COPY . /code

# build python package
WORKDIR /code
RUN hatch build -t wheel

FROM python:3.10

# copy wheel package from stage 1
COPY --from=builder /code/dist /code/dist
RUN pip install --no-cache-dir --upgrade /code/dist/*

# copy the serving code and our database
COPY app.py /code
COPY articles.sqlite /code

# run web server
WORKDIR /code
CMD ["uvicorn", "app:app", "--host", "0.0.0.0", "--port", "8080"]

```

{{% callout warning %}}
If your project directory contains many files (or some big ones), building this image might take a long time.
If that is the case, you should make a `.dockerignore` file to avoid having docker run through all your files.
See more about this in [Docker's docs](https://docs.docker.com/engine/reference/builder/#dockerignore-file).
{{% /callout %}}

To simplify our workflow, we should also add a couple of scripts to our `pyproject.toml`, to build and run our
docker image, like these:

```toml
[tool.hatch.envs.default.scripts]
build = "docker buildx build . -t ai-web-app:latest"
serve-docker = ["docker run -p 5000:8080 ai-web-app:latest"]
```

You can then run

```bash
hatch run build
```

and the docker image will be built.
Afterwards, you should run

```bash
hatch run serve-docker
```

which will serve the app in your local port `5000`.

You can again test with curl, by running this command in a new terminal window:

```bash
curl -X GET "http://127.0.0.1:5000/search?query=symptoms%20of%20covid"
```

This request should get the same result as the one in [Part 5](/post/2023-03-05-ai-web-app)

{{% callout note %}}
If you've been following these instructions, your code should look like this:
https://github.com/dcferreira/ai-web-app/tree/1a9b73f17d99536801e6672d6161056d71fa44b4
{{% /callout %}}

# \[Optional\] Optimize the Image

In this case, the container startup is taking around 2 minutes in my machine.
That could be fine if I'm deploying the image in a server I control, and we just run once and forget about it.
However, we want to deploy it to some serverless provider, which means the container will need to startup
from scratch more or less for each request.

Therefore, we should have a look at what we can do to minimize this startup time.
In our app, every time the container starts, the database is being indexed.
Another thing that takes time is downloading the `all-MiniLM-L6-v2` model (see [Part 2](/post/2023-03-02-ai-web-app)),
which is also happening every time the container starts.

There is an easy fix in this case: since the model and the database will be the same for all containers
we launch, we should just move these 2 steps to the build process.
The build process will then take a bit more time, but the startup will be almost instant.

Indeed this is [what we did in this commit](https://github.com/dcferreira/ai-web-app/commit/92099b561bc4e8db3d567244cebf2e7eb1a2df56).
After doing those optimizations, the startup is now almost instant.
