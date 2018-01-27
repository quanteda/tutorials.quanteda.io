---
title: Group documents
weight: 40
draft: false
---


```r
require(quanteda)
```


```r
ie_dfm <- dfm(data_corpus_irishbudget2010)
ndoc(ie_dfm)
```

```
## [1] 14
```

```r
head(colSums(ie_dfm), 10)
```

```
##          when             i     presented           the supplementary 
##            90           272             3          3598            10 
##        budget            to          this         house          last 
##           260          1633           559            49            47
```

`dfm_group()` merge documents by taking the sums of feature frequencies.


```r
paty_ie_dfm <- dfm_group(ie_dfm, groups = docvars(ie_dfm, 'party'))
ndoc(paty_ie_dfm)
```

```
## [1] 5
```

```r
head(colSums(paty_ie_dfm), 10)
```

```
##          when             i     presented           the supplementary 
##            90           272             3          3598            10 
##        budget            to          this         house          last 
##           260          1633           559            49            47
```

You can also use `groups` argument in `dfm()` to simplify your code. 


```r
paty_ie_dfm <- dfm(data_corpus_irishbudget2010, groups = docvars(ie_dfm, "party"))
ndoc(paty_ie_dfm)
```

```
## [1] 5
```

