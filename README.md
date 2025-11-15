# Gene Editing Guide Language Model
CRISPR gene editing has revolutionized genetic engineering ever since its discovery in 2014. However, CRISPR's success is limited by the extensive screening needed to efficiently edit a target gene. CRISPR's enyme Cas9 is chaperoned by a guide RNA (gRNA) sequence complementary to its target DNA sequence. Not all gRNA sequences are created equal, as some are more efficient binders to their target. Additionally, off-target binding of the gRNA is detrimental as it causes non-specific activity of Cas9, which can cause cellular dysfunction or cancer in the worst cases. As a result, predicting the on- and off-target activity of a given gRNA sequence is important to CRISPR's application in biomedical research and therapeutic development.

Here, we used language models to predict CRISPR gRNA on- and off-target activity by the gRNA target sequence. We began by expanding upon a previously published paper detailing the GNL scorer model, using the same datasets to replicate their gradient-boosted regression tree (GBRT) and Bayesian ridge regression (BRR) models. We then sought to use advancements in language models to develop embeddings for the gRNA sequences that could be fed into feed-forward neural networks. We compared embeddings created from the generalized pretrained transformer BERT with the Word2Vec model from Gensim.

We began by training the Word2Vec model on the gRNA sequences and their on-target scores to obtain embeddings for each nucleotide: A, C, G, and T. The first 4 features of the 25 featured embeddings can be seen below:

| Nucleotide | Feature 1  | Feature 2  | Feature 3 | Feature 4 |
|---|--------------|---------------|--------------|--------------|
| A | -0.03263167  | 0.017983193   | -0.016548304 | 0.0032981443 |
| C | -0.002144909 | 0.0009457254  | 0.020413399  | 0.03603709   |
| G | 0.0022871399 | 0.029767632   | -0.003253131 | -0.010553655 |
| T | -0.013621463 | -0.0037856055 | 0.023074294  | -0.03008655  |

We then replaced the nucleotide sequence with the corresponding embedding for each nucleotide, turning the 20-mer gRNA sequence into an array of 500 floats. These were then passed into a feed forward neural network with the architecture below:

Input: 500x1
Layer 1: 128x1
Layer 2: 128x1
Layer 3: 128X1
Output: 1x1

We performed 10-fold cross-validation on the on-target data to evaluate these embeddings in the ffNN architecture and compared to previously published GBRT and BRR implementations.

![10-fold spearman](https://github.com/hayitsian/Gene-Editing-Guide-LM/blob/main/on-target%20sgrna%20spearman%20barchart.png)
