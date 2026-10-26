---
layout: page
permalink: /cv/
title: CV
nav: true
nav_order: 4
description:
---

<p>
  <a class="btn btn-sm z-depth-0" role="button" href="{{ '/assets/pdf/Zeyi_Xu_CV.pdf' | relative_url }}" download>
    <i class="fa-solid fa-file-pdf"></i> Download CV (PDF)
  </a>
</p>

<object class="cv-pdf" data="{{ '/assets/pdf/Zeyi_Xu_CV.pdf' | relative_url }}" type="application/pdf">
  <p>Your browser cannot display the PDF here. <a href="{{ '/assets/pdf/Zeyi_Xu_CV.pdf' | relative_url }}">Open the CV</a> instead.</p>
</object>

<style>
  .cv-pdf {
    display: block;
    width: 100%;
    height: 1100px;
    border: 1px solid var(--global-divider-color);
  }
</style>
