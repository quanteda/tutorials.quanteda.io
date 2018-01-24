---
title: readtext and quanteda
weight: 10
chapter: false
draft: true
---




# 3. Inter-operability with quanteda

**readtext** was originally developed in early versions of the [**quanteda**](http://github.com/quanteda/quanteda) package for the quantitative analysis of textual data.  It was spawned from the `textfile()` function from that package, and now lives exclusively in **readtext**. Because **quanteda**'s corpus constructor recognizes the data.frame format returned by `readtext()`, it can construct a corpus directly from a `readtext` object, preserving all docvars and other meta-data.


```r
require(quanteda)
```

You can easily contruct a corpus from a **readtext** object.


```r
# read in comma-separated values with readtext
rt_csv <- readtext(paste0(DATA_DIR, "/csv/inaugCorpus.csv"), text_field = "texts")

# create quanteda corpus
corpus_csv <- corpus(rt_csv)
summary(corpus_csv, 5)
## Corpus consisting of 5 documents:
## 
##   Text Types Tokens Sentences            doc_id Year  President FirstName
##  text1   625   1540        23 inaugCorpus.csv.1 1789 Washington    George
##  text2    96    147         4 inaugCorpus.csv.2 1793 Washington    George
##  text3   826   2578        37 inaugCorpus.csv.3 1797      Adams      John
##  text4   717   1927        41 inaugCorpus.csv.4 1801  Jefferson    Thomas
##  text5   804   2381        45 inaugCorpus.csv.5 1805  Jefferson    Thomas
## 
## Source:  /Users/stefan/GitHub/quanteda_tutorials/content/import-data/* on x86_64 by stefan
## Created: Wed Jan 24 02:01:33 2018
## Notes:
```
