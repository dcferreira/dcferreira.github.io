---
# An instance of the Experience widget.
# Documentation: https://wowchemy.com/docs/page-builder/
widget: experience

# This file represents a page section.
headless: true

# Order that this section appears on the page.
weight: 40

title: EXPERIENCE
subtitle:

# Date format for experience
#   Refer to https://wowchemy.com/docs/customization/#date-format
date_format: Jan 2006

# Experiences.
#   Add/remove as many `experience` items below as you like.
#   Required fields are `title`, `company`, and `date_start`.
#   Leave `date_end` empty if it's your current employer.
#   Begin multi-line descriptions with YAML's `|2-` multi-line prefix.
experience:
  - title: Staff AI Engineer
    company: Unitary
    company_url: "https://www.unitary.ai"
    location: Remote
    date_start: "2025-09-01"
    description: |2-
        Technical lead for how Unitary builds AI agents: the platform they run on, the workflow format they're generated from, and the decision engine they use.

        * Co-founded Agent Factory, the platform every Unitary customer agent is built and run on, drastically cutting new-customer delivery effort.
        * Spearheaded the workflow design format, a structured spec of each customer's process that serves as the single source of truth agents are generated from.
        * Led the team that built a shared decision engine with automated prompt optimisation, now standard for every new customer with automated decisions.
        * Drove company-wide adoption of AI coding agents, from a few early adopters to every engineer.

  - title: Senior Machine Learning Engineer
    company: Unitary
    company_url: "https://www.unitary.ai"
    location: Remote
    date_start: "2024-02-01"
    date_end: "2025-08-31"
    description: |2-
        Led new-customer AI deliveries and helped shape Unitary's move from ML classifiers to AI agents.

        * Authored Automation Rate as the company's core measure of value, and the product vision behind its pivot to AI agents.
        * Led the team that built Trustpilot's AI agents, cutting manual moderation effort by 78% and making decisions 4x faster ([case study](https://www.unitary.ai/customer-stories/trustpilot)).
        * Took a global gaming company's content moderation from manual review to its first production LLM system.
        * Delivered AI automation for four more customers, including +19% customer satisfaction for [Attune](https://www.unitary.ai/customer-stories/attune).

  - title: Machine Learning Engineer
    company: Unitary
    company_url: "https://www.unitary.ai"
    location: Remote
    date_start: "2023-06-28"
    date_end: "2024-01-31"
    description: |2-
        Built the data and evaluation foundations behind Unitary's content-moderation models.

        * Built the ML data pipeline platform (Spark, Delta Lake, AWS) and one-command model training.
        * Set the evaluation standards for custom customer models: gold-standard test sets and traffic-realistic benchmarks that model releases were measured against.
        * Led the internal team that delivered a resale marketplace's image-moderation classifier.

  - title: Career break
    company: Personal projects
    date_start: "2022-10-01"
    date_end: "2023-06-27"
    description: |2-
        Took time off after CYAN, then explored independent machine-learning work: launched [Toxic News](/project/toxic-news/),
        built [Tweet Fake](/project/tweetfake/) at a weekend hackathon, wrote a [nine-part series](/post/) on building and
        deploying an AI web app end to end, and contributed a fix to [detoxify](https://github.com/unitaryai/detoxify).

  - title: Machine Learning Engineer
    company: CYAN Security
    company_url: "https://cyansecurity.com"
    # company_logo: org-gc
    location: Vienna, Austria
    date_start: "2019-06-01"
    date_end: "2022-09-30"
    description: |2-
        Built the machine-learning systems that classified the web for CYAN's content-filtering products.

        * Led modelling for a multimodal (text and image) website classifier covering 100+ languages, which found over 10x more harmful sites than the system it replaced.
        * Built the Spark/Databricks pipelines on AWS feeding it ~100k websites a day, plus a serverless scraper fetching millions of pages daily.
        * Built a domain-embedding model over DNS traffic for phishing detection.
        * Stepped in as interim ML team lead (4 engineers), and represented CYAN in an FFG-funded research collaboration with SBA Research and Politecnico di Torino.

  - title: Researcher
    company: TU Wien
    company_url: "https://www.cn.tuwien.ac.at/"
    # company_logo: org-x
    location: Vienna, Austria
    date_start: "2016-08-01"
    date_end: "2019-02-28"
    description: |2-
        I was part of the [BigDAMA project](https://bigdama.ait.ac.at/), a fundamental research project focused on big data analytics for network traffic monitoring and analysis.

        My research topics are mostly related to how to represent network traffic for detecting attacks at the
        network level (i.e., which features to use). Experimented with both classical features (what have people
        been using and why?), and feature learning approaches using Deep Learning techniques, and in particular
        representing traffic in 2-dimensional spaces.

  - title: Junior Researcher
    company: Priberam
    company_url: "https://www.priberam.pt"
    # company_logo: org-x
    location: Lisbon, Portugal
    date_start: "2016-03-01"
    date_end: "2016-07-31"
    description: |2-
        I was part of the [SUMMA project](https://www.youtube.com/watch?v=OkLwDI-CvYM), an H2020 project for media monitoring
        with multiple industry and academic international partners.

        I specifically tackled the problem of entity linking in which, given a text with some entity mentions
        (e.g., David and Victoria), the goal is to find the corresponding entities in Wikipedia (e.g., David Beckham and
        Victoria Beckham). We used both classical methods (such as SVM) and modern Deep Learning approaches.

design:
  columns: "2"
---
