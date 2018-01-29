---
title: Group documents
weight: 40
draft: false
---


```r
require(quanteda)
```


```r
irish_dfm <- dfm(data_corpus_irishbudget2010)
ndoc(irish_dfm)
```

```
## [1] 14
```

```r
head(colSums(irish_dfm), 10)
```

```
##          when             i     presented           the supplementary 
##            90           272             3          3598            10 
##        budget            to          this         house          last 
##           260          1633           559            49            47
```

`dfm_group()` merges documents based on a vector given to the `groups` argument. In grouping documents, it takes the sums of feature frequencies.


```r
party_dfm <- dfm_group(irish_dfm, groups = docvars(irish_dfm, "party"))
ndoc(party_dfm)
```

```
## [1] 5
```

```r
head(colSums(party_dfm), 10)
```

```
##          when             i     presented           the supplementary 
##            90           272             3          3598            10 
##        budget            to          this         house          last 
##           260          1633           559            49            47
```

You can also use the `groups` argument in `dfm()` to simplify your code. 


```r
party_dfm <- dfm(data_corpus_irishbudget2010, groups = docvars(irish_dfm, "party"))
ndoc(party_dfm)
```

```
## [1] 5
```
