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
    - title: 👋 Welcome to the webpage.
      align: center
      background:
        position: right
        color: '#666'
        brightness: 0.3
        media: code.jpg
        fit: cover
    - title: Meet the growth story!
      content: There's a lot of my growth story in here.
      align: left
      background:
        position: center
        color: '#555'
        brightness: 0.3
        media: github.jpg
        fit: cover
    - title: I'm looking for someone to join me.
      content: If you'd like to join us, click the button below to join us!
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