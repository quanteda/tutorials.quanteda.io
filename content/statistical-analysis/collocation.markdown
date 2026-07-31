---
title: Collocation analysis
weight: 50
chapter: false
draft: false
---

You met `textstat_collocations()` briefly in [Advanced Operations](/advanced-operations/), where it was used to compound multi-word expressions automatically. Here we look at the function itself in more detail, since it is one of the most generally useful tools for exploring a new corpus.


``` r
library(quanteda)
library(quanteda.textstats)
library(quanteda.corpora)
```

The corpus contains 6,000 Guardian news articles from 2012 to 2016.


``` r
corp_news <- download("data_corpus_guardian")
```



A collocation analysis identifies contiguous sequences of words, "collocations", that occur together far more often than chance would predict. One of the most common types of multi-word expression is proper names, which can be identified based on capitalisation in English texts. Setting `min_count = 50` restricts the results to collocations that occur at least fifty times, which keeps the output manageable and filters out one-off coincidences.


``` r
toks_news <- tokens(corp_news, remove_punct = TRUE)
tstat_col_caps <- tokens_select(toks_news, pattern = "^[A-Z]", 
                                valuetype = "regex", 
                                case_insensitive = FALSE, 
                                padding = TRUE) |> 
                  textstat_collocations(min_count = 50)

# print output
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

Each row is a candidate two-word collocation. The `count` column shows how often it occurred. The `lambda` and `z` columns measure how strongly the two words are associated, with higher values indicating a stronger association. By default, `textstat_collocations()` looks for collocations of exactly two words, but you can also discover longer collocations. In the example below we identify collocations consisting of three words, using the `size` argument.


``` r
tstat_col2 <- tokens_select(toks_news, pattern = "^[A-Z]", 
                                valuetype = "regex", 
                                case_insensitive = FALSE, 
                                padding = TRUE) |> 
              textstat_collocations(min_count = 3, size = 3)

# print output
head(tstat_col2, 20)
```

```
##                 collocation count count_nested length   lambda        z
## 1    photograph guardian in     4            0      3 5.008890 5.385860
## 2              UK london UK     3            0      3 8.902310 5.256421
## 3             labour in for     4            0      3 8.584031 4.920076
## 4                  BST a UK     4            0      3 4.087041 4.834290
## 5             BST clinton i     4            0      3 3.496041 4.770604
## 6         the great british     5            0      3 3.363887 4.635129
## 7    the american president     4            0      3 4.744033 4.591031
## 8            in for britain     4            0      3 7.414830 4.478423
## 9             the new south    14            0      3 3.906304 4.448319
## 10          new south wales    85            0      3 8.939543 4.403684
## 11    european central bank    95            0      3 4.431117 4.335124
## 12      january but cameron     3            0      3 4.653571 4.327389
## 13           donald j trump    54            0      3 7.448472 4.314147
## 14     the south australian     6            0      3 2.374545 4.008380
## 15            related why i     4            0      3 3.564346 3.903065
## 16            EU related EU     3            0      3 3.333649 3.889284
## 17  national australia bank     8            0      3 6.618000 3.872595
## 18          paris the paris     3            0      3 3.664042 3.539989
## 19 the wisconsin republican     3            0      3 3.829913 3.507866
## 20          york way london    32            0      3 6.463133 3.507585
```

Once you have identified collocations you trust, the natural next step is to compound them into single tokens with `tokens_compound()`, exactly as demonstrated in [Advanced Operations](/advanced-operations/).

{{% notice tip %}}
If you find `textstat_collocations()` is taking too much time, increase the `min_count` threshold to speed up the estimation. You also do not need to set `sizes` larger than 2 to compound multi-word expressions, because overlapped collocations are chained if `join = TRUE` in `tokens_compound()`.
{{% /notice %}}
