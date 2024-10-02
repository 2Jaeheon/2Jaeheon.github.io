---
widget: pages # As of v5.8-dev, 'pages' is renamed 'collection'
headless: true  # This file represents a page section.

# Put Your Section Options Here (title, background, etc.) ...
title: Recent Book Report
subtitle: '최근에 읽었던 책'

# Position of this section on the page
weight: 40

content:
  # Page type to display. E.g. project.
  page_type: project

  # Default filter index (e.g. 0 corresponds to the first `filter_button` instance below).
  filter_default: 0

  filter_button:
    - name: All
      tag: '*'
    - name: Machine Learning
      tag: test
    - name: Computer Vision
      tag: test2
    - name: NLP
      tag: test3

design:
  columns: '1'
  view: masonry
  flip_alt_rows: true
  background: {}
  spacing: {padding: [0, 0, 0, 0]}
  
---

Check out my recent blog posts below!
