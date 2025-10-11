---
layout: project
title: "Automatically Detecting Online Deceptive Patterns"
authors:
  - name: '<a href="https://asmitnayak.com" target="_blank">Asmit Nayak</a>'
    email: "anayak6@wisc.edu"
  - name: '<a href="https://wiscprivacy.com/member/member_yash/" target="_blank">Yash Wani</a>*'
    email: "ywani@wisc.edu"
  - name: '<a href="https://www.annienobear.com/" target="_blank">Shirley Zhang</a>*'
    email: "hzhang664@wisc.edu"
  - name: 'Rishabh Khandelwal'
    email: "rkhandelwal3@wisc.edu"
  - name: '<a href="https://kassemfawaz.com" target="_blank">Kassem Fawaz</a>'
    email: "kfawaz@wisc.edu"
affiliation: "University of Wisconsin - Madison"
venue: "ACM CCS 2025"
equal_contribution: "*Indicates Equal Contribution"
paper_url: "https://arxiv.org/pdf/2411.07441"
code_url: ""
#arxiv_url: "https://arxiv.org/abs/2411.07441"
dataset_url: ""
demo_url: "https://huggingface.co/spaces/WIPI/DeceptivePatternDetector"
description: "AutoBot accurately identifies and localizes deceptive patterns from website screenshots without relying on HTML code, achieving an F1-score of 0.93."
permalink: /
bibtex: |
  @article{nayak2025autobot,
    title={Automatically Detecting Online Deceptive Patterns},
    author={Nayak, Asmit and Wani, Yash and Zhang, Shirley and Khandelwal, Rishabh and Fawaz, Kassem},
    year={2025}
  }
---

## Abstract

<div class="abstract">
<p>Deceptive patterns in digital interfaces manipulate users into making unintended decisions, exploiting cognitive biases and psychological vulnerabilities. These patterns have become ubiquitous on various digital platforms. While efforts to mitigate deceptive patterns have emerged from legal and technical perspectives, a significant gap remains in creating usable and scalable solutions. We introduce our <strong>AutoBot framework</strong> to address this gap and help web stakeholders navigate and mitigate online deceptive patterns. <em><strong>AutoBot</strong></em> accurately identifies and localizes deceptive patterns from a screenshot of a website without relying on the underlying HTML code. <em><strong>AutoBot</strong></em> employs a two-stage pipeline that leverages the capabilities of specialized vision models to analyze website screenshots, identify interactive elements, and extract textual features. Next, using a large language model, <em><strong>AutoBot</strong></em> understands the context surrounding these elements to determine the presence of deceptive patterns. We also use <em><strong>AutoBot</strong></em>, to create a synthetic dataset to distill knowledge from ‘teacher’ LLMs to smaller language models. Through extensive evaluation, we demonstrate <em><strong>AutoBot</strong></em>’s effectiveness in detecting deceptive patterns on the web, achieving an F1-score of 0.93 in this task, underscoring its potential as an essential tool for mitigating online deceptive patterns.</p>
<p>We implement <em><strong>AutoBot</strong></em>, across three downstream applications targeting different web stakeholders: (1) a local browser extension providing users with real-time feedback, (2) a Lighthouse audit to inform developers of potential deceptive patterns on their sites, and (3) as a measurement tool for researchers and regulators.</p>
</div>


## Why Off-the-Shelf LLMs Fall Short

While state-of-the-art vision-language models show promise in understanding visual content, directly applying them to detect deceptive patterns reveals significant limitations. These models often struggle with hallucination and lack of localization needed to identify deceptive patterns in real-world web interfaces. 

<div class="project-figure">
  <div style="display: flex; gap: 20px; justify-content: space-evenly; align-items: flex-start; flex-wrap: wrap;">
    <div style="width: 35%; min-width: 300px; display: flex; flex-direction: column; align-items: center;">
      <img src="{{ '/assets/images/gemini-failure.jpg' | relative_url }}" alt="Gemini Failure Case" style="width: 100%;">
      <figcaption style="margin-top: 10px; font-size: 0.9em; text-align: center;">Gemini hallucinates that the "Accept all cookies" button being more visually prominent than the "Necessary cookies only" one.</figcaption>
    </div>
    <div style="width: 35%; min-width: 300px; display: flex; flex-direction: column; align-items: center;">
      <img src="{{ '/assets/images/gpt-failure.jpg' | relative_url }}" alt="GPT-4V Failure Case" style="width: 100%;">
      <figcaption style="margin-top: 10px; font-size: 0.9em; text-align: center;">GPT-4.5 hallucinates that the "Accept all cookies" and "Reject all" button are visually different.</figcaption>
    </div>
  </div>
  <figcaption>Out-of-the-box LLMs demonstrate high false positive rates and poor localization accuracy when detecting deceptive patterns.</figcaption>
</div>

These limitations motivated us to develop a specialized framework that combines vision and language models in a structured framework, rather than relying on end-to-end models that lack the precision required for this task.


## The AutoBot Framework

<div class="project-figure">
  <img src="{{ '/assets/images/system_overview.jpg' | relative_url }}" alt="AutoBot Two-Stage Pipeline">
  <figcaption>AutoBot's two-stage pipeline: (1) Vision Module to localize UI elements and extract features, (2) Language Module to detect deceptive patterns.</figcaption>
</div>

AutoBot adopts a modular design, breaking down the task into two distinct modules: a Vision Module for element localization and feature extraction, and a Language Module for deceptive pattern detection. This approach allows AutoBot to work with screenshots alone, without requiring access to the underlying HTML code, which tends to be less stable across different webpage implementations.

### Vision Module

To address high false positive rates and localization issues, the Vision Module parses a webpage screenshot and maps it to a tabular representation we call *ElementMap*. As illustrated in the figure above, the *ElementMap* contains the text associated with each UI element, along with its features: element type, bounding box coordinates, font size, background color, and font color. For UI element detection, we train a YOLOv10 model on a synthetically generated dataset. The evaluation of our model is presented below.
<div class="project-figure" style="margin-top: -2rem; margin-bottom: 2rem;">
  <div class="vision-module-flex" style="display: flex; gap: 20px; justify-content: center; align-items: flex-end;">
    <div class="vision-module-item" style="width: 48%; display: flex; flex-direction: column; align-items: center;">
      <img src="{{ '/assets/images/ui-detector.png' | relative_url }}" alt="UI Detector" style="width: 100%;">
      <figcaption style="margin-top: 10px; font-size: 0.9em; text-align: center;">(a) Synthetic dataset generation pipeline for training the Web-UI Detector</figcaption>
    </div>
    <div class="vision-module-item" style="width: 48%; display: flex; flex-direction: column; align-items: center;">
      <div style="display: flex; justify-content: center; align-items: flex-end; height: 100%;">
        <div style="width: 1008px; height: 294px; overflow: hidden; display: flex; justify-content: center; align-items: flex-end;">
          <iframe src="{{ '/assets/plotly/ccs-2025-vision-eval.html' | relative_url }}"
                  style="width: 1008px; height: 530px; border: none; transform: scale(0.55); transform-origin: center bottom;margin-bottom: -30px"
                  frameborder="0">
          </iframe>
        </div>
      </div>
      <figcaption style="margin-top: 10px; font-size: 0.9em; text-align: center;">(b) Evaluation of YOLO (Ours) vs Molmo across different UI element types</figcaption>
    </div>
  </div>
  <figcaption>The Vision Module processes screenshots to extract UI elements and their features into an <em>ElementMap</em> representation.</figcaption>
</div>

### Language Module

The Language Module takes the *ElementMap* as input and maps each element to a deceptive pattern from our taxonomy. This module reasons about each element considering its spatial context and visual features. We explore different instantiations of this module—such as distilling smaller models like Qwen and T5 from a larger teacher model like Gemini—to achieve various trade-offs in terms of cost, need for training, and accuracy.

<div class="project-figure">
  <img src="{{ '/assets/images/language-module.png' | relative_url }}" alt="Language Module Detection">
  <figcaption>The Language Module analyzes <em>ElementMap data</em> to identify and classify deceptive patterns in context.</figcaption>
</div>

## E2E Evaluation

We evaluate AutoBot's end-to-end performance by comparing different instantiations of our Language Module on the task of deceptive pattern detection. The interactive visualization below presents the performance metrics of *AutoBot* using a range of LLM -- Gemini, distilled Qwen-2.5-1.5B and distilled T5-base models. These results demonstrate how different model choices affect detection accuracy, precision, and recall across our deceptive pattern taxonomy.

<div class="project-figure" style="margin: 30px auto !important; text-align: center; display: flex; flex-direction: column; align-items: center;">
  <iframe src="{{ '/assets/plotly/ccs-2025-llm-eval-category.html' | relative_url }}" 
          style="width: 100%; max-width:90vw; height: 531px; border: none;" 
          frameborder="0">
  </iframe>
  <figcaption style="max-width:90vw;">Interactive comparison of performance of <em>AutoBot</em>(with three underlying language models: Gemini, Qwen, and T5) at the Category Level.</figcaption>
</div>

<div class="project-figure" style="margin: 30px auto !important; text-align: center; display: flex; flex-direction: column; align-items: center;">
  <iframe src="{{ '/assets/plotly/ccs-2025-llm-eval-subtype.html' | relative_url }}" 
          style="width: 1510px; max-width:90vw; height: 531px; border: none;" 
          frameborder="0">
  </iframe>
  <figcaption style="">Interactive comparison of performance of <em>AutoBot</em>(with three underlying language models: Gemini, Qwen, and T5) and <em>DPGuard</em> at the Subtype Level</figcaption>
</div>


## Demo Video 

<div class="video-section">
  <video controls muted loop playsinline preload="auto" autoplay>
    <source src="{{ '/assets/video/demo_detailed.webm' | relative_url }}" type="video/webm">
    Your browser does not support the video tag.
  </video>
  <figcaption>AutoBot in action: Detection of deceptive patterns on live websites. <a href="{{ page.demo_url }}" target="_blank">Try the interactive demo →</a></figcaption>
</div>