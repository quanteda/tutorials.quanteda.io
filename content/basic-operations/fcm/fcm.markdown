---
title: Construct a FCM
weight: 10
draft: false
---

A feature-ouccerances matrix (FCM) records number of co-occurances of tokens. This is a special object in **quanteda**, but behaves similarly to a DFM. 


```r
require(quanteda)
require(quanteda.corpora)
```


```r
corp <- download('data_corpus_guardian')
```



When a corpus is large, you have to select features of a DFM before constructing a FCM.


```r
news_dfm <- dfm(corp, remove = stopwords('en'), remove_punct = TRUE)
news_dfm <- dfm_remove(news_dfm, c('*-time', 'updated-*', 'gmt', 'bst'))
news_dfm <- dfm_trim(news_dfm, min_count = 100)

topfeatures(news_dfm)
```

```
##       said     people        one        new       also         us 
##      28413      11169       9884       8024       7901       7091 
##        can government       year       last 
##       6972       6821       6570       6335
```

```r
nfeat(news_dfm)
```

```
## [1] 4209
```

You can construct a FCM from a DFM or a tokens object using `fcm()`. `topfeatures()` returns the most frequntly co-occuring words.


```r
news_fcm <- fcm(news_dfm)
dim(news_fcm)
```

```
## [1] 4209 4209
```

You can select features of a FCM using `fcm_select()`.


```r
feat <- names(topfeatures(news_fcm, 50))
news_fcm <- fcm_select(news_fcm, feat)
dim(news_fcm)
```

```
## [1] 50 50
```

A FCM can be used to train word embedding models with the **text2vec** package, or to visualize a semantic network analysis with ` textplot_network()`.


```r
size <- log(colSums(dfm_select(news_dfm, feat)))
textplot_network(news_fcm, min_freq = 0.8, vertex_size = size / max(size) * 3)
```

<img src="/basic-operations/fcm/fcm_files/figure-html/unnamed-chunk-7-1.svg" width="768" />

