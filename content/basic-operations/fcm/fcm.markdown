---
title: Construct a FCM
weight: 10
draft: false
---

A feature co-occurrence matrix (FCM) records the number of co-occurrences of tokens. This is a special object in **quanteda**, but behaves similarly to a DFM. 


```r
require(quanteda)
require(quanteda.corpora)
```


```r
corp_news <- download("data_corpus_guardian")
```



When a corpus is large, you have to select features of a DFM before constructing a FCM. In the example below, we first remove all stopwords and punctuation characters. Afterwards, we remove certain patterns that usually desciribe the publication time and date of articles. The third row keeps only terms that occur at least 100 times in the document-feature matrix. 


```r
dfmat_news <- dfm(corp_news, remove = stopwords("en"), remove_punct = TRUE)
dfmat_news <- dfm_remove(dfmat_news, pattern = c("*-time", "updated-*", "gmt", "bst", "|"))
dfmat_news <- dfm_trim(dfmat_news, min_termfreq = 100)

topfeatures(dfmat_news)
```

```
##       said     people        one        new       also         us        can 
##      28412      11168       9879       8024       7901       7090       6972 
## government       year       last 
##       6821       6570       6335
```

```r
nfeat(dfmat_news)
```

```
## [1] 4210
```

You can construct a FCM from a DFM or a tokens object using `fcm()`. `topfeatures()` returns the most frequntly co-occuring words.


```r
fcmat_news <- fcm(dfmat_news)
dim(fcmat_news)
```

```
## [1] 4210 4210
```

```r
topfeatures(fcmat_news)
```

```
##   trump    said clinton    cruz sanders     one     new    also    2015  donald 
## 2864015 2181723 2003017 1873907 1806663 1721386 1507496 1408247 1388364 1181398
```

You can select features of a FCM using `fcm_select()`.


```r
feat <- names(topfeatures(fcmat_news, 50))
fcmat_news_select <- fcm_select(fcmat_news, pattern = feat, selection = "keep")
dim(fcmat_news_select)
```

```
## [1] 50 50
```

A FCM can be used to train word embedding models with the **text2vec** package, or to visualize a semantic network analysis with ` textplot_network()`.


```r
size <- log(colSums(dfm_select(dfmat_news, feat, selection = "keep")))

set.seed(144)
textplot_network(fcmat_news_select, min_freq = 0.8, vertex_size = size / max(size) * 3)
```

<img src="/basic-operations/fcm/fcm_files/figure-html/unnamed-chunk-7-1.png" width="672" />

