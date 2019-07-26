---
title: Naive Bayes classifier
weight: 10
draft: false
---

Naive Bayes is a supervised model usually used to classify documents into two or more categories. We train the classifier using class labels attached to documents, and predict the most likely class(es) of new unlabelled documents.


```r
require(quanteda)
require(quanteda.corpora)
require(caret)
```

`data_corpus_movies` from the **quanteda.corpora** package contains 2000 movie reviews classified either as "positive" or "negative".


```r
corp_movies <- data_corpus_movies
summary(corp_movies, 5)
```

```
## Warning in nsentence.character(object, ...): nsentence() does not correctly
## count sentences in all lower-cased text
```

```
## Corpus consisting of 2000 documents, showing 5 documents:
## 
##             Text Types Tokens Sentences Sentiment   id1   id2
##  neg_cv000_29416   354    841         9       neg cv000 29416
##  neg_cv001_19502   156    278         1       neg cv001 19502
##  neg_cv002_17424   276    553         3       neg cv002 17424
##  neg_cv003_12683   314    564         2       neg cv003 12683
##  neg_cv004_12641   380    842         2       neg cv004 12641
## 
## Source: http://www.cs.cornell.edu/people/pabo/movie-review-data/
## Created: Sat Nov 15 18:43:25 2014
## Notes:
```

"Sentiment" indicates whether a movie review was classified as positive or negative. In this example we use 1500 reviews as the training set and build a Naive Bayes classifier based on this subset. In a second step, we predict the sentiment for the remaining reviews (our test set).

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
docvars(corp_movies, "id_numeric") <- 1:ndoc(corp_movies)

# get training set
dfmat_training <- corpus_subset(corp_movies, id_numeric %in% id_train) %>%
    dfm(stem = TRUE)

# get test set (documents not in id_train)
dfmat_test <- corpus_subset(corp_movies, !id_numeric %in% id_train) %>%
    dfm(stem = TRUE)
```

Next we train the naive Bayes classifier using `textmodel_nb()`.


```r
tmod_nb <- textmodel_nb(dfmat_training, docvars(dfmat_training, "Sentiment"))
summary(tmod_nb)
```

```
## 
## Call:
## textmodel_nb.dfm(x = dfmat_training, y = docvars(dfmat_training, 
##     "Sentiment"))
## 
## Class Priors:
## (showing first 2 elements)
## neg pos 
## 0.5 0.5 
## 
## Estimated Feature Scores:
##       the  happi bastard  quick   movi review   damn   that    y2k    bug
## neg 0.482 0.3865  0.3492 0.5201 0.5682 0.5185 0.5751 0.5199 0.6903 0.5136
## pos 0.518 0.6135  0.6508 0.4799 0.4318 0.4815 0.4249 0.4801 0.3097 0.4864
##          .     it    got     a   head  start     in   this   star   jami
## neg 0.5151 0.5028 0.6071 0.495 0.5322 0.5627 0.4878 0.5354 0.5107 0.5563
## pos 0.4849 0.4972 0.3929 0.505 0.4678 0.4373 0.5122 0.4646 0.4893 0.4437
##        lee  curti    and  anoth baldwin brother      ( william   time
## neg 0.5246 0.3865 0.4658 0.5342   0.576  0.5594 0.5084  0.4777 0.5034
## pos 0.4754 0.6135 0.5342 0.4658   0.424  0.4406 0.4916  0.5223 0.4966
##          )
## neg 0.5125
## pos 0.4875
```


Naive Bayes can only take features into consideration that occur both in the training set and the test set, but we can make the features identical by passing `training_dfm` to `dfm_match()` as a pattern.


```r
dfmat_matched <- dfm_match(dfmat_test, features = featnames(dfmat_training))
```

Let's inspect how well the classification worked.


```r
actual_class <- docvars(dfmat_matched, "Sentiment")
predicted_class <- predict(tmod_nb, newdata = dfmat_matched)
tab_class <- table(actual_class, predicted_class)
tab_class
```

```
##             predicted_class
## actual_class neg pos
##          neg 208  50
##          pos  38 204
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
##          neg 208  50
##          pos  38 204
##                                           
##                Accuracy : 0.824           
##                  95% CI : (0.7877, 0.8564)
##     No Information Rate : 0.508           
##     P-Value [Acc > NIR] : <2e-16          
##                                           
##                   Kappa : 0.6482          
##                                           
##  Mcnemar's Test P-Value : 0.241           
##                                           
##             Sensitivity : 0.8455          
##             Specificity : 0.8031          
##          Pos Pred Value : 0.8062          
##          Neg Pred Value : 0.8430          
##               Precision : 0.8062          
##                  Recall : 0.8455          
##                      F1 : 0.8254          
##              Prevalence : 0.4920          
##          Detection Rate : 0.4160          
##    Detection Prevalence : 0.5160          
##       Balanced Accuracy : 0.8243          
##                                           
##        'Positive' Class : neg             
## 
```

{{% notice note %}}
Precision, recall and the F1 score are frequently used to assess the classification performance. Precision is measured as `TP / (TP + FP)`, where `TP` are the number of true positives and  `FP`  the false positives. Recall divides the false positives by the sum of true positives and false negatives `TP / (TP + FN)`. Finally, the F1 score is a harmonic mean of precision and recall `2 * (Precision * Recall) / (Precision + Recall)`.
{{% /notice %}}

{{% notice info %}}
If you want to learn more about Naive Bayes classification, see [Chapter 4](https://web.stanford.edu/~jurafsky/slp3/4.pdf) of Jurafsky and Martin (2018).
{{% /notice%}}
