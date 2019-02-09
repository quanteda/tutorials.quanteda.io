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
## [1] 58
```

```r
head(docvars(corp))
```

```
##                 Year  President FirstName
## 1789-Washington 1789 Washington    George
## 1793-Washington 1793 Washington    George
## 1797-Adams      1797      Adams      John
## 1801-Jefferson  1801  Jefferson    Thomas
## 1805-Jefferson  1805  Jefferson    Thomas
## 1809-Madison    1809    Madison     James
```

```r
corp_recent <- corpus_subset(corp, Year >= 1990)
ndoc(corp_recent)
```

```
## [1] 7
```

```r
corp_dem <- corpus_subset(corp, President %in% c('Obama', 'Clinton', 'Carter'))
ndoc(corp_dem)
```

```
## [1] 5
```
