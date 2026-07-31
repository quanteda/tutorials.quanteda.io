---
title: Construct a FCM
weight: 10
draft: false
---

A feature co-occurrence matrix (FCM) records how often pairs of tokens occur in the same document or near each other, rather than how often each token occurs on its own, letting you study relationships between words, such as which ones tend to appear together, rather than just their individual frequencies. An FCM is a special object in **quanteda**, but it behaves similarly to a DFM in how you print, subset and inspect it.


``` r
library(quanteda)
library(quanteda.textplots)
library(quanteda.corpora)
```

We use a corpus of 6,000 Guardian news articles published between 2012 and 2016. Since it's too large to include directly in this tutorial's package, we retrieve it with the `download()` function from **quanteda.corpora**.



``` r
corp_news <- download("data_corpus_guardian")
```



Co-occurrence matrices grow very large, since they record a relationship between every pair of words rather than a single count per word. When a corpus is large, you might need cut a DFM down to a manageable set of features before constructing an FCM from it. In the example below, we first remove all stopwords and punctuation characters. Afterwards, we remove certain patterns that usually describe the publication time and date of articles, which are not part of the article's content. The final line keeps only terms that occur at least 100 times across the corpus.


``` r
toks_news <- tokens(corp_news, remove_punct = TRUE)
dfmat_news <- dfm(toks_news)
dfmat_news <- dfm_remove(dfmat_news, pattern = c(stopwords("en"), "*-time", "updated-*", "gmt", "bst"))
dfmat_news <- dfm_trim(dfmat_news, min_termfreq = 100)

topfeatures(dfmat_news)
```

```
##       said     people        one        new       also         us        can 
##      28413      11169       9884       8024       7901       7091       6972 
## government       year       last 
##       6821       6570       6335
```

``` r
nfeat(dfmat_news)
```

```
## [1] 4212
```

You can construct an FCM from a DFM or a tokens object using `fcm()`. 

``` r
fcmat_news <- fcm(dfmat_news)
dim(fcmat_news)
```

```
## [1] 4212 4212
```

You can select features of an FCM using `fcm_select()`, in the same way you selected features of a DFM in the [previous chapter](/basic-operations/dfm/dfm_select). Here we keep only the 50 most frequent words from the DFM, so that the network plot below stays readable.


``` r
feat <- names(topfeatures(dfmat_news, 50))
fcmat_news_select <- fcm_select(fcmat_news, pattern = feat, selection = "keep")
dim(fcmat_news_select)
```

```
## [1] 50 50
```

An FCM can be used to train word embedding models with the **text2vec** package, or to visualise a semantic network analysis with `textplot_network()`.


``` r
size <- log(colSums(dfm_select(dfmat_news, feat, selection = "keep")))

set.seed(144)
textplot_network(fcmat_news_select, min_freq = 0.8, vertex_size = size / max(size) * 3)
```

<img src="/basic-operations/fcm/fcm_files/figure-html/unnamed-chunk-7-1.png" alt="" width="672" />

