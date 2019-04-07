+++
title = "Extreme Dimensionality Reduction for Network Attack Visualization with Autoencoders"
date = 2019-07-01
draft = false

# Authors. Comma separated list, e.g. `["Bob Smith", "David Jones"]`.
authors = ["Daniel C Ferreira", "Félix Iglesias Vázquez", "Tanja Zseby"]

# Publication type.
# Legend:
# 0 = Uncategorized
# 1 = Conference paper
# 2 = Journal article
# 3 = Manuscript
# 4 = Report
# 5 = Book
# 6 = Book section
publication_types = ["1"]

# Publication name and optional abbreviated version.
publication = "Proceedings of the International Joint Conference on Neural Networks 2019"
publication_short = "IJCNN 2019"

# Abstract and optional shortened version.
abstract = "The visualization of network traffic flows is an open problem that affects the control and administration of communication networks. Feature vectors used for representing traffic commonly have from tens to hundreds of dimensions and hardly tolerate visual conceptualizations. In this work we use neural networks to obtain extremely low-dimensional data representations that are meaningful from an attack-detection perspective. We focus on a simple Autoencoder architecture, as well as an extension that benefits from pre-knowledge, and evaluate their performances by comparing them with reductions based on Principal Component Analysis and Linear Discriminant Analysis. Experiments are conducted with a modern Intrusion Detection dataset that collects legitimate traffic mixed with a wide variety of attack classes. Results show that feature spaces can be strongly reduced up to two dimensions with tolerable classification degradation while providing a clear visualization of the data. Visualizing traffic flows in two-dimensional spaces is extremely useful to understand what is happening in networks, also to enhance and refocus classification, trigger refined analysis, and aid the security experts’ decision-making. We  dditionally developed a tool prototype that covers such functions, therefore supporting the optimization of network traffic attack detectors in both design and application phases."
abstract_short = "We used semi-supervised Autoencoders to obtain 2d visualizations of network traffic that separate between distinct types of attacks."

# Is this a featured publication? (true/false)
featured = true

# Projects (optional).
#   Associate this publication with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["deep-learning"]` references 
#   `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects = []

# Slides (optional).
#   Associate this page with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides = "example-slides"` references 
#   `content/slides/example-slides.md`.
#   Otherwise, set `slides = ""`.
slides = ""

# Tags (optional).
#   Set `tags = []` for no tags, or use the form `tags = ["A Tag", "Another Tag"]` for one or more tags.
tags = []

# Links (optional).
url_pdf = ""
url_preprint = ""
url_code = "https://github.com/dcferreira/network_analysis_feature_reduction"
url_dataset = ""
url_project = ""
url_slides = ""
url_video = ""
url_poster = ""
url_source = ""

# Custom links (optional).
#   Uncomment line below to enable. For multiple links, use the form `[{...}, {...}, {...}]`.
# url_custom = [{name = "Custom Link", url = "http://example.org"}]

# Digital Object Identifier (DOI)
doi = ""

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder. 
[image]
  # Caption (optional)
  caption = ""

  # Focal point (optional)
  # Options: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight
  focal_point = ""
+++
