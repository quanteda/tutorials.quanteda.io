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
## Source:  C:/Users/Kohei/Documents/R/quanteda_tutorials/content/basic-operations/corpus/* on x86-64 by Kohei
## Created: Sun Feb 25 08:16:52 2018
## Notes:
```


## Data frame

Using **readtext**, loead an example file from `data_dir` as a data frame called `inaug_data`.


```r
data_dir <- system.file("extdata/", package = "readtext")
inaug_data <- readtext(paste0(data_dir, "/csv/inaugCorpus.csv"), text_field = "texts")
names(inaug_data)
```

```
## [1] "doc_id"    "text"      "Year"      "President" "FirstName"
```

Construct a corpus from `inaug_data`.


```r
inaug_corp <- corpus(inaug_data)
summary(inaug_corp, 5)
```

```
## Corpus consisting of 5 documents:
## 
##   Text Types Tokens Sentences            doc_id Year  President FirstName
##  text1   625   1540        23 inaugCorpus.csv.1 1789 Washington    George
##  text2    96    147         4 inaugCorpus.csv.2 1793 Washington    George
##  text3   826   2578        37 inaugCorpus.csv.3 1797      Adams      John
##  text4   717   1927        41 inaugCorpus.csv.4 1801  Jefferson    Thomas
##  text5   804   2381        45 inaugCorpus.csv.5 1805  Jefferson    Thomas
## 
## Source:  C:/Users/Kohei/Documents/R/quanteda_tutorials/content/basic-operations/corpus/* on x86-64 by Kohei
## Created: Sun Feb 25 08:16:52 2018
## Notes:
```

You can edit the `docnames` for a corpus to change them from `text1`, `text2` etc to a meaningful identifier. 


```r
docid <- paste(inaug_data$Year, 
                inaug_data$FirstName, 
                inaug_data$President, sep = " ")
docnames(inaug_corp) <- docid
summary(inaug_corp, 5)
```

```
## Corpus consisting of 5 documents:
## 
##                    Text Types Tokens Sentences            doc_id Year
##  1789 George Washington   625   1540        23 inaugCorpus.csv.1 1789
##  1793 George Washington    96    147         4 inaugCorpus.csv.2 1793
##         1797 John Adams   826   2578        37 inaugCorpus.csv.3 1797
##   1801 Thomas Jefferson   717   1927        41 inaugCorpus.csv.4 1801
##   1805 Thomas Jefferson   804   2381        45 inaugCorpus.csv.5 1805
##   President FirstName
##  Washington    George
##  Washington    George
##       Adams      John
##   Jefferson    Thomas
##   Jefferson    Thomas
## 
## Source:  C:/Users/Kohei/Documents/R/quanteda_tutorials/content/basic-operations/corpus/* on x86-64 by Kohei
## Created: Sun Feb 25 08:16:52 2018
## Notes:
```

## Vcorpus

**quanteda** also allows to import a **tm** `VCorpus` object


```r
vcorp <- tm::VCorpus(tm::VectorSource(data_char_ukimmig2010))
corp <- corpus(vcorp)
```
