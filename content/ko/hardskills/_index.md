---
# Page title
title: My page

# Page type - we want a landing page (such as a homepage)
type: landing


# Your landing page sections - add as many different content blocks as you like
sections:
  # A section to display blog posts
  - block: collection
    id: hardSkill
    content:
      title: Hard Skills
      subtitle: 하드 스킬
      # Display content from the `content/post/` folder
      filters:
        folders:
          - hardskills
        #recursive: true
      sort_by: 'Date'
      sort_ascending: false
      
    design:
      # Choose how many columns the section has. Valid values: '1' or '2'.
      columns: '1'
      # Choose your content listing view - here we use the `showcase` view
      view: showcase
      # For the Showcase view, do you want to flip alternate rows?
      #flip_alt_rows: true
---