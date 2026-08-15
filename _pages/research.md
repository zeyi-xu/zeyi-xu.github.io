---
layout: page
permalink: /research/
title: research
nav: true
nav_order: 1
---

<p>My research focuses on large-scale optimization, with an emphasis on the design and convergence analysis of accelerated first-order methods.</p>

<p>Gradient descent is a simple and widely used optimization method, but it can be slow to converge. We can accelerate the convergence of gradient descent using various techniques:</p>

<ul>Momentum methods, such as Nesterov's accelerated gradient method, which uses a combination of the current and previous gradients to update the parameters.</ul>
<ul>Mirror descent, which uses a different geometry to update the parameters, allowing for faster convergence in certain cases.</ul>
<ul>Adaptive methods, such as AdaGrad and Adam, which adjust the learning rate based on the history of gradients.</ul>

<p>My research aims to combine these techniques in a principled way to design new optimization algorithms that are both fast and robust.</p>

<p> Below are some visualizations of the different optimization techniques. Have fun exploring!</p>

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
    src="{{ '/assets/html/mirror-vs-gd.html' | relative_url }}"
    title="Gradient descent vs. mirror descent"
    loading="lazy"
  ></iframe>
</div>

## Adaptivity

<div class="embed-frame">
  <iframe
    id="adaptive"
    src="{{ '/assets/html/adaptive-rule.html' | relative_url }}"
    title="Fixed step gradient descent vs. adaptive gradient descent"
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
