---
title: Regularized regression classifier
weight: 15
draft: false
bibliography: ../references.bib
---

{{% author %}}By Frederik Hjorth{{% /author %}}

The [previous chapter](/machine-learning/nb) used Naive Bayes to classify text; this chapter tackles the same classification task with a different technique, so that you can compare the two. Regularised regression is a classification technique where the category of interest is regressed on text features, using a penalised form of regression in which parameter estimates are biased towards zero. Biasing coefficients towards zero might sound counterproductive, but with thousands of word features and comparatively few documents, an ordinary regression would badly overfit the training data; penalisation keeps the model simpler and more likely to generalise to new text. Here we will use a specific type of regularised regression, the Least Absolute Shrinkage and Selection Operator (or simply LASSO). The main alternative to LASSO, ridge regression, is conceptually similar, but LASSO has the added property of shrinking some coefficients all the way to zero. In effect, this selects a subset of the most useful features.

In the LASSO estimator, the degree of penalisation is controlled by a regularisation parameter called `lambda`: larger values penalise more heavily and produce a simpler model, while smaller values stay closer to an ordinary, unpenalised regression. We can use a cross-validation function from the **glmnet** package to select a good value for `lambda` automatically, rather than guessing it ourselves. As in the [previous chapter](/machine-learning/nb), we train the classifier using class labels attached to documents, then predict the most likely class of new, unlabelled documents. Although regularised regression is not part of the **quanteda.textmodels** package, its functions from **glmnet** can be worked into a quanteda workflow, since a DFM behaves like an ordinary sparse matrix that **glmnet** already knows how to use.

``` r
library(quanteda)
library(quanteda.textmodels)
library(glmnet)
library(mltest)
```

As in the [previous chapter](/machine-learning/nb), we use `data_corpus_moviereviews` from the **quanteda.textmodels** package, which contains 2,000 movie reviews, each already labelled as “positive” or “negative”.

``` r
corp_movies <- data_corpus_moviereviews
summary(corp_movies, 5)
```

    ## Corpus consisting of 2000 documents, showing 5 documents:
    ## 
    ##             Text Types Tokens Sentences sentiment   id1   id2
    ##  cv000_29416.txt   354    841         9       neg cv000 29416
    ##  cv001_19502.txt   156    278         1       neg cv001 19502
    ##  cv002_17424.txt   276    553         3       neg cv002 17424
    ##  cv003_12683.txt   314    558         2       neg cv003 12683
    ##  cv004_12641.txt   380    841         2       neg cv004 12641

The `sentiment` variable indicates whether a movie review was labelled positive or negative. In this example, we will again use 1,500 reviews as the training set and build a regularised regression classifier based on this subset, then predict the sentiment for the remaining 500 reviews, our test set, so that we can compare predictions against known, true labels.

Since the reviews are ordered with all negative reviews first and all positive reviews second, we need to draw a random sample of the documents to avoid ending up with a training set that is all one class.

``` r
# generate 1500 numbers without replacement
set.seed(300)
id_train <- sample(1:2000, 1500, replace = FALSE)
head(id_train, 10)
```

    ##  [1]  590  874 1602  985 1692  789  553 1980 1875 1705

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

Next we choose `lambda` using `cv.glmnet()` from the **glmnet** package, rather than picking a value by trial and error. `cv.glmnet()` requires an input matrix `x` and a response vector `y`. For the input matrix, we use the training DFM directly, since it already behaves as a sparse matrix. For the response vector, we convert review sentiment in the training set into a 0/1 indicator, with positive reviews coded as 1 and negative reviews as 0, since `glmnet` expects a numeric outcome rather than a text label.

`cv.glmnet()` repeatedly refits the model on different subsets of the training data, held out in turn, to see which value of `lambda` generalises best, and then selects the value of `lambda` that yields the smallest classification error. Setting `alpha = 1` selects the LASSO estimator specifically, rather than ridge regression or a mixture of the two. Setting `nfolds = 5` partitions the data into five subsets for this cross-validation process, a standard choice that balances accuracy against computation time.

{{% notice tip %}}
This cross-validation serves one specific purpose: `cv.glmnet()` here tunes a hyperparameter, `lambda`, using only the training set, and is not an estimate of how the final model will perform on new documents. Further down this page, after fitting the model, we return to cross-validation for a second purpose — estimating performance — which needs its own procedure.
{{% /notice %}}

``` r
lasso <- cv.glmnet(x = dfmat_training,
                   y = as.integer(dfmat_training$sentiment == "pos"),
                   alpha = 1,
                   nfolds = 5,
                   family = "binomial")
```

As an initial evaluation of the model, we can inspect which features it found most predictive, in a similar spirit to reading off a Naive Bayes model’s most informative words. We begin by obtaining the coefficients associated with the best value of `lambda` found by cross-validation:

``` r
index_best <- which(lasso$lambda == lasso$lambda.min)
beta <- lasso$glmnet.fit$beta[, index_best]
```

Each value in `beta` is the estimated effect of one word feature on the log-odds of a review being positive; features that LASSO judged uninformative will have been shrunk to exactly zero. We can now look at the most positive, and therefore most predictive, features for the chosen `lambda`:

``` r
head(sort(beta, decreasing = TRUE), 20)
```

    ##   standoff  chopsocki   flammabl     haywir     immers    refresh     finest 
    ##  1.4000036  1.1670750  0.9891503  0.9459774  0.8692535  0.8596709  0.8506897 
    ##  breathtak    maniaci     darker    ratchet      brisk   sullivan  anti-soci 
    ##  0.8129285  0.7657701  0.7499248  0.7482090  0.7337313  0.7225341  0.7221267 
    ##   gingrich neccessari cornerston      meryl   murtaugh      anger 
    ##  0.7181020  0.6931785  0.6848684  0.6048239  0.6039334  0.5967162

As with the Naive Bayes classifier in the [previous chapter](/machine-learning/nb), the model was trained on the vocabulary of the training set only, so `predict.glmnet` can only use features that occur in both the training set and the test set. We again make the features identical using `dfm_match()`.

``` r
dfmat_matched <- dfm_match(dfmat_test, features = featnames(dfmat_training))
```

Like Naive Bayes, a regularised regression model can return either a class label or the underlying probability behind it; here we ask for the probability directly. Next, we obtain the predicted probability that each review in the test set is positive.

``` r
pred <- predict(lasso, dfmat_matched, type = "response", s = lasso$lambda.min)
head(pred)
```

    ##                 s=0.01126341
    ## cv000_29416.txt  0.416067462
    ## cv013_10494.txt  0.085527701
    ## cv032_23718.txt  0.491436400
    ## cv033_25680.txt  0.410002751
    ## cv036_18385.txt  0.224686450
    ## cv038_9781.txt   0.003931455

Convert these probabilities into class predictions and compare them against the reviews’ true sentiment, as with Naive Bayes.

``` r
actual_class <- as.integer(dfmat_matched$sentiment == "pos")
predicted_class <- as.integer(predict(lasso, dfmat_matched, type = "class"))
tab_class <- table(actual_class, predicted_class)
tab_class
```

    ##             predicted_class
    ## actual_class   0   1
    ##            0 194  64
    ##            1  32 210

From the cross-table we can see that the model produces about twice as many false positives as false negatives: it is more likely to call a negative review positive than the reverse, but most reviews are correctly predicted either way. As with Naive Bayes, we use the **mltest** package’s `ml_test()` function to quantify the performance of the classification with standard metrics, computed directly from the predicted and actual labels.

``` r
tstat_lasso <- ml_test(predicted_class, actual_class)
tstat_lasso$accuracy
```

    ## [1] 0.808

``` r
data.frame(precision = tstat_lasso$precision, recall = tstat_lasso$recall, F1 = tstat_lasso$F1)
```

    ##   precision    recall        F1
    ## 0 0.8584071 0.7519380 0.8016529
    ## 1 0.7664234 0.8677686 0.8139535

{{% notice note %}}
Precision, recall and the F1 score are frequently used to assess the classification performance. Precision is measured as `TP / (TP + FP)`, where `TP` are the number of true positives and `FP` the false positives. Recall divides the true positives by the sum of true positives and false negatives `TP / (TP + FN)`. Finally, the F1 score is a harmonic mean of precision and recall `2 * (Precision * Recall) / (Precision + Recall)`.
{{% /notice %}}

## How much would a different split change this?

As in the Naive Bayes chapter, everything reported above comes from one particular training/test split. All quantitative models of language are wrong, and Grimmer and Stewart (2013) argue that what matters is not chasing an illusory “correct” model but validating that a model’s output is stable and useful for the task at hand; checking sensitivity to the train/test split is one of the most basic forms that validation can take. We reuse the `lambda` already selected above, and fit the LASSO across five folds of the full corpus to see how much performance moves around from one split to another.

The code below uses a `for` loop to repeat the fit-and-evaluate process for each of the five folds: fold `i` is held out as the test set, the model is trained on the other four folds, and the result is stored in position `i` of the output.

``` r
set.seed(300)
folds <- sample(rep(1:5, length.out = ndoc(dfmt_movie)))
accuracy_by_fold <- numeric(5)
metrics_by_fold <- data.frame()

for (i in 1:5) {
    # hold out fold i as the test set; train on the remaining four folds
    dfmat_train_i <- dfm_subset(dfmt_movie, folds != i)
    dfmat_test_i <- dfm_subset(dfmt_movie, folds == i) |>
        dfm_match(features = featnames(dfmat_train_i))

    # fit LASSO on the training folds, reusing the lambda selected earlier
    lasso_i <- glmnet(x = dfmat_train_i,
                      y = as.integer(dfmat_train_i$sentiment == "pos"),
                      alpha = 1, lambda = lasso$lambda.min, family = "binomial")

    # predict on the held-out fold and compare against its true labels
    pred_i <- as.integer(predict(lasso_i, dfmat_test_i, type = "class"))
    actual_i <- as.integer(dfmat_test_i$sentiment == "pos")
    accuracy_by_fold[i] <- mean(pred_i == actual_i)

    # compute precision, recall and F1 for this fold and append them to the running table
    tstat_i <- ml_test(pred_i, actual_i)
    metrics_by_fold <- rbind(metrics_by_fold,
                             data.frame(fold = i,
                                        class = names(tstat_i$precision),
                                        precision = tstat_i$precision,
                                        recall = tstat_i$recall,
                                        F1 = tstat_i$F1))
}

accuracy_by_fold
```

    ## [1] 0.8400 0.8300 0.8225 0.8350 0.8325

``` r
mean(accuracy_by_fold)
```

    ## [1] 0.832

We keep `lambda` fixed at the value chosen earlier rather than re-running `cv.glmnet()` inside every fold; refitting the penalty within each outer fold would be more rigorous but considerably slower, and is not necessary to make the basic point here, that fold-level performance varies. As with Naive Bayes, a spread across folds, rather than a single number, is the honest way to report how well this model can be expected to generalise.

Accuracy alone can hide uneven performance across classes, so `metrics_by_fold` collects precision, recall and F1 for both classes on every fold, ten rows in total (five folds times two classes: `0` for negative and `1` for positive). Averaging each metric within class shows whether the model is consistently better at recognising one class of review than the other, rather than just how often it is right overall.

``` r
metrics_by_fold
```

    ##    fold class precision    recall        F1
    ## 0     1     0 0.8647059 0.7819149 0.8212291
    ## 1     1     1 0.8217391 0.8915094 0.8552036
    ## 01    2     0 0.8307692 0.8223350 0.8265306
    ## 11    2     1 0.8292683 0.8374384 0.8333333
    ## 02    3     0 0.8600000 0.8000000 0.8289157
    ## 12    3     1 0.7850000 0.8486486 0.8155844
    ## 03    4     0 0.8615385 0.8115942 0.8358209
    ## 13    4     1 0.8097561 0.8601036 0.8341709
    ## 04    5     0 0.8058252 0.8601036 0.8320802
    ## 14    5     1 0.8608247 0.8067633 0.8329177

``` r
aggregate(cbind(precision, recall, F1) ~ class, data = metrics_by_fold, FUN = mean)
```

    ##   class precision    recall        F1
    ## 1     0 0.8445678 0.8151895 0.8289153
    ## 2     1 0.8213177 0.8488927 0.8342420

Precision, recall and the F1 score are discussed further in Jurafsky and Martin (2026) (chapter 4).

## References

<div id="refs" class="references csl-bib-body hanging-indent">

<div id="ref-grimmer2013" class="csl-entry">

Grimmer, Justin, and Brandon M. Stewart. 2013. “Text as Data: The Promise and Pitfalls of Automatic Content Analysis Methods for Political Texts.” *Political Analysis* 21 (3): 267–97. <https://doi.org/10.1093/pan/mps028>.

</div>

<div id="ref-jurafskymartin2026" class="csl-entry">

Jurafsky, Daniel, and James H. Martin. 2026. *Speech and Language Processing: An Introduction to Natural Language Processing, Computational Linguistics, and Speech Recognition with Language Models*. <https://web.stanford.edu/~jurafsky/slp3/>.

</div>

</div>
