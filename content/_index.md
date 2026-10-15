---
title: ''
summary: ''
date: 2022-10-24
type: landing

sections:
  - block: resume-biography-3
    content:
      username: admin
      text: ''
      button:
        text: Download CV
        url: uploads/CVDanielFerreira2023_unabridged.pdf
      headings:
        about: ''
        education: ''
        interests: ''
    design:
      background:
        gradient_mesh:
          enable: true
      name:
        size: md
      avatar:
        size: medium
        shape: circle
  - block: resume-experience-grouped
    id: experience
    content:
      title: Experience
      username: admin
    design:
      date_format: 'Jan 2006'
  - block: collection
    id: featured
    content:
      title: Featured Publications
      filters:
        folders:
          - publication
        featured_only: true
      count: 0
    design:
      view: article-grid
      columns: 2
  - block: collection
    id: posts
    content:
      title: Recent Posts
      count: 5
      filters:
        folders:
          - post
      archive:
        enable: true
        link: /post/
    design:
      view: date-title-summary
  - block: collection
    id: projects
    content:
      title: Projects
      count: 0
      filters:
        folders:
          - project
    design:
      view: article-grid
      fill_image: false
      columns: 2
      show_date: false
      show_read_time: false
      show_read_more: false
  - block: collection
    id: publications
    content:
      title: Recent Publications
      count: 0
      filters:
        folders:
          - publication
    design:
      view: citation
---
