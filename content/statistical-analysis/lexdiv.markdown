---
title: Lexical diversity
weight: 20
chapter: false
draft: false
---


```r
require(quanteda)
```

`textstat_lexdiv()` calculates various lexical diversity measures based on the number of unique types of tokens and the length of a document. It is useful, for instance, for analysing speakers' or writers' linguistic skills, or the complexity of ideas expressed in documents.


```r
toks_inaug <- tokens(data_corpus_inaugural)
dfmat_inaug <- dfm(toks_inaug, remove = stopwords("en"))
tstat_lexdiv <- textstat_lexdiv(dfmat_inaug)
tail(tstat_lexdiv, 5)
```

```
##      document       TTR
## 54  2001-Bush 0.6424010
## 55  2005-Bush 0.6176753
## 56 2009-Obama 0.6828645
## 57 2013-Obama 0.6605238
## 58 2017-Trump 0.6409537
```


```r
plot(tstat_lexdiv$TTR, type = "l", xaxt = "n", xlab = NULL, ylab = "TTR")
grid()
axis(1, at = seq_len(nrow(tstat_lexdiv)), labels = dfmat_inaug$President)
```

<img src="/statistical-analysis/lexdiv_files/figure-html/unnamed-chunk-3-1.png" width="672" />


