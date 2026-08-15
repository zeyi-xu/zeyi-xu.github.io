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

## Mirror

<div class="embed-frame">
  <iframe
    id="mirror"
    src="{{ '/assets/html/mirror.html' | relative_url }}"
    title="Mirror"
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
    height: 1250px;
    border: 0;
  }
</style>

<script>
  (function () {
    function fit(frame) {
      try {
        var doc = frame.contentDocument;
        if (doc && doc.documentElement) {
          frame.style.height = doc.documentElement.scrollHeight + "px";
        }
      } catch (e) {
        /* Keep fallback height. */
      }
    }

    document.querySelectorAll(".embed-frame iframe").forEach(function (frame) {
      frame.addEventListener("load", function () {
        fit(frame);
        setTimeout(function () {
          fit(frame);
        }, 300);
      });
    });

    window.addEventListener("resize", function () {
      document.querySelectorAll(".embed-frame iframe").forEach(fit);
    });
  })();
</script>