---
widget: pages # As of v5.8-dev, 'pages' is renamed 'collection'
headless: true  # This file represents a page section.

# Put Your Section Options Here (title, background, etc.) ...
title: 독후감
subtitle: '최근에 작성한 독후감을 기록합니다'

# Position of this section on the page
weight: 50

content:
  # Filter content to display
  filters:
    # The folders to display content from
    folders:
      - books
    tag: ''
    category: ''
    publication_type: ''
    author: ''
    featured_only: false
    exclude_featured: false
    exclude_future: false
    exclude_past: false
  # Choose how many pages you would like to display (0 = all pages)
  count: 5
  # Choose how many pages you would like to offset by

  # Useful if you wish to show the first item in the Featured widget
  offset: 0
  # Field to sort by, such as Date or Title
  sort_by: 'Date'
  sort_ascending: false
design:
  # Choose a listing view
  view: community/list_view
  # Choose how many columns the section has. Valid values: '1' or '2'.
  columns: '1'
---