---
title: Naive Bayes classification
weight: 70
chapter: false
draft: true
---


```r
require(quanteda)
require(quanteda.corpora)
require(caret)
```

Naive Bayes can be used to classify documents into two or more categories given a `dfm` and some training labels. 

{{% notice note %}}
Naive Bayes is a probabilistic classifier. This means that for a document, out of all classes the classifier returns the class which has the maximum posterior probability given the document.

For a comprehensible introduction to Naive Bayes and the assumptions underlying the method see [Chapter 6](https://web.stanford.edu/~jurafsky/slp3/6.pdf) of Jurafsky and Martin (2017).

Jurafsky, Daniel and James H. Martin. (2017) Speech and Language Processing. Draft of August 7, 2017. https://web.stanford.edu/~jurafsky/slp3/6.pdf
{{% /notice %}}

We use corpus object containing 2000 movie reviews classified by positive or negative sentiment from the **quanteda.corpora** package. 



```r
summary(data_corpus_movies, 5)
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
## Source:  /Users/kbenoit/Dropbox/QUANTESS/quantedaData_kenlocal_gh/* on x86_64 by kbenoit
## Created: Sat Nov 15 18:43:25 2014
## Notes:
```

The `docvar` Sentiment shows whether a movie review was classified as positive or negative. In this example we use the fist 1500 revivews as the training set and build a Naive Bayes classifier based on this subset. In a second step, we predict the sentiment for the test set (the remaining reviews).

Because the first 1000 reviews are negative and the remaining reviews are classified as positive, we need to draw a random sample of the documents (instead of choosing just by position).


```r
# generate 1500 numbers without replacement
set.seed(300)
sample_use <- sample(1:2000, 1500, replace = FALSE)

head(sample_use, 10)
```

```
##  [1] 1831 1526 1610 1466 1362   25 1513  995  929 1794
```

```r
# create docvar with ID
docvars(data_corpus_movies, "id_numeric") <- 1:ndoc(data_corpus_movies)


# get training set
dfm_nb_training <- corpus_subset(data_corpus_movies, id_numeric %in% sample_use) %>% 
  dfm(stem = TRUE)

# get test set (documents not in sample_use)
dfm_nb_test <- corpus_subset(data_corpus_movies, !id_numeric %in% sample_use) %>% 
  dfm(stem = TRUE)
```

Next we train the Naive Bayes classifier using `textmodel_nb()`.


```r
nb_training <- textmodel_nb(dfm_nb_training, docvars(corpus_subset(data_corpus_movies, id_numeric %in% sample_use), "Sentiment"))

summary(nb_training)
```

```
## 
## Call:
## textmodel_nb.dfm(x = dfm_nb_training, y = docvars(corpus_subset(data_corpus_movies, 
##     id_numeric %in% sample_use), "Sentiment"))
## 
## Class Priors:
## (showing first 2 elements)
## neg pos 
## 0.5 0.5 
## 
## Estimated Feature Scores:
##       plot      :    two   teen  coupl     go     to      a church  parti
## neg 0.6181 0.5255 0.5001 0.6236 0.5221 0.5315 0.5093 0.4945 0.4771 0.5374
## pos 0.3819 0.4745 0.4999 0.3764 0.4779 0.4685 0.4907 0.5055 0.5229 0.4626
##          ,  drink    and   then  drive     .   they    get   into     an
## neg 0.4777 0.5887 0.4627 0.5786 0.5124 0.516 0.5214 0.5471 0.5066 0.4924
## pos 0.5223 0.4113 0.5373 0.4214 0.4876 0.484 0.4786 0.4529 0.4934 0.5076
##      accid    one     of    the    guy    die   but    his girlfriend
## neg 0.3694 0.4921 0.4785 0.4815 0.6077 0.4975 0.507 0.4432     0.4945
## pos 0.6306 0.5079 0.5215 0.5185 0.3923 0.5025 0.493 0.5568     0.5055
##     continu
## neg  0.4968
## pos  0.5032
```


Naive Bayes can only take features into consideration that occur both in the training set and the test set. Thus, we first need to use `dfm_select()` to ensure that the features in the test and training set are identical. 


```r
data_nb_select <- dfm_select(dfm_nb_test, dfm_nb_training)

data_test_predicted <- predict(nb_training, data_nb_select)
```

Let's inspect how well the classification worked.


```r
actual_class <- data_corpus_movies %>% 
  corpus_subset(!id_numeric %in% sample_use) %>% 
  docvars("Sentiment")

predicted_class <- data_test_predicted$nb.predicted 

(table_classification <- table(actual_class, predicted_class))
```

```
##             predicted_class
## actual_class neg pos
##          neg 202  47
##          pos  49 202
```

From the table we see that the number of false positives and false negatives is similar. The classifier made mistakes in both directions, but does not seem to over- or underestimate one class.

We can use the function `confusionMatrix` from the **caret** package to assess the performance of the classification.

{{% notice note %}}
Precision, recall and the F1 score are frequently used to assess the classification performance. Precision is measured as `\(\frac{TP}{TP+FP}\)`, where _TP_ are the number of true positives and  _FP_  the false positives. Recall divides the false positives by the sum of true positives and false negatives ($\frac{TP}{TP+FN}$). Finally, the F1 score is a harmonic mean of precision and recall ($\frac{2\times TP}{2\times TP + FP + FN}).$
{{% /notice %}}



```r
caret::confusionMatrix(table_classification, mode = "everything")
```

```
## Confusion Matrix and Statistics
## 
##             predicted_class
## actual_class neg pos
##          neg 202  47
##          pos  49 202
##                                           
##                Accuracy : 0.808           
##                  95% CI : (0.7707, 0.8416)
##     No Information Rate : 0.502           
##     P-Value [Acc > NIR] : <2e-16          
##                                           
##                   Kappa : 0.616           
##  Mcnemar's Test P-Value : 0.9187          
##                                           
##             Sensitivity : 0.8048          
##             Specificity : 0.8112          
##          Pos Pred Value : 0.8112          
##          Neg Pred Value : 0.8048          
##               Precision : 0.8112          
##                  Recall : 0.8048          
##                      F1 : 0.8080          
##              Prevalence : 0.5020          
##          Detection Rate : 0.4040          
##    Detection Prevalence : 0.4980          
##       Balanced Accuracy : 0.8080          
##                                           
##        'Positive' Class : neg             
## 
```

