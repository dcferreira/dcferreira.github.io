---
title: Towards modular and programmable architecture search
authors:
- Renato Negrinho
- Matthew Gormley
- Geoffrey J Gordon
- Darshan Patil
- Nghia Le
- admin
date: '2019-10-30T00:00:00Z'
publishDate: '2019-10-30T00:00:00Z'
publication_types:
- paper-conference
abstract: Neural architecture search methods are able to find high performance deep learning architectures with minimal effort from an expert. However, current systems focus on specific use-cases (eg convolutional image classifiers and recurrent language models), making them unsuitable for general use-cases that an expert might wish to write. Hyperparameter optimization systems are general-purpose but lack the constructs needed for easy application to architecture search. In this work, we propose a formal language for encoding search spaces over general computational graphs. The language constructs allow us to write modular, composable, and reusable search space encodings and to reason about search space design. We use our language to encode search spaces from the architecture search literature. The language allows us to decouple the implementations of the search space and the search algorithm, allowing us to expose search spaces to search algorithms through a consistent interface. Our experiments show the ease with which we can experiment with different combinations of search spaces and search algorithms without having to implement each combination from scratch. We release an implementation of our language with this paper.
summary: We propose a formal language for encoding search spaces over general computational graphs, applicable in particular to neural network architecture search.
tags: []
featured: false
publication:
  name: In Advances in Neural Information Processing Systems 2019
  short_name: In NeurIPS 2019
links:
- type: pdf
  url: https://papers.nips.cc/paper/9524-towards-modular-and-programmable-architecture-search.pdf
- type: code
  url: https://github.com/negrinho/deep_architect
- type: project
  url: /project/deep-architect/
---
