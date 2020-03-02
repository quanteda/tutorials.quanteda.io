---
title: Group documents
weight: 40
draft: false
---


```r
require(quanteda)
```


```r
dfmat_irish <- dfm(data_corpus_irishbudget2010)
ndoc(dfmat_irish)
```

```
## [1] 14
```

```r
head(colSums(dfmat_irish), 10)
```

```
##          when             i     presented           the supplementary 
##            90           272             3          3598            10 
##        budget            to          this         house          last 
##           260          1633           559            49            47
```

`dfm_group()` merges documents based on a vector given to the `groups` argument. In grouping documents, it takes the sums of feature frequencies. 


```r
dfmat_party <- dfm_group(dfmat_irish, groups = "party")
ndoc(dfmat_party)
```

```
## [1] 5
```

```r
head(colSums(dfmat_party), 10)
```

```
##          when             i     presented           the supplementary 
##            90           272             3          3598            10 
##        budget            to          this         house          last 
##           260          1633           559            49            47
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
