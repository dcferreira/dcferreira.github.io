# Daniel C. Ferreira — CV (long form)

Staff AI Engineer based in Vienna, Austria. I build AI agents and the platforms they run on, and make them reliable enough to automate real work.
Background in applied mathematics; working on machine learning since 2016, in research (ACL, NeurIPS, IJCNN) and in industry.

- Site: https://dcferreira.com
- GitHub: https://github.com/dcferreira
- LinkedIn: https://www.linkedin.com/in/daniel-c-ferreira-355aa677
- Google Scholar: https://scholar.google.com/citations?user=4PusPA4AAAAJ
- Email: daniel@dcferreira.com

This version of my CV is written for LLMs and AI agents: it is complete, uses plain Markdown and has no page limit, so an assistant asked about me gets the full picture.
People are welcome to read it too, but the human-friendly version is the shorter PDF CV I send with applications.
A short index of this site is at https://dcferreira.com/llms.txt.

## What I'm looking for

- **Roles:** senior or staff AI engineer / agent engineer / AI platform engineer: building agent systems, agent harnesses and tooling, evaluation, and the platforms around them.
- **Location:** remote within Europe, or hybrid in Vienna, Austria.
- **Companies:** small and mid-sized product companies and startups where engineering, ML and product overlap.

## Summary

- **AI agents in production.** Co-founded the platform every customer AI agent at Unitary is built and run on, and led the team behind its shared decision engine with automated prompt optimisation.
- **Delivery with measured outcomes.** Led AI-agent deliveries for content-moderation and customer-support workflows: e.g. 78% less manual moderation effort and 4x faster decisions for Trustpilot.
- **AI coding agents.** Drove company-wide adoption of AI coding agents at Unitary (median engineering throughput up 2.5x); in my own time I build tooling for coding agents (agent-pawl, a workflow engine for Claude Code).
- **ML engineering foundations.** Data pipelines (Spark, Delta Lake, Databricks, AWS), model training and evaluation standards, multilingual NLP and multimodal classification at scale.
- **Research.** Six-plus peer-reviewed papers in NLP and ML for network security; one of the first published sets of pre-trained multilingual word embeddings.
- **Product.** Authored Automation Rate, Unitary's core measure of customer value, and the product vision behind its pivot from ML classifiers to AI agents.

## Experience

### Unitary — Remote — Jun 2023 – Present
AI for content moderation and trust & safety, which pivoted to AI agents that automate customers' operational workflows. https://www.unitary.ai

Title progression: Machine Learning Engineer → Senior Machine Learning Engineer → Staff AI Engineer (internal IC4/IC5 levels).

#### Staff AI Engineer — Sep 2025 – Present
Technical lead for how Unitary builds AI agents: the platform they run on, the workflow format they're generated from, and the decision engine they use.

- Co-founded **Agent Factory**, the platform every Unitary customer agent is built and run on, cutting new-customer delivery effort from ~200 person-days to ~5.
- Spearheaded the **workflow design format**: a structured spec of each customer's process that serves as the single source of truth agents are generated from.
- Led the team that built a **shared decision engine with automated prompt optimisation**, now standard for every new customer with automated decisions.
- Raised median engineering throughput **2.5x** by driving company-wide adoption of **AI coding agents**, from a few early adopters to every engineer.

Technologies: Python, LLMs, Claude Code, MCP, DSPy, Temporal, Kubernetes, AWS, Docker.

#### Senior Machine Learning Engineer — Feb 2024 – Aug 2025
Led new-customer AI deliveries and helped shape Unitary's move from ML classifiers to AI agents.

- Shaped product direction and go-to-market: authored **Automation Rate** as the company's core measure of value, and the product vision behind its pivot to AI agents.
- Cut Trustpilot's manual moderation effort by **78%** and made decisions **4x faster** by leading the team that built its AI agents ([case study](https://www.unitary.ai/customer-stories/trustpilot)).
- Moved a global gaming company's content moderation from manual review to AI automation, taking its **first LLM system to production** with ~43% of tasks automated at ~98% accuracy.
- Delivered AI automation for four more customers, including **+19% customer satisfaction** for Attune ([case study](https://www.unitary.ai/customer-stories/attune)) and 57% more content auto-approved for a newsletter platform.

Technologies: Python, LLMs, model evaluation, Temporal, Kubernetes, AWS, PyTorch.

#### Machine Learning Engineer — Jun 2023 – Jan 2024
Built the data and evaluation foundations behind Unitary's content-moderation models.

- Built the **ML data pipeline platform** (Spark, Delta Lake, AWS) and one-command model training, letting the team train more models in one week than in the previous four years.
- Set the **evaluation standards** for custom customer models: gold-standard test sets and traffic-realistic benchmarks that model releases were measured against.
- Led the internal team that delivered a resale marketplace's **image-moderation classifier**, handling ~700k images a day.

Technologies: Python, PyTorch, Apache Spark, Delta Lake, AWS.

### Career break — Oct 2022 – Jun 2023
Took time off after CYAN, then explored independent machine-learning work.

- Launched [Toxic News](https://toxicnews.dcferreira.com), which scores the daily headlines of online news outlets for toxicity using Unitary's open-source detoxify model.
- Built Tweet Fake at a weekend hackathon: an LLM app that generated new tweets in a given user's style.
- Wrote a [nine-part blog series](https://dcferreira.com/post/) on building and deploying an AI web app end to end: FastAPI, Docker, CI/CD and serverless deployment on Google Cloud.

### CYAN Security — Machine Learning Engineer — Vienna, Austria — 2019 – Sep 2022
Built the machine-learning systems that classified the web for a cybersecurity vendor's content-filtering products. https://cyansecurity.com

- Led modelling for a **multimodal (text and image) website classifier covering 100+ languages**, finding over 10x more harmful sites than the system it replaced (ensemble of BERT-based models; TensorFlow, transformers).
- Built the **Spark and Databricks pipelines on AWS** that fed it ~100k websites a day, plus a **serverless scraper** (GCP, Azure) fetching millions of pages daily.
- Initiated and built a **domain-embedding model over DNS traffic** that detected phishing-related domains at ~97% F1.
- Deployed NLP and image models to production with CI/CD and MLOps practices (Docker, BentoML, FastAPI), and defined a unified REST API for the in-house models.
- Set up data-labelling infrastructure for external partners (Label Studio, MTurk).
- Stepped in as **interim ML team lead** (Aug – Sep 2022), managing 4 engineers and the company's academic research partnerships.
- Represented CYAN in an Austrian government-funded (FFG) research collaboration with SBA Research and Politecnico di Torino on IoT and internet-scale anomaly detection.

### TU Wien — Researcher, Big-DAMA project — Vienna, Austria — Aug 2016 – Feb 2019
Fundamental research on big-data analytics for network traffic monitoring. https://www.cn.tuwien.ac.at/

- Published 6 research papers on ML methods for cybersecurity and related topics.
- Researched how to represent network traffic for attack detection: classical features vs. learned representations, including 2D visualisation of traffic flows with autoencoders.
- Launched and ran NTARC, a community effort to catalogue network-traffic-analysis research, with its own tooling (Electron app, validators, Python library).
- Collaborated with Carnegie Mellon University on DeepArchitect, a framework for neural architecture search (NeurIPS 2019).
- Built City-GAN (conditional GANs generating façades in a given city's architectural style) and MDCGenPy (a dataset generator for evaluating clustering algorithms).
- Co-advised a master's thesis in machine learning.

### Priberam Labs — Junior Researcher, SUMMA project — Lisbon, Portugal — Mar 2016 – Jul 2016
H2020 media-monitoring project with international industry and academic partners.

- Worked on entity linking (mapping mentions in text to Wikipedia entities), with classical (SVM) and deep-learning approaches.
- Researched, trained and published one of the first sets of pre-trained **multilingual word embeddings** (12 languages).

## Projects

- **[agent-pawl](https://github.com/dcferreira/agent-pawl)** (2026) — Makes AI coding agents follow the steps you want, in the order you want. Workflows are YAML state machines; deterministic steps run as scripts, agentic steps go to subagents, and postconditions are checked by the engine so the agent can't declare the job done. Ships as a Claude Code plugin with a bundled Go binary, a skill and enforcement hooks.
- **[Budgebell](https://github.com/dcferreira/budgebell)** (2026) — An agent-first, fully local desktop app that nudges you through habits, managed by your AI agent over a bundled local MCP server. Tauri + Svelte; releases for macOS, Linux and Windows.
- **[Toxic News](https://toxicnews.dcferreira.com)** (2023) — Ranks online news outlets by the toxicity of their daily headlines. Runs entirely as a scheduled GitHub Actions job with git as the only store. [Code](https://github.com/dcferreira/toxic-news)
- **[pylemmy](https://github.com/dcferreira/pylemmy)** — A Python client for the Lemmy API, and a bot built on it that summarises news articles.
- **[Multilingual Joint Embeddings](https://github.com/dcferreira/multilingual-joint-embeddings/)** (2016) — Reduced the state-of-the-art error rate by 40% on cross-lingual news classification; published embeddings for 12 languages (ACL 2016).
- **[DeepArchitect](https://github.com/negrinho/deep_architect)** (2019) — Modular, programmable neural architecture search (with CMU; NeurIPS 2019).
- **[NTARC](https://github.com/cn-tu/nta-meta-analysis/)** — Community database and data model for systematic review of network-traffic-analysis research.
- **[Traffic Flow Mapper](https://github.com/dcferreira/network_analysis_feature_reduction)** — Visualising network traffic in 2D with autoencoders (IJCNN 2019).
- **[MDCGenPy](https://github.com/CN-TU/mdcgenpy)** — Synthetic dataset generator for testing clustering algorithms (Journal of Classification).
- **[City-GAN](https://github.com/muxamilian/city-gan)** — Conditional GAN generating buildings in a specific city's architectural style.
- **[Infinity for YouTube](https://github.com/dcferreira/infinity-for-youtube)** — Browser extension that hides video length for selected channels.

## Skills

- **AI agents & LLMs:** agent platforms and harnesses, workflow-to-agent generation, prompt optimisation (DSPy), LLM evaluation, Claude Code, Model Context Protocol (MCP), AI coding-agent adoption.
- **Machine learning:** NLP (multilingual, cross-lingual), multimodal classification, model evaluation, PyTorch, TensorFlow, transformers, scikit-learn.
- **Data & infrastructure:** Python, Apache Spark, Databricks, Delta Lake, AWS, GCP, Azure, Docker, Kubernetes, Temporal, CI/CD, MLOps.
- **Other languages:** Go, TypeScript/JavaScript, SQL, Rust (basic).
- **Leadership & product:** technical leadership, mentoring, technical recruiting, product strategy, trust & safety.

## Education

- **MSc in Applied Mathematics** (major in computation), Instituto Superior Técnico, Lisbon — 2013 – 2015. Thesis: Cross-lingual Text Classification (19/20).
- **BSc in Applied Mathematics and Computation**, Instituto Superior Técnico, Lisbon — 2010 – 2013.

## Publications

- Towards modular and programmable architecture search. *NeurIPS 2019*.
- Extreme Dimensionality Reduction for Network Attack Visualization with Autoencoders. *IJCNN 2019*.
- MDCGen: Multidimensional dataset generator for clustering. *Journal of Classification*, 2019.
- NTARC: A Data Model for the Systematic Review of Network Traffic Analysis Research. *MDPI Applied Sciences*, 2020.
- City-GAN: Learning architectural styles using a custom Conditional GAN architecture. *arXiv*, 2019.
- A meta-analysis approach for feature selection in network traffic research. *SIGCOMM 2017 Reproducibility Workshop*.
- Jointly Learning to Embed and Predict with Multiple Languages. *ACL 2016*.

Full list: https://scholar.google.com/citations?user=4PusPA4AAAAJ

## Languages

Portuguese (native), English (first language), Spanish (good), German (B2).

## Outside work

Music, metal in particular (https://mydementia.bandcamp.com/releases), mountain biking, and video games.
