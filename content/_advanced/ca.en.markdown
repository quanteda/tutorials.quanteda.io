---
title: Correspondence analysis
---



## Correspondence analysis


```r
require(quanteda)
## Loading required package: quanteda
## quanteda version 0.99.9
## Using 3 of 4 threads for parallel computing
## 
## Attaching package: 'quanteda'
## The following object is masked from 'package:utils':
## 
##     View
dfm(data_corpus_irishbudget2010) %>%
    textmodel_ca() %>% 
    textplot_scale1d()
```

<img src="/_advanced/ca.en_files/figure-html/unnamed-chunk-2-1.svg" width="768" />
