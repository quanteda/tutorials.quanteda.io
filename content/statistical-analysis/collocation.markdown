---
title: Collocation analysis
weight: 50
chapter: false
draft: false
---


```r
require(quanteda)
```

By collocation analysis, we can identify contiguous collocations of words. One of the most common type of multi-word expressions is proper names, which can be identified simply based on capitalization in English texts.


```r
news_corp <- quanteda.corpora::download('data_corpus_guardian')
news_toks <- tokens(news_corp, remove_punct = TRUE)
cap_col <- tokens_select(news_toks, '^[A-Z]', valuetype = 'regex', case_insensitive = FALSE, padding = TRUE) %>% 
           textstat_collocations(min_count = 5)
head(cap_col, 20)
```

```
##           collocation count count_nested length    lambda         z
## 1       david cameron   861            0      2  8.159403 147.28033
## 2        donald trump   774            0      2  8.327021 122.69669
## 3      george osborne   364            0      2  8.660850 107.62167
## 4     hillary clinton   527            0      2  9.106769 102.39786
## 5            new york  1016            0      2 10.447920 100.26710
## 6       islamic state   330            0      2  9.802253  98.14614
## 7         white house   479            0      2  9.922038  96.17659
## 8      european union   351            0      2  8.261629  94.76270
## 9       jeremy corbyn   244            0      2  8.729599  90.79494
## 10      boris johnson   245            0      2  9.664231  84.76850
## 11     bernie sanders   394            0      2  9.901472  84.61828
## 12 guardian australia   237            0      2  6.327399  83.68384
## 13   northern ireland   205            0      2  9.883258  83.25817
## 14        home office   216            0      2  9.681955  78.79433
## 15        ed miliband   174            0      2  9.868133  78.44900
## 16           ted cruz   417            0      2 10.756439  77.87897
## 17       barack obama   344            0      2  9.775375  77.72771
## 18       south africa   172            0      2  7.568724  77.61283
## 19     south carolina   271            0      2  9.404827  76.80194
## 20       black friday   190            0      2  8.459064  76.80003
```
