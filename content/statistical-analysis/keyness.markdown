---
title: Relative frequency analysis (keyness)
weight: 40
chapter: false
draft: false
---


```r
require(quanteda)
require(lubridate)
```

Keyness is a statistical measure originally implemented in [WordSmith](http://www.lexically.net/wordsmith/) to discover frequent words in target documents. This statistic is essentially a signed chi-square, where words more frequent than expected are given positive sign. 


```r
news_corp <- quanteda.corpora::download('data_corpus_guardian')
news_toks <- tokens(news_corp, remove_punct = TRUE) 
news_dfm <- dfm(news_toks)
key <- textstat_keyness(news_dfm, 6 <= month(docvars(news_dfm, 'date'))) 
head(key, 20)
```

```
##       feature      chi2 p n_target n_reference
## 1     october 176.47475 0      732         261
## 2      august 175.44999 0      569         170
## 3     climate 156.19111 0     1042         475
## 4      corbyn 136.08017 0      645         249
## 5       sharm 129.40436 0      144           0
## 6   september 127.02452 0      608         236
## 7   emissions 122.49636 0      427         135
## 8        july 117.80318 0      574         225
## 9          vw 111.11408 0      139           5
## 10    flights 110.15854 0      229          42
## 11   november 108.64113 0      758         352
## 12 volkswagen  99.74377 0      132           7
## 13       said  91.57670 0    15768       12645
## 14  el-sheikh  83.57271 0       93           0
## 15  #plebgate  80.87677 0       90           0
## 16    suicide  80.16133 0      316         109
## 17        was  79.53702 0    18200       14824
## 18   barclays  72.96325 0      209          56
## 19  customers  72.52186 0      632         319
## 20      paris  70.30945 0      602         302
```

```r
textplot_keyness(key)
```

<img src="/statistical-analysis/keyness_files/figure-html/unnamed-chunk-2-1.svg" width="768" />


