---
title: Lexical diversity
weight: 20
chapter: false
draft: false
---

Lexical diversity measures how varied a document's vocabulary is: a document that reuses the same few words repeatedly has low diversity, while one that draws on many distinct words has high diversity. `textstat_lexdiv()` calculates several measures of lexical diversity, based on the number of unique word types relative to the length of a document, useful for analysing speakers' or writers' linguistic style, or the complexity of ideas expressed in documents.


``` r
library(quanteda)
library(quanteda.textstats)
library(ggplot2)
```




``` r
toks_inaug <- tokens(data_corpus_inaugural)
dfmat_inaug <- dfm(toks_inaug) |>
  dfm_remove(stopwords("en"))
tstat_lexdiv <- textstat_lexdiv(dfmat_inaug)
tail(tstat_lexdiv, 5)
```

```
##      document       TTR
## 56 2009-Obama 0.6828645
## 57 2013-Obama 0.6605238
## 58 2017-Trump 0.6409537
## 59 2021-Biden 0.5572316
## 60 2025-Trump 0.5814917
```

By default, `textstat_lexdiv()` reports the type-token ratio (TTR): the number of distinct word types divided by the total number of word tokens in a document. A higher TTR means a more varied vocabulary relative to the document's length. Plotting TTR across all the inaugural speeches, in chronological order, lets us see whether presidential vocabulary has become more or less varied over time.


``` r
# create data frame storing the Year and President document-level variables
# and the lexical diversity score
dat_lexdiv <- data.frame(year = dfmat_inaug$Year,
                          president = dfmat_inaug$President,
                          TTR = tstat_lexdiv$TTR)

ggplot(dat_lexdiv, aes(x = year, y = TTR)) +
  geom_line() +
  labs(x = "Year", y = "TTR") +
  theme(axis.text.x = element_text(angle = 90, hjust = 1, vjust = 0.5))
```

<img src="/statistical-analysis/lexdiv_files/figure-html/unnamed-chunk-4-1.png" alt="" width="672" />
