---
title: Naive Bayes classifier
weight: 10
draft: false
---

Naive Bayes is a supervised model usually used to classify documents into two or more categories. We train the classifier using class labels attached to documents, and predict the most likely class(es) of new unlabeled documents.


``` r
library(quanteda)
library(quanteda.textmodels)
library(caret)
```

`data_corpus_moviereviews` from the **quanteda.textmodels** package contains 2000 movie reviews classified either as "positive" or "negative".


``` r
corp_movies <- data_corpus_moviereviews
summary(corp_movies, 5)
```

```
## Corpus consisting of 2000 documents, showing 5 documents:
## 
##             Text Types Tokens Sentences sentiment   id1   id2
##  cv000_29416.txt   354    841         9       neg cv000 29416
##  cv001_19502.txt   156    278         1       neg cv001 19502
##  cv002_17424.txt   276    553         3       neg cv002 17424
##  cv003_12683.txt   314    558         2       neg cv003 12683
##  cv004_12641.txt   380    841         2       neg cv004 12641
```

The variable "Sentiment" indicates whether a movie review was classified as positive or negative. In this example, we will use 1500 reviews as the training set and build a Naive Bayes classifier based on this subset. In the second step, we will predict the sentiment for the remaining reviews (our test set).

Since the first 1000 reviews are negative and the remaining reviews are classified as positive, we need to draw a random sample of the documents.


``` r
# generate 1500 numbers without replacement
set.seed(300)
id_train <- sample(1:2000, 1500, replace = FALSE)
head(id_train, 10)
```

```
##  [1]  590  874 1602  985 1692  789  553 1980 1875 1705
```

``` r
# create docvar with ID
corp_movies$id_numeric <- 1:ndoc(corp_movies)

# tokenize texts
toks_movies <- tokens(corp_movies, remove_punct = TRUE, remove_number = TRUE) |> 
               tokens_remove(pattern = stopwords("en")) |> 
               tokens_wordstem()
dfmt_movie <- dfm(toks_movies)

# get training set
dfmat_training <- dfm_subset(dfmt_movie, id_numeric %in% id_train)

# get test set (documents not in id_train)
dfmat_test <- dfm_subset(dfmt_movie, !id_numeric %in% id_train)
```

Next, we will train the naive Bayes classifier using `textmodel_nb()`.


``` r
tmod_nb <- textmodel_nb(dfmat_training, dfmat_training$sentiment)
summary(tmod_nb)
```

```
## 
## Call:
## textmodel_nb.dfm(x = dfmat_training, y = dfmat_training$sentiment)
## 
## Class Priors:
## (showing first 2 elements)
## neg pos 
## 0.5 0.5 
## 
## Estimated Feature Scores:
##         plot     two      teen     coupl       go    church     parti     drink
## neg 0.002582 0.00232 0.0002873 0.0007163 0.002665 9.090e-05 0.0002654 1.200e-04
## pos 0.001508 0.00234 0.0001658 0.0005460 0.002350 8.775e-05 0.0002730 9.425e-05
##         drive      get     accid      one       guy       die girlfriend
## neg 0.0003054 0.004491 9.454e-05 0.007403 0.0014472 0.0005491  0.0003127
## pos 0.0002633 0.003786 1.853e-04 0.007365 0.0009945 0.0005493  0.0002340
##       continu      see     life  nightmar      deal    watch     movi     sorta
## neg 0.0003163 0.002560 0.001436 0.0001200 0.0004327 0.001644 0.010127 1.091e-05
## pos 0.0003218 0.003023 0.002499 0.0001203 0.0005200 0.001541 0.007667 1.625e-05
##         find   critiqu mind-fuck   generat     touch      cool      idea
## neg 0.001454 9.454e-05 3.636e-06 0.0002654 0.0002291 0.0003054 0.0008218
## pos 0.001632 8.450e-05 3.250e-06 0.0002925 0.0004453 0.0002275 0.0005850
```

Naive Bayes can only take features into consideration that occur both in the training set and the test set, but we can make the features identical using `dfm_match()`


``` r
dfmat_matched <- dfm_match(dfmat_test, features = featnames(dfmat_training))
```

Let's inspect how well the classification worked.


``` r
actual_class <- dfmat_matched$sentiment
predicted_class <- predict(tmod_nb, newdata = dfmat_matched)
tab_class <- table(actual_class, predicted_class)
tab_class
```

```
##             predicted_class
## actual_class neg pos
##          neg 213  45
##          pos  37 205
```

From the cross-table we can see that the number of false positives and false negatives is similar. The classifier made mistakes in both directions, but does not seem to over- or under-estimate one class.

We can use the function `confusionMatrix()` from the **caret** package to assess the performance of the classification.


``` r
confusionMatrix(tab_class, mode = "everything", positive = "pos")
```

```
## Confusion Matrix and Statistics
## 
##             predicted_class
## actual_class neg pos
##          neg 213  45
##          pos  37 205
##                                           
##                Accuracy : 0.836           
##                  95% CI : (0.8006, 0.8674)
##     No Information Rate : 0.5             
##     P-Value [Acc > NIR] : <2e-16          
##                                           
##                   Kappa : 0.672           
##                                           
##  Mcnemar's Test P-Value : 0.4395          
##                                           
##             Sensitivity : 0.8200          
##             Specificity : 0.8520          
##          Pos Pred Value : 0.8471          
##          Neg Pred Value : 0.8256          
##               Precision : 0.8471          
##                  Recall : 0.8200          
##                      F1 : 0.8333          
##              Prevalence : 0.5000          
##          Detection Rate : 0.4100          
##    Detection Prevalence : 0.4840          
##       Balanced Accuracy : 0.8360          
##                                           
##        'Positive' Class : pos             
## 
```

{{% notice note %}}
Precision, recall and the F1 score are frequently used to assess the classification performance. Precision is measured as `TP / (TP + FP)`, where `TP` are the number of true positives and  `FP` are the false positives. Recall divides the true positives by the sum of true positives and false negatives `TP / (TP + FN)`. Finally, the F1 score is a harmonic mean of precision and recall `2 * (Precision * Recall) / (Precision + Recall)`.
{{% /notice %}}

{{% notice ref %}}
- Jurafsky, Daniel, and James H. Martin. 2021 [_Speech and Language Processing. An Introduction to Natural Language Processing, Computational Linguistics, and Speech Recognition_](https://web.stanford.edu/~jurafsky/slp3/4.pdf). Draft of 3rd edition, December 29, 2021 (Chapter 4). 
{{% /notice%}}
