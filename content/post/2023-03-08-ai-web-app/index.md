---
title: Making and Deploying an AI Web App in 2023 (Part 8)
subtitle: Deploy the serverless backend to Google Cloud

# Summary for listings and search engines
summary: Making and Deploying an AI Web App in 2023 (Part 8) -- Deploy the serverless backend to Google Cloud.

# Link this post with a project
projects: []

# Date published
date: "2023-03-03T00:08:00Z"

# Date updated
lastmod: "2023-03-03T00:08:00Z"

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

This post uses [Google's Cloud Run](https://cloud.google.com/run).
Alternatives would be [AWS's Lambda](https://aws.amazon.com/lambda/), [Azure Functions](https://learn.microsoft.com/en-us/azure/azure-functions/functions-overview).
{{% /callout %}}

It's finally time to deploy our web app so that everyone can reach it.
We'll deploy our app in a [serverless](https://en.wikipedia.org/wiki/Serverless_computing) fashion.
This means that every time someone makes a request, a new container will be started, which will respond
to it.
By doing it this way, we avoid wasting money on compute when no one is making requests to our server.
Furthermore, scaling happens automatically: if suddenly our web app gets very popular, our cloud
provider automatically starts as many containers as needed.

In this post we'll do everything with the [`gcloud` CLI](https://cloud.google.com/sdk/gcloud),
but it can all be done with the graphical interface of the
[Google Cloud Console website](https://console.cloud.google.com/).

# Setup

The first step is to [install the `gcloud` CLI](https://cloud.google.com/sdk/docs/install)
and run `gcloud init` to configure your account.

Then we need the following setup steps:

1. Create a new project

   ```bash
   gcloud projects create my-example-webapp-23867 --name="AI Web App"
   ```

   Note that the project ID (`my-example-webapp-23867`) needs to be unique.
   So this exact command won't work for you, you will need to create your own project ID. 2. Configure `gcloud` to use your project:

   ```bash
   gcloud config set project my-example-webapp-23867
   ```

2. Activate the [Cloud Run API](https://cloud.google.com/run/docs/reference/rest)

   ```bash
   gcloud services enable run.googleapis.com
   ```

3. Activate the [Artifact Registry](https://cloud.google.com/artifact-registry),
   which we will use to upload our docker images.

   ```bash
   gcloud services enable artifactregistry.googleapis.com
   ```

   Note that this step requires that billing is enabled in the project.
   This step is not strictly required: you could upload the docker images to anywhere else.
   The [docker hub](https://hub.docker.com/) would be a good alternative.

   To activate billing for the project, first set up a billing account on the [Google Cloud Console](https://console.cloud.google.com/).
   Using the CLI, you can list the existing billing accounts:

   ```bash
   gcloud billing accounts list
   ```

   You can then link the project to a specific billing account

   ```bash
   gcloud billing projects link my-example-webapp-23867 --billing-account 0X0X0X-0X0X0X-0X0X0X
   ```

   (don't forget to replace the billing account ID by your own)

4. Create a new artifacts repository

   ```bash
   gcloud artifacts repositories create ai-web-app-artifacts --repository-format=docker --location=us-central1 --description="Docker artifacts"
   ```

   Choose the location that is closer to you and your users.

   Setup docker authentication for this repo with

   ```bash
   gcloud auth configure-docker us-central1-docker.pkg.dev
   ```

   If you chose something else than `us-central1` for the location in the last step, be sure to reflect that in this auth command.
   If your region is `europe-west1`, you would instead run

   ```bash
   gcloud auth configure-docker europe-west1-docker.pkg.dev
   ```

# Deploy

By this point, we've done all the necessary setup to deploy our app.
Now all that's left is to upload our docker image and start our serverless function.

We should add a helper script in `pyproject.toml` to push our image to Google Cloud:

```toml
[tool.hatch.envs.default.scripts]
push = [
    "docker tag ai-web-app:latest us-central1-docker.pkg.dev/my-example-webapp-23867/ai-web-app-artifacts/ai-web-app:latest",
    "docker push us-central1-docker.pkg.dev/my-example-webapp-23867/ai-web-app-artifacts/ai-web-app:latest"
]
```

We can then build and push our docker image

```bash
hatch run build && hatch run push
```

And finally we can deploy our serverless function (when asked, choose unauthenticated access)

```bash
gcloud run deploy ai-web-app --image="us-central1-docker.pkg.dev/my-example-webapp-23867/ai-web-app-artifacts/ai-web-app:latest" --region=us-central1 --memory=2GiB
```

The output of this deploy command will give you a Service URL for your app, which you should use
to access it.

The app should now be up (if it's not, have a look at the Cloud Run logs for errors),
and you can reach it with

```bash
curl -X GET "https://<SERVICE-URL>/search?query=risk%20factors"
```

{{% callout warning %}}
About keeping your budget low:

- If you setup your Cloud Run function without authentication, it's a good idea
  to set [budget limits](https://cloud.google.com/billing/docs/how-to/budgets)
  and avoid unpleasant billing surprises.
- If you're pushing many images to the Artifact Registry, the costs can also quickly
  add up.
  You should regularly delete old images, or create a
  [cleanup policy](https://cloud.google.com/artifact-registry/docs/repositories/cleanup-policy).

{{% /callout %}}
