---
title: Newsmap
weight: 60
draft: false
---

Topics models are unsupervised document classification techniques. By modeling distributions of topics over words and words over documents, topic models identify the most discriminatory groups of documents automatically. 


```r
require(quanteda)
require(quanteda.corpora)
require(lubridate)
require(newsmap)
require(rworldmap)
```


```r
news_corp <- download('data_corpus_guardian')
```



We only select news stories published in 2016 using `corpus_subset()`. 


```r
news_corp <- corpus_subset(news_corp, year(docvars(news_corp, 'date')) >= 2016)
ndoc(news_corp)
```

```
## [1] 1959
```


```r
# Custom stopwords
month <- c('January', 'February', 'March', 'April', 'May', 'June',
           'July', 'August', 'September', 'October', 'November', 'December')
day <- c('Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday')
agency <- c('AP', 'AFP', 'Reuters')


# Tokenize
toks <- tokens(news_corp)
toks <- tokens_remove(toks, stopwords('english'), valuetype = 'fixed', padding = TRUE)
toks <- tokens_remove(toks, c(month, day, agency), valuetype = 'fixed', padding = TRUE)

# Seed dictionaries supplied by this package
# English: data_dictionary_newsmap_en
# German: data_dictionary_newsmap_de
# Japanese: data_dictionary_newsmap_ja
# Spanish: data_dictionary_newsmap_es
# Russian: data_dictionary_newsmap_ru

label_toks <- tokens_lookup(toks, data_dictionary_newsmap_en, levels = 3) # level 3 is countries
label_dfm <- dfm(label_toks)

feat_dfm <- dfm(toks, tolower = FALSE)
feat_dfm <- dfm_select(feat_dfm, selection = "keep", '^[A-Z][A-Za-z1-2]+', valuetype = 'regex', case_insensitive = FALSE) # include only proper nouns to model
feat_dfm <- dfm_trim(feat_dfm, min_count = 10)
```

```
## Warning in dfm_trim.dfm(feat_dfm, min_count = 10): min_count is deprecated,
## use min_termfreq
```

```r
## Warning in dfm_trim.dfm(feat_dfm, min_count = 10): min_count is deprecated,
## use min_termfreq

model <- textmodel_newsmap(feat_dfm, label_dfm)

# Features with largest weights
coef(model, n = 7)[c("us", "gb", "fr", "br", "jp")]
```

```
## $us
##         US   American  Americans Washington     States      Cuban 
##   6.597971   5.515851   4.870607   4.854347   3.978222   3.904719 
## Litvinenko 
##   3.870233 
## 
## $gb
##         UK     London    British    Britain  Britain's Litvinenko 
##   7.505087   6.752939   6.699439   6.683143   5.724340   4.602605 
##  Goldsmith 
##   4.461527 
## 
## $fr
##   French    Paris   France Hollande Abdeslam      EDF   Yudkin 
## 7.024415 6.710442 6.620445 5.886103 4.974474 4.902153 4.824192 
## 
## $br
##    Brazil  Rousseff Brazilian       Rio     Dilma     MINEM     Chapa 
##  6.980052  6.728737  6.540685  6.308883  6.117828  5.812446  5.658296 
## 
## $jp
##       Japan    Japanese      Yudkin Marshallese   Hiroshima       Tokyo 
##    7.028631    6.671956    6.034379    5.978809    5.642337    5.514504 
##        Keys 
##    5.367900
```


{{% notice info %}}
If you want to learn more about Newsmap, see:  
Watanabe, Kohei, 2018, Newsmap: A semi-supervised approach to geographical news classification, _Digital Journalism_ 6 (3): 294-309.
{{% /notice %}}

