---
title: Relative frequency analysis (keyness)
weight: 40
chapter: false
draft: false
---

Keyness is a signed two-by-two association scores originally implemented in [WordSmith](http://www.lexically.net/wordsmith/) to identify frequent words in documents in a target and reference group.


```r
require(quanteda)
require(quanteda.corpora)
require(lubridate)
```



```r
news_corp <- download('data_corpus_guardian')
```



Using `textstat_keyness()`, you can compare frequencies of words between target and reference documents. Target documents are news articles published in 2016 and reference documents are those published in 2012-2015 in this example.


```r
news_toks <- tokens(news_corp, remove_punct = TRUE) 
news_dfm <- dfm(news_toks)
 
key <- textstat_keyness(news_dfm, target = year(docvars(news_dfm, 'date')) >= 2016)
attr(key, 'documents') <- c('2016', '2012-2015')

textplot_keyness(key)
```

<img src="/statistical-analysis/keyness_files/figure-html/unnamed-chunk-4-1.png" width="960" />


