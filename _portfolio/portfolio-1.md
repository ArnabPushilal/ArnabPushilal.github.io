---
title: "Project-1"
excerpt: "Automatic Neutralization of Sexist language<br/><img src='/images/flowshabri.png'>"
collection: portfolio
---

## Abstract

Sexism is very common online. Therefore, it is essential to effectively detect and neutralise sexist language to create a safe and inclusive online environment. A model to neutralise sexist language may act as a filter that neutralises sexist statements online. In this study, we built on the work of [Pryzant2020AutomaticallyNS](https://www.semanticscholar.org/paper/Automatically-Neutralizing-Subjective-Bias-in-Text-Pryzant-Martinez/16981cc4ddefd3ea7655754fd83a2a8ff2203a8b), who created a model to neutralise biased language on Wikipedia. We fine-tuned this model using the "Call Me Sexist" dataset [Samory2021CallMS](https://ojs.aaai.org/index.php/ICWSM/article/view/18085), consisting of sexist tweets and their neutralised pairs. We present a new version of this dataset, with 2,405 manually neutralised tweets. We also presented two new automatic evaluation methods; sentence embeddings and the use of a sexism classification model. In addition, we proposed an automated training data augmentation pipeline to improve our model further. Fine-tuning the model with the new dataset resulted in high quality and coherent sentences as per human evaluation, and 72% of the predicted sentences were classified as not sexist by automatic evaluation. 


## Hypothesis

In this study, we investigated the following hypotheses:

1. The baseline model created by [Pryzant2020AutomaticallyNS](https://www.semanticscholar.org/paper/Automatically-Neutralizing-Subjective-Bias-in-Text-Pryzant-Martinez/16981cc4ddefd3ea7655754fd83a2a8ff2203a8b) can neutralise sexist language as it has been trained on data with demographic bias.
2. The baseline model can be improved by fine-tuning a dataset of sexist examples with their respective neutralisations. 
3. An automatic data augmentation pipeline can be built to create additional training data to improve the fine-tuned model.
4. item Neutralised sentences can be automatically evaluated without human intervention.


### Model <img src='/images/modelarch.png'>

The model used in this study was developed by [Pryzant2020AutomaticallyNS](https://www.semanticscholar.org/paper/Automatically-Neutralizing-Subjective-Bias-in-Text-Pryzant-Martinez/16981cc4ddefd3ea7655754fd83a2a8ff2203a8b). The modular model is comprised of two models; a tagger model, which uses BERT  to classify words as biased and a debiaser model, which is an LSTM (Long Short-Term Memory) based model to edit the text. Each of these models was pretrained separately and subsequently combined. The training data used was the WNC. The data included text with demographic bias (data with pre-conceived notions about particular genders and demographic categories). Therefore, we hypothesised that this model would work well for sexism neutralisation.


### New Dataset

A new dataset was prepared for this task by manually editing the "Call me Sexist" Dataset ( due to existing biases in the dataset )

