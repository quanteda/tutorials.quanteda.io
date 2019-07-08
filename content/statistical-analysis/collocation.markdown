---
title: Collocation analysis
weight: 50
chapter: false
draft: false
---


```r
require(quanteda)
require(quanteda.corpora)
```

This corpus contains 6,000 Guardian news articles from 2012 to 2016.


```r
corp_news <- download('data_corpus_guardian')
```




By collocation analysis, we can identify contiguous collocations of words. One of the most common types of multi-word expressions are proper names, which can be identified simply based on capitalization in English texts.


```r
toks_news <- tokens(corp_news, remove_punct = TRUE)
tstat_col_caps <- tokens_select(toks_news, pattern = '^[A-Z]', 
                                valuetype = 'regex', 
                                case_insensitive = FALSE, 
                                padding = TRUE) %>% 
           textstat_collocations(min_count = 100)
head(tstat_col_caps, 20)
```

```
##           collocation count count_nested length    lambda         z
## 1       david cameron   861            0      2  8.159404 147.28035
## 2        donald trump   774            0      2  8.327022 122.69670
## 3      george osborne   364            0      2  8.660851 107.62169
## 4     hillary clinton   527            0      2  9.106770 102.39787
## 5            new york  1016            0      2 10.447921 100.26711
## 6       islamic state   330            0      2  9.802254  98.14615
## 7         white house   479            0      2  9.922039  96.17660
## 8      european union   351            0      2  8.261630  94.76272
## 9       jeremy corbyn   244            0      2  8.729600  90.79495
## 10      boris johnson   245            0      2  9.664232  84.76851
## 11     bernie sanders   394            0      2  9.901473  84.61829
## 12 guardian australia   237            0      2  6.327400  83.68386
## 13   northern ireland   205            0      2  9.883259  83.25818
## 14        home office   216            0      2  9.681956  78.79434
## 15        ed miliband   174            0      2  9.868134  78.44901
## 16           ted cruz   417            0      2 10.756440  77.87898
## 17       barack obama   344            0      2  9.775376  77.72772
## 18       south africa   172            0      2  7.568725  77.61284
## 19     south carolina   271            0      2  9.404828  76.80195
## 20       black friday   190            0      2  8.459065  76.80004
```

You can also discover collocations larger than two words.


```r
tstat_col2 <- tokens_select(toks_news, pattern = '^[A-Z]', 
                                valuetype = 'regex', 
                                case_insensitive = FALSE, 
                                padding = TRUE) %>% 
            textstat_collocations(min_count = 100, size = 3)
head(tstat_col2, 20)
```

```
##                   collocation count count_nested length     lambda
## 1 international monetary fund   101            0      3  2.2425556
## 2              new york times   128            0      3 -0.5072603
##            z
## 1  1.0791468
## 2 -0.3338532
```

{{% notice tip %}}
If you find `textstat_collocations()` is talking too much time, increase the `min_count` threashold to speed up the estimation. You also do not need to set `sizes` larger than 2 to compound multi-word expressions, because overlapped collocations are chained if `join = TRUE` in `tokens_compound()`.
{{% /notice %}}
