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
news_corp <- download('data_corpus_guardian')
```




```r
docvars(news_corp, 'year') <- year(docvars(news_corp, 'date'))
docvars(news_corp, 'month') <- month(docvars(news_corp, 'date'))
docvars(news_corp, 'week') <- week(docvars(news_corp, 'date'))

news_corp <- corpus_subset(news_corp, 'year' >= 2016)
news_toks <- tokens(news_corp, remove_punct = TRUE)
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
lsd_toks <- tokens_lookup(news_toks, dictionary =  data_dictionary_LSD2015[1:2])
head(lsd_toks, 2)
```

```
## tokens from 2 documents.
## text136751 :
## [1] "positive" "positive"
## 
## text118588 :
##  [1] "positive" "positive" "negative" "negative" "negative" "positive"
##  [7] "negative" "positive" "positive" "negative" "positive" "positive"
## [13] "positive" "positive" "positive" "negative" "negative" "negative"
## [19] "positive" "negative" "positive" "negative" "positive" "positive"
## [25] "positive" "positive" "negative" "positive" "positive" "positive"
## [31] "positive" "positive" "positive" "positive" "positive" "positive"
```

```r
lsd_dfm <- dfm(lsd_toks)
head(lsd_dfm, 2)
```

```
## Document-feature matrix of: 2 documents, 2 features (25.0% sparse).
## 2 x 2 sparse Matrix of class "dfm"
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
eu_toks <- tokens_keep(news_toks, pattern = phrase(eu), window = 10)
eu_lsd_dfm <- dfm(eu_toks, dictionary = data_dictionary_LSD2015[1:2]) %>% 
                  dfm_group(group = 'week', fill = TRUE) 

matplot(eu_lsd_dfm, type = 'l', xaxt = 'n', lty = 1, ylab = 'Frequency')
grid()
axis(1, seq_len(ndoc(eu_lsd_dfm)), ymd("2016-01-01") + weeks(seq_len(ndoc(eu_lsd_dfm)) - 1))
legend('topleft', col = 1:2, legend = c('Negative', 'Positive'), lty = 1, bg = 'white')
```

<img src="/advanced-operations/targeted-dictionary-analysis_files/figure-html/unnamed-chunk-6-1.png" width="672" />


```r
eu_n <- ntoken(dfm(eu_toks, group = docvars(eu_toks, 'week')))
plot((eu_lsd_dfm[,2] - eu_lsd_dfm[,1]) / eu_n, 
     type = 'l', ylab = 'Sentiment', xlab = '', xaxt = 'n')
axis(1, seq_len(ndoc(eu_lsd_dfm)), ymd("2016-01-01") + weeks(seq_len(ndoc(eu_lsd_dfm)) - 1))
grid()
abline(h = 0, lty = 2)
```

<img src="/advanced-operations/targeted-dictionary-analysis_files/figure-html/unnamed-chunk-7-1.png" width="672" />

### Immigration


```r
us <- c('immig*', 'migra*')
immig_toks <- tokens_keep(news_toks, pattern = phrase(us), window = 10)
immig_lsd_dfm <- dfm(immig_toks, dictionary = data_dictionary_LSD2015[1:2]) %>% 
                  dfm_group(group = 'week', fill = TRUE) 

matplot(immig_lsd_dfm, type = 'l', xaxt = 'n', lty = 1, ylab = 'Frequency')
grid()
axis(1, seq_len(ndoc(immig_lsd_dfm)), ymd("2016-01-01") + weeks(seq_len(ndoc(immig_lsd_dfm)) - 1))
legend('topleft', col = 1:2, legend = c('Negative', 'Positive'), lty = 1, bg = 'white')
```

<img src="/advanced-operations/targeted-dictionary-analysis_files/figure-html/unnamed-chunk-8-1.png" width="672" />


```r
immig_n <- ntoken(dfm(immig_toks, group = docvars(immig_toks, 'week')))
plot((immig_lsd_dfm[,2] - immig_lsd_dfm[,1]) / immig_n, 
     type = 'l', ylab = 'Sentiment', xlab = '', xaxt = 'n')
axis(1, seq_len(ndoc(immig_lsd_dfm)), ymd("2016-01-01") + weeks(seq_len(ndoc(immig_lsd_dfm)) - 1))
grid()
abline(h = 0, lty = 2)
```

<img src="/advanced-operations/targeted-dictionary-analysis_files/figure-html/unnamed-chunk-9-1.png" width="672" />

