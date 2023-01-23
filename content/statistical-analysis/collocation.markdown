---
title: Collocation analysis
weight: 50
chapter: false
draft: false
---


```r
require(quanteda)
require(quanteda.textstats)
require(quanteda.corpora)
```

This corpus contains 6,000 Guardian news articles from 2012 to 2016.


```r
corp_news <- download("data_corpus_guardian")
```




A collocation analysis allows us to identify contiguous collocations of words. One of the most common types of multi-word expressions are proper names, which can be identified simply based on capitalization in English texts.


```r
toks_news <- tokens(corp_news, remove_punct = TRUE)
tstat_col_caps <- tokens_select(toks_news, pattern = "^[A-Z]", 
                                valuetype = "regex", 
                                case_insensitive = FALSE, 
                                padding = TRUE) %>% 
                  textstat_collocations(min_count = 100)
head(tstat_col_caps, 20)
```

```
##           collocation count count_nested length    lambda         z
## 1       david cameron   861            0      2  8.160098 147.29289
## 2        donald trump   774            0      2  8.327716 122.70693
## 3      george osborne   364            0      2  8.661545 107.63031
## 4     hillary clinton   527            0      2  9.107464 102.40567
## 5            new york  1016            0      2 10.448615 100.27377
## 6       islamic state   330            0      2  9.802948  98.15309
## 7         white house   479            0      2  9.922733  96.18333
## 8      european union   351            0      2  8.262324  94.77068
## 9       jeremy corbyn   244            0      2  8.730294  90.80217
## 10      boris johnson   245            0      2  9.664926  84.77459
## 11     bernie sanders   394            0      2  9.902167  84.62422
## 12 guardian australia   237            0      2  6.330225  83.71292
## 13   northern ireland   205            0      2  9.883953  83.26403
## 14        home office   216            0      2  9.691271  78.67675
## 15        ed miliband   174            0      2  9.868828  78.45452
## 16           ted cruz   417            0      2 10.757134  77.88400
## 17       barack obama   344            0      2  9.776070  77.73324
## 18       south africa   172            0      2  7.569419  77.61996
## 19     south carolina   271            0      2  9.405522  76.80761
## 20       black friday   190            0      2  8.459759  76.80634
```

You can also discover collocations longer than two words. In the example below we identify collocations consisting of three words.


```r
tstat_col2 <- tokens_select(toks_news, pattern = "^[A-Z]", 
                                valuetype = "regex", 
                                case_insensitive = FALSE, 
                                padding = TRUE) %>% 
              textstat_collocations(min_count = 100, size = 3)
head(tstat_col2, 20)
```

```
##                   collocation count count_nested length     lambda          z
## 1 international monetary fund   101            0      3  2.2418609  1.0788125
## 2              new york times   128            0      3 -0.5079552 -0.3343106
```

{{% notice tip %}}
If you find `textstat_collocations()` is taking too much time, increase the `min_count` threshold to speed up the estimation. You also do not need to set `sizes` larger than 2 to compound multi-word expressions, because overlapped collocations are chained if `join = TRUE` in `tokens_compound()`.
{{% /notice %}}
