---
widget: slider  # Use the Slider widget as this page section
weight: 5  # Position of this section on the page
active: true  # Publish this section?
headless: true  # This file represents a page section.

design:
  # Slide height is automatic unless you force a specific height (e.g. '400px')
  slide_height: '300px'
  slide_width: '100px'
  is_fullscreen: false
  # Automatically transition through slides?
  loop: true
  # Duration of transition between slides (in ms)
  interval: 5000

content:
  slides:
    - title: 👋 웹 페이지에 오신 것을 환영합니다.
      align: center
      background:
        position: right
        color: '#666'
        brightness: 0.3
        media: code.jpg
        fit: cover
    - title: 성장 스토리를 만나보세요!
      content: 이 곳에는 저의 성장 이야기가 잔뜩 담겨있습니다.
      align: left
      background:
        position: center
        color: '#555'
        brightness: 0.3
        media: github.jpg
        fit: cover
    - title: 저와 함께하실 분을 구합니다.
      content: 함께하실 분은 아래의 버튼을 눌러 함께해요!
      align: right
      background:
        position: center
        color: '#333'
        brightness: 0.4
        media: call.jpg
        fit: cover
      link:
        icon: phone
        icon_pack: fas
        text: Contact Me
        url: ../contact/
---
<br><br>