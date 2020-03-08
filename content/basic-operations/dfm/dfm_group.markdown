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
dfmat_irish <- dfm(data_corpus_irishbudget2010)
print(dfmat_irish)
```

```
## Document-feature matrix of: 14 documents, 5,141 features (81.2% sparse) and 6 docvars.
##                       features
## docs                   when  i presented the supplementary budget  to this house last
##   Lenihan, Brian (FF)     5 73         1 539             7     23 305   99    10    6
##   Bruton, Richard (FG)    2  6         0 305             0     27 172   55     0    5
##   Burton, Joan (LAB)     11 40         0 428             0     37 157   53     6    4
##   Morgan, Arthur (SF)    21 26         0 501             1     26 204   85     5    4
##   Cowen, Brian (FF)       4 17         0 394             0     21 209   43     4    6
##   Kenny, Enda (FG)       12 25         1 304             1     23 119   47     0    4
## [ reached max_ndoc ... 8 more documents, reached max_nfeat ... 5,131 more features ]
```

```r
head(colSums(dfmat_irish), 10)
```

```
##          when             i     presented           the supplementary        budget            to 
##            90           272             3          3598            10           260          1633 
##          this         house          last 
##           559            49            47
```

`dfm_group()` merges documents based on a vector given to the `groups` argument. In grouping documents, it takes the sums of feature frequencies. 


```r
dfmat_party <- dfm_group(dfmat_irish, groups = "party")
print(dfmat_party)
```

```
## Document-feature matrix of: 5 documents, 5,141 features (61.9% sparse) and 3 docvars.
##        features
## docs    when  i presented the supplementary budget  to this house last
##   FF       9 90         1 933             7     44 514  142    14   12
##   FG      19 42         1 802             1     71 359  113     3   11
##   Green    9 33         0 224             0     26 114   53     2    3
##   LAB     25 76         0 856             0     66 333  112    23    8
##   SF      28 31         1 783             2     53 313  139     7   13
## [ reached max_nfeat ... 5,131 more features ]
```

```r
head(colSums(dfmat_party), 10)
```

```
##          when             i     presented           the supplementary        budget            to 
##            90           272             3          3598            10           260          1633 
##          this         house          last 
##           559            49            47
```

`dfm_group()` identify document-level variables that are the same within groups and keeps them.


```r
docvars(dfmat_party)
```

```
##   year debate party
## 1 2010 BUDGET    FF
## 2 2010 BUDGET    FG
## 3 2010 BUDGET Green
## 4 2010 BUDGET   LAB
## 5 2010 BUDGET    SF
```

You can also use the `groups` argument in `dfm()` to simplify your code. 


```r
dfmat_party <- dfm(data_corpus_irishbudget2010, groups = "party")
ndoc(dfmat_party)
```

```
## [1] 5
```
