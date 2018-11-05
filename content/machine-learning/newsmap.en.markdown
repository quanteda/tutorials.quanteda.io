---
title: Newsmap
weight: 60
draft: false
---

Newsmap is a semi-supervised model for geographical document classification. While (full) supervised models are trained on manually classified data, this semi-supervised model learn from "seed words" in dictionaries. 


```r
require(quanteda)
require(quanteda.corpora)
require(newsmap)
require(maps)
require(ggplot2)
```


```r
rss_corp <- download(url = 'https://www.dropbox.com/s/r8zhsu8zvjzhnml/data_corpus_yahoonews.rds?dl=1')
```



`rss_corp` contains 10,000 news summaries downloaded from Yahoo News in 2014.


```r
ndoc(rss_corp)
```

```
## [1] 10000
```

```r
range(docvars(rss_corp, "date"))
```

```
## [1] "2014-01-01" "2014-12-31"
```

In geographical classification, proper nouns are the most useful features of documents, but not all capitalized words are proper nouns, so we define custom stopwords


```r
month <- c('January', 'February', 'March', 'April', 'May', 'June',
           'July', 'August', 'September', 'October', 'November', 'December')
day <- c('Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday')
agency <- c('AP', 'AFP', 'Reuters')
```


```r
toks <- tokens(rss_corp)
toks <- tokens_remove(toks, stopwords('english'), valuetype = 'fixed', padding = TRUE)
toks <- tokens_remove(toks, c(month, day, agency), valuetype = 'fixed', padding = TRUE)
```

The **newsmap** package contains [seed geographical dictionaries](https://github.com/koheiw/newsmap/tree/master/dict) in English, German, Spanish, Japanese and Russian languages. `data_dictionary_newsmap_en` is the seed dictionary for English texts.


```r
label_toks <- tokens_lookup(toks, data_dictionary_newsmap_en, levels = 3) # level 3 is countries
label_dfm <- dfm(label_toks, tolower = FALSE)

feat_dfm <- dfm(toks, tolower = FALSE)
feat_dfm <- dfm_select(feat_dfm, "^[A-Z][A-Za-z1-2]+", valuetype = 'regex', case_insensitive = FALSE)
feat_dfm <- dfm_trim(feat_dfm, min_termfreq = 10)

model <- textmodel_newsmap(feat_dfm, label_dfm)
```

The seed dictionary contains only names of countries and capital cities, but the model additional extracts features associated to the countries.


```r
coef(model, n = 10)[c("US", "GB", "FR", "BR", "JP")]
```

```
## $US
## WASHINGTON         US   American Washington       YORK     States 
##   7.153905   7.036452   6.829497   6.605441   6.369661   6.054237 
##  Americans       York Brunnstrom      Kirby 
##   5.359504   4.992954   3.781319   3.701276 
## 
## $GB
##   British    LONDON    London   Britain Britain's        UK      UKIP 
##  7.878120  7.848022  7.564036  7.265223  6.671448  5.488278  4.906357 
##   Kingdom     Tesco     Hamza 
##  4.775328  4.446824  4.359813 
## 
## $FR
##        French        France         PARIS         Paris      Hollande 
##      8.185413      8.091340      7.543559      7.305600      6.534895 
##    Hollande's        Fabius         Valls      Francois Saint-Germain 
##      5.403493      5.298132      5.298132      5.279440      4.972710 
## 
## $BR
##    Brazil       SAO     PAULO       RIO   JANEIRO Brazilian       Rio 
##  8.175273  7.261726  7.247932  7.048804  7.048804  6.996618  6.922510 
##        DE   Janeiro       Sao 
##  6.355656  6.303471  5.966998 
## 
## $JP
##     Japan  Japanese     TOKYO       Abe     Tokyo    Shinzo     Abe's 
##  8.191764  7.895498  7.762951  7.061899  6.961815  6.789965  5.809136 
##   Tokyo's Fukushima   Japan's 
##  5.316659  5.170056  4.389616
```

{{% notice tip %}}
Names of people, organizations and places nouns are often multi-word expressions. To distiguish between "New York" and "York", for example, it is useful to compound tokens using `tokens_compound()` as explained in [Advanced Operations](../advanced-operations/compound-mutiword-expressions/).
{{% /notice %}}

You can predict most strongly associated countries using `predidct()` and count the frequency using `table()`. Factor levels are set to obtain zero counts for countries that did not appear in the corpus.


```r
pred <- predict(model)
head(pred, 20)
```

```
##  text1  text2  text3  text4  text5  text6  text7  text8  text9 text10 
##   "KP"   "SY"   "IQ"   "RU"   "TH"   "CN"   "UA"   "SY"   "GB"   "US" 
## text11 text12 text13 text14 text15 text16 text17 text18 text19 text20 
##   "SY"   "US"   "UA"   "SY"   "LK"   "ES"   "AU"   "CR"   "ID"   "BH"
```

```r
count <- sort(table(factor(pred, levels = colnames(label_dfm))), decreasing = TRUE)
head(count, 20)
```

```
## 
##  GB  US  RU  UA  AU  CN  CA  FR  IQ  BR  SY  DE  ZA  NZ  JP  IL  IN  ES 
## 622 578 516 439 367 363 319 312 295 278 260 251 237 227 197 197 187 182 
##  EG  PS 
## 157 155
```

You can visualize the distribution of global news attention using `geom_map()`.


```r
data_country <- as.data.frame(count, stringsAsFactors = FALSE)
colnames(data_country) <- c("id", "frequency")

world_map <- map_data(map = "world")
world_map$region <- iso.alpha(world_map$region) # convert contry name to ISO code

ggplot(data_country, aes(map_id = id)) +
      geom_map(aes(fill = frequency), map = world_map) +
      expand_limits(x = world_map$long, y = world_map$lat) +
      scale_fill_continuous(name = "Frequency") +
      theme_void() +
      coord_fixed()
```

<img src="/machine-learning/newsmap.en_files/figure-html/unnamed-chunk-10-1.png" width="960" />

{{% notice info %}}
If you want to learn more about Newsmap, see:  
Watanabe, Kohei, 2018, Newsmap: A semi-supervised approach to geographical news classification, _Digital Journalism_ 6 (3): 294-309.
{{% /notice %}}
