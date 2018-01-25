---
title: Group documents
weight: 40
chapter: false
draft: false
---


```r
require(quanteda)
```


## Combine documents in a dfm by a grouping variable

Often, it might be useful to combine documents in a `dfm` by a grouping variable. `dfm_group()` returns a dfm whose documents are equal to the unique group combinations, and whose cell values are the sums of the previous values summed by group. 

In this example, we use the Irish budget speeches from 2010 and group the speeches by party.


```r
dfm_irishbudget <- dfm(data_corpus_irishbudget2010)
```

The `dfm` contains 14 documents (1 document per speech).


```r
dfm_group(dfm_irishbudget, groups = docvars(data_corpus_irishbudget2010, "party") )
```

```
## Document-feature matrix of: 5 documents, 5,140 features (61.9% sparse).
```

Now the grouped `dfm` consists of 5 documents (1 for each party).

Note that the grouping can also be done by specifying `group` when constructing a `dfm`.


```r
dfm(data_corpus_irishbudget2010, groups = docvars(data_corpus_irishbudget2010, "party"))
```

```
## Document-feature matrix of: 5 documents, 5,140 features (61.9% sparse).
```

