---
title: "Extreme Dimensionality Reduction for Network Attack Visualization with Autoencoders"

# Authors
# If you created a profile for a user (e.g. the default `admin` user), write the username (folder name) here
# and it will be replaced with their full name and linked to their profile.
authors:
  - admin
  - Félix Iglesias Vázquez
  - Tanja Zseby

date: "2019-07-01T00:00:00Z"
doi: ""

# Schedule page publish date (NOT publication's date).
publishDate: "2019-10-30T00:00:00Z"

# Publication type.
# Legend: 0 = Uncategorized; 1 = Conference paper; 2 = Journal article;
# 3 = Preprint / Working Paper; 4 = Report; 5 = Book; 6 = Book section;
# 7 = Thesis; 8 = Patent
publication_types: ["1"]

# Publication name and optional abbreviated publication name.
publication: Proceedings of the International Joint Conference on Neural Networks 2019
publication_short: IJCNN 2019

abstract: The visualization of network traffic flows is an open problem that affects the control and administration of communication networks. Feature vectors used for representing traffic commonly have from tens to hundreds of dimensions and hardly tolerate visual conceptualizations. In this work we use neural networks to obtain extremely low-dimensional data representations that are meaningful from an attack-detection perspective. We focus on a simple Autoencoder architecture, as well as an extension that benefits from pre-knowledge, and evaluate their performances by comparing them with reductions based on Principal Component Analysis and Linear Discriminant Analysis. Experiments are conducted with a modern Intrusion Detection dataset that collects legitimate traffic mixed with a wide variety of attack classes. Results show that feature spaces can be strongly reduced up to two dimensions with tolerable classification degradation while providing a clear visualization of the data. Visualizing traffic flows in two-dimensional spaces is extremely useful to understand what is happening in networks, also to enhance and refocus classification, trigger refined analysis, and aid the security experts’ decision-making. We  dditionally developed a tool prototype that covers such functions, therefore supporting the optimization of network traffic attack detectors in both design and application phases.

# Summary. An optional shortened abstract.
summary: We used semi-supervised Autoencoders to obtain 2d visualizations of network traffic that separate between distinct types of attacks.

tags: []

# Display this page in the Featured widget?
featured: true

# Custom links (uncomment lines below)
# links:
# - name: Custom Link
#   url: http://example.org

url_pdf: ""
url_code: "https://github.com/dcferreira/network_analysis_feature_reduction"
url_dataset: ""
url_poster: ""
url_project: ""
url_slides: ""
url_source: ""
url_video: ""

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Associated Projects (optional).
#   Associate this publication with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `internal-project` references `content/project/internal-project/index.md`.
#   Otherwise, set `projects: []`.
projects:
  - tfm

# Slides (optional).
#   Associate this publication with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides: "example"` references `content/slides/example/index.md`.
#   Otherwise, set `slides: ""`.
slides: ""
---
