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


## Character vector

`data_char_ukimmig2010` is a named character vector and consists of sections of British election manifestos on immigration and asylum.


```r
corp_immig <- corpus(data_char_ukimmig2010, 
                     docvars = data.frame(party = names(data_char_ukimmig2010)))
print(corp_immig)
```

```
## Corpus consisting of 9 documents and 1 docvar.
## BNP :
## "IMMIGRATION: AN UNPARALLELED CRISIS WHICH ONLY THE BNP CAN S..."
## 
## Coalition :
## "IMMIGRATION.  The Government believes that immigration has e..."
## 
## Conservative :
## "Attract the brightest and best to our country. Immigration h..."
## 
## Greens :
## "Immigration. Migration is a fact of life.  People have alway..."
## 
## Labour :
## "Crime and immigration The challenge for Britain We will cont..."
## 
## LibDem :
## "firm but fair immigration system Britain has always been an ..."
## 
## [ reached max_ndoc ... 3 more documents ]
```

```r
summary(corp_immig)
```

```
## Corpus consisting of 9 documents, showing 9 documents:
## 
##          Text Types Tokens Sentences        party
##           BNP  1125   3280        88          BNP
##     Coalition   142    260         4    Coalition
##  Conservative   251    499        15 Conservative
##        Greens   322    677        21       Greens
##        Labour   298    680        29       Labour
##        LibDem   251    483        14       LibDem
##            PC    77    114         5           PC
##           SNP    88    134         4          SNP
##          UKIP   346    722        26         UKIP
```


## Data frame

Using `read.csv()`, load an example file from `path_data` as a data frame called `dat_inaug`. Note that your file does not to be formatted as `.csv`. You can build a **quanteda** corpus from any file format that R can import as a data frame (see, for instance, the [**rio**](https://cran.r-project.org/web/packages/rio/index.html) package for importing various files as data frames into R).


```r
# set path
path_data <- system.file("extdata/", package = "readtext")

# import csv file
dat_inaug <- read.csv(paste0(path_data, "/csv/inaugCorpus.csv"))
names(dat_inaug)
```

```
## [1] "texts"     "Year"      "President" "FirstName"
```

Construct a corpus from the "texts" column in `dat_inaug`.


```r
corp_inaug <- corpus(dat_inaug, text_field = "texts")
print(corp_inaug)
```

```
## Corpus consisting of 5 documents and 3 docvars.
## text1 :
## "Fellow-Citizens of the Senate and of the House of Representa..."
## 
## text2 :
## "Fellow citizens, I am again called upon by the voice of my c..."
## 
## text3 :
## "When it was first perceived, in early times, that no middle ..."
## 
## text4 :
## "Friends and Fellow Citizens: Called upon to undertake the du..."
## 
## text5 :
## "Proceeding, fellow citizens, to that qualification which the..."
```

```r
summary(corp_inaug, 5)
```

```
## Corpus consisting of 5 documents, showing 5 documents:
## 
##   Text Types Tokens Sentences Year  President FirstName
##  text1   625   1537        23 1789 Washington    George
##  text2    96    147         4 1793 Washington    George
##  text3   826   2577        37 1797      Adams      John
##  text4   717   1923        41 1801  Jefferson    Thomas
##  text5   804   2380        45 1805  Jefferson    Thomas
```

You can edit the `docnames` for a corpus to change them from `text1`, `text2` etc., to a meaningful identifier. 


```r
docid <- paste(dat_inaug$Year, 
               dat_inaug$FirstName, 
               dat_inaug$President, sep = " ")
docnames(corp_inaug) <- docid
print(corp_inaug)
```

```
## Corpus consisting of 5 documents and 3 docvars.
## 1789 George Washington :
## "Fellow-Citizens of the Senate and of the House of Representa..."
## 
## 1793 George Washington :
## "Fellow citizens, I am again called upon by the voice of my c..."
## 
## 1797 John Adams :
## "When it was first perceived, in early times, that no middle ..."
## 
## 1801 Thomas Jefferson :
## "Friends and Fellow Citizens: Called upon to undertake the du..."
## 
## 1805 Thomas Jefferson :
## "Proceeding, fellow citizens, to that qualification which the..."
```

## Vcorpus

**quanteda** also allows you to import a **tm** `VCorpus` object.


```r
corp_tm <- tm::VCorpus(tm::VectorSource(data_char_ukimmig2010))
corp_quanteda <- corpus(corp_tm)
```
