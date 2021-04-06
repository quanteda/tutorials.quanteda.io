---
title: Subset corpus
weight: 20
draft: false
---


```r
require(quanteda)
```

`corpus_subset()` allows you to select documents in a corpus based on document-level variables.


```r
corp <- data_corpus_inaugural
ndoc(corp)
```

```
## [1] 59
```

```r
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

```r
corp_recent <- corpus_subset(corp, Year >= 1990)
ndoc(corp_recent)
```

```
## [1] 8
```

```r
corp_dem <- corpus_subset(corp, President %in% c("Obama", "Clinton", "Carter"))
ndoc(corp_dem)
```

```
## [1] 5
```
