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
## 1       david cameron   861            0      2  8.186242 147.76492
## 2        donald trump   774            0      2  8.353863 123.09227
## 3      george osborne   364            0      2  8.687680 107.95511
## 4     hillary clinton   526            0      2  9.125705 102.77269
## 5            new york  1016            0      2 10.474755 100.52465
## 6       islamic state   330            0      2  9.829080  98.41476
## 7         white house   478            0      2  9.938249  96.55993
## 8      european union   351            0      2  8.288463  95.07054
## 9       jeremy corbyn   244            0      2  8.756427  91.07400
## 10      boris johnson   245            0      2  9.691057  85.00382
## 11     bernie sanders   394            0      2  9.928301  84.84758
## 12 guardian australia   237            0      2  6.356369  84.05870
## 13   northern ireland   204            0      2  9.896001  83.42229
## 14        home office   216            0      2  9.717402  78.88891
## 15        ed miliband   174            0      2  9.894957  78.66226
## 16           ted cruz   417            0      2 10.783266  78.07322
## 17       barack obama   344            0      2  9.802205  77.94106
## 18       south africa   172            0      2  7.595554  77.88799
## 19       black friday   190            0      2  8.485894  77.04363
## 20     south carolina   271            0      2  9.431656  77.02105
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
## 1 international monetary fund   101            0      3  2.2156965  1.0662219
## 2              new york times   128            0      3 -0.5341301 -0.3515376
```

{{% notice tip %}}
If you find `textstat_collocations()` is talking too much time, increase the `min_count` threashold to speed up the estimation. You also do not need to set `sizes` larger than 2 to compound multi-word expressions, because overlapped collocations are chained if `join = TRUE` in `tokens_compound()`.
{{% /notice %}}
