# hmm-viterbi-tagger
HW3 Report

1 Introduction

This project implement and evaluate a Part-of- Speech (POS) tagging system based on a Hidden Markov Model (HMM). The system predicts the most likely sequence of POS tags for a given sen- tence using probabilistic modeling. Additionally, a baseline tagger is implemented for comparison, which assigns the most frequent tag to each word observed in the training data.

2 Methodology

1. Dataset Preparation

The dataset used is the Penn Treebank WSJ corpus, divided into three splits:

- Training Set: Used to compute transition and emission probabilities.
- Development Set: Used to tune the smooth- ing hyperparameter (α).
- Test Set: Used to evaluate the final perfor- mance of the models.

Each sentence is padded with <START>and <STOP> tokens to indicate the boundaries of the sentence.

2. Hidden Markov Model

The HMM POS tagger models the tagging process as a sequence prediction problem, using:

- Transition Probabilities (P (tk|tk−1)): The likelihood of transitioning from one tag to another.
- Emission Probabilities (P (w|t)): The likeli- hoodofawordbeinggeneratedbyaparticular tag.

Both probabilities are estimated using Maximum Likelihood Estimation (MLE) with add-α smooth- ing:

P (t |t ) = Count(tk−1 → tk) + α

k k−1 Count(tk−1) + α · |T|

Count(t → w) + α P (w|t) =

Count(t) + α · |V|

where α is the smoothing parameter, |T| is the number of tags, and |V| is the vocabulary size.

3. Baseline Tagger

The baseline tagger assigns the most frequent tag to each word based on the training data. For unknown words, it assigns the default tag (NN).

4. Decoding with the Viterbi Algorithm

The Viterbi algorithm is used to decode the most probable sequence of tags for a given sentence:

- Initialization: V[0][t] = log(P (<START>→ t))+log(P (w0|t))
- Recursion:

  V[t][k] = max (V[t − 1][tk−1] + log(P (tk−1 → tk)) + log(P

tk−1

- Termination:

  max (V[n − 1][tk] + log(P (tk → <STOP>)))

tk

- Backtracking: Extract the best sequence of tags from the computed probabilities.
5. Evaluation Metrics

The models are evaluated using the following met- rics:

- Accuracy: Proportion of correctly predicted tags.
- Precision: True Positives

True Positives+False Positives

- Recall: True PositiTruevesPositi+FalsevesNegatives
- F1 Score: Harmonic mean of precision and recall.
- Confusion Matrix: A table showing actual vs. predicted tags to analyze common errors.

3 Experiments 4.3 Improvements

1. Hyperparameter Tuning

The smoothing parameter α is fine-tuned on the development set. Values tested include {0.1,0.5,1,2,5}. The best α is selected based on the highest F1 score on the development set.

2. Results

The best α measure turns out to be 0.1.



|Model|Accuracy (%)|Macro F1 (%)|Precision (%|
| - | - | - | - |
|Baseline Tagger HMM Tagger|84\.7 91.2|78\.5 88.1|81\.2 89.4|

Table 1: Comparison of HMM and Baseline Tagger on the Test Set.

![](Aspose.Words.4b77fb09-c466-49f3-92ea-b5cc50b1bcdb.001.png)

Figure 1: Confusion Matrix for the HMM Tagger on the Test Set.

3. Observations
- The HMM tagger significantly outperforms the baseline tagger in terms of accuracy and F1 score.
- Common errors include confusion between NN (noun) and VB(verb), indicating ambiguity in the dataset.

Using trigrams instead of bigrams for transition probabilities. Incorporating word embeddings or other contextual features. Using optimization tech- niques for faster Viterbi decoding.

5 Conclusion

The HMM POS tagger achieves high accuracy and F1 scores compared to the baseline tagger. How- ever, errors due to ambiguity remain a challenge. Future work could explore more complex models like CRFs or neural network-based taggers to ad- dress these limitations.

4 Discussion

1. HMM vs. Baseline

The HMM tagger performs better than the baseline because it captures context through transition prob- abilities, while the baseline model relies solely on word frequency.

2. Error Analysis

The most frequent misclassifications involve am- biguous tags such as NNvs. VB. For example, the word “run” can be a noun or a verb, depending on context.
