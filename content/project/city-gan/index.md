---
title: City-GAN
summary: City-GAN uses Conditional Generative Adversarial Neural Networks to generate pictures of fake buildings, with the architectural characteristics of a specific city.
tags:
- Deep Learning
date: '2019-07-01T00:00:00Z'
image:
  focal_point: Smart
links:
- type: pdf
  url: https://arxiv.org/pdf/1907.05280.pdf
- type: code
  url: https://github.com/muxamilian/city-gan
---

Generative Adversarial Networks (GANs) are a well-known technique that is trained on samples (e.g. pictures
of fruits) and which after training is able to generate realistic new samples. Conditional GANs (CGANs)
additionally provide label information for subclasses (e.g. apple, orange, pear) which enables the GAN to learn
more easily and increase the quality of its output samples. We use GANs to learn architectural features of
major cities and to generate images of buildings which do not exist. We show that currently available GAN
and CGAN architectures are unsuited for this task and propose a custom architecture and demonstrate that
our architecture has superior performance for this task and verify its capabilities with extensive experiments.
