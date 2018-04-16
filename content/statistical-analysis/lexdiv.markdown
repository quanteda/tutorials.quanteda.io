---
title: Lexical diversity
weight: 20
chapter: false
draft: false
---


```r
require(quanteda)
```

`textstat_lexdiv()` calcuates lexical diversity in various measures based on the number of unique types of tokens and the length of a document. It is useful for analysing speakers' or writers' linguistic skill, or complexity of ideas expressed in documents.


```r
inaug_toks <- tokens(data_corpus_inaugural)
inaug_dfm <- dfm(inaug_toks, remove = stopwords('en'))
lexdiv <- textstat_lexdiv(inaug_dfm)
tail(lexdiv, 5)
```

```
##      document       TTR         C        R     CTTR        U         S
## 54  2001-Bush 0.5069444 0.9017668 16.09499 11.38087 30.57479 0.9059805
## 55  2005-Bush 0.5065943 0.9050432 18.18807 12.86091 32.75439 0.9120717
## 56 2009-Obama 0.5408300 0.9159074 20.90432 14.78159 37.74830 0.9239549
## 57 2013-Obama 0.5501592 0.9162593 19.49769 13.78695 37.00697 0.9226790
## 58 2017-Trump 0.5010753 0.8989056 15.28074 10.80512 29.36347 0.9020475
##         Maas     lgV0    lgeV0
## 54 0.1808499 6.266259 14.42859
## 55 0.1747291 6.618347 15.23931
## 56 0.1627614 7.243366 16.67847
## 57 0.1643835 7.088316 16.32145
## 58 0.1845425 6.090226 14.02326
```


```r
plot(lexdiv$TTR, type = 'l', xaxt = 'n', xlab = NULL, ylab = "TTR")
grid()
axis(1, at = seq_len(nrow(lexdiv)), labels = docvars(inaug_dfm, 'President'))
```

<img src="/statistical-analysis/lexdiv_files/figure-html/unnamed-chunk-3-1.svg" width="768" />


