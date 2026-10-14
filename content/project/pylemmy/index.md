---
title: pylemmy
summary: A Python client for the Lemmy API, and a bot built on it that summarizes news articles.
tags:
  - NLP
date: "2023-06-19T00:00:00Z"

# Optional external URL for project (replaces project detail page).
external_link: ""

url_code: "https://github.com/dcferreira/pylemmy"
url_pdf: ""
url_slides: ""
url_video: ""

slides: ""
---

[pylemmy](https://pypi.org/project/pylemmy/) enables simple access to the API of [Lemmy](https://join-lemmy.org/), the federated link aggregator, from Python.

On top of it I built [lemmy-summarizer](https://github.com/dcferreira/lemmy-summarizer), a Lemmy bot that summarizes the news articles
linked in posts, using [trafilatura](https://github.com/adbar/trafilatura) to extract the article text and a Hugging Face transformers model to summarize it.
