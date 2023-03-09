---
title: Making and Deploying an AI Web App in 2023 (Part 1)
subtitle: A Complete Guide from 0 to Deployment for AI Web Apps

# Summary for listings and search engines
summary: A complete guide to developing and deploying an AI Web App with modern tools.

# Link this post with a project
projects: []

# Date published
date: "2023-03-08T00:01:00Z"

# Date updated
lastmod: "2023-03-08T00:01:00Z"

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

Did you hear the news?
The days of training your own Machine Learning models are over!
In 2023, the best way to make an AI Web App is to not train any model at all.
After all, ["the hottest new programming language is English"](https://twitter.com/karpathy/status/1617979122625712128).
Now it's easier and cheaper than ever to make an AI Web App.

Thanks to communities such [HuggingFace models](https://huggingface.co/models)
and new popular APIs (such as [OpenAI](https://openai.com/blog/openai-api),
[co:here](https://cohere.ai/), or [AssemblyAI](https://www.assemblyai.com/)),
we now have access to lots of high-quality pre-trained models.

In this blog post series, we will go over how to develop an AI Web App
using modern Python tools, and deploy it almost for free on the cloud using
serverless infrastructure.
We will take an example of a simple AI search engine in Python, and take it all
the way from concept to deployment.
This tutorial will hold your hand through the whole process with lots of code and
commands, and give you resources to look at when your use case differs.

For this guide, we're assuming you're using Linux (or WSL on Windows), and are comfortable with command line and Python.
For the deployment part we also assume you have a valid credit card, but that part is optional and some alternatives are suggested.

Here is the full table of contents:

1. [Introduction (this page)](/post/2023-03-01-ai-web-app)
2. [Make a Proof of Concept for an AI App](/post/2023-03-02-ai-web-app)
3. [Setup the Python Project with Modern Tools](/post/2023-03-03-ai-web-app)
4. [Develop an App with Test-Driven Development](/post/2023-03-04-ai-web-app)
5. [Implement and Test a Python Backend Server](/post/2023-03-05-ai-web-app)
6. [Containerize an App with Docker](/post/2023-03-06-ai-web-app)
7. [Build a CI/CD Pipeline for an AI App](/post/2023-03-07-ai-web-app)
8. [Deploy a Serverless AI App with Google Cloud](/post/2023-03-08-ai-web-app)
9. [Deploy a Simple Frontend for an AI App](/post/2023-03-09-ai-web-app)

See you in [Part 2](/post/2023-03-02-ai-web-app)!

For comments or questions, use the
[Reddit discussion](https://www.reddit.com/user/dlcferreira/comments/11mv27x/making_and_deploying_an_ai_web_app_in_2023_part_1/)
or reach out to me [directly via email](mailto:daniel.ferreira.1@gmail.com).
