---
title: Select features
weight: 20
draft: false
---

You already used `tokens_select()` to remove stopwords from a tokens object. `dfm_select()` does the same job on a document-feature matrix instead, by pattern-matching against feature (column) names rather than tokens.


``` r
library(quanteda)
```




``` r
toks_inaug <- tokens(data_corpus_inaugural, remove_punct = TRUE)
dfmat_inaug <- dfm(toks_inaug)
print(dfmat_inaug)
```

```
## Document-feature matrix of: 60 documents, 9,573 features (91.99% sparse) and 4 docvars.
##                  features
## docs              fellow-citizens  of the senate and house representatives among vicissitudes incident
##   1789-Washington               1  71 116      1  48     2               2     1            1        1
##   1793-Washington               0  11  13      0   2     0               0     0            0        0
##   1797-Adams                    3 140 163      1 130     0               2     4            0        0
##   1801-Jefferson                2 104 130      0  81     0               0     1            0        0
##   1805-Jefferson                0 101 143      0  93     0               0     7            0        0
##   1809-Madison                  1  69 104      0  43     0               0     0            0        0
## [ reached max_ndoc ... 54 more documents, reached max_nfeat ... 9,563 more features ]
```


``` r
dfmat_inaug_nostop <- dfm_select(dfmat_inaug, pattern = stopwords("en"), selection = "remove")
print(dfmat_inaug_nostop)
```

```
## Document-feature matrix of: 60 documents, 9,435 features (92.79% sparse) and 4 docvars.
##                  features
## docs              fellow-citizens senate house representatives among vicissitudes incident life event filled
##   1789-Washington               1      1     2               2     1            1        1    1     2      1
##   1793-Washington               0      0     0               0     0            0        0    0     0      0
##   1797-Adams                    3      1     0               2     4            0        0    2     0      0
##   1801-Jefferson                2      0     0               0     1            0        0    1     0      0
##   1805-Jefferson                0      0     0               0     7            0        0    2     0      0
##   1809-Madison                  1      0     0               0     0            0        0    1     0      1
## [ reached max_ndoc ... 54 more documents, reached max_nfeat ... 9,425 more features ]
```

As with tokens, `dfm_remove()` is a shortcut alias for `dfm_select(selection = "remove")`. The code above and below are equivalent, and you can see the feature count drop identically in both.


``` r
dfmat_inaug_nostop <- dfm_remove(dfmat_inaug, pattern = stopwords("en"))
print(dfmat_inaug_nostop)
```

```
## Document-feature matrix of: 60 documents, 9,435 features (92.79% sparse) and 4 docvars.
##                  features
## docs              fellow-citizens senate house representatives among vicissitudes incident life event filled
##   1789-Washington               1      1     2               2     1            1        1    1     2      1
##   1793-Washington               0      0     0               0     0            0        0    0     0      0
##   1797-Adams                    3      1     0               2     4            0        0    2     0      0
##   1801-Jefferson                2      0     0               0     1            0        0    1     0      0
##   1805-Jefferson                0      0     0               0     7            0        0    2     0      0
##   1809-Madison                  1      0     0               0     0            0        0    1     0      1
## [ reached max_ndoc ... 54 more documents, reached max_nfeat ... 9,425 more features ]
```

You can also select features based on their length rather than matching them against a specific list of words. In the example below, we only keep features that consist of at least five characters, which is a quick way of filtering out short function words without needing a stopword list at all.


``` r
dfmat_inaug_long <- dfm_keep(dfmat_inaug, min_nchar = 5)
print(dfmat_inaug_long)
```

```
## Document-feature matrix of: 60 documents, 8,695 features (93.12% sparse) and 4 docvars.
##                  features
## docs              fellow-citizens senate house representatives among vicissitudes incident event could filled
##   1789-Washington               1      1     2               2     1            1        1     2     3      1
##   1793-Washington               0      0     0               0     0            0        0     0     0      0
##   1797-Adams                    3      1     0               2     4            0        0     0     1      0
##   1801-Jefferson                2      0     0               0     1            0        0     0     0      0
##   1805-Jefferson                0      0     0               0     7            0        0     0     2      0
##   1809-Madison                  1      0     0               0     0            0        0     0     1      1
## [ reached max_ndoc ... 54 more documents, reached max_nfeat ... 8,685 more features ]
```

``` r
topfeatures(dfmat_inaug_long, 10)
```

```
##      which      their     people government      great     states      those      world     should     nation 
##       1009        773        592        575        354        343        339        329        328        324
```

While `dfm_select()` and its relatives choose features based on patterns or length, `dfm_trim()` chooses them based on how often they occur. Use it to cut extremely rare features, which add noise and computational cost without adding much information. If `min_termfreq = 10`, features that occur fewer than ten times across the whole corpus are removed.


``` r
dfmat_inaug_freq <- dfm_trim(dfmat_inaug, min_termfreq = 10)
print(dfmat_inaug_freq)
```

```
## Document-feature matrix of: 60 documents, 1,551 features (69.26% sparse) and 4 docvars.
##                  features
## docs              fellow-citizens  of the senate and house representatives among to life
##   1789-Washington               1  71 116      1  48     2               2     1 48    1
##   1793-Washington               0  11  13      0   2     0               0     0  5    0
##   1797-Adams                    3 140 163      1 130     0               2     4 72    2
##   1801-Jefferson                2 104 130      0  81     0               0     1 61    1
##   1805-Jefferson                0 101 143      0  93     0               0     7 83    2
##   1809-Madison                  1  69 104      0  43     0               0     0 61    1
## [ reached max_ndoc ... 54 more documents, reached max_nfeat ... 1,541 more features ]
```

You can trim from the other direction too. If `max_docfreq = 0.1`, features that occur in more than 10% of documents are removed. That's useful for stripping out words so common across your corpus that they no longer help distinguish one document from another.


``` r
dfmat_inaug_docfreq <- dfm_trim(dfmat_inaug, max_docfreq = 0.1, docfreq_type = "prop")
print(dfmat_inaug_docfreq)
```

```
## Document-feature matrix of: 60 documents, 7,800 features (96.66% sparse) and 4 docvars.
##                  features
## docs              vicissitudes incident filled anxieties notification transmitted 14th month summoned
##   1789-Washington            1        1      1         1            1           1    1     1        1
##   1793-Washington            0        0      0         0            0           0    0     0        0
##   1797-Adams                 0        0      0         0            0           0    0     0        0
##   1801-Jefferson             0        0      0         0            0           0    0     0        0
##   1805-Jefferson             0        0      0         0            0           0    0     0        0
##   1809-Madison               0        0      1         0            0           0    0     0        0
##                  features
## docs              veneration
##   1789-Washington          1
##   1793-Washington          0
##   1797-Adams               2
##   1801-Jefferson           0
##   1805-Jefferson           0
##   1809-Madison             0
## [ reached max_ndoc ... 54 more documents, reached max_nfeat ... 7,790 more features ]
```

{{% notice tip %}}
Every filtering choice here, which stopword list, which length or frequency cutoff, is a preprocessing decision, and these can change downstream results more than you might expect. See the note on this in the [Tokens chapter](/basic-operations/tokens/)'s [Select tokens](/basic-operations/tokens/tokens_select) page for more on why this is worth checking rather than assuming.
{{% /notice %}}
