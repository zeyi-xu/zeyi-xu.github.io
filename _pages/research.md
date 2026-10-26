---
layout: page
permalink: /research/
title: research
nav: true
nav_order: 1
---

## overview

<p>
  Optimization is the computational engine of data science and machine learning. Training a model, matching one data distribution to another, and
  simulating how a system evolves all come down to minimizing a function. The workhorse algorithms are first-order methods, which use only
  gradients. They are simple and scale to very large problems, but they slow down dramatically on poorly conditioned problems, which are much
  steeper in some directions than in others.
</p>

<p>
  Three ideas make these methods fast in practice. <em>Acceleration</em> uses momentum to reach a solution in far fewer steps.
  <em>Preconditioning</em>, of which mirror descent is one form, changes the geometry of the problem so that it becomes easier to solve.
  <em>Adaptivity</em> tunes the step size and other parameters during the run, instead of relying on constants that are rarely known in advance.
  These ideas are usually designed and analyzed one at a time.
</p>

<p>
  My research develops a single framework that combines all three, which I call the <em>accelerated adaptive mirror</em> (AAM) framework. It views
  an algorithm as a time-stepping scheme for a continuous-time process, and uses a quantity that decreases along that process, a Lyapunov function,
  to design the algorithm, choose its parameters, and prove how fast it converges. This turns widely used algorithms into much faster ones that
  stay as simple as the originals. The main results so far are:
</p>

<ul>
  <li>
    <b>A unified theory</b> of acceleration, adaptivity, and preconditioning, including <a href="https://arxiv.org/abs/2606.02787">AAMD</a>, a
    method that provably combines all three.
  </li>
  <li>
    <b><a href="https://arxiv.org/abs/2605.30267">Acc-Sinkhorn</a></b>, a faster version of the Sinkhorn algorithm for optimal transport, the
    standard tool for comparing and matching probability distributions. Each step costs the same as a Sinkhorn step, and the speedup grows on the
    hardest problems, where Sinkhorn is slowest.
  </li>
  <li>
    <b><a href="https://arxiv.org/abs/2610.25473">AGCG</a></b>, an accelerated method for Wasserstein gradient flows, which describe how a
    distribution of mass, such as a granular material, evolves over time. As the problem gets harder, its cost grows like the square root of the
    difficulty, while that of existing adaptive methods grows almost in proportion to it.
  </li>
  <li>
    <b>Generative modeling.</b> In recent experiments, Acc-Sinkhorn cut the training time of a class of image-generation models, one-step
    flow-matching models, by about 30%.
  </li>
</ul>

<figure class="research-map">
  <img src="{{ '/assets/img/research-map.svg' | relative_url }}" alt="Research map: the AAM framework (HNAG++, A2GD, Acc-MD, AAMD) leads to Acc-Sinkhorn, AGCG and the ongoing Acc-NGD, with applications to entropic optimal transport and flow matching, Wasserstein gradient flows, and information theory and experimental design.">
</figure>

## interactive demos

<p>Each topic below opens to a short introduction and an interactive demo.</p>

<div class="research-topics">
<details>
<summary>Acceleration</summary>

<p>
  Consider \(\min_{x \in \mathbb{R}^n} f(x)\), where \(f\) is \(L\)-smooth and \(\mu\)-strongly convex, with condition number \(\kappa = L/\mu\).
  Gradient descent with step size \(1/L\),
  $$x_{k+1} = x_k - \tfrac{1}{L} \nabla f(x_k),$$
  satisfies \(f(x_k) - f^\star \le (1 - 1/\kappa)^k \, (f(x_0) - f^\star)\). Nesterov's accelerated gradient method adds a momentum step,
  $$y_k = x_k + \beta (x_k - x_{k-1}), \qquad x_{k+1} = y_k - \tfrac{1}{L} \nabla f(y_k), \qquad \beta = \frac{\sqrt{\kappa} - 1}{\sqrt{\kappa} + 1},$$
  and satisfies
  $$f(x_k) - f^\star \le (1 - 1/\sqrt{\kappa})^k \, \big(f(x_0) - f^\star + \tfrac{\mu}{2} \|x_0 - x^\star\|^2\big).$$
  The iteration count
  to reach accuracy \(\varepsilon\) drops from \(O(\kappa \log(1/\varepsilon))\) to \(O(\sqrt{\kappa} \log(1/\varepsilon))\), which is optimal among
  first-order methods for this class.
</p>

<p class="demo-note">
  <b>Demo.</b> On \(\mathbb{R}^2\),
  $$f(x) = \log \sum_i \exp(\langle a_i, x \rangle + c_i) + \tfrac{1}{2}(m_1 x_1^2 + m_2 x_2^2).$$
  The \(\kappa\)
  slider shrinks \(m_1\). \(L\) and \(\mu\) are the extreme Hessian eigenvalues over the plotted region; dashed lines show the two rates. Surface
  height is \(\sqrt{f - f^\star}\), rescaled, which keeps the level sets. Drag the surface to rotate it; click it to move the starting point.
</p>

<div class="embed-frame">
  <iframe
    id="nesterov-vs-gd"
    data-src="{{ '/assets/html/nesterov-vs-gd.html' | relative_url }}"
    title="Gradient descent vs. Nesterov acceleration"
  ></iframe>
</div>

</details>

<details>
<summary>Mirror descent</summary>

<p>
  Let \(h\) be a differentiable convex reference function with Bregman divergence \(D_h(x, y) = h(x) - h(y) - \langle \nabla h(y), x - y \rangle\).
  Mirror descent replaces the Euclidean proximity term of gradient descent with \(D_h\):
  $$x_{k+1} = \arg\min_x \big\{ \langle \nabla f(x_k), x \rangle + L \, D_h(x, x_k) \big\} \quad \Longleftrightarrow \quad \nabla h(x_{k+1}) = \nabla h(x_k) - \tfrac{1}{L} \nabla f(x_k).$$
  We say \(f\) is \(L\)-smooth and \(\mu\)-strongly convex relative to \(h\) if
  $$\mu \, D_h(x, y) \le D_f(x, y) \le L \, D_h(x, y) \quad \text{for all } x, y.$$
  Under these conditions mirror descent converges linearly, \(f(x_k) - f^\star = O\big((1 - \mu/L)^k\big)\) (Lu, Freund and Nesterov, 2018). With
  \(h = \tfrac{1}{2}\|x\|^2\) this is gradient descent. The gain comes when \(\kappa = L/\mu\) relative to \(h\) is far smaller than the Euclidean
  condition number, or when no Euclidean \(L\) exists at all.
</p>

<p class="demo-note">
  <b>Demo.</b> \(f(x) = \tfrac{1}{4}\|Ex\|^4 + \tfrac{1}{4}\|Ax - b\|_4^4 + \tfrac{1}{2}\|Cx - d\|^2\) and \(h(x) = \tfrac{1}{4}\|x\|^4 + \tfrac{1}{2}\|x\|^2\).
  The Hessian of \(f\) grows without bound, so \(f\) has no global Euclidean \(L\), but it is smooth and strongly convex relative to \(h\). The mirror
  step inverts \(\nabla h(x) = (1 + \|x\|^2)\,x\) by solving a cubic in \(\|x\|\). Both condition numbers are measured over the plotted region:
  Euclidean from the eigenvalues of \(\nabla^2 f\), relative from those of \(\nabla^2 h^{-1/2} \nabla^2 f \, \nabla^2 h^{-1/2}\). The slider scales
  \(A\). Drag the surface to rotate it; click it to move the starting point.
</p>

<div class="embed-frame">
  <iframe
    id="mirror"
    data-src="{{ '/assets/html/mirror-vs-gd.html' | relative_url }}"
    title="Gradient descent vs. mirror descent"
  ></iframe>
</div>

</details>

<details>
<summary>Adaptivity</summary>

<p>
  Gradient descent with a fixed step \(\eta\) on a quadratic \(f(x) = \tfrac{1}{2} x^\top H x\) scales the component along each eigenvector of \(H\)
  by \(1 - \eta \lambda_i\). It is therefore stable only for \(0 \lt \eta \lt 2/L\), and the best fixed step \(\eta^\star = 2/(L + \mu)\) gives the
  contraction factor \(\rho = (\kappa - 1)/(\kappa + 1)\), so \(f(x_k) - f^\star\) decays like \(\rho^{2k}\). Choosing \(\eta\) well requires knowing
  \(L\) and \(\mu\). Adaptive methods instead set the step from quantities observed during the run:
</p>
<ul>
  <li>
    <b>Polyak step</b>: \(\displaystyle \eta_k = \frac{f(x_k) - f^\star}{\|\nabla f(x_k)\|^2}\), which needs only \(f^\star\).
  </li>
  <li>
    <b>AdaGrad</b> (diagonal):
    $$G_{k,i} = \sum_{j \le k} \big(\partial_i f(x_j)\big)^2, \qquad x_{k+1,i} = x_{k,i} - \frac{\eta_0}{\sqrt{G_{k,i}} + \epsilon} \, \partial_i f(x_k).$$
    Each coordinate gets its own step. This is mirror descent with the data-dependent reference function \(h_k(x) = \tfrac{1}{2} x^\top \operatorname{diag}(\sqrt{G_k}) \, x\).
  </li>
</ul>

<p class="demo-note">
  <b>Demo.</b> \(f(x) = \tfrac{1}{2}(\kappa x_1^2 + x_2^2)\), so \(L = \kappa\) and \(\mu = 1\) exactly. The fixed-step slider has ticks at
  \(\eta^\star\) and \(2/L\); the dashed line is the optimal fixed-step rate \(\rho^{2k}\). The lower panel shows \(\eta_k\), per coordinate for
  AdaGrad. Drag the surface to rotate it; click it to move the starting point.
</p>

<div class="embed-frame">
  <iframe
    id="adaptive"
    data-src="{{ '/assets/html/adaptive-rule.html' | relative_url }}"
    title="Fixed step gradient descent vs. adaptive gradient descent"
  ></iframe>
</div>

</details>
</div>

<style>
  /* The map has dark labels on a transparent background, so it keeps a light
     card in both themes. */
  .research-map {
    margin: 1.5rem 0;
    padding: 0.75rem;
    background: #fff;
    border-radius: 6px;
  }

  .research-map img {
    display: block;
    width: 100%;
    height: auto;
  }

  .research-topics details {
    border-top: 1px solid var(--global-divider-color);
    padding: 0.75rem 0;
  }

  .research-topics details:last-child {
    border-bottom: 1px solid var(--global-divider-color);
  }

  .research-topics summary {
    cursor: pointer;
    font-size: 1.625rem;
    font-weight: 600;
    color: var(--global-theme-color);
  }

  .research-topics details[open] summary {
    margin-bottom: 1rem;
  }

  .research-topics mjx-container[display="true"] {
    overflow-x: auto;
    overflow-y: hidden;
  }

  .research-topics .demo-note {
    font-size: 1rem;
    color: var(--global-text-color-light);
  }

  .embed-frame {
    margin: 1.5rem 0;
  }

  .embed-frame iframe {
    display: block;
    width: 100%;
    height: 900px;
    border: 0;
  }
</style>

<script>
  (function () {
    // Measure the demo's content rather than the document: scrollHeight never
    // reports less than the iframe's current height, so it could only grow.
    function fit(frame) {
      try {
        var doc = frame.contentDocument;
        var wrap = doc && doc.querySelector(".wrap");
        if (wrap) {
          var pad = parseFloat(doc.defaultView.getComputedStyle(doc.body).paddingBottom) || 0;
          frame.style.height = Math.ceil(wrap.getBoundingClientRect().bottom + doc.defaultView.scrollY + pad) + "px";
        }
      } catch (e) {
        /* Keep fallback height. */
      }
    }

    // Demos load on first open: a closed <details> has no layout, so a demo
    // loaded inside one would size its canvases to zero width.
    document.querySelectorAll(".research-topics details").forEach(function (details) {
      details.addEventListener("toggle", function () {
        var frame = details.querySelector("iframe");
        if (!details.open || !frame) return;
        if (!frame.getAttribute("src")) {
          frame.addEventListener("load", function () {
            fit(frame);
            setTimeout(function () {
              fit(frame);
            }, 300);
          });
          frame.setAttribute("src", frame.dataset.src);
        } else {
          fit(frame);
        }
      });
    });

    window.addEventListener("resize", function () {
      document.querySelectorAll(".embed-frame iframe[src]").forEach(fit);
    });
  })();
</script>
