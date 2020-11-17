---
title: Naive Bayes classifier
weight: 10
draft: false
---

Naive Bayes is a supervised model usually used to classify documents into two or more categories. We train the classifier using class labels attached to documents, and predict the most likely class(es) of new unlabelled documents.


```r
require(quanteda)
require(quanteda.textmodels)
require(caret)
```

`data_corpus_moviereviews` from the **quanteda.textmodels** package contains 2000 movie reviews classified either as "positive" or "negative".


```r
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
##  cv003_12683.txt   313    555         2       neg cv003 12683
##  cv004_12641.txt   380    841         2       neg cv004 12641
```

The variable "Sentiment" indicates whether a movie review was classified as positive or negative. In this example we use 1500 reviews as the training set and build a Naive Bayes classifier based on this subset. In a second step, we predict the sentiment for the remaining reviews (our test set).

Since the first 1000 reviews are negative and the remaining reviews are classified as positive, we need to draw a random sample of the documents.


```r
# generate 1500 numbers without replacement
set.seed(300)
id_train <- sample(1:2000, 1500, replace = FALSE)
head(id_train, 10)
```

```
##  [1]  590  874 1602  985 1692  789  553 1980 1875 1705
```

```r
# create docvar with ID
corp_movies$id_numeric <- 1:ndoc(corp_movies)

# get training set
dfmat_training <- corpus_subset(corp_movies, id_numeric %in% id_train) %>%
    dfm(remove = stopwords("en"), stem = TRUE)

# get test set (documents not in id_train)
dfmat_test <- corpus_subset(corp_movies, !id_numeric %in% id_train) %>%
    dfm(remove = stopwords("en"), stem = TRUE)
```

Next we train the naive Bayes classifier using `textmodel_nb()`.


```r
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
##         happi   bastard     quick     movi    review      damn       y2k
## neg 0.0001875 3.749e-05 0.0004038 0.008032 0.0004932 0.0001471 1.154e-05
## pos 0.0002984 7.005e-05 0.0003736 0.006118 0.0004567 0.0001090 5.189e-06
##           bug       .       got      head     start     star      jami
## neg 0.0001557 0.06932 0.0005999 0.0005682 0.0008624 0.001272 5.192e-05
## pos 0.0001479 0.06543 0.0003892 0.0005008 0.0006720 0.001222 4.151e-05
##           lee     curti    anoth   baldwin   brother       (   william     time
## neg 0.0002827 3.749e-05 0.001226 1.125e-04 0.0004961 0.01218 0.0004355 0.002982
## pos 0.0002569 5.968e-05 0.001072 8.303e-05 0.0003918 0.01181 0.0004774 0.002953
##           )    stori    regard      crew   tugboat     come    across    desert
## neg 0.01242 0.002120 3.173e-05 0.0002625 5.768e-06 0.001803 0.0002192 6.634e-05
## pos 0.01184 0.002727 8.303e-05 0.0001894 2.595e-06 0.001702 0.0002128 9.600e-05
```

Naive Bayes can only take features into consideration that occur both in the training set and the test set, but we can make the features identical using `dfm_match()`


```r
dfmat_matched <- dfm_match(dfmat_test, features = featnames(dfmat_training))
```

Let's inspect how well the classification worked.


```r
actual_class <- dfmat_matched$sentiment
predicted_class <- predict(tmod_nb, newdata = dfmat_matched)
tab_class <- table(actual_class, predicted_class)
tab_class
```

```
##             predicted_class
## actual_class neg pos
##          neg 211  47
##          pos  36 206
```

From the cross-table we see that the number of false positives and false negatives is similar. The classifier made mistakes in both directions, but does not seem to over- or underestimate one class.

We can use the function `confusionMatrix()` from the **caret** package to assess the performance of the classification.


```r
confusionMatrix(tab_class, mode = "everything")
```

```
## Confusion Matrix and Statistics
## 
##             predicted_class
## actual_class neg pos
##          neg 211  47
##          pos  36 206
##                                           
##                Accuracy : 0.834           
##                  95% CI : (0.7984, 0.8656)
##     No Information Rate : 0.506           
##     P-Value [Acc > NIR] : <2e-16          
##                                           
##                   Kappa : 0.6681          
##                                           
##  Mcnemar's Test P-Value : 0.2724          
##                                           
##             Sensitivity : 0.8543          
##             Specificity : 0.8142          
##          Pos Pred Value : 0.8178          
##          Neg Pred Value : 0.8512          
##               Precision : 0.8178          
##                  Recall : 0.8543          
##                      F1 : 0.8356          
##              Prevalence : 0.4940          
##          Detection Rate : 0.4220          
##    Detection Prevalence : 0.5160          
##       Balanced Accuracy : 0.8342          
##                                           
##        'Positive' Class : neg             
## 
```

{{% notice note %}}
Precision, recall and the F1 score are frequently used to assess the classification performance. Precision is measured as `TP / (TP + FP)`, where `TP` are the number of true positives and  `FP`  the false positives. Recall divides the false positives by the sum of true positives and false negatives `TP / (TP + FN)`. Finally, the F1 score is a harmonic mean of precision and recall `2 * (Precision * Recall) / (Precision + Recall)`.
{{% /notice %}}

{{% notice ref %}}
- Jurafsky, Daniel, and James H. Martin. 2018. [_Speech and Language Processing. An Introduction to Natural Language Processing, Computational Linguistics, and Speech Recognition_](https://web.stanford.edu/~jurafsky/slp3/4.pdf). Draft of 3rd edition, September 23, 2018 (Chapter 4). 
{{% /notice%}}
