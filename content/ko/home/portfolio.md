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
  page_type: web

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

<style>
  .overlay-container {
    position: relative;
    display: inline-block;
  }

  .overlay-image {
    display: block;
    width: 100%;
    max-width: 400px; /* 이미지의 최대 크기를 400px로 제한 */
    height: auto;
    margin: 0 auto; /* 이미지를 중앙에 배치 */
  }

  .overlay-link {
    text-decoration: none;
    color: white;
  }

  .overlay-text {
    position: absolute;
    bottom: 0;
    left: 0;
    width: 100%;
    background: rgba(0, 0, 0, 0.5); /* 반투명 검정 배경 */
    color: white;
    text-align: center;
    padding: 10px;
    font-size: 16px;
    opacity: 0;
    transition: opacity 0.5s ease;
  }

  .overlay-container:hover .overlay-text {
    opacity: 1; /* 호버 시 텍스트가 나타남 */
  }
</style>

<div class="overlay-container">
  <a href="/cs" class="overlay-link">
    <img src="your-image.jpg" alt="CS 프로젝트 이미지" class="overlay-image">
    <div class="overlay-text">CS 프로젝트를 보시려면 이 이미지를 눌러주세요</div>
  </a>
</div>