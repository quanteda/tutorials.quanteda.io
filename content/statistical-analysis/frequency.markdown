---
title: Simple frequency analysis
weight: 10
chapter: false
draft: false
---


```r
require(quanteda)
require(quanteda.corpora)
require(ggplot2)
```

Unlike `topfeatures()`, `textstat_frequency()` shows both term and document frequencies. You can also use the function to find the most frequent features within groups.

Using the `download()` function from **quanteda.corpora**, you can retrieve a text corpus of tweets.


```r
corp_tweets <- download(url = 'https://www.dropbox.com/s/846skn1i5elbnd2/data_corpus_sampletweets.rds?dl=1')
```



We analyse the most frequent hashtags using `select = "#*"` when creating the `dfm`.


```r
toks_tweets <- tokens(corp_tweets, remove_punct = TRUE) 
dfmat_tweets <- dfm(toks_tweets, select = "#*")
tstat_freq <- textstat_frequency(dfmat_tweets, n = 5, groups = "lang")
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
## 10              <NA>        NA   NA      NA Bulgarian
## 11 #savedonbaspeople         1    1       1  Croatian
## 12   #vitoriagasteiz         1    1       1  Croatian
## 13              <NA>        NA   NA      NA  Croatian
## 14              <NA>        NA   NA      NA  Croatian
## 15              <NA>        NA   NA      NA  Croatian
## 16           #ep14dk        31    1      31    Danish
## 17            #dkpol        17    2      17    Danish
## 18            #eupol         7    3       7    Danish
## 19        #vindtilep         6    4       6    Danish
## 20    #patentdomstol         4    5       4    Danish
```

You can also plot the Twitter hashtag frequencies easily using `ggplot()`.


```r
dfmat_tweets %>% 
  textstat_frequency(n = 15) %>% 
  ggplot(aes(x = reorder(feature, frequency), y = frequency)) +
  geom_point() +
  coord_flip() +
  labs(x = NULL, y = "Frequency") +
  theme_minimal()
```

<img src="/statistical-analysis/frequency_files/figure-html/unnamed-chunk-5-1.png" width="672" />

Alternative, you can create a Wordcloud of the  100 most common tags.


```r
set.seed(132)
textplot_wordcloud(dfmat_tweets, max_words = 100)
```

<img src="/statistical-analysis/frequency_files/figure-html/unnamed-chunk-6-1.png" width="672" />

Finally, it is possible to compare different groups within one Wordcloud. We first create a dummy variable that indicates whether a tweet was posted in English or a different language. Afterwards, we compare the most frequent hashtags of English and non-English tweets.


```r
# create document-level variable indicating whether Tweet was in English or other language
docvars(corp_tweets, "dummy_english") <- factor(ifelse(docvars(corp_tweets, "lang") == "English", "English", "Not English"))

# create a grouped dfm and compare groups
dfmat_corp_language <- dfm(corp_tweets, select = "#*", groups = "dummy_english")

# create worcloud
set.seed(132)
textplot_wordcloud(dfmat_corp_language, comparison = TRUE, max_words = 200)
```

<img src="/statistical-analysis/frequency_files/figure-html/unnamed-chunk-7-1.png" width="672" />


