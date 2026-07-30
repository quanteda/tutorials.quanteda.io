---
title: Collocation analysis
weight: 50
chapter: false
draft: false
---


``` r
library(quanteda)
library(quanteda.textstats)
library(quanteda.corpora)
```

This corpus contains 6,000 Guardian news articles from 2012 to 2016.


``` r
corp_news <- download("data_corpus_guardian")
```




A collocation analysis allows us to identify contiguous collocations of words. One of the most common types of multi-word expressions are proper names, which can be identified simply based on capitalization in English texts.


``` r
toks_news <- tokens(corp_news, remove_punct = TRUE)
tstat_col_caps <- tokens_select(toks_news, pattern = "^[A-Z]", 
                                valuetype = "regex", 
                                case_insensitive = FALSE, 
                                padding = TRUE) |> 
                  textstat_collocations(min_count = 100)
head(tstat_col_caps, 20)
```

```
##           collocation count count_nested length    lambda         z
## 1       david cameron   861            0      2  8.160196 147.29466
## 2        donald trump   774            0      2  8.327814 122.70838
## 3      george osborne   364            0      2  8.661643 107.63153
## 4     hillary clinton   527            0      2  9.107562 102.40678
## 5            new york  1016            0      2 10.448713 100.27471
## 6       islamic state   330            0      2  9.803046  98.15407
## 7         white house   479            0      2  9.922831  96.18428
## 8      european union   351            0      2  8.262422  94.77180
## 9       jeremy corbyn   244            0      2  8.730392  90.80319
## 10      boris johnson   245            0      2  9.665024  84.77545
## 11     bernie sanders   394            0      2  9.902265  84.62506
## 12 guardian australia   237            0      2  6.330323  83.71422
## 13   northern ireland   205            0      2  9.884051  83.26485
## 14        home office   216            0      2  9.691369  78.67755
## 15        ed miliband   174            0      2  9.868926  78.45530
## 16           ted cruz   417            0      2 10.757232  77.88471
## 17       barack obama   344            0      2  9.776168  77.73402
## 18       south africa   172            0      2  7.569517  77.62096
## 19     south carolina   271            0      2  9.405620  76.80841
## 20       black friday   190            0      2  8.459857  76.80723
```

You can also discover collocations longer than two words. In the example below we identify collocations consisting of three words.


``` r
tstat_col2 <- tokens_select(toks_news, pattern = "^[A-Z]", 
                                valuetype = "regex", 
                                case_insensitive = FALSE, 
                                padding = TRUE) |> 
              textstat_collocations(min_count = 100, size = 3)
head(tstat_col2, 20)
```

```
##                   collocation count count_nested length     lambda          z
## 1 international monetary fund   101            0      3  2.2417628  1.0787653
## 2              new york times   128            0      3 -0.5080533 -0.3343752
```

{{% notice tip %}}
If you find `textstat_collocations()` is taking too much time, increase the `min_count` threshold to speed up the estimation. You also do not need to set `sizes` larger than 2 to compound multi-word expressions, because overlapped collocations are chained if `join = TRUE` in `tokens_compound()`.
{{% /notice %}}
