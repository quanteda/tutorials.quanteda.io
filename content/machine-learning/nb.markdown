---
title: Naive Bayes classifier
weight: 10
draft: false
bibliography: ../references.bib
---

We now move from describing text statistically to using it to make predictions. The models here fall into two broad families: supervised models, which learn from documents that a human has already labelled, and unsupervised models, which find structure in unlabelled text on their own. Naive Bayes is a supervised model, usually used to classify documents into two or more categories. We train the classifier using class labels already attached to a set of documents, and it then predicts the most likely class of new, unlabelled documents, based on the patterns it learned during training.

``` r
library(quanteda)
library(quanteda.textmodels)
library(mltest)
```

`data_corpus_moviereviews` from the **quanteda.textmodels** package contains 2,000 movie reviews, each already labelled as “positive” or “negative”.

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

The document-level variable `sentiment` records whether a movie review was labelled positive or negative. To evaluate a classifier fairly, you cannot test it on the same documents you trained it on, since it could memorise them rather than learning generalisable patterns. In this example, we will therefore hold back part of the labelled data: we use 1,500 reviews as the training set to build a Naive Bayes classifier, then predict the sentiment of the remaining 500 reviews, our test set, and check the predictions against their true, known labels.

The reviews in this corpus are ordered with all the negative reviews first and all the positive reviews second, so taking the first 1,500 would give us an unbalanced and biased training set. We therefore draw a random sample of document numbers instead.

`set.seed()` fixes R’s random number generator so the “random” sample is reproducible, which matters if you want your results, and this tutorial’s, to match every time the code runs. We also add an `id_numeric` document-level variable as a simple way to identify which reviews belong to the training set and which to the test set. `tokens_wordstem()` is used here for the first time in this tutorial: it reduces words to their root form, so that, for example, “acting” and “acted” both become “act”, helping a bag-of-words classifier like Naive Bayes generalise better by treating related word forms as the same feature.

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

We now train the Naive Bayes classifier using `textmodel_nb()`. We give it the training DFM and the true sentiment labels for those same documents, so that it can learn which words are associated with positive versus negative reviews.

``` r
tmod_nb <- textmodel_nb(dfmat_training, dfmat_training$sentiment)
summary(tmod_nb)
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

The model was trained on a specific set of features, those present in `dfmat_training`, but the test set may contain different words that the model has never seen and cannot use. Naive Bayes can only use features that occur in the training set, so we make the two DFMs’ features identical using `dfm_match()`, aligning the test set’s columns to the training set’s and discarding any features the model never learned about.

``` r
dfmat_matched <- dfm_match(dfmat_test, features = featnames(dfmat_training))
```

Now we can compare the model’s predictions against the reviews’ true sentiment to see how well the classifier performed. `predict()` returns the classifier’s best guess for each test document; cross-tabulating it against the actual labels shows where it succeeded and failed.

``` r
actual_class <- dfmat_matched$sentiment
predicted_class <- predict(tmod_nb, newdata = dfmat_matched)
tab_class <- table(actual_class, predicted_class)
tab_class
```

    ##             predicted_class
    ## actual_class neg pos
    ##          neg 213  45
    ##          pos  37 205

The diagonal of this table (top-left and bottom-right) counts documents the classifier got right; the off-diagonal cells count its mistakes. From the cross-table we can see that the number of false positives and false negatives is similar. The classifier made mistakes in both directions, but does not seem to systematically over- or under-estimate one class.

Reading raw counts out of a table works for a quick check, but it does not summarise performance in a single, comparable number. The **mltest** package’s `ml_test()` function computes standard classification metrics directly from two labelled vectors, predicted and true, rather than from a table, which avoids having to remember whether predictions or true labels belong in the rows.

``` r
tstat_nb <- ml_test(predicted_class, actual_class)
tstat_nb$accuracy
```

    ## [1] 0.836

``` r
data.frame(precision = tstat_nb$precision, recall = tstat_nb$recall, F1 = tstat_nb$F1)
```

    ##     precision    recall        F1
    ## neg     0.852 0.8255814 0.8385827
    ## pos     0.820 0.8471074 0.8333333

`accuracy` is the proportion of all test documents the classifier labelled correctly. The data frame below it breaks performance down by class: precision for “pos” is the proportion of documents the model called positive that were genuinely positive, and recall for “pos” is the proportion of genuinely positive documents the model actually found. The F1 score summarises the two as a single number, their harmonic mean. `print(tstat_nb)` prints all of these metrics at once, and you can get the same output as a data frame directly with `data.frame(tstat_nb)`.

{{% notice note %}}
Precision, recall and the F1 score are frequently used to assess the classification performance. Precision is measured as `TP / (TP + FP)`, where `TP` are the number of true positives and `FP` are the false positives. Recall divides the true positives by the sum of true positives and false negatives `TP / (TP + FN)`. Finally, the F1 score is a harmonic mean of precision and recall `2 * (Precision * Recall) / (Precision + Recall)`.
{{% /notice %}}

## How much would a different split change this?

The accuracy above comes from a single train/test split, and that split was itself the product of one arbitrary random seed. If a different 1,500 reviews had ended up in the training set, would accuracy have come out noticeably different? K-fold cross-validation answers this directly: it splits the full, labelled data into *k* roughly equal folds, then repeatedly trains on all but one fold and tests on the held-out fold, until every fold has served as the test set once. Here we use five folds on the whole labelled corpus.

``` r
set.seed(300)
folds <- sample(rep(1:5, length.out = ndoc(dfmt_movie)))
accuracy_by_fold <- numeric(5)
metrics_by_fold <- data.frame()

for (i in 1:5) {
    dfmat_train_i <- dfm_subset(dfmt_movie, folds != i)
    dfmat_test_i <- dfm_subset(dfmt_movie, folds == i) |>
        dfm_match(features = featnames(dfmat_train_i))

    tmod_i <- textmodel_nb(dfmat_train_i, dfmat_train_i$sentiment)
    pred_i <- predict(tmod_i, newdata = dfmat_test_i)
    actual_i <- dfmat_test_i$sentiment

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

    ## [1] 0.8025 0.7950 0.8075 0.8200 0.8050

``` r
mean(accuracy_by_fold)
```

    ## [1] 0.806

The five fold-level accuracies are not identical to each other, and their average can differ from the accuracy of the single split computed earlier in this page. The spread directly measures how much your estimate of model performance depends on which documents happened to end up in the test set, rather than on anything about the model itself.

Accuracy alone can hide uneven performance across classes, so `metrics_by_fold` collects precision, recall and F1 for both classes on every fold, ten rows in total (five folds times two classes: “neg” and “pos”).

``` r
# show performance metrics for each run
metrics_by_fold
```

    ##      fold class precision    recall        F1
    ## neg     1   neg 0.7853403 0.7978723 0.7915567
    ## pos     1   pos 0.8181818 0.8066038 0.8123515
    ## neg1    2   neg 0.7777778 0.8172589 0.7970297
    ## pos1    2   pos 0.8134715 0.7733990 0.7929293
    ## neg2    3   neg 0.8165138 0.8279070 0.8221709
    ## pos2    3   pos 0.7967033 0.7837838 0.7901907
    ## neg3    4   neg 0.8110599 0.8502415 0.8301887
    ## pos3    4   pos 0.8306011 0.7875648 0.8085106
    ## neg4    5   neg 0.7725118 0.8445596 0.8069307
    ## pos4    5   pos 0.8412698 0.7681159 0.8030303

``` r
# aggregate performance metrics by calculating the means
aggregate(cbind(precision, recall, F1) ~ class, data = metrics_by_fold, FUN = mean)
```

    ##   class precision    recall        F1
    ## 1   neg 0.7926407 0.8275679 0.8095753
    ## 2   pos 0.8200455 0.7838935 0.8014025

Precision, recall and the F1 score are discussed further in Jurafsky and Martin (2026) (chapter 4).

## References

<div id="refs" class="references csl-bib-body hanging-indent">

<div id="ref-jurafskymartin2026" class="csl-entry">

Jurafsky, Daniel, and James H. Martin. 2026. *Speech and Language Processing: An Introduction to Natural Language Processing, Computational Linguistics, and Speech Recognition with Language Models*. <https://web.stanford.edu/~jurafsky/slp3/>.

</div>

</div>
