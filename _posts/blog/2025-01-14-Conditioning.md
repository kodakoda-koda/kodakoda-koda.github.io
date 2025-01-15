---
categories:
  - BLOG
date: 2025-01-14 00:00:00 +0900
math: true
tags:
  - Paper
  - NLP
title: "Expert Units in Conditioning Large Language Models"
parse_block_html: true
published: true
---

LLMs acquire a vast amount of information from pre-training data.
However, the mechanisms by which LLMs store this information remain unclear.
In this page, I will review the paper "Self-conditioning Pre-Trained Language Models" published in ICML 2022.
The authors of this paper focus on the conditioning generation of LLMs.
They propose a method to identify expert units that can effectively condition the generation of LLMs.
Surprisingly, these expert units constitute only a small fraction of the LLMs, yet they can condition the generation of LLMs effectively.
This discovery provides a deeper understanding of how LLMs can store and utilize information efficiently, highlighting the potential for more targeted and effective conditioning in future models.

## Paper Review: "Self-conditioning Pre-Trained Language Models"
### Background
Transformers have achieved remarkable success in various NLP tasks, thanks to their ability to capture complex patterns in text data.
Since its development, the transformer architecture has been widely adopted in various pre-trained language models (TLMs), such as BERT, GPT, and RoBERTa.
Despite their success, the mechanisms that govern and control text generation using TLMs remain unclear.
TLMs have two important drawbacks:
1. Conditioning their generation requires either expensive fine-tuning or the use of additional parameters.
2. TLMs might inherit and perpetuate biases present in the training data.

For instance, Chen et al. (2019) employ two latent embeddings for syntax and semantics, enabling flexible conditioning.
Romanov et al. (2019) utilize adversarial training to separate meaning and format.
Hu et al. (2017) integrate a Variational Auto-Encoder with attribute-specific discriminators to control sentiment and tense.
Peng et al. (2018) leverage human-specified control factors to influence story endings.
CTRL (Keskar et al., 2019) prepends control codes to training sentences for conditioning purposes.
Schiller et al. (2020) extend CTRL to generate arguments within specific contexts. 
These methods often necessitate pre-known conditioning, substantial data, and complex computations.

### Related Work
