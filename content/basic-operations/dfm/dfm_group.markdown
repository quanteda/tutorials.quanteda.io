---
title: Group documents
weight: 40
draft: false
---


```r
require(quanteda)
options(width = 110)
```


```r
toks_inaug <- tokens(data_corpus_inaugural)
dfmat_inaug <- dfm(toks_inaug)
print(dfmat_inaug)
```

```
## Document-feature matrix of: 59 documents, 9,439 features (91.84% sparse) and 4 docvars.
##                  features
## docs              fellow-citizens  of the senate and house representatives : among vicissitudes
##   1789-Washington               1  71 116      1  48     2               2 1     1            1
##   1793-Washington               0  11  13      0   2     0               0 1     0            0
##   1797-Adams                    3 140 163      1 130     0               2 0     4            0
##   1801-Jefferson                2 104 130      0  81     0               0 1     1            0
##   1805-Jefferson                0 101 143      0  93     0               0 0     7            0
##   1809-Madison                  1  69 104      0  43     0               0 0     0            0
## [ reached max_ndoc ... 53 more documents, reached max_nfeat ... 9,429 more features ]
```

```r
head(colSums(dfmat_inaug), 10)
```

```
## fellow-citizens              of             the          senate             and           house 
##              39            7180           10183              15            5406              11 
## representatives               :           among    vicissitudes 
##              19             144             108               5
```

`dfm_group()` merges documents based on a vector given to the `groups` argument. In grouping documents, it takes the sums of feature frequencies. 


```r
dfmat_party <- dfm_group(dfmat_inaug, groups = Party)
print(dfmat_party)
```

```
## Document-feature matrix of: 6 documents, 9,439 features (66.93% sparse) and 1 docvar.
##                        features
## docs                    fellow-citizens   of  the senate  and house representatives  : among vicissitudes
##   Democratic                          3 1994 2742      2 1728     4               3 54    25            3
##   Democratic-Republican              10  945 1416      0  640     0               2  1    16            1
##   Federalist                          3  140  163      1  130     0               2  0     4            0
##   none                                1   82  129      1   50     2               2  2     1            1
##   Republican                          9 3055 4408      5 2386     4               6 86    52            0
##   Whig                               13  964 1325      6  472     1               4  1    10            0
## [ reached max_nfeat ... 9,429 more features ]
```

```r
head(colSums(dfmat_party), 10)
```

```
## fellow-citizens              of             the          senate             and           house 
##              39            7180           10183              15            5406              11 
## representatives               :           among    vicissitudes 
##              19             144             108               5
```

{{% notice note %}}
From **quanteda** package version 3.0 onwards, `dfm_group()` supports non-standard evaluation. This means that the name of the grouping variable should not be quoted by `"`.
{{% /notice %}}

`dfm_group()` identifies document-level variables that are the same within groups and keeps these variables.


```r
docvars(dfmat_party)
```

```
##                   Party
## 1            Democratic
## 2 Democratic-Republican
## 3            Federalist
## 4                  none
## 5            Republican
## 6                  Whig
```
