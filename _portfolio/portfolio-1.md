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

A new dataset was prepared for this task by manually editing the "Call me Sexist" Dataset ( due to existing biases in the dataset ) and an additional workplace sexism dataset.

### Procedure

### Baseline Model

 A pretrained modular model checkpoint trained on the WNC by  [Pryzant2020AutomaticallyNS](https://www.semanticscholar.org/paper/Automatically-Neutralizing-Subjective-Bias-in-Text-Pryzant-Martinez/16981cc4ddefd3ea7655754fd83a2a8ff2203a8b) served as the baseline for our experiments. Inference was run directly on the model with our test sets and subsequently evaluated as per Section 4.3.

### Fine tuning the pretrained model

 Using the baseline as a starting checkpoint, the tagger and debiaser models were jointly trained on the modified "Call Me Sexist" dataset. This model, called checkpoint-1, was stored and evaluated as a benchmark.

### Data augmentation

 Using checkpoint-1, inference was carried out on the "Workplace Sexism" dataset along with the unneutralised 1,305 pairs from the "Call me Sexist" dataset. The predicted sequences were then used as the gold sequences for further fine-tuning to try and improve our current model. 
 A perplexity score was calculated for each prediction to find good neutralisations. A low perplexity score is indicative of a coherent sentence based on a language model.
 The predictions were run through a pretrained GPT2-large model from hugging face with a sliding window to calculate the log-likelihood. Then, the sentences with the highest perplexities were removed. In this case, sentences in the top 40th percentile of perplexities were filtered out.  

 Additionally, Perspective API was used to calculate a toxicity score for each of the sentences. This API uses Machine Learning to calculate a percentage for the toxicity score, which indicates how "toxic" a sentence may be perceived. Sentences with a toxicity of over 50% were removed.

### Fine-tuned model with augmented data
 The model loaded from checkpoint-1 was further fine-tuned with the filtered augmented data. 

### Evaluation

Following our models were firstly evaluated using a BLEU score and true hits (true hits is a scale of how many gold sentences exactly matched our predictions). However, we felt that this was an inadequate representation of the difference in the quality of neutralisations between models. To align with human evaluation, we designed two metrics that reflected the performance improvement. All quantitative results were reported with the mean of the three test sets and a range of two standard deviations.


### Sexism Classification Model 
A linear Support Vector Machine (SVM) was trained on the cleaned "Call me Sexist" dataset, using the term-frequency inverse document frequency (tf-idf) of the sentences as features and "sexist or non-sexist" as labels. On a test set, the accuracy of the classifier was approximately 86\%. Passing the generated sentences through this classifier returns a quantitative idea of the number of sentences in the test set that are not sexist after neutralisation. Unlike human evaluation, we could calculate this number over multiple test sets in a time-efficient manner.  

### Sentence Embedding
 Another form of evaluation was conducted using embeddings of the sequences using a pretrained sentence transformer. We used hugging face's "distilroberta-v1" model to map the sentences into sentence vectors of size 768. We calculated two sets of embeddings; a gold embedding matrix consisting of all the manually neutralised gold sentences and a predictions embedding matrix containing the embeddings of our generated sentences from any model. The closer the embeddings of the generated sequences of a model were to the embeddings of the gold sequences, the better the performance of that particular model was. This ensured that this metric could quantify similarity without human intervention, even if the neutralised sentences were not identical to the gold sequences, just as long as they had a similar meaning.


For further details of the results please see our report [here](https://github.com/ArnabPushilal/NLPreport/blob/master/NLP_report.pdf) and for the code please visit the repo [here](https://github.com/ArnabPushilal/NLPreport/blob/master/NLP_report.pdf).

