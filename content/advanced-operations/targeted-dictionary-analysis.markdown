---
title: Apply dictionary to specific contexts
weight: 30
draft: false
---

You have applied a dictionary to a tokens object, and you have used a window of context around a keyword. Here we combine these two approaches. We detect occurrences of dictionary words in specific contexts by selectively applying a dictionary only within a window around a keyword, rather than across the whole document.


``` r
require(quanteda)
require(quanteda.corpora)
require(ggplot2)
```



The corpus contains 6,000 Guardian news articles from 2012 to 2016.


``` r
corp_news <- download("data_corpus_guardian")
```



We tokenise the texts and select tokens surrounding keywords related to the government using `tokens_keep()`, the same window-based approach introduced in the [Tokens chapter](/basic-operations/tokens/tokens_select).


``` r
# tokenize corpus
toks_news <- tokens(corp_news, remove_punct = TRUE)

# get relevant keywords and phrases
gov <- c("government", "cabinet", "prime minister")

# only keep tokens specified above and their context of ±10 tokens
# note: use phrase() to correctly score multi-word expressions
toks_gov <- tokens_keep(toks_news, pattern = phrase(gov), window = 10)
```

We now apply the Lexicoder Sentiment Dictionary, a widely used dictionary for political and news text that classifies words as positive or negative, to the selected contexts using `tokens_lookup()`. The dictionary has four categories in total, so we first subset it down to just "negative" and "positive" for this example.


``` r
lengths(data_dictionary_LSD2015)
```

```
##     negative     positive neg_positive neg_negative 
##         2858         1709         1721         2860
```

``` r
# select only the "negative" and "positive" categories
data_dictionary_LSD2015_pos_neg <- data_dictionary_LSD2015[1:2]

toks_gov_lsd <- tokens_lookup(toks_gov, dictionary = data_dictionary_LSD2015_pos_neg)

# create a document document-feature matrix and group it by day
dfmat_gov_lsd <- dfm(toks_gov_lsd) |>
  dfm_group(groups = date)
```

Grouping by `date` gives us one row per day, with two columns counting how many positive and negative words appeared near government-related keywords that day. Plotting the two columns over time shows how the volume of positive and negative language changed day by day.


``` r
dat_gov_lsd <- rbind(
  data.frame(date = dfmat_gov_lsd$date, sentiment = "positive",
             frequency = as.numeric(dfmat_gov_lsd[, "positive"])),
  data.frame(date = dfmat_gov_lsd$date, sentiment = "negative",
             frequency = as.numeric(dfmat_gov_lsd[, "negative"]))
)

ggplot(dat_gov_lsd, aes(x = date, y = frequency, colour = sentiment)) +
  geom_line() +
  labs(x = NULL, y = "Frequency", colour = NULL)
```

<img src="/advanced-operations/targeted-dictionary-analysis_files/figure-html/unnamed-chunk-7-1.png" alt="" width="672" />

Raw counts of positive and negative words are still two separate lines, which makes them harder to compare at a glance. We can compute a single daily sentiment score by taking the difference between the frequency of positive and negative words: a positive value means positive language outweighed negative language that day, and a negative value means the reverse.


``` r
dat_gov_sentiment <- data.frame(date = dfmat_gov_lsd$date,
                                 sentiment = as.numeric(dfmat_gov_lsd[, "positive"]) -
                                             as.numeric(dfmat_gov_lsd[, "negative"]))

ggplot(dat_gov_sentiment, aes(x = date, y = sentiment)) +
  geom_line() +
  geom_hline(yintercept = 0, linetype = "dashed") +
  labs(x = NULL, y = "Sentiment")
```

<img src="/advanced-operations/targeted-dictionary-analysis_files/figure-html/unnamed-chunk-8-1.png" alt="" width="672" />

Day-to-day sentiment is noisy, since a single unusual news story can swing one day's score sharply either way. We can apply kernel smoothing to average out this noise and show the underlying trend more clearly, making it easier to see whether sentiment was rising, falling or stable over a longer period.


``` r
dat_smooth <- ksmooth(x = dat_gov_sentiment$date,
                       y = dat_gov_sentiment$sentiment,
                       kernel = "normal", bandwidth = 30)
dat_smooth <- data.frame(date = dat_smooth$x, sentiment = dat_smooth$y)

ggplot(dat_smooth, aes(x = date, y = sentiment)) +
  geom_line() +
  geom_hline(yintercept = 0, linetype = "dashed") +
  labs(x = NULL, y = "Sentiment")
```

<img src="/advanced-operations/targeted-dictionary-analysis_files/figure-html/unnamed-chunk-9-1.png" alt="" width="672" />

