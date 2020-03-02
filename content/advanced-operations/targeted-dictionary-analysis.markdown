---
title: Targeted dictionary analysis
weight: 30
draft: false
---


```r
require(quanteda)
require(lubridate)
```

This corpus contains 6,000 Guardian news articles from 2012 to 2016.


```r
corp_news <- download('data_corpus_guardian')
```




```r
docvars(corp_news, 'year') <- year(docvars(corp_news, 'date'))
docvars(corp_news, 'month') <- month(docvars(corp_news, 'date'))
docvars(corp_news, 'week') <- week(docvars(corp_news, 'date'))

corp_news <- corpus_subset(corp_news, 'year' >= 2016)
toks_news <- tokens(corp_news, remove_punct = TRUE)
```

You can use `tokens_lookup()` or `dfm_looup()` to count dictionary values. **quanteda** contains Lexicoder Sentiment Dictionary created by Young and Soroka, so you can perfrom sentiment analysis of English texts right away.


```r
lengths(data_dictionary_LSD2015)
```

```
##     negative     positive neg_positive neg_negative 
##         2858         1709         1721         2860
```

```r
toks_news_lsd <- tokens_lookup(toks_news, dictionary =  data_dictionary_LSD2015[1:2])
head(toks_news_lsd, 2)
```

```
## Tokens consisting of 2 documents and 12 docvars.
## text136751 :
## [1] "positive" "positive"
## 
## text118588 :
##  [1] "positive" "positive" "negative" "negative" "negative" "positive"
##  [7] "negative" "positive" "positive" "negative" "positive" "positive"
## [ ... and 24 more ]
```

```r
dfmat_news_lsd <- dfm(toks_news_lsd)
head(dfmat_news_lsd, 2)
```

```
## Document-feature matrix of: 2 documents, 2 features (25.0% sparse) and 12 docvars.
##             features
## docs         negative positive
##   text136751        0        2
##   text118588       11       25
```

## Targeted analysis

You can use `tokens_select()` with `window` argument to perform more targeted sentiment analysis.

### European Union


```r
eu <- c('EU', 'europ*', 'european union')
toks_eu <- tokens_keep(toks_news, pattern = phrase(eu), window = 10)
dfmat_eu_lsd <- dfm(toks_eu, dictionary = data_dictionary_LSD2015[1:2]) %>% 
    dfm_group(group = 'week', fill = TRUE) 

matplot(dfmat_eu_lsd, type = 'l', xaxt = 'n', lty = 1, ylab = 'Frequency')
grid()
axis(1, seq_len(ndoc(dfmat_eu_lsd)), ymd("2016-01-01") + weeks(seq_len(ndoc(dfmat_eu_lsd)) - 1))
legend('topleft', col = 1:2, legend = c('Negative', 'Positive'), lty = 1, bg = 'white')
```

<img src="/advanced-operations/targeted-dictionary-analysis_files/figure-html/unnamed-chunk-6-1.png" width="672" />


```r
n_eu <- ntoken(dfm(toks_eu, group = docvars(toks_eu, 'week')))
plot((dfmat_eu_lsd[,2] - dfmat_eu_lsd[,1]) / n_eu, 
     type = 'l', ylab = 'Sentiment', xlab = '', xaxt = 'n')
axis(1, seq_len(ndoc(dfmat_eu_lsd)), ymd("2016-01-01") + weeks(seq_len(ndoc(dfmat_eu_lsd)) - 1))
grid()
abline(h = 0, lty = 2)
```

<img src="/advanced-operations/targeted-dictionary-analysis_files/figure-html/unnamed-chunk-7-1.png" width="672" />

### Immigration


```r
immig <- c('immig*', 'migra*')
toks_immig <- tokens_keep(toks_news, pattern = phrase(immig), window = 10)
dfmat_immig_lsd <- dfm(toks_immig, dictionary = data_dictionary_LSD2015[1:2]) %>% 
    dfm_group(group = 'week', fill = TRUE) 

matplot(dfmat_immig_lsd, type = 'l', xaxt = 'n', lty = 1, ylab = 'Frequency')
grid()
axis(1, seq_len(ndoc(dfmat_immig_lsd)), ymd("2016-01-01") + weeks(seq_len(ndoc(dfmat_immig_lsd)) - 1))
legend('topleft', col = 1:2, legend = c('Negative', 'Positive'), lty = 1, bg = 'white')
```

<img src="/advanced-operations/targeted-dictionary-analysis_files/figure-html/unnamed-chunk-8-1.png" width="672" />


```r
n_immig <- ntoken(dfm(toks_immig, group = docvars(toks_immig, 'week')))
plot((dfmat_immig_lsd[,2] - dfmat_immig_lsd[,1]) / n_immig, 
     type = 'l', ylab = 'Sentiment', xlab = '', xaxt = 'n')
axis(1, seq_len(ndoc(dfmat_immig_lsd)), ymd("2016-01-01") + weeks(seq_len(ndoc(dfmat_immig_lsd)) - 1))
grid()
abline(h = 0, lty = 2)
```

<img src="/advanced-operations/targeted-dictionary-analysis_files/figure-html/unnamed-chunk-9-1.png" width="672" />

