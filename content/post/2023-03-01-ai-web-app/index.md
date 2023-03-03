---
title: Making and Deploying an AI Web App in 2023 (Part 1)
subtitle: The complete guide from 0 to deployment for AI Web Apps

# Summary for listings and search engines
summary: Making and Deploying an AI Web App in 2023 (Part 1) -- The complete guide from 0 to deployment for AI Web Apps.

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

In 2023, the best way to make an AI Web App is to not train any model at all.
Training any model takes quite some effort, and nowadays it is possible to skip that step completely in many cases.
After all, ["the hottest new programming language is English"](https://twitter.com/karpathy/status/1617979122625712128).
There are basically two ways to do that:

- one is to use external APIs, such as [OpenAI](https://openai.com/blog/openai-api), [co:here](https://cohere.ai/), or [AssemblyAI](https://www.assemblyai.com/);
- the alternative is using pre-trained models and running them yourself.

In this blog post series, we will do the 2nd alternative: we will deploy a pre-trained model onto the cloud.
We will use serverless infrastructure for both cost saving and easy scalability.

For this guide, we're assuming you're using Linux (or WSL on Windows), and are comfortable with command line and Python.
For the deployment part we also assume you have a valid credit card, but that part is optional and some alternatives are suggested.
