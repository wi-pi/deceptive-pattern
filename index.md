---
layout: project
title: "Automatically Detecting Online Deceptive Patterns"
authors: '<a href="https://asmitnayak.com" target="_blank">Asmit Nayak</a>, <a href="https://wiscprivacy.com/member/member_yash/" target="_blank">Yash Wani*</a>, <a href="https://www.annienobear.com/" target="_blank">Shirley Zhang</a>*, Rishabh Khandelwal, <a href="https://kassemfawaz.com" target="_blank">Kassem Fawaz</a>'
affiliation: "University of Wisconsin - Madison"
venue: "ACM CCS 2025"
equal_contribution: "*Indicates Equal Contribution"
paper_url: "https://arxiv.org/pdf/2411.07441"
code_url: ""
arxiv_url: "https://arxiv.org/abs/2411.07441"
dataset_url: ""
demo_url: "https://huggingface.co/spaces/WIPI/DeceptivePatternDetector"
description: "AutoBot accurately identifies and localizes deceptive patterns from website screenshots without relying on HTML code, achieving an F1-score of 0.93."
permalink: /
bibtex: |
  @article{nayak2025autobot,
    title={Automatically Detecting Online Deceptive Patterns},
    author={Nayak, Asmit and Wani, Yash and Zhang, Shirley and Khandelwal, Rishabh and Fawaz, Kassem},
    year={2024}
  }
---

[//]: # (## Detecting Deceptive Patterns on the Web)

[//]: # (<div class="project-figure">)

[//]: # (  <img src="{{ '/assets/images/autobot-teaser.svg' | relative_url }}" alt="AutoBot Framework Overview">)

[//]: # (  <figcaption>AutoBot accurately identifies and localizes deceptive patterns from website screenshots, providing real-time feedback to users, developers, and regulators.</figcaption>)

[//]: # (</div>)

[//]: # (Deceptive patterns in digital interfaces manipulate users into making unintended decisions, exploiting cognitive biases and psychological vulnerabilities. These patterns have become ubiquitous on various digital platforms. While efforts to mitigate deceptive patterns have emerged from legal and technical perspectives, a significant gap remains in creating usable and scalable solutions.)

## Abstract

<div class="abstract">
<p>We introduce our <strong>AutoBot framework</strong> to address this gap and help web stakeholders navigate and mitigate online deceptive patterns. AutoBot accurately identifies and localizes deceptive patterns from a screenshot of a website without relying on the underlying HTML code. AutoBot employs a two-stage pipeline that leverages the capabilities of specialized vision models to analyze website screenshots, identify interactive elements, and extract textual features. Next, using a large language model, AutoBot understands the context surrounding these elements to determine the presence of deceptive patterns.</p>

<p>We also use AutoBot to create a synthetic dataset to distill knowledge from 'teacher' LLMs to smaller language models. Through extensive evaluation, we demonstrate AutoBot's effectiveness in detecting deceptive patterns on the web, achieving an <strong>F1-score of 0.93</strong> when detecting deceptive patterns, underscoring its potential as an essential tool for mitigating online deceptive patterns. We implement AutoBot across three downstream applications targeting different web stakeholders:</p>

<ol>
<li> A local browser extension providing users with real-time feedback </li>
<li> A Lighthouse audit to inform developers of potential deceptive patterns on their sites </li>
<li> A measurement tool designed for researchers and regulators </li>
</ol>
</div>


## The AutoBot Framework

<div class="project-figure">
  <img src="{{ '/assets/images/system_overview.jpg' | relative_url }}" alt="AutoBot Two-Stage Pipeline">
  <figcaption>AutoBot's two-stage pipeline: (1) Vision Module to localize UI elements and extract features, (2) Language Module to detect deceptive patterns.</figcaption>
</div>

AutoBot adopts a modular design, breaking down the task into two distinct modules: a Vision Module for element localization and feature extraction, and a Language Module for deceptive pattern detection. This approach allows AutoBot to work with screenshots alone, without requiring access to the underlying HTML code, which tends to be less stable across different webpage implementations.

### Vision Module

To address high false positive rates and localization issues, the Vision Module parses a webpage screenshot and maps it to a tabular representation we call *ElementMap*. As illustrated in the figure above, the *ElementMap* contains the text associated with each UI element, along with its features: element type, bounding box coordinates, font size, background color, and font color. For UI element detection, we train a YOLOv10 model on a synthetically generated dataset. The evaluation of our model is presented below.

<div class="project-figure">
  <div style="display: flex; gap: 20px; justify-content: center; align-items: flex-end;">
    <div style="width: 48%; display: flex; flex-direction: column; align-items: center;">
      <img src="{{ '/assets/images/ui-detector.png' | relative_url }}" alt="UI Detector" style="width: 100%;">
      <figcaption style="margin-top: 10px; font-size: 0.9em; text-align: center;">(a) Synthetic dataset generation pipeline for training the Web-UI Detector</figcaption>
    </div>
    <div style="width: 48%; display: flex; flex-direction: column; align-items: center;">
      <img src="{{ '/assets/images/ccs-2025-vision-eval.png' | relative_url }}" alt="Vision Module Evaluation" style="width: 100%;">
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

<!-- ## Key Results

### Performance Metrics

AutoBot achieves state-of-the-art results on the task of deceptive pattern detection:

<div class="features-grid">
  <div class="feature-card">
    <h3>F1-Score: 0.93</h3>
    <p>Significantly outperforming existing methods in overall detection accuracy</p>
  </div>
  <div class="feature-card">
    <h3>Precision: 0.92</h3>
    <p>Minimizing false positives to ensure reliable detection</p>
  </div>
  <div class="feature-card">
    <h3>Recall: 0.94</h3>
    <p>Catching the vast majority of deceptive patterns in the wild</p>
  </div>
</div>

### Comparison with State-of-the-Art Models

<div class="project-figure">
  <img src="{{ '/assets/images/comparison-chart.svg' | relative_url }}" alt="Performance Comparison">
  <figcaption>AutoBot outperforms existing approaches including GPT-4, Gemini, and heuristic-based methods in detecting deceptive patterns across multiple categories.</figcaption>
</div>

Our evaluation shows that leading vision-language models like GPT-4.5 and Gemini 2.5 Pro struggle with accurately identifying deceptive patterns, often misclassifying visual distinctions such as button prominence. AutoBot's specialized architecture addresses these limitations.

## Deceptive Pattern Categories

AutoBot detects deceptive patterns across four main categories:

### Interface Interference

Manipulating the interface to mislead users through visual design choices.

- **Confirmshaming**: Using guilt-inducing language to pressure users
- **Fake Urgency/Scarcity**: Creating false sense of time pressure or limited availability
- **Nudge**: Visual asymmetry to guide users toward specific choices

### Forced Action

Compelling users to take unwanted actions to achieve their goals.

- **Forced Registration**: Requiring account creation for basic functionality
- **Forced Enrollment**: Automatically subscribing users to services

### Obstruction

Making desired actions difficult or hiding important information.

- **Pre-selection**: Default selections that favor the service provider
- **Visual Interference**: Using design to hide or obscure information
- **Hard to Cancel**: Making cancellation processes deliberately complex

### Sneaking

Concealing or delaying disclosure of important information.

- **Hidden Costs**: Not showing full price until late in the process
- **Hidden Subscription**: Unclear subscription terms
- **Disguised Ads**: Advertisements that look like content
- **Trick Wording**: Confusing or misleading language

## Applications

### 1. Browser Extension for Users

<div class="project-figure">
  <img src="{{ '/assets/images/browser-extension.svg' | relative_url }}" alt="AutoBot Browser Extension">
  <figcaption>Real-time browser extension that highlights deceptive patterns as users browse the web, providing instant feedback and explanations.</figcaption>
</div>

A local browser extension provides users with real-time feedback, highlighting detected deceptive patterns with visual indicators and detailed explanations.

### 2. Lighthouse Audit for Developers

<div class="project-figure">
  <img src="{{ '/assets/images/lighthouse-audit.svg' | relative_url }}" alt="Lighthouse Audit Integration">
  <figcaption>Custom Lighthouse audit that helps developers identify and fix potential deceptive patterns during the development process.</figcaption>
</div>

Our custom Lighthouse audit integrates directly into developer workflows, providing quantifiable scores for deceptive pattern presence and specific recommendations for improvement.

### 3. Measurement Tool for Researchers

<div class="project-figure">
  <img src="{{ '/assets/images/measurement-tool.svg' | relative_url }}" alt="Large-Scale Measurement">
  <figcaption>Large-scale measurement capabilities enable researchers and regulators to analyze deceptive pattern prevalence across thousands of websites.</figcaption>
</div>

AutoBot serves as a large-scale measurement tool for researchers and regulators, enabling systematic analysis of deceptive pattern prevalence across the web.

## User Study Findings

<div class="project-figure">
  <img src="{{ '/assets/images/user-study.svg' | relative_url }}" alt="User Study Results">
  <figcaption>User study results showing that AutoBot's highlighting improves user awareness without negatively impacting website usability (p=0.106).</figcaption>
</div>

Our user study with real participants demonstrates that:

- AutoBot's visual highlighting significantly improves user awareness of deceptive patterns
- The highlighting system does not negatively impact perceived website usability
- Users appreciate the real-time feedback and find it helpful for making informed decisions 

## Knowledge Distillation

To make AutoBot more efficient and accessible, we developed a knowledge distillation approach:

1. **Synthetic Dataset Generation**: Using AutoBot with teacher LLMs to create a large annotated dataset
2. **Model Distillation**: Training smaller, faster models (T5-based) that maintain high accuracy
3. **Deployment Efficiency**: Enabling real-time detection in resource-constrained environments like browser extensions

The distilled models achieve comparable performance to teacher models while being significantly faster and more cost-effective.

## Impact and Future Work

AutoBot represents a significant step forward in combating deceptive patterns on the web:

- **For Users**: Empowering informed decision-making through real-time detection
- **For Developers**: Providing tools to build more ethical and transparent interfaces
- **For Regulators**: Enabling large-scale monitoring and enforcement

Future work includes:
- Expanding the taxonomy to cover emerging deceptive pattern types
- Improving multilingual support for global deployment
- Developing automated remediation suggestions for developers -->

## Demo Video 

<div class="video-section">
  <video controls muted loop playsinline preload="auto" autoplay>
    <source src="{{ '/assets/video/demo.webm' | relative_url }}" type="video/webm">
    Your browser does not support the video tag.
  </video>
  <figcaption>AutoBot in action: Detection of deceptive patterns on live websites. <a href="{{ page.demo_url }}" target="_blank">Try the interactive demo →</a></figcaption>
</div>