---
title: Toxic News
summary: Ranking online news outlets by the toxicity of their daily headlines, using machine learning.
tags:
- NLP
date: '2023-03-11T00:00:00Z'
links:
- type: code
  url: https://github.com/dcferreira/toxic-news
- type: site
  url: https://toxicnews.dcferreira.com
---

A website enabling automatic ranking of online media outlets using machine learning models (for now, only using Unitary.ai's [detoxify model](https://github.com/unitaryai/detoxify)).

Once per day, the headlines from the frontpage of multiple online media outlets are scraped, and sent to machine learning models. The results are displayed in the website.

It runs entirely as a scheduled GitHub Actions job, with git as the only store: no database and no server.

Check out the website at https://toxicnews.dcferreira.com, and have a look at the [About page](https://toxicnews.dcferreira.com/about.html) for more details
