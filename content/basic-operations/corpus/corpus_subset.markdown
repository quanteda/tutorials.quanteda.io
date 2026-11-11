---
title: Subset corpus
weight: 20
draft: false
---

Once you have a corpus, you will often want to work with only part of it, for example, speeches from a particular time period or by a particular speaker. `corpus_subset()` lets you select documents based on their document-level variables, in exactly the same way that you used `subset()` on a data frame in the [Introduction chapter](/introduction/r-commands).


``` r
library(quanteda)
```

We will use `data_corpus_inaugural`, a corpus built into **quanteda** containing every US presidential inaugural speech. `ndoc()` tells you how many documents a corpus contains, and `docvars()` shows the document-level variables attached to it, here the year, the president's name, their first name and their party.


``` r
corp <- data_corpus_inaugural
ndoc(corp)
```

```
## [1] 60
```

``` r
head(docvars(corp))
```

```
##   Year  President FirstName                 Party
## 1 1789 Washington    George                  none
## 2 1793 Washington    George                  none
## 3 1797      Adams      John            Federalist
## 4 1801  Jefferson    Thomas Democratic-Republican
## 5 1805  Jefferson    Thomas Democratic-Republican
## 6 1809    Madison     James Democratic-Republican
```

``` r
corp_recent <- corpus_subset(corp, Year >= 1990)
ndoc(corp_recent)
```

```
## [1] 9
```

``` r
corp_dem <- corpus_subset(corp, President %in% c("Obama", "Clinton", "Carter"))
ndoc(corp_dem)
```

```
## [1] 5
```

`ndoc()` drops from the full corpus to a smaller number each time we subset it. `corp_recent` keeps only speeches from 1990 onwards, and `corp_dem` keeps only speeches by the three named presidents, using `%in%` to match against several values at once. The original `corp` object is untouched: subsetting always creates a new object rather than modifying the one you started with.
