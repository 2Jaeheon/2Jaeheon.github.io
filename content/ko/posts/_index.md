---
# Page title
title: My page
# Page type - we want a landing page (such as a homepage)
type: landing

banner:
  caption: 'Image credit: [**Unsplash**](https://unsplash.com/)'
  image: 'library.jpg'

# Your landing page sections - add as many different content blocks as you like
sections:
  # A section to display blog posts
  - block: collection
    id: section-1
    content:
      title: Section 1
      subtitle: All Posts
      text: Add any **markdown** formatted content here - text, images, videos, galleries - and even HTML code!
      # Display content from the `content/post/` folder
      filters:
        folders:
          - hardskills
          - softskills
        recursive: true
      sort_by: 'Date'
      sort_ascending: false
      
    design:
      # Choose how many columns the section has. Valid values: '1' or '2'.
      columns: '1'
      # Choose your content listing view - here we use the `showcase` view
      view: showcase
      # For the Showcase view, do you want to flip alternate rows?
      flip_alt_rows: true
---