---
title: Extreme Dimensionality Reduction for Network Attack Visualization with Autoencoders
authors:
- admin
- Félix Iglesias Vázquez
- Tanja Zseby
date: '2019-07-01T00:00:00Z'
publishDate: '2019-10-30T00:00:00Z'
publication_types:
- paper-conference
abstract: The visualization of network traffic flows is an open problem that affects the control and administration of communication networks. Feature vectors used for representing traffic commonly have from tens to hundreds of dimensions and hardly tolerate visual conceptualizations. In this work we use neural networks to obtain extremely low-dimensional data representations that are meaningful from an attack-detection perspective. We focus on a simple Autoencoder architecture, as well as an extension that benefits from pre-knowledge, and evaluate their performances by comparing them with reductions based on Principal Component Analysis and Linear Discriminant Analysis. Experiments are conducted with a modern Intrusion Detection dataset that collects legitimate traffic mixed with a wide variety of attack classes. Results show that feature spaces can be strongly reduced up to two dimensions with tolerable classification degradation while providing a clear visualization of the data. Visualizing traffic flows in two-dimensional spaces is extremely useful to understand what is happening in networks, also to enhance and refocus classification, trigger refined analysis, and aid the security experts’ decision-making. We  dditionally developed a tool prototype that covers such functions, therefore supporting the optimization of network traffic attack detectors in both design and application phases.
summary: We used semi-supervised Autoencoders to obtain 2d visualizations of network traffic that separate between distinct types of attacks.
tags: []
featured: true
publication:
  name: Proceedings of the International Joint Conference on Neural Networks 2019
  short_name: IJCNN 2019
links:
- type: code
  url: https://github.com/dcferreira/network_analysis_feature_reduction
- type: project
  url: /project/tfm/
---
