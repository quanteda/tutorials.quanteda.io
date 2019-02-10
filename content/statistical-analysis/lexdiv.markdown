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
##      document       TTR
## 54  2001-Bush 0.6424010
## 55  2005-Bush 0.6176753
## 56 2009-Obama 0.6828645
## 57 2013-Obama 0.6605238
## 58 2017-Trump 0.6409537
```


```r
plot(lexdiv$TTR, type = 'l', xaxt = 'n', xlab = NULL, ylab = "TTR")
grid()
axis(1, at = seq_len(nrow(lexdiv)), labels = docvars(inaug_dfm, 'President'))
```

<img src="/statistical-analysis/lexdiv_files/figure-html/unnamed-chunk-3-1.png" width="672" />


