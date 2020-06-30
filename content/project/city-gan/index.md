+++
title = "City-GAN"
date = 2019-07-01
draft = false

# Tags: can be used for filtering projects.
# Example: `tags = ["machine-learning", "deep-learning"]`
tags = []

# Project summary to display on homepage.
summary = "City-GAN uses Conditional Generative Adversarial Neural Networks to generate pictures of fake buildings, with the architectural characteristics of a specific city."

# Slides (optional).
#   Associate this page with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides = "example-slides"` references 
#   `content/slides/example-slides.md`.
#   Otherwise, set `slides = ""`.
slides = ""

# Optional external URL for project (replaces project detail page).
#external_link = "https://github.com/muxamilian/city-gan"

# Links (optional).
url_pdf = "https://arxiv.org/pdf/1907.05280.pdf"
url_code = "https://github.com/muxamilian/city-gan"
url_dataset = ""
url_slides = ""
url_video = ""
url_poster = ""

# Custom links (optional).
#   Uncomment line below to enable. For multiple links, use the form `[{...}, {...}, {...}]`.
# url_custom = [{icon_pack = "fab", icon="twitter", name="Follow", url = "https://twitter.com"}]

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder. 
[image]
  # Caption (optional)
  caption = ""

  # Focal point (optional)
  # Options: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight
  focal_point = ""
+++

Generative Adversarial Networks (GANs) are a well-known technique that is trained on samples (e.g. pictures
of fruits) and which after training is able to generate realistic new samples. Conditional GANs (CGANs)
additionally provide label information for subclasses (e.g. apple, orange, pear) which enables the GAN to learn
more easily and increase the quality of its output samples. We use GANs to learn architectural features of
major cities and to generate images of buildings which do not exist. We show that currently available GAN
and CGAN architectures are unsuited for this task and propose a custom architecture and demonstrate that
our architecture has superior performance for this task and verify its capabilities with extensive experiments.
