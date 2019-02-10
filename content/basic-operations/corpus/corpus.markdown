---
title: Construct a corpus
weight: 10
draft: false
---


```r
require(quanteda)
require(readtext)
```

You can create a corpus from various available sources:

1. A character vector consisting of one document per element

2. A data frame consisting of a character vector for documents, and additional vectors for document-level variables

3. A VCorpus or SimpleCorpus class object created by the **tm** package 

4. A keywords-in-context object constructed by `kwic()`


## Character vector

`data_char_ukimmig2010` is a named character vector and consists of sections of British election manifestos on immigration and asylum.


```r
immig_corp <- corpus(data_char_ukimmig2010, 
                     docvars = data.frame(party = names(data_char_ukimmig2010)))
summary(immig_corp)
```

```
## Corpus consisting of 9 documents:
## 
##          Text Types Tokens Sentences        party
##           BNP  1125   3280        88          BNP
##     Coalition   142    260         4    Coalition
##  Conservative   251    499        15 Conservative
##        Greens   322    679        21       Greens
##        Labour   298    683        29       Labour
##        LibDem   251    483        14       LibDem
##            PC    77    114         5           PC
##           SNP    88    134         4          SNP
##          UKIP   346    723        27         UKIP
## 
## Source: /home/kohei/packages/quanteda.tutorials/content/basic-operations/corpus/* on x86_64 by kohei
## Created: Sun Feb 10 17:35:15 2019
## Notes:
```


## Data frame

Using **readtext**, loead an example file from `path_data` as a data frame called `dat_inaug`.


```r
path_data <- system.file("extdata/", package = "readtext")
dat_inaug <- readtext(paste0(path_data, "/csv/inaugCorpus.csv"), text_field = "texts")
names(dat_inaug)
```

```
## [1] "doc_id"    "text"      "Year"      "President" "FirstName"
```

Construct a corpus from `dat_inaug`.


```r
corp_inaug <- corpus(dat_inaug)
summary(corp_inaug, 5)
```

```
## Corpus consisting of 5 documents, showing 5 documents:
## 
##               Text Types Tokens Sentences Year  President FirstName
##  inaugCorpus.csv.1   625   1540        23 1789 Washington    George
##  inaugCorpus.csv.2    96    147         4 1793 Washington    George
##  inaugCorpus.csv.3   826   2578        37 1797      Adams      John
##  inaugCorpus.csv.4   717   1927        41 1801  Jefferson    Thomas
##  inaugCorpus.csv.5   804   2381        45 1805  Jefferson    Thomas
## 
## Source: /home/kohei/packages/quanteda.tutorials/content/basic-operations/corpus/* on x86_64 by kohei
## Created: Sun Feb 10 17:35:15 2019
## Notes:
```

You can edit the `docnames` for a corpus to change them from `text1`, `text2` etc to a meaningful identifier. 


```r
docid <- paste(dat_inaug$Year, 
               dat_inaug$FirstName, 
               dat_inaug$President, sep = " ")
docnames(corp_inaug) <- docid
summary(corp_inaug, 5)
```

```
## Corpus consisting of 5 documents, showing 5 documents:
## 
##                    Text Types Tokens Sentences Year  President FirstName
##  1789 George Washington   625   1540        23 1789 Washington    George
##  1793 George Washington    96    147         4 1793 Washington    George
##         1797 John Adams   826   2578        37 1797      Adams      John
##   1801 Thomas Jefferson   717   1927        41 1801  Jefferson    Thomas
##   1805 Thomas Jefferson   804   2381        45 1805  Jefferson    Thomas
## 
## Source: /home/kohei/packages/quanteda.tutorials/content/basic-operations/corpus/* on x86_64 by kohei
## Created: Sun Feb 10 17:35:15 2019
## Notes:
```

## Vcorpus

**quanteda** also allows to import a **tm** `VCorpus` object.


```r
vcorp <- tm::VCorpus(tm::VectorSource(data_char_ukimmig2010))
corp <- corpus(vcorp)
```
