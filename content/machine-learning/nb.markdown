---
title: Naive Bayes classifier
weight: 10
draft: false
---

Naive Bayes is a supervised model usually used to classify documents into two or more categories. We train the classifier using class labels attached to documents, and predict the most likely class(es) of new unlabelled documents.


```r
require(quanteda)
require(quanteda.textmodels)
require(quanteda.corpora)
require(caret)
```

`data_corpus_movies` from the **quanteda.corpora** package contains 2000 movie reviews classified either as "positive" or "negative".


```r
corp_movies <- data_corpus_movies
summary(corp_movies, 5)
```

```
## Corpus consisting of 2000 documents, showing 5 documents:
## 
##             Text Types Tokens Sentences Sentiment   id1   id2
##  neg_cv000_29416   354    841         9       neg cv000 29416
##  neg_cv001_19502   156    278         1       neg cv001 19502
##  neg_cv002_17424   276    553         3       neg cv002 17424
##  neg_cv003_12683   313    555         2       neg cv003 12683
##  neg_cv004_12641   380    841         2       neg cv004 12641
##                                                                               _source
##  /Users/kbenoit/Dropbox/QUANTESS/corpora/movieReviews/smaller/all/neg_cv000_29416.txt
##  /Users/kbenoit/Dropbox/QUANTESS/corpora/movieReviews/smaller/all/neg_cv001_19502.txt
##  /Users/kbenoit/Dropbox/QUANTESS/corpora/movieReviews/smaller/all/neg_cv002_17424.txt
##  /Users/kbenoit/Dropbox/QUANTESS/corpora/movieReviews/smaller/all/neg_cv003_12683.txt
##  /Users/kbenoit/Dropbox/QUANTESS/corpora/movieReviews/smaller/all/neg_cv004_12641.txt
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
corp_movies$id_numeric <- 1:ndoc(corp_movies)

# get training set
dfmat_training <- corpus_subset(corp_movies, id_numeric %in% id_train) %>%
    dfm(remove = stopwords("english"), stem = TRUE)

# get test set (documents not in id_train)
dfmat_test <- corpus_subset(corp_movies, !id_numeric %in% id_train) %>%
    dfm(remove = stopwords("english"), stem = TRUE)
```

Next we train the naive Bayes classifier using `textmodel_nb()`.


```r
tmod_nb <- textmodel_nb(dfmat_training, dfmat_training$Sentiment)
summary(tmod_nb)
```

```
## 
## Call:
## textmodel_nb.dfm(x = dfmat_training, y = dfmat_training$Sentiment)
## 
## Class Priors:
## (showing first 2 elements)
## neg pos 
## 0.5 0.5 
## 
## Estimated Feature Scores:
##      happi bastard  quick   movi review   damn    y2k    bug      .    got
## neg 0.3859  0.3486 0.5211 0.5676 0.5192 0.5744 0.6898 0.5083 0.5144 0.6065
## pos 0.6141  0.6514 0.4789 0.4324 0.4808 0.4256 0.3102 0.4917 0.4856 0.3935
##       head start   star   jami    lee  curti  anoth baldwin brother      (
## neg 0.5315 0.563 0.5105 0.5557 0.5239 0.3859 0.5336  0.5753  0.5587 0.5077
## pos 0.4685 0.437 0.4895 0.4443 0.4761 0.6141 0.4664  0.4247  0.4413 0.4923
##     william   time      )  stori regard   crew tugboat   come across desert
## neg  0.4771 0.5025 0.5118 0.4373 0.2765 0.5808  0.6898 0.5147 0.5105 0.4086
## pos  0.5229 0.4975 0.4882 0.5627 0.7235 0.4192  0.3102 0.4853 0.4895 0.5914
```


Naive Bayes can only take features into consideration that occur both in the training set and the test set, but we can make the features identical by passing `training_dfm` to `dfm_match()` as a pattern.


```r
dfmat_matched <- dfm_match(dfmat_test, features = featnames(dfmat_training))
```

Let's inspect how well the classification worked.


```r
actual_class <- dfmat_matched$Sentiment
predicted_class <- predict(tmod_nb, newdata = dfmat_matched)
tab_class <- table(actual_class, predicted_class)
tab_class
```

```
##             predicted_class
## actual_class neg pos
##          neg 212  46
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
##          neg 212  46
##          pos  36 206
##                                           
##                Accuracy : 0.836           
##                  95% CI : (0.8006, 0.8674)
##     No Information Rate : 0.504           
##     P-Value [Acc > NIR] : <2e-16          
##                                           
##                   Kappa : 0.6721          
##                                           
##  Mcnemar's Test P-Value : 0.3203          
##                                           
##             Sensitivity : 0.8548          
##             Specificity : 0.8175          
##          Pos Pred Value : 0.8217          
##          Neg Pred Value : 0.8512          
##               Precision : 0.8217          
##                  Recall : 0.8548          
##                      F1 : 0.8379          
##              Prevalence : 0.4960          
##          Detection Rate : 0.4240          
##    Detection Prevalence : 0.5160          
##       Balanced Accuracy : 0.8361          
##                                           
##        'Positive' Class : neg             
## 
```

{{% notice note %}}
Precision, recall and the F1 score are frequently used to assess the classification performance. Precision is measured as `TP / (TP + FP)`, where `TP` are the number of true positives and  `FP`  the false positives. Recall divides the false positives by the sum of true positives and false negatives `TP / (TP + FN)`. Finally, the F1 score is a harmonic mean of precision and recall `2 * (Precision * Recall) / (Precision + Recall)`.
{{% /notice %}}

{{% notice info %}}
If you want to learn more about Naive Bayes classification, see:  
Jurafsky, Daniel, and James H. Martin. 2018. [_Speech and Language Processing. An Introduction to Natural Language Processing, Computational Linguistics, and Speech Recognition_](https://web.stanford.edu/~jurafsky/slp3/4.pdf). Draft of 3rd edition, September 23, 2018 (Chapter 4). 
{{% /notice%}}
