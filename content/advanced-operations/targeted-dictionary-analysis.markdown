---
title: Apply dictionary to specific contexts
weight: 30
draft: false
---

We can detect occurrences of words in specific contexts by selectively applying dictionary. In this example, we apply a sentiment dictionary to segments of news articles where the (British) government is mentioned.


```r
require(quanteda)
require(quanteda.corpora)
```

This corpus contains 6,000 Guardian news articles from 2012 to 2016.


```r
corp_news <- download("data_corpus_guardian")
```



Tokenize texts and select tokens only around the keywords related to the government using `tokens_keep()`.


```r
# tokenize
toks_news <- tokens(corp_news, remove_punct = TRUE)

# get relevant keywords and phrases
gov <- c("government", "cabinet", "prime minister")

# only keep tokens specified above and their context of ±10 tokens
toks_gov <- tokens_keep(toks_news, pattern = phrase(gov), window = 10)
```

Apply the the Lexicoder Sentiment Dictionary to the selected contexts using `tokens_lookup()`. 


```r
lengths(data_dictionary_LSD2015)
```

```
##     negative     positive neg_positive neg_negative 
##         2858         1709         1721         2860
```

```r
toks_gov_lsd <- tokens_lookup(toks_gov, dictionary = data_dictionary_LSD2015[1:2])

# create a document document-feature matrix and group it by day
dfmat_gov_lsd <- dfm(toks_gov_lsd) %>% 
  dfm_group("date")
```


```r
matplot(dfmat_gov_lsd$date, dfmat_gov_lsd, type = "l", lty = 1, col = 1:2,
        ylab = "Frequency", xlab = "")
grid()
legend("topleft", col = 1:2, legend = c("Negative", "Positive"), lty = 1, bg = "white")
```

<img src="/advanced-operations/targeted-dictionary-analysis_files/figure-html/unnamed-chunk-6-1.png" width="960" />

We compute daily sentiment scores by taking the difference between the frequency of positive and negative words.


```r
plot(dfmat_gov_lsd$date, dfmat_gov_lsd[,2] - dfmat_gov_lsd[,1], 
     type = "l", ylab = "Sentiment", xlab = "")
grid()
abline(h = 0, lty = 2)
```

<img src="/advanced-operations/targeted-dictionary-analysis_files/figure-html/unnamed-chunk-7-1.png" width="960" />

We can apply kernel smoothing to show the trend more clearly.


```r
dat_smooth <- ksmooth(x = dfmat_gov_lsd$date, y = dfmat_gov_lsd[,2] - dfmat_gov_lsd[,1],
                      kernel = "normal", bandwidth = 30)
plot(dat_smooth$x, dat_smooth$y, type = "l", ylab = "Sentiment", xlab = "")
grid()
abline(h = 0, lty = 2)
```

<img src="/advanced-operations/targeted-dictionary-analysis_files/figure-html/unnamed-chunk-8-1.png" width="960" />

