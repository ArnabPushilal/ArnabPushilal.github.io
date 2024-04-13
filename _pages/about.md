---
permalink: /
title: "Automatic Neutralization of Sexist language"
excerpt: ""
author_profile: true
redirect_from: 
  - /about/
  - /about.html
---

<br/><img src='/images/flowshabri.png'>"

---



## Abstract

Sexism is very common online. Therefore, it is essential to effectively detect and neutralise sexist language to create a safe and inclusive online environment. A model to neutralise sexist language may act as a filter that neutralises sexist statements online. In this study, we built on the work of [Pryzant2020AutomaticallyNS](https://www.semanticscholar.org/paper/Automatically-Neutralizing-Subjective-Bias-in-Text-Pryzant-Martinez/16981cc4ddefd3ea7655754fd83a2a8ff2203a8b), who created a model to neutralise biased language on Wikipedia. We fine-tuned this model using the "Call Me Sexist" dataset [Samory2021CallMS](https://ojs.aaai.org/index.php/ICWSM/article/view/18085), consisting of sexist tweets and their neutralised pairs. We present a new version of this dataset, with 2,405 manually neutralised tweets. We also presented two new automatic evaluation methods; sentence embeddings and the use of a sexism classification model. In addition, we proposed an automated training data augmentation pipeline to improve our model further. Fine-tuning the model with the new dataset resulted in high quality and coherent sentences as per human evaluation, and 72% of the predicted sentences were classified as not sexist by automatic evaluation. 

## Code and Report

For further details of the results please see our report [here](https://github.com/ArnabPushilal/NLPreport/blob/master/NLP_report.pdf) and for the code please visit the repo [here](https://github.com/ArnabPushilal/NLPreport/blob/master/NLP_report.pdf).


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

 A pretrained modular model checkpoint trained on the WNC by  [Pryzant2020AutomaticallyNS](https://www.semanticscholar.org/paper/Automatically-Neutralizing-Subjective-Bias-in-Text-Pryzant-Martinez/16981cc4ddefd3ea7655754fd83a2a8ff2203a8b) served as the baseline for our experiments. Inference was run directly on the model with our test sets.

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


### Results and Analysis

<img src='/images/resultsnlp.png'>
<img src='/images/table1.png'>
<img src='/images/table2.png'>

#### Baseline Model

We first analysed the results of the baseline model. The qualitative results revealed that the model neutralised sentences in two main ways. In some cases, references to gender were removed; for example, the word "woman" was removed, as seen in rows 1c and 4c of Table 5. We hypothesised that this was because the expert features included the word "woman". Therefore the word was treated as a biased word and was removed by the model. In other cases, the word "claim" was added to neutralise a definitive opinion, for example neutralising "women must prove they are as good as men" (Table 5, 5a) into "women claim to prove they are as good as men" (Table 5, 5c). The baseline model was trained to do this as sentences that expressed opinions were not classed as neutral. The quantitative results corroborated this observation. The non-sexist classification results were moderately high - around 0.65 (refer Tables 3 and 4). This was because the neutralised sentences without the word "women" were often classified as not sexist. The neutralisation score (Table 4) reflects the actual results better as the neutralisations were of low quality, and the score for the baseline model was just 0.0149.
There were many examples with missing words (Table 5, rows 1c and 4c) and repeated words, which is reflected in a low fluency score of 0.317 (Table 4). The baseline had a high BLEU score of 66.6. However, we were unhappy with the quality of the neutralisations because of both the quantitative metrics (fluency, true hits, neutralisation score and classifier score - refer Tables 3 and 4) and the qualitative evaluation (a subset of this is shown in Table 5).

#### Fine-tuned model

The baseline model was fine-tuned using sexist examples after running it. When looking at the qualitative results of the fine-tuned model, we can observe that the model appeared to neutralise sentences well. Some examples are shown in Table 5, rows 1d, 3d and 4d. 
Particularly example 1d was interesting as the model was able to change the input sentence to express an equality. 
In addition, the model had an excellent ability to choose which references to gender needed to be neutralised. For instance, in Table 5, row 4d, "women" has been changed to "parents" to reflect that the bias in this sentence was towards female parents. This is in contrast to rows 1d and 3d. Instead of changing both "women" and "men" into a gender-neutral term and changing the sentence's meaning, the model has neutralised the sentence by equating women and men. The fine-tuned model is better than the baseline because it has a higher true hits score (0.105), embedding score (0.82), classifier score (0.72), fluency (0.697) and neutralisation score (0.473) (Tables 3 and 4). 

##### Augmented data model

Subsequently, the model was further fine-tuned using the augmented data. Analysing the results of the augmented data model showed that the additional fine-tuning did not improve the neutralisations. The qualitative results revealed repeated words and many gendered words that were changed. This meant that the sentence lost meaning or context. Some examples of this are listed in Table 5, rows 1e and 4e. As many gendered words were removed, the sentences were no longer classed as sexist. This resulted in the highest non-sexist classification across models, both by human evaluation with a score of 0.851 (Table 4) and using an SVM classifier, with a score of 0.93 (Table 3). However, this does not mean that the model was better, as reflected in reduced fluency (0.22, Table 4) and neutralisation score (0.0134, Table 4). This showed the lack of coherence in the predicted sentences. The sentence embedding was also the lowest across models (0.72, Table 3), showing that the predictions greatly differed from the golds. 
The fine-tuned model was seen to be the best model, as shown by the highest sentence embedding score (0.82, Table 3), fluency (0.697, Table 4), neutralisation score (0.473, Table 4) and true hits (0.105, Table 3). 

#### Evaluation metrics

When analysing the evaluation methods for this use case, we found that BLEU was not the best metric for evaluation as it heavily relies on the exact similarity to the gold sentence. The sentence embedding overcame this by understanding whether the predicted sentence is similar to the gold (even if it was not an exact match). This is illustrated when calculating the BLEU score and sentence embedding for examples 5b and 5d (Table 5). These sentences are similar in context, and the BLEU score obtained was 60 (this is a good score). However, we needed a metric to ensure that such sentences received the highest score possible. The sentence embedding score was 0.9, revealing that this metric can capture similar contexts even with multiple edits between the gold and predicted sentences. However, the sentence embedding is reliant on the gold sentence. 
The non-sexist classifier scores (Tables 3 and 4) overcome this as they only use the predicted sentences. We also found that human classification (Table 4) had similar results to the SVM classifier (Table 3).
Furthermore, the classification metrics are limited because predicted outputs will not be classified as sexist if they do not contain gendered words.  However, this may not be a correct neutralisation. An example of this is seen in Table 5, where row 1e is not sexist but loses the input sentence's context and meaning in 1a. Therefore, fluency and neutralisation score metrics (Table 4) were used to reflect the coherence and quality of the predicted neutralisation. To conclude, we believe that a combination of the mentioned metrics are needed to evaluate an automatic neutralisation model.  


