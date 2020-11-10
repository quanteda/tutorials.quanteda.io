---
title: Newsmap
weight: 60
draft: false
---

Newsmap is a semisupervised model for geographical document classification. While (full) supervised models are trained on manually classified data, this semi-supervised model learns from "seed words" in dictionaries. 

Install the **newsmap** package from CRAN.


```r
install.packages("newsmap")
```


```r
require(quanteda)
require(quanteda.corpora)
require(newsmap)
require(maps)
require(ggplot2)
```

Download a corpus with news articles using **quanteda.corpora**'s `download()` function.


```r
corp_news <- download(url = "https://www.dropbox.com/s/r8zhsu8zvjzhnml/data_corpus_yahoonews.rds?dl=1")
```



`corp_news` contains 10,000 news summaries downloaded from Yahoo News in 2014.


```r
ndoc(corp_news)
```

```
## [1] 10000
```

```r
range(corp_news$date)
```

```
## [1] "2014-01-01" "2014-12-31"
```

In geographical classification, proper nouns are the most useful features of documents, but not all capitalized words are proper nouns, so we define custom stopwords.


```r
month <- c("January", "February", "March", "April", "May", "June",
           "July", "August", "September", "October", "November", "December")
day <- c("Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday")
agency <- c("AP", "AFP", "Reuters")
```


```r
toks_news <- tokens(corp_news)
toks_news_small <- tokens_remove(toks_news, pattern = stopwords("en"), valuetype = "fixed", padding = TRUE) %>% 
                   tokens_remove(pattern = c(month, day, agency), valuetype = "fixed", padding = TRUE)
```

**newsmap** contains [seed geographical dictionaries](https://github.com/koheiw/newsmap/tree/master/dict) in English, German, Spanish, Japanese and Russian languages. `data_dictionary_newsmap_en` is the seed dictionary for English texts.


```r
toks_label <- tokens_lookup(toks_news_small, dictionary = data_dictionary_newsmap_en, levels = 3) # level 3 is countries
dfmat_label <- dfm(toks_label, tolower = FALSE)

dfmat_feat <- dfm(toks_news_small, tolower = FALSE)
dfmat_feat_select <- dfm_select(dfmat_feat, pattern = "^[A-Z][A-Za-z1-2]+", valuetype = "regex", 
                                case_insensitive = FALSE) %>% 
                     dfm_trim(min_termfreq = 10)

tmod_nm <- textmodel_newsmap(dfmat_feat_select, y = dfmat_label)
```

The seed dictionary contains only names of countries and capital cities, but the model additional extracts features associated to the countries. These country codes are defined in [ISO 3166-1](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2).


```r
coef(tmod_nm, n = 10)[c("US", "GB", "FR", "BR", "JP")]
```

```
## $US
## WASHINGTON         US   American Washington       YORK     States  Americans 
##   7.154365   7.036912   6.829957   6.605901   6.370121   6.054697   5.359964 
##       York Brunnstrom      Kirby 
##   4.993414   3.781779   3.701736 
## 
## $GB
##   British    LONDON    London   Britain Britain's        UK      UKIP   Kingdom 
##  7.877779  7.847681  7.563696  7.264882  6.671107  5.487937  4.906016  4.774988 
##     Tesco     Hamza 
##  4.446484  4.359472 
## 
## $FR
##        French        France         PARIS         Paris      Hollande 
##      8.185087      8.091015      7.543233      7.305275      6.534569 
##    Hollande's        Fabius         Valls      Francois Saint-Germain 
##      5.403167      5.297807      5.297807      5.279115      4.972384 
## 
## $BR
##    Brazil       SAO     PAULO       RIO   JANEIRO Brazilian       Rio        DE 
##  8.175207  7.261659  7.247866  7.048737  7.048737  6.996552  6.922444  6.355590 
##   Janeiro       Sao 
##  6.303404  5.966932 
## 
## $JP
##     Japan  Japanese     TOKYO       Abe     Tokyo    Shinzo     Abe's   Tokyo's 
##  8.167129  7.896838  7.764292  7.063239  6.963156  6.791306  5.810476  5.318000 
## Fukushima   Japan's 
##  5.171397  4.390956
```

{{% notice tip %}}
Names of people, organizations and places are often multi-word expressions. To distiguish between "New York" and "York", for example, it is useful to compound tokens using `tokens_compound()` as explained in [Advanced Operations](../advanced-operations/compound-mutiword-expressions/).
{{% /notice %}}

You can predict the most strongly associated countries using `predict()` and count the frequency using `table()`. 


```r
pred_nm <- predict(tmod_nm)
head(pred_nm, 20)
```

```
##  text1  text2  text3  text4  text5  text6  text7  text8  text9 text10 text11 
##   "KP"   "SY"   "IQ"   "RU"   "TH"   "CN"   "UA"   "SY"   "GB"   "US"   "SY" 
## text12 text13 text14 text15 text16 text17 text18 text19 text20 
##   "US"   "UA"   "SY"   "LK"   "ES"   "AU"   "CR"   "ID"   "BH"
```

Factor levels are set to obtain zero counts for countries that did not appear in the corpus.


```r
count <- sort(table(factor(pred_nm, levels = colnames(dfmat_label))), decreasing = TRUE)
head(count, 20)
```

```
## 
##  GB  US  RU  UA  AU  CN  CA  FR  IQ  BR  SY  DE  ZA  NZ  JP  IL  IN  ES  EG  PS 
## 621 578 516 439 367 362 319 312 295 278 260 251 236 227 198 197 187 182 157 155
```

You can visualise the distribution of global news attention using `geom_map()`.


```r
dat_country <- as.data.frame(count, stringsAsFactors = FALSE)
colnames(dat_country) <- c("id", "frequency")

world_map <- map_data(map = "world")
world_map$region <- iso.alpha(world_map$region) # convert country name to ISO code

ggplot(dat_country, aes(map_id = id)) +
      geom_map(aes(fill = frequency), map = world_map) +
      expand_limits(x = world_map$long, y = world_map$lat) +
      scale_fill_continuous(name = "Frequency") +
      theme_void() +
      coord_fixed()
```

<img src="/machine-learning/newsmap.en_files/figure-html/unnamed-chunk-12-1.png" width="960" />

{{% notice ref %}}
- Watanabe, Kohei. 2018. "Newsmap: A semi-supervised approach to geographical news classification." _Digital Journalism_ 6(3): 294-309.
{{% /notice %}}
