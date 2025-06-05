---
# You can also start simply with 'default'
theme: seriph
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://cover.sli.dev
# some information about your slides (markdown enabled)
title: EDM-DDO
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

<h1><a href="https://github.com/NVlabs/DDO"><span class="text-14">D</span>IRECT <span class="text-14">D</span>ISCRIMINATIVE <span class="text-14">O</span>PTIMIZATION</a></h1>

## Your Likelihood-Based Visual Generative Model is Secretly a GAN Discriminator

<br/>

Kaiwen Zheng$^{1,2}$,
Yongxin Chen$^1$,
Huayu Chen$^2$,
Guande He$^3$,  
Ming-Yu Liu$^1$,
Jun Zhu$^2$,
Qinsheng Zhang$^1$

<p class="text-justify pl-80">
<katex-elem expr='^1' /> NVIDIA<br/>
<katex-elem expr='^2' /> Tsinghua University<br/>
<katex-elem expr='^3' /> The University of Texas at Austin
</p>

## ICML 2025 Spotlight

<div class="abs-br m-6 flex gap-2">
  <a href="https://github.com/toonnyy8-notes/edm-ddo" target="_blank" alt="GitHub" title="Open in GitHub"
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

# Issue of Maximum Likelihood Estimation
<span></span>

<div class="text-xl text-justify">

Diffusion Probabilistic Models (DPMs) and Autoregressive Models (ARs) based on **Maximum Likelihood Estimation (MLE)** have achieved dominance in both continuous and discrete data generation. Its <span v-click>**Stability**,</span> 
</div>


<div class="ml-34 text-xl text-justify">
 
<span v-click>**Scalability** and</span>

<span v-click>**Generalization**</span> are better than Generative Adversarial Networks (GANs).
</div>

---

# Issue of Maximum Likelihood Estimation
<span></span>

<div class="text-xl text-justify">

However, when the model capacity is limited, the training method of MLE will cause the generation of blur in order to cover a wider range of samples, <span v-click>also known as **"MODE-COVERING."**</span>
</div>

<div v-click class="text-xl text-justify">
In order to solve this problem, introducing the adversarial loss of GANs is one of the solutions, but
</div>
<div v-click class="text-justify">

- GANs need to construct additional discriminators for alternating optimization, which increases engineering complexity.
- The iterative sampling of DPMs and ARs makes it difficult to easily optimize through GANs.
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

# Direct Preference Optimization (DPO)

<div class="grid grid-cols-3">
<img class="m-auto col-span-2" src="/assets/dpo.png" />

<div class="text-2xl ml-3">

<ul>
  <li v-click>Paired Data</li>
  <li v-click>Human-Annotated <span class="text-xl">preferred/non-preferred</span></li>
  <li v-click>Supervised Learning</li>
</ul>

</div>

</div>

<div class=text-xl v-click>

Optimizing the reward function
$$
p(y_w\succ y_l|x)\coloneqq\frac{e^{r(x,y_w)}}{e^{r(x,y_l)}+e^{r(x,y_w)}}=\sigma(r(x,y_w)-r(x,y_l))
$$

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

# Direct Preference Optimization (DPO)

<div class="grid grid-cols-3">
<img class="m-auto col-span-2" src="/assets/dpo.png" />

<div class="text-2xl ml-3">

<ul>
  <li>Paired Data</li>
  <li>Human-Annotated <span class="text-xl">preferred/non-preferred</span></li>
  <li>Supervised Learning</li>
</ul>

</div>

</div>


<div>

## <span class=text-2xl>Your Likelihood-Based Generative Model is Secretly a Discriminator</span>

$$
\begin{aligned}
\mathcal{L}^{\rm DPO}(\theta)=-\mathbb{E}_{(x,y_w,y_l)\sim\mathcal{D}}\log\sigma\left(\beta\log\frac{\pi_\theta(y_w|x)}{\pi_{\theta_\text{ref}}(y_w|x)}-\beta\log\frac{\pi_\theta(y_l|x)}{\pi_{\theta_\text{ref}}(y_l|x)}\right)
\end{aligned}
$$

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

<div class="mt-40 text-center text-2xl">Can we use <strong class="text-3xl">Secret Discriminator</strong> to enhance MLE-based Models?</div>

<div class="grid grid-cols-21 text-xl mt-10">

<div v-click class="m-auto col-span-10">

<strong class="text-2xl">Direct Preference Optimization</strong>

<ul>
  <li>Paired Data</li>
  <li>Preferred / Non-preferred</li>
</ul>

</div>

<div v-click class="mt-5">>></div>

<div v-after class="m-auto col-span-10">

<strong class="text-2xl">Direct Discriminative Optimization</strong>

<ul class="m-auto">
  <li>Real Data / Generated Data</li>
  <li>Truth / Fake</li>
</ul>

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

# Direct Discriminative Optimization

<pdf class="m-auto w-1/1" src="/assets/pipeline-crop.pdf"/>

<div class="grid grid-cols-2 text-xl mt-10">

<div v-click class="m-auto">Log likelihood scales significantly with data dimensions, causing the <strong>gradient to vanish</strong>.</div>
<div v-click class="m-auto">Evaluating the <strong>DPMs</strong> likelihood for a specific data point can be <strong>computationally intensive</strong>.</div>

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

# Direct Discriminative Optimization
## Practical Implementation

<span class="text-xl">**Generalized Objective with Extra Coeffecients**</span>  
To address gradient vanishing and numerical precision issues.

$$
\begin{aligned}
    \mathcal{L}_{\red\alpha,\red\beta}(\theta)=&-\mathbb{E}_{p_\text{data}(\text{x})}\left[\log \sigma\left(\red\beta\log\frac{p_\theta(\text{x})}{p_{\theta_\text{ref}}(\text{x})}\right)\right]\\
    &-\red\alpha\mathbb{E}_{p_{\theta_\text{ref}(\text{x})}}\left[\log\left(1-\sigma\left(\red\beta\log\frac{p_\theta(\text{x})}{p_{\theta_\text{ref}}(\text{x})}\right)\right)\right],\;\alpha\in [0.5, 50]\;\text{and}\;\beta\in [0.01, 0.1]
\end{aligned}
$$

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

# Direct Discriminative Optimization
## Practical Implementation

<span class="text-xl">**Handling Compute-Intensive Likelihood**</span>

<div class="grid grid-cols-3">

$$
\log\frac{p_\theta(\text{x})}{p_{\theta_\text{ref}}(\text{x})}\approx \mathbb{E}_{t,\epsilon}\left[\Delta_{\text{x}_t,t,\epsilon}\right]
$$

<div class="col-span-2">

where $\text{x}_t=\alpha_t\text{x}+\sigma_t\epsilon,\;\epsilon\sim\mathcal{N}(0,I)$ and  
$\Delta_{\text{x}_t,t,\epsilon}=- w(t)\left(\|\epsilon_\theta(\text{x}_t,t)-\epsilon\|_2^2-\|\epsilon_{\theta_\text{ref}}(\text{x}_t,t)-\epsilon\|_2^2\right)$

</div>
</div>

<span v-click class=text-xl>Apply Jensen’s inequalit: $\psi(\mathbb{E}[X])\leq\mathbb{E}[\psi(X)]$, if $\psi$ is a convex function e.g. $-\log\sigma(\cdot)$.</span>

<div v-click>

$$
\begin{aligned}
    &\mathcal{L}(\theta)\\
    \approx&-\mathbb{E}_{p_\text{data}(\text{x})}\log \sigma\left(\mathbb{E}_{t,\epsilon}\left[\Delta\right]\right)-\mathbb{E}_{p_{\theta\text{ref}}(\text{x})}\log(1-\sigma\left(\mathbb{E}_{t,\epsilon}\left[\Delta\right]\right))\\
    \leq &\underbrace{-\mathbb{E}_{t,\epsilon}\left[\mathbb{E}_{p_\text{data}(\text{x})}\log \sigma(\Delta)+\mathbb{E}_{p_{\theta_\text{ref}}(\text{x})}\log(1-\sigma(\Delta)))\right]}_\text{upper bound}
\end{aligned}
$$

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

# Direct Discriminative Optimization
## Amazing Feature: Multi-Round Refinement via Self-Play

$$
\begin{aligned}
\text{Round $n$:}\quad \ldots &\quad  \rightarrow  \underbrace{p_{\theta^*_{n-1}}}_{\text{Reference}} \rightarrow  \quad \underbrace{\sigma\left(\beta \log \frac{p_{\theta_n}}{p_{\theta_{n-1}^*}}\right)}_{\text{Discriminator}} \\
\text{Round $n+1$:}\quad&\quad \rightarrow \underbrace{p_{\theta_n^*}}_{\text{Reference} } \rightarrow \quad \ldots
\end{aligned}
$$
<img v-click class="m-auto w-9/10" src="/assets/fig6.png" />

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

# Experiment
<div class="grid grid-cols-2">
<div>
<span class="text-2xl">ImageNet 64x64</span>
<img v-after class="w-10/11" src="/assets/img64.png"/>
</div>
<div>
<span class="text-2xl">ImageNet 512x512</span>
<img v-after class="m-auto" src="/assets/img512.png"/>
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

## Experiment

<img v-after class="w-1/1 m-auto" src="/assets/fig7.png"/>


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

- Hight efficiency and effectiveness.

- No additional redundant structure required. 

<pdf class="w-8/9" src="/assets/compared-crop.pdf" />

</div>


<div>

## Weaknesses

- The need for hyperparameter searching.  

- It takes time to iteratively sample fake samples as training data.  
  <span v-click>Maybe it can be combined with a single-step generation method, e.g. Consistency Models \[[Song et al., 2023](https://openreview.net/forum?id=FmqFfMTNnv); [Song et al., 2023](https://openreview.net/forum?id=WNzy9bRDvG); [Lu et al., 2025](https://openreview.net/forum?id=LyJi5ugyJx)\]</span>

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
