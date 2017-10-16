---
title: Wordfish
draft: true
---



Poisson scaling


```r
ieWF <- dfm(data_corpus_irishbudget2010, removePunct = TRUE) %>%
    textmodel_wordfish(dir = c(6,5))
textplot_scale1d(ieWF)
summary(ieWF)
```


```r
knitr::kable(summary(ieWF))
```
