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
## 1       david cameron   861            0      2  8.186204 147.76424
## 2        donald trump   774            0      2  8.353825 123.09172
## 3      george osborne   364            0      2  8.687642 107.95464
## 4     hillary clinton   526            0      2  9.125667 102.77227
## 5            new york  1016            0      2 10.474717 100.52429
## 6       islamic state   330            0      2  9.829042  98.41439
## 7         white house   478            0      2  9.938211  96.55956
## 8      european union   351            0      2  8.288426  95.07011
## 9       jeremy corbyn   244            0      2  8.756390  91.07361
## 10      boris johnson   245            0      2  9.691020  85.00349
## 11     bernie sanders   394            0      2  9.938365  84.61889
## 12 guardian australia   237            0      2  6.354201  84.03836
## 13   northern ireland   204            0      2  9.895964  83.42198
## 14        home office   216            0      2  9.717364  78.88860
## 15        ed miliband   174            0      2  9.894920  78.66196
## 16           ted cruz   417            0      2 10.783229  78.07295
## 17       barack obama   344            0      2  9.802167  77.94076
## 18       south africa   172            0      2  7.595517  77.88760
## 19       black friday   190            0      2  8.485856  77.04329
## 20     south carolina   271            0      2  9.431619  77.02074
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
##                   collocation count count_nested length     lambda          z
## 1 international monetary fund   101            0      3  2.2157340  1.0662399
## 2              new york times   128            0      3 -0.5340926 -0.3515129
```

{{% notice tip %}}
If you find `textstat_collocations()` is talking too much time, increase the `min_count` threashold to speed up the estimation. You also do not need to set `sizes` larger than 2 to compound multi-word expressions, because overlapped collocations are chained if `join = TRUE` in `tokens_compound()`.
{{% /notice %}}
