---
# An instance of the Blank widget with a Gallery page element.
# Documentation: https://wowchemy.com/docs/getting-started/page-builder/
widget: blank

# SAMMY! If public gets deleted, run this by itself below {{< gallery album="demo" >}} and then replace with the code below. Annoying, I know, but it works. 

# This file represents a page section.
headless: true
active: true
# Order that this section appears on the page.
weight: 117

title: Gallery
subtitle:

design:
  columns: '1'
---
<!-- Include the required lightGallery JavaScript and CSS files -->
<script src="https://cdn.jsdelivr.net/npm/lg-fullscreen@1.3.0/dist/lg-fullscreen.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/lightgallery@1.10.0/dist/js/lightgallery.min.js"></script>
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/lightgallery@1.10.0/dist/css/lightgallery.min.css">

<style>
#lightgallery {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-gap: 10px;
}

#lightgallery img {
  width: 100%;
  height: auto;
  transition: transform 0.3s;
}

#lightgallery img:hover {
  transform: scale(1.1);
}
</style>
<div id="lightgallery">
  <!-- Replace the image URLs below with your actual image URLs -->
  <a href="/media/albums/demo/1.jpg">
    <img src="/media/albums/demo/1.jpg">
  </a>
  <a href="/media/albums/demo/2.jpg">
    <img src="/media/albums/demo/2.jpg">
  </a>
  <a href="/media/albums/demo/3.jpg">
    <img src="/media/albums/demo/3.jpg">
  </a>
  <a href="/media/albums/demo/4.jpg">
    <img src="/media/albums/demo/4.jpg">
  </a>
  <a href="/media/albums/demo/5.jpg">
    <img src="/media/albums/demo/5.jpg">
  </a>
  <a href="/media/albums/demo/6.jpg">
    <img src="/media/albums/demo/6.jpg">
  </a>
  <a href="/media/albums/demo/7.jpg">
    <img src="/media/albums/demo/7.jpg">
  </a>
  <a href="/media/albums/demo/8.jpg">
    <img src="/media/albums/demo/8.jpg">
  </a>
  <a href="/media/albums/demo/9.jpg">
    <img src="/media/albums/demo/9.jpg">
  </a>

</div>

<script>
document.addEventListener('DOMContentLoaded', function () {
  lightGallery(document.getElementById('lightgallery'), {
    selector: 'a',
    download: false,
    counter: false,
  });
});
</script>

