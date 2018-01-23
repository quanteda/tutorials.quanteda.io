---
title: Correspondence analysis
draft: true
---




```r
require(quanteda)
dfm(data_corpus_irishbudget2010) %>%
    textmodel_ca() %>% 
    textplot_scale1d()
```

<img src="/machine-learning/ca.en_files/figure-html/unnamed-chunk-2-1.svg" width="768" />
