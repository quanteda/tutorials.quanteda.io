---
title: Regularized regression classifier
weight: 15
draft: false
---

{{% author %}}By Frederik Hjorth{{% /author %}} 


Regularized regression is a classification technique where the category of interest is regressed on text features using a penalized form of regression where parameter estimates are biased towards zero. Here we will be using a specific type of regularized regression, the Least Absolute Shrinkage and Selection Operator (or simply LASSO). However, the main alternative to LASSO, ridge regression, is conceptually very similar. 

In the LASSO estimator, the degree of penalization is determined by the regularization parameter `lambda`. We can use the cross-validation function available in the **glmnet** package to select the optimal value for `lambda`. 
We train the classifier using class labels attached to documents, and predict the most likely class(es) of new unlabelled documents. Although regularized regression is not part of the **quanteda.textmodels** package, the functions for regularized regression from the **glmnet** package can be easily worked into a quanteda workflow.


```r
require(quanteda)
require(quanteda.textmodels)
require(glmnet)
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

The variable "Sentiment" indicates whether a movie review was classified as positive or negative. In this example, we will use 1500 reviews as the training set and build a regularized regression classifier based on this subset. In the second step, we will predict the sentiment for the remaining reviews (our test set).

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

# tokenize texts
toks_movies <- tokens(corp_movies, remove_punct = TRUE, remove_number = TRUE) %>% 
               tokens_remove(pattern = stopwords("en")) %>% 
               tokens_wordstem()
dfmt_movie <- dfm(toks_movies)

# get training set
dfmat_training <- dfm_subset(dfmt_movie, id_numeric %in% id_train)

# get test set (documents not in id_train)
dfmat_test <- dfm_subset(dfmt_movie, !id_numeric %in% id_train)
```

Next we choose `lambda` using `cv.glmnet` from the **glmnet** package. `cv.glmnet` requires an input matrix `x` and a response vector `y`. For the input matrix, we will use the training set converted to a sparse matrix. For the response vector, we will use a dichotomous indicator of review sentiment in the training set with positive reviews coded as 1 and negative reviews as 0. 

We use `cv.glmnet()` to select the value of `lambda` that yields the smallest classification error. If you set `alpha = 1`, it selects the LASSO estimator. If you set `nfold = 5`, it partitions the data into five subsets.


```r
lasso <- cv.glmnet(x = dfmat_training,
                   y = as.integer(dfmat_training$sentiment == "pos"),
                   alpha = 1,
                   nfold = 5,
                   family = "binomial")
```

As an initial evaluation of the model, we can print the most predictive features. We begin by obtaining the best value of `lambda`:


```r
index_best <- which(lasso$lambda == lasso$lambda.min)
beta <- lasso$glmnet.fit$beta[, index_best]
```

We can now look at the most predictive features for the chosen `lambda`:


```r
head(sort(beta, decreasing = TRUE), 20)
```

```
##   standoff  chopsocki   flammabl     haywir     immers    refresh     finest 
##  1.4077996  1.1774772  0.9933831  0.9579847  0.8660077  0.8568582  0.8485373 
##  breathtak    ratchet    maniaci     darker      brisk  anti-soci   sullivan 
##  0.8161629  0.7507401  0.7482757  0.7455572  0.7424568  0.7315236  0.7243110 
##   gingrich cornerston neccessari      meryl   murtaugh      anger 
##  0.7138940  0.6877455  0.6801743  0.6066800  0.5970543  0.5950011
```

`predict.glmnet` can only take features into consideration that occur both in the training set and the test set, but we can make the features identical using `dfm_match()`.


```r
dfmat_matched <- dfm_match(dfmat_test, features = featnames(dfmat_training))
```

Next, we can obtain predicted probabilities for each review in the test set.


```r
pred <- predict(lasso, dfmat_matched, type = "response", s = lasso$lambda.min)
head(pred)
```

```
##                          s1
## cv000_29416.txt 0.419026170
## cv013_10494.txt 0.087619356
## cv032_23718.txt 0.485367184
## cv033_25680.txt 0.406659883
## cv036_18385.txt 0.226308927
## cv038_9781.txt  0.003946577
```

Let's inspect how well the classification worked.


```r
actual_class <- as.integer(dfmat_matched$sentiment == "pos")
predicted_class <- as.integer(predict(lasso, dfmat_matched, type = "class"))
tab_class <- table(actual_class, predicted_class)
tab_class
```

```
##             predicted_class
## actual_class   0   1
##            0 194  64
##            1  32 210
```

From the cross-table we can see that the model slightly under-predicts negative reviews, i.e. produces slightly more false negatives than false positives, but most reviews are correctly predicted. 

We can use the function `confusionMatrix()` from the **caret** package to quantify the performance of the classification.


```r
confusionMatrix(tab_class, mode = "everything")
```

```
## Confusion Matrix and Statistics
## 
##             predicted_class
## actual_class   0   1
##            0 194  64
##            1  32 210
##                                           
##                Accuracy : 0.808           
##                  95% CI : (0.7707, 0.8416)
##     No Information Rate : 0.548           
##     P-Value [Acc > NIR] : < 2.2e-16       
##                                           
##                   Kappa : 0.6172          
##                                           
##  Mcnemar's Test P-Value : 0.001557        
##                                           
##             Sensitivity : 0.8584          
##             Specificity : 0.7664          
##          Pos Pred Value : 0.7519          
##          Neg Pred Value : 0.8678          
##               Precision : 0.7519          
##                  Recall : 0.8584          
##                      F1 : 0.8017          
##              Prevalence : 0.4520          
##          Detection Rate : 0.3880          
##    Detection Prevalence : 0.5160          
##       Balanced Accuracy : 0.8124          
##                                           
##        'Positive' Class : 0               
## 
```

{{% notice note %}}
Precision, recall and the F1 score are frequently used to assess the classification performance. Precision is measured as `TP / (TP + FP)`, where `TP` are the number of true positives and  `FP`  the false positives. Recall divides the false positives by the sum of true positives and false negatives `TP / (TP + FN)`. Finally, the F1 score is a harmonic mean of precision and recall `2 * (Precision * Recall) / (Precision + Recall)`.
{{% /notice %}}

{{% notice ref %}}
- Jurafsky, Daniel, and James H. Martin. 2018. [_Speech and Language Processing. An Introduction to Natural Language Processing, Computational Linguistics, and Speech Recognition_](https://web.stanford.edu/~jurafsky/slp3/4.pdf). Draft of 3rd edition, September 23, 2018 (Chapter 4). 
{{% /notice%}}
