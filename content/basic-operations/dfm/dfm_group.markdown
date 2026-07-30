---
title: Group documents
weight: 40
draft: false
---

`dfm_group()` merges several documents into one by adding up their feature counts, so that you can compare groups of documents, such as speeches from the same party, instead of comparing every document individually. We return to `data_corpus_inaugural`, the corpus of US presidential inaugural speeches used earlier in this chapter, and build a document-feature matrix from it in the usual way.


``` r
library(quanteda)
```




``` r
toks_inaug <- tokens(data_corpus_inaugural)
dfmat_inaug <- dfm(toks_inaug)
print(dfmat_inaug)
```

```
## Document-feature matrix of: 60 documents, 9,591 features (91.94% sparse) and 4 docvars.
##                  features
## docs              fellow-citizens  of the senate and house representatives : among vicissitudes
##   1789-Washington               1  71 116      1  48     2               2 1     1            1
##   1793-Washington               0  11  13      0   2     0               0 1     0            0
##   1797-Adams                    3 140 163      1 130     0               2 0     4            0
##   1801-Jefferson                2 104 130      0  81     0               0 1     1            0
##   1805-Jefferson                0 101 143      0  93     0               0 0     7            0
##   1809-Madison                  1  69 104      0  43     0               0 0     0            0
## [ reached max_ndoc ... 54 more documents, reached max_nfeat ... 9,581 more features ]
```

``` r
head(colSums(dfmat_inaug), 10)
```

```
## fellow-citizens              of             the          senate             and           house 
##              39            7271           10309              15            5550              11 
## representatives               :           among    vicissitudes 
##              19             148             108               5
```

Right now, every row of `dfmat_inaug` is a single speech, but you might instead want to compare, say, Democratic speeches against Republican ones as a whole, rather than president by president. `dfm_group()` merges documents together based on a variable passed to the `groups` argument, adding up the feature frequencies of every document that shares the same value.


``` r
dfmat_party <- dfm_group(dfmat_inaug, groups = Party)
print(dfmat_party)
```

```
## Document-feature matrix of: 6 documents, 9,591 features (67.07% sparse) and 1 docvar.
##                        features
## docs                    fellow-citizens   of  the senate  and house representatives  : among vicissitudes
##   Democratic                          3 1994 2742      2 1728     4               3 54    25            3
##   Democratic-Republican              10  945 1416      0  640     0               2  1    16            1
##   Federalist                          3  140  163      1  130     0               2  0     4            0
##   none                                1   82  129      1   50     2               2  2     1            1
##   Republican                          9 3146 4534      5 2530     4               6 90    52            0
##   Whig                               13  964 1325      6  472     1               4  1    10            0
## [ reached max_nfeat ... 9,581 more features ]
```

``` r
head(colSums(dfmat_party), 10)
```

```
## fellow-citizens              of             the          senate             and           house 
##              39            7271           10309              15            5550              11 
## representatives               :           among    vicissitudes 
##              19             148             108               5
```

Notice that `dfmat_party` has far fewer rows than `dfmat_inaug`, one per party rather than one per speech, while `colSums()` stays exactly the same. Grouping redistributes counts across fewer rows; it never creates or destroys word occurrences.

{{% notice note %}}
From **quanteda** package version 3.0 onwards, `dfm_group()` supports non-standard evaluation, so the name of the grouping variable should not be quoted by `"`.
{{% /notice %}}

`dfm_group()` also identifies document-level variables that are the same within a group, such as `Party` itself, and keeps them. Variables that vary within a group, such as `Year`, get dropped, since they would no longer have a single, unambiguous value.


``` r
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
