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
dfmat_irish <- dfm(data_corpus_irishbudget2010, remove_punct = TRUE)
print(dfmat_irish)
```

```
## Document-feature matrix of: 14 documents, 5,129 features (81.3% sparse) and 6 docvars.
##                       features
## docs                   when  i presented the supplementary budget  to this house last
##   Lenihan, Brian (FF)     5 73         1 539             7     23 305   99    10    6
##   Bruton, Richard (FG)    2  6         0 305             0     27 172   55     0    5
##   Burton, Joan (LAB)     11 40         0 428             0     37 157   53     6    4
##   Morgan, Arthur (SF)    21 26         0 501             1     26 204   85     5    4
##   Cowen, Brian (FF)       4 17         0 394             0     21 209   43     4    6
##   Kenny, Enda (FG)       12 25         1 304             1     23 119   47     0    4
## [ reached max_ndoc ... 8 more documents, reached max_nfeat ... 5,119 more features ]
```

You can select features from a DFM using `dfm_select()`.


```r
dfmat_irish_nostop <- dfm_select(dfmat_irish, pattern = stopwords('en'), selection = 'remove')
print(dfmat_irish_nostop)
```

```
## Document-feature matrix of: 14 documents, 5,010 features (82.7% sparse) and 6 docvars.
##                       features
## docs                   presented supplementary budget house last april said work way period
##   Lenihan, Brian (FF)          1             7     23    10    6     1    1   13  12      2
##   Bruton, Richard (FG)         0             0     27     0    5     1    0    4   8      0
##   Burton, Joan (LAB)           0             0     37     6    4     0    4    1   5      2
##   Morgan, Arthur (SF)          0             1     26     5    4     3    0    4  13      3
##   Cowen, Brian (FF)            0             0     21     4    6     0    0    7  10      4
##   Kenny, Enda (FG)             1             1     23     0    4     4    4    6   8      1
## [ reached max_ndoc ... 8 more documents, reached max_nfeat ... 5,000 more features ]
```

`dfm_remove()` is an alias to `dfm_select(selection = 'remove')`. Therefore, the code above and below are equivalent.


```r
dfmat_irish_nostop <- dfm_remove(dfmat_irish, pattern = stopwords('en'))
print(dfmat_irish_nostop)
```

```
## Document-feature matrix of: 14 documents, 5,010 features (82.7% sparse) and 6 docvars.
##                       features
## docs                   presented supplementary budget house last april said work way period
##   Lenihan, Brian (FF)          1             7     23    10    6     1    1   13  12      2
##   Bruton, Richard (FG)         0             0     27     0    5     1    0    4   8      0
##   Burton, Joan (LAB)           0             0     37     6    4     0    4    1   5      2
##   Morgan, Arthur (SF)          0             1     26     5    4     3    0    4  13      3
##   Cowen, Brian (FF)            0             0     21     4    6     0    0    7  10      4
##   Kenny, Enda (FG)             1             1     23     0    4     4    4    6   8      1
## [ reached max_ndoc ... 8 more documents, reached max_nfeat ... 5,000 more features ]
```

You can also select features based on the length of features. 


```r
dfmat_irish_long <- dfm_select(dfmat_irish, min_nchar = 5)
print(dfmat_irish_long)
```

```
## Document-feature matrix of: 14 documents, 4,275 features (83.4% sparse) and 6 docvars.
##                       features
## docs                   presented supplementary budget house april could through period severe economic
##   Lenihan, Brian (FF)          1             7     23    10     1     1       9      2      3       17
##   Bruton, Richard (FG)         0             0     27     0     1    10       0      0      0        9
##   Burton, Joan (LAB)           0             0     37     6     0     9       4      2      0        8
##   Morgan, Arthur (SF)          0             1     26     5     3    10       6      3      0       15
##   Cowen, Brian (FF)            0             0     21     4     0     3      14      4      1       16
##   Kenny, Enda (FG)             1             1     23     0     4     8       3      1      0        6
## [ reached max_ndoc ... 8 more documents, reached max_nfeat ... 4,265 more features ]
```

```r
topfeatures(dfmat_irish_long, 10)
```

```
##     people     budget government     public   minister    economy      those      which      there      their 
##        266        260        236        179        170        160        160        156        147        144
```

While `dfm_select()` selects features based on patterns, `dfm_trim()` does this based on feature frequencies. If `min_termfreq = 10`, features that occur less than 10 times in the corpus are removed.


```r
dfmat_irish_freq <- dfm_trim(dfmat_irish, min_termfreq = 10)
print(dfmat_irish_freq)
```

```
## Document-feature matrix of: 14 documents, 661 features (39.9% sparse) and 6 docvars.
##                       features
## docs                   when  i the supplementary budget  to this house last april
##   Lenihan, Brian (FF)     5 73 539             7     23 305   99    10    6     1
##   Bruton, Richard (FG)    2  6 305             0     27 172   55     0    5     1
##   Burton, Joan (LAB)     11 40 428             0     37 157   53     6    4     0
##   Morgan, Arthur (SF)    21 26 501             1     26 204   85     5    4     3
##   Cowen, Brian (FF)       4 17 394             0     21 209   43     4    6     0
##   Kenny, Enda (FG)       12 25 304             1     23 119   47     0    4     4
## [ reached max_ndoc ... 8 more documents, reached max_nfeat ... 651 more features ]
```

If `max_docfreq = 0.1`, features that occur in more than 10% of the documents are removed.


```r
dfmat_irish_docfreq <- dfm_trim(dfmat_irish, max_docfreq = 0.1, docfreq_type = "prop")
print(dfmat_irish_docfreq)
```

```
## Document-feature matrix of: 14 documents, 2,714 features (92.9% sparse) and 6 docvars.
##                       features
## docs                   distress notwithstanding eight sends weakened condition self-confidence shaken concern
##   Lenihan, Brian (FF)         1               1     1     2        1         1               2      1       3
##   Bruton, Richard (FG)        0               0     0     0        0         0               0      0       0
##   Burton, Joan (LAB)          0               0     0     0        0         0               0      0       0
##   Morgan, Arthur (SF)         0               0     0     0        0         0               0      0       0
##   Cowen, Brian (FF)           0               0     0     0        0         0               0      0       0
##   Kenny, Enda (FG)            0               0     0     0        0         0               0      0       0
##                       features
## docs                   functioning
##   Lenihan, Brian (FF)            1
##   Bruton, Richard (FG)           0
##   Burton, Joan (LAB)             0
##   Morgan, Arthur (SF)            0
##   Cowen, Brian (FF)              0
##   Kenny, Enda (FG)               0
## [ reached max_ndoc ... 8 more documents, reached max_nfeat ... 2,704 more features ]
```
