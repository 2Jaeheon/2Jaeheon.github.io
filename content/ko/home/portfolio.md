---
# A section created with the Portfolio widget.
# This section displays content from `content/project/`.
# See https://docs.hugoblox.com/widget/portfolio/
widget: portfolio

# This file represents a page section.
headless: true

# Order that this section appears on the page.
weight: 30

title: 'Project'
subtitle: ''

content:
  # Page type to display. E.g. project.
  page_type: cs

  # Default filter index (e.g. 0 corresponds to the first `filter_button` instance below).
  filter_default: 0

  # Filter toolbar (optional).
  # Add or remove as many filters (`filter_button` instances) as you like.
  # To show all items, set `tag` to "*".
  # To filter by a specific tag, set `tag` to an existing tag name.
  # To remove the toolbar, delete the entire `filter_button` block.
  filter_button:
    - name: All
      tag: '*'
    - name: Language
      tag: language
    - name: WEB
      tag: web  

design:
  columns: '1'
  view: masonry
  flip_alt_rows: true
  background: {}
  spacing: {padding: [0, 0, 0, 0]}
  
---
<div style="text-align: center;">
    <a href="www.2jaeheon.site/cs" id="hoverLink" style="display: block; text-align: center; padding: 10px; font-size: 18px; color: black;">
    CS 프로젝트가 궁금하다면 여기로 가보세요!
    </a>
</div>

<script>
    const link = document.getElementById('hoverLink');
    
    link.addEventListener('mouseenter', function() {
        link.textContent = "지금 클릭해서 확인해보세요!";
        link.style.color = 'blue';
        link.style.fontSize = '20px';
    });

    link.addEventListener('mouseleave', function() {
        link.textContent = "CS 프로젝트가 궁금하다면 여기로 가보세요!";
        link.style.color = 'black';
        link.style.fontSize = '18px';
    });
</script>