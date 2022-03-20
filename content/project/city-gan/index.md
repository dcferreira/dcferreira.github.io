---
title: City-GAN
summary: City-GAN uses Conditional Generative Adversarial Neural Networks to generate pictures of fake buildings, with the architectural characteristics of a specific city.
tags:
  - Deep Learning
date: "2019-07-01T00:00:00Z"

# Optional external URL for project (replaces project detail page).
external_link: ""

image:
  caption: ""
  focal_point: Smart

url_code: "https://github.com/muxamilian/city-gan"
url_pdf: "https://arxiv.org/pdf/1907.05280.pdf"
url_slides: ""
url_video: ""

# Slides (optional).
#   Associate this project with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides = "example-slides"` references `content/slides/example-slides.md`.
#   Otherwise, set `slides = ""`.
slides: ""
---

Generative Adversarial Networks (GANs) are a well-known technique that is trained on samples (e.g. pictures
of fruits) and which after training is able to generate realistic new samples. Conditional GANs (CGANs)
additionally provide label information for subclasses (e.g. apple, orange, pear) which enables the GAN to learn
more easily and increase the quality of its output samples. We use GANs to learn architectural features of
major cities and to generate images of buildings which do not exist. We show that currently available GAN
and CGAN architectures are unsuited for this task and propose a custom architecture and demonstrate that
our architecture has superior performance for this task and verify its capabilities with extensive experiments.
