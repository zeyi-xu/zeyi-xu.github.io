---
layout: page
permalink: /research/
title: research
nav: true
nav_order: 1
---

## Acceleration

<div class="embed-frame">
  <iframe
    id="nesterov-vs-gd"
    src="{{ '/assets/html/nesterov-vs-gd.html' | relative_url }}"
    title="Gradient descent vs. Nesterov acceleration"
    loading="lazy"
  ></iframe>
</div>

<style>
  .embed-frame {
    margin: 1.5rem 0;
  }
  .embed-frame iframe {
    display: block;
    width: 100%;
    /* fallback until the load handler measures the real content height */
    height: 1250px;
    border: 0;
  }
</style>

<script>
  // The demo is a self-contained document, so it is framed rather than inlined:
  // that keeps its own :root/body styles and its Space/arrow key handler from
  // fighting the site. Same-origin, so we can size the frame to its content.
  (function () {
    var frame = document.getElementById("nesterov-vs-gd");
    if (!frame) return;
    function fit() {
      try {
        var doc = frame.contentDocument;
        if (doc && doc.documentElement) frame.style.height = doc.documentElement.scrollHeight + "px";
      } catch (e) {
        /* keep the CSS fallback height */
      }
    }
    frame.addEventListener("load", function () {
      fit();
      setTimeout(fit, 300); // re-measure once webfonts have settled
    });
    window.addEventListener("resize", fit);
  })();
</script>
