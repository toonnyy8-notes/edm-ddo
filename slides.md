---
# You can also start simply with 'default'
theme: seriph
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://cover.sli.dev
# some information about your slides (markdown enabled)
title: Idempotent Generative Network
info: |
  ## Slidev Starter Template
  Presentation slides for developers.

  Learn more at [Sli.dev](https://sli.dev)
# apply unocss classes to the current slide
class: text-center
# https://sli.dev/features/drawing
drawings:
  persist: false
# slide transition: https://sli.dev/guide/animations.html#slide-transitions
transition: slide-left
# enable MDC Syntax: https://sli.dev/features/mdc
mdc: true
download: true
---

<h1><a href="https://openreview.net/forum?id=XIaS66XkNA"><span class="text-14">I</span>DEMPOTET <span class="text-14">G</span>ENERATIVE <span class="text-14">N</span>ETWORK</a></h1>

<br/>

Assaf Shocher$^{1,2}$,
Amil Dravid$^1$
Yossi Gandelsman$^1$  
Inbar Mosseri$^2$
Michael Rubinstein$^2$
Alexei A. Efros$^1$

<p class="text-justify pl-90">
<katex-elem expr='^1' /> UC Berkeley<br/>
<katex-elem expr='^2' /> Google Research
</p>

## ICLR 2024

<div class="abs-br m-6 flex gap-2">
  <a href="https://github.com/toonnyy8-notes/Idempotent-Generative-Network" target="_blank" alt="GitHub" title="Open in GitHub"
    class="text-xl slidev-icon-btn opacity-50 !border-none !hover:text-white">
    <carbon-logo-github />
  </a>
</div>

<!--
The last comment block of each slide will be treated as slide notes. It will be visible and editable in Presenter Mode along with the slide. [Read more in the docs](https://sli.dev/guide/syntax.html#notes)
-->

<style>
  .slidev-layout.cover h1 {
    font-size: 2.5rem;
    line-height: 3rem;
  }
</style>

---

# 『Make It Real』

<span></span>

<p class="text-xl first-letter">
The Idempotent Generative Network (IGN) aims to create a model that handles diverse inputs and projects them onto the target distribution in one step.
</p>

<p class="text-xl ">
Unlike GANs or VAEs, which need specific inputs, IGN acts as a “global projector.” It turns any input—corrupted images, sketches, or noise—into real images instantly, like a “Make It Real” button.
</p>

<img src="/assets/proj2.jpg"/>

<SlideCurrentNo class="absolute bottom-4 right-8" />

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}

.first-letter::first-letter {
  -webkit-initial-letter: 2;
  initial-letter: 2;
}
</style>

---

## Idempotency: One Step Generation, Optional Refinement

<span class="text-xl">Disadvantages and Problems</span>

<p class="text-xl first-letter">
IGN is built on idempotency, where its operator <katex-elem expr="f"/> ensures <katex-elem expr="f(f(z)) = f(z)"/> and <katex-elem expr="f(x) = x"/> for target data. It generates outputs in one step like GANs, with optional refinement akin to diffusion models. Unlike those, IGN keeps a consistent latent space for easy manipulation.
</p>

<div class="relative">
<img class="w-3/4 m-auto" src="/assets/project.jpg"/>

<katex-elem v-click expr="L_\text{rec}(x;\theta)=D\Big(x\Big|\Big|f_\theta(x)\Big)" class="text-#373 absolute top-60 left-58"/>
<katex-elem v-click expr="L_\text{idem}(z;\theta)=D\Big(f_\theta(z)\Big|\Big|f_{\text{SG}[\theta]}\big(f_\theta(x)\big)\Big)" class="text-#33A absolute top-25 left-80"/>
</div>

<SlideCurrentNo class="absolute bottom-4 right-8" />

<style>
h2 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
.slidev-layout h2 + p {
    margin-top: 0rem;
    margin-bottom: 1rem;
    opacity: 0.5;
}
.first-letter::first-letter {
  -webkit-initial-letter: 2;
  initial-letter: 2;
}
</style>

---

<div class="relative">
<img class="w-3/4 m-auto" src="/assets/project.jpg"/>

<katex-elem expr="L_\text{rec}(x;\theta)=D\Big(x\Big|\Big|f_\theta(x)\Big)" class="text-#373 absolute top-60 left-58"/>
<katex-elem expr="L_\text{idem}(z;\theta)=D\Big(f_\theta(z)\Big|\Big|f_{\text{SG}[\theta]}\big(f_\theta(x)\big)\Big)" class="text-#33A absolute top-25 left-80"/>
</div>

<p class="text-xl first-letter">
However, the combination of these two loss functions by itself does not guarantee that the target manifold is compact and contains only the true data distribution.
</p>

# <span v-click>The Issue of Manifold Expansion</span>

<SlideCurrentNo class="absolute bottom-4 right-8" />

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
.slidev-layout h2 + p {
    margin-top: 0rem;
    margin-bottom: 1rem;
    opacity: 0.5;
}
.first-letter::first-letter {
  -webkit-initial-letter: 2;
  initial-letter: 2;
}
</style>

---

# The Issue of Manifold Expansion
<span></span>

<p class="text-xl">
If the model <katex-elem expr="f" /> directly becomes the identity function, that is, <katex-elem expr="f(z) = z,\forall z" />. Both <katex-elem expr="L_\text{rec}" /> and <katex-elem expr="L_\text{idem}" /> are perfectly satisfied.
However, this model does <strong>NOT LEARN</strong> to generate any new samples that <strong>RESEMBLE THE TARGET DISTRIBUTION</strong>.
</p>

<div class="relative">
<img class="w-3/4 m-auto" src="/assets/identity.svg"/>

<katex-elem expr="L_\text{rec}(x;\theta)" class="text-#373 absolute top-60 left-58"/>
<katex-elem expr="L_\text{idem}(z;\theta)" class="text-#33A absolute top-10 left-140"/>
</div>

<SlideCurrentNo class="absolute bottom-4 right-8" />

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
.slidev-layout h2 + p {
    margin-top: 0rem;
    margin-bottom: 1rem;
    opacity: 0.5;
}
.first-letter::first-letter {
  -webkit-initial-letter: 2;
  initial-letter: 2;
}
</style>

---

## Tightness Objective<span v-click>: Self-Adversarial Training</span> 
<br/>

$$\tilde L_\text{tight}(z;\theta)=-D\Big(f_{\text{SG}[\theta]}(z)\Big|\Big|f_\theta\big(f_{\text{SG}[\theta]}(z)\big)\Big)$$

<div v-after>

$$\Updownarrow$$
$$L_\text{idem}(z;\theta)=D\Big(f_\theta(z)\Big|\Big|f_{\text{SG}[\theta]}\big(f_\theta(x)\big)\Big)$$
</div>

<p v-click class="text-xl">
However, optimizing directly against the tightness objective will conflict with the generation target.
</p>

<div v-click>

$$L_\text{tight}(z)=\tanh\Big(\cfrac{\tilde{L}_\text{tight}(z)}{a L_\text{rec}(z)}\Big)a L_\text{rec}(z)$$
</div>

<img v-after class="absolute top-20 left-0 z--1" src="/assets/geogebra-export.svg" />

<SlideCurrentNo class="absolute bottom-4 right-8" />

<style>
h2 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
.slidev-layout h2 + p {
    margin-top: 0rem;
    margin-bottom: 1rem;
    opacity: 0.5;
}
</style>

---

## Training Objective
<br/>

$$\mathcal{L}(\theta)=\lambda_r\mathbb{E}_x[L_\text{rec}(x;\theta)]+\lambda_i\mathbb{E}_z[L_\text{idem}(z;\theta)]+\lambda_t\mathbb{E}_z[L_\text{tight}(z;\theta)]$$

<img class="w-3/4 m-auto" src="/assets/archi2.jpg" />


<SlideCurrentNo class="absolute bottom-4 right-8" />

<style>
h2 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
.slidev-layout h2 + p {
    margin-top: 0rem;
    margin-bottom: 1rem;
    opacity: 0.5;
}
</style>

---

<div class="grid grid-cols-12">
<div class="col-span-3">

# Experiment
</div>
<img class="col-span-9" src="/assets/generation.jpg"/>

</div>

<h2 v-click>Make It Real</h2>
<br/>

<img v-after class="w-4/5 m-auto" src="/assets/proj3.jpg"/>

<SlideCurrentNo class="absolute bottom-4 right-8" />

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
h2 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

---

<div class="grid grid-cols-4">
<div class="col-span-2">

## Experiment
<span class="text-2xl">Latent Space Manipulations.</span>

<img src="/assets/composit.png"/>

</div>

<div class="col-span-2">
<img v-click src="/assets/noise_arithmtic.jpg"/>
<video v-click class="w-full" autoplay loop muted>
  <source src="/assets/celeba.mp4" type="video/mp4">
</video>
</div>

</div>

<SlideCurrentNo class="absolute bottom-4 right-8" />

<style>
h2 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
.slidev-layout h2 + p {
    margin-top: 0rem;
    margin-bottom: 1rem;
    opacity: 0.5;
}
</style>

---

## Experiment
<span class="text-2xl">$f^{k\to\infty}(z)$</span>


<img class="w-3/5 absolute top-28 left-50 z--1" src="/assets/k-to-infty.png"/>

<SlideCurrentNo class="absolute bottom-4 right-8" />

<style>
h2 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
.slidev-layout h2 + p {
    margin-top: 0rem;
    margin-bottom: 1rem;
    opacity: 0.5;
}
</style>

---

# Conclusions


<div class="grid grid-cols-2">

<div>

## Advantages

- Single-step generation is fast and efficient.  

- Optional sequence refinement. 
- Maintains consistent latent space. 
- Good generalization and projection capabilities. 
- Simplifies editing process.

</div>


<div>

## Weaknesses

- Generation quality needs to be improved.  

- Mode collapse may occur.  

- Potential issue tightness objective.  
It may impose large modifications on relatively good generated instances and may encourage high gradients and instability.

</div>

</div>

<SlideCurrentNo class="absolute bottom-4 right-8" />

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>



---
layout: end
---
