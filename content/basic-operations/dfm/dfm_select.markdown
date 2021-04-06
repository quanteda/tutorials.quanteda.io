---
title: Select features
weight: 20
draft: false
---


```r
require(quanteda)
options(width = 110)
```


```r
toks_inaug <- tokens(data_corpus_inaugural, remove_punct = TRUE)
dfmat_inaug <- dfm(toks_inaug)
print(dfmat_inaug)
```

```
## Document-feature matrix of: 59 documents, 9,423 features (91.89% sparse) and 4 docvars.
##                  features
## docs              fellow-citizens  of the senate and house representatives among vicissitudes incident
##   1789-Washington               1  71 116      1  48     2               2     1            1        1
##   1793-Washington               0  11  13      0   2     0               0     0            0        0
##   1797-Adams                    3 140 163      1 130     0               2     4            0        0
##   1801-Jefferson                2 104 130      0  81     0               0     1            0        0
##   1805-Jefferson                0 101 143      0  93     0               0     7            0        0
##   1809-Madison                  1  69 104      0  43     0               0     0            0        0
## [ reached max_ndoc ... 53 more documents, reached max_nfeat ... 9,413 more features ]
```

You can select features from a DFM using `dfm_select()`.


```r
dfmat_inaug_nostop <- dfm_select(dfmat_inaug, pattern = stopwords("en"), selection = "remove")
print(dfmat_inaug_nostop)
```

```
## Document-feature matrix of: 59 documents, 9,285 features (92.70% sparse) and 4 docvars.
##                  features
## docs              fellow-citizens senate house representatives among vicissitudes incident life event filled
##   1789-Washington               1      1     2               2     1            1        1    1     2      1
##   1793-Washington               0      0     0               0     0            0        0    0     0      0
##   1797-Adams                    3      1     0               2     4            0        0    2     0      0
##   1801-Jefferson                2      0     0               0     1            0        0    1     0      0
##   1805-Jefferson                0      0     0               0     7            0        0    2     0      0
##   1809-Madison                  1      0     0               0     0            0        0    1     0      1
## [ reached max_ndoc ... 53 more documents, reached max_nfeat ... 9,275 more features ]
```

`dfm_remove()` is an alias to `dfm_select(selection = "remove")`. Therefore, the code above and below are equivalent.


```r
dfmat_inaug_nostop <- dfm_remove(dfmat_inaug, pattern = stopwords("en"))
print(dfmat_inaug_nostop)
```

```
## Document-feature matrix of: 59 documents, 9,285 features (92.70% sparse) and 4 docvars.
##                  features
## docs              fellow-citizens senate house representatives among vicissitudes incident life event filled
##   1789-Washington               1      1     2               2     1            1        1    1     2      1
##   1793-Washington               0      0     0               0     0            0        0    0     0      0
##   1797-Adams                    3      1     0               2     4            0        0    2     0      0
##   1801-Jefferson                2      0     0               0     1            0        0    1     0      0
##   1805-Jefferson                0      0     0               0     7            0        0    2     0      0
##   1809-Madison                  1      0     0               0     0            0        0    1     0      1
## [ reached max_ndoc ... 53 more documents, reached max_nfeat ... 9,275 more features ]
```

You can also select features based on the length of features. In the example below, we only keep features consisting of at least five characters.


```r
dfmat_inaug_long <- dfm_keep(dfmat_inaug, min_nchar = 5)
print(dfmat_inaug_long)
```

```
## Document-feature matrix of: 59 documents, 8,564 features (93.04% sparse) and 4 docvars.
##                  features
## docs              fellow-citizens senate house representatives among vicissitudes incident event could filled
##   1789-Washington               1      1     2               2     1            1        1     2     3      1
##   1793-Washington               0      0     0               0     0            0        0     0     0      0
##   1797-Adams                    3      1     0               2     4            0        0     0     1      0
##   1801-Jefferson                2      0     0               0     1            0        0     0     0      0
##   1805-Jefferson                0      0     0               0     7            0        0     0     2      0
##   1809-Madison                  1      0     0               0     0            0        0     0     1      1
## [ reached max_ndoc ... 53 more documents, reached max_nfeat ... 8,554 more features ]
```

```r
topfeatures(dfmat_inaug_long, 10)
```

```
##      which      their     people government      great      those     states     should      world      shall 
##       1007        761        584        564        344        338        334        325        319        316
```

While `dfm_select()` selects features based on patterns, `dfm_trim()` does this based on feature frequencies. If `min_termfreq = 10`, features that occur less than 10 times in the corpus are removed.


```r
dfmat_inaug_freq <- dfm_trim(dfmat_inaug, min_termfreq = 10)
print(dfmat_inaug_freq)
```

```
## Document-feature matrix of: 59 documents, 1,523 features (68.92% sparse) and 4 docvars.
##                  features
## docs              fellow-citizens  of the senate and house representatives among to life
##   1789-Washington               1  71 116      1  48     2               2     1 48    1
##   1793-Washington               0  11  13      0   2     0               0     0  5    0
##   1797-Adams                    3 140 163      1 130     0               2     4 72    2
##   1801-Jefferson                2 104 130      0  81     0               0     1 61    1
##   1805-Jefferson                0 101 143      0  93     0               0     7 83    2
##   1809-Madison                  1  69 104      0  43     0               0     0 61    1
## [ reached max_ndoc ... 53 more documents, reached max_nfeat ... 1,513 more features ]
```

If `max_docfreq = 0.1`, features that occur in more than 10% of the documents are removed.


```r
dfmat_inaug_docfreq <- dfm_trim(dfmat_inaug, max_docfreq = 0.1, docfreq_type = "prop")
print(dfmat_inaug_docfreq)
```

```
## Document-feature matrix of: 59 documents, 7,392 features (96.87% sparse) and 4 docvars.
##                  features
## docs              vicissitudes filled anxieties notification transmitted 14th month summoned veneration
##   1789-Washington            1      1         1            1           1    1     1        1          1
##   1793-Washington            0      0         0            0           0    0     0        0          0
##   1797-Adams                 0      0         0            0           0    0     0        0          2
##   1801-Jefferson             0      0         0            0           0    0     0        0          0
##   1805-Jefferson             0      0         0            0           0    0     0        0          0
##   1809-Madison               0      1         0            0           0    0     0        0          0
##                  features
## docs              fondest
##   1789-Washington       1
##   1793-Washington       0
##   1797-Adams            0
##   1801-Jefferson        0
##   1805-Jefferson        0
##   1809-Madison          0
## [ reached max_ndoc ... 53 more documents, reached max_nfeat ... 7,382 more features ]
```
