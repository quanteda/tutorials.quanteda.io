---
title: Simple frequency analysis
weight: 10
chapter: false
draft: false
---

Here we open a new part of the tutorial, on statistical analysis. You have already used `topfeatures()` to find the most common words in a DFM. `textstat_frequency()` builds on this idea. Unlike `topfeatures()`, it shows both term and document frequency side by side and returns a proper data frame you can filter and sort. You can also use it to find the most frequent features within groups, not just overall.


``` r
library(quanteda)
library(quanteda.textstats)
library(quanteda.textplots)
library(quanteda.corpora)
library(ggplot2)
```

We will work with a corpus of tweets in this chapter. Some of the corpora used across this tutorial, including this one, are too large to bundle with **quanteda** itself and are instead hosted online. `download()`, from **quanteda.corpora**, fetches them by URL the first time you need them. Here it is passed a direct link rather than a short name like `"data_corpus_guardian"`, because this dataset is not registered under a name in the package.


``` r
corp_tweets <- download(url = "https://www.dropbox.com/s/846skn1i5elbnd2/data_corpus_sampletweets.rds?dl=1")
```



We can analyse the most frequent hashtags in this corpus by applying `tokens_keep(pattern = "#*")`, which keeps only tokens starting with a hash symbol, before creating a DFM.

Setting `groups = lang` asks for the top hashtags separately within each language recorded in the corpus, rather than one overall ranking, so we can see whether different language communities are talking about different things.


``` r
toks_tweets <- tokens(corp_tweets, remove_punct = TRUE) |>
               tokens_keep(pattern = "#*")
dfmat_tweets <- dfm(toks_tweets)

tstat_freq <- textstat_frequency(dfmat_tweets, n = 5, groups = lang)
head(tstat_freq, 20)
```

```
##              feature frequency rank docfreq     group
## 1           #twitter         1    1       1    Basque
## 2     #canviemeuropa         1    1       1    Basque
## 3             #prest         1    1       1    Basque
## 4           #psifizo         1    1       1    Basque
## 5     #ekloges2014gr         1    1       1    Basque
## 6            #ep2014         1    1       1 Bulgarian
## 7         #yourvoice         1    1       1 Bulgarian
## 8      #eudebate2014         1    1       1 Bulgarian
## 9            #велико         1    1       1 Bulgarian
## 10           #ep2014         1    1       1  Croatian
## 11 #savedonbaspeople         1    1       1  Croatian
## 12   #vitoriagasteiz         1    1       1  Croatian
## 13           #ep14dk        32    1      32    Danish
## 14            #dkpol        18    2      18    Danish
## 15            #eupol         7    3       7    Danish
## 16        #vindtilep         7    3       7    Danish
## 17    #patentdomstol         4    5       4    Danish
## 18           #ep2014        35    1      35     Dutch
## 19              #vvd        11    2      11     Dutch
## 20               #eu         9    3       7     Dutch
```

Because `textstat_frequency()` returns an ordinary data frame, it plugs directly into **ggplot2**. You can plot the Twitter hashtag frequencies by piping the result straight into `ggplot()`.


``` r
dfmat_tweets |> 
  textstat_frequency(n = 15) |> 
  ggplot(aes(x = reorder(feature, frequency), y = frequency)) +
  geom_point() +
  coord_flip() +
  labs(x = NULL, y = "Frequency") +
  theme_minimal()
```

<img src="/statistical-analysis/frequency_files/figure-html/unnamed-chunk-5-1.png" alt="" width="672" />

Alternatively, you can create a word cloud of the 100 most common hashtags: a quick way to get a first impression of a corpus, though less precise than a bar chart for comparing exact frequencies. Larger words appear more often in the corpus. We set a random seed with `set.seed()` before plotting, since the word cloud's layout is random, and fixing the seed makes the picture reproducible.


``` r
set.seed(132)
textplot_wordcloud(dfmat_tweets, max_words = 100)
```

<img src="/statistical-analysis/frequency_files/figure-html/unnamed-chunk-6-1.png" alt="" width="672" />

You can also compare groups within a single word cloud. First, create a dummy variable indicating whether a tweet was posted in English or another language, using `ifelse()` as in the Basic Operations chapter. Then compare the most frequent hashtags of English and non-English tweets directly.


``` r
# create document-level variable indicating whether tweet was in English or other language
corp_tweets$dummy_english <- factor(ifelse(corp_tweets$lang == "English", "English", "Not English"))

# tokenize texts
toks_tweets <- tokens(corp_tweets)

# create a grouped dfm and compare groups
dfmat_corp_language <- dfm(toks_tweets) |>
                       dfm_keep(pattern = "#*") |>
                       dfm_group(groups = dummy_english)

# create wordcloud
set.seed(132) # set seed for reproducibility
textplot_wordcloud(dfmat_corp_language, comparison = TRUE, max_words = 200)
```

<img src="/statistical-analysis/frequency_files/figure-html/unnamed-chunk-7-1.png" alt="" width="672" />

Setting `comparison = TRUE` splits the word cloud into two halves, one per group, with each hashtag coloured and sized according to its frequency within that group, making it easy to spot hashtags that are distinctive to one language versus hashtags that are common to both.

