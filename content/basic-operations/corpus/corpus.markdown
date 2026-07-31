---
title: Construct a corpus
weight: 10
draft: false
---

A corpus is a collection of texts, stored together with information about each text, such as its author, date or source, and is almost always the first object you create in **quanteda**. Everything else in this tutorial, tokens and the document-feature matrix, is built from it. Creating a corpus does not change your texts in any way; it only wraps them in a container that keeps the texts and their accompanying information together.


``` r
library(quanteda)
library(readtext)
```

The function that creates a corpus is `corpus()`, which accepts text from several starting points: a character vector with one document per element, a data frame that combines document texts with columns for document-level variables, or a `VCorpus` or `SimpleCorpus` object created by the **tm** package.

## Character vector

The simplest way to build a corpus is from a character vector, in which each element holds the full text of one document. `data_char_ukimmig2010`, bundled with **quanteda**, is a named character vector of this kind, containing sections of British election manifestos on immigration and asylum, one per political party.

The `docvars` argument attaches document-level variables, additional information about each document that is not part of the text itself, such as which party wrote it. Here we record the name of each vector element (the party) as a document-level variable called `party`.


``` r
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

``` r
summary(corp_immig)
```

```
## Corpus consisting of 9 documents, showing 9 documents:
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
##          UKIP   346    723        26         UKIP
```

`print()` confirms how many documents the corpus contains and how large it is. `summary()` gives you one row per document: the number of types (unique words), tokens (total word occurrences) and sentences, alongside the `party` variable you just attached.

## Data frame

If your texts already live inside a data frame, for example because you exported them from a spreadsheet, you can build a corpus from that instead. A data frame is often more convenient than a character vector, since it can hold document-level variables and the texts side by side in one object.

Using `read.csv()`, we load an example file from `path_data` as a data frame called `dat_inaug`. Note that your file does not need to be formatted as `.csv`: you can build a **quanteda** corpus from any file format that R can import as a data frame (see, for instance, the [**rio**](https://cran.r-project.org/web/packages/rio/index.html) package for importing other file formats as data frames into R).


``` r
# set path
path_data <- system.file("extdata/", package = "readtext")

# import csv file
dat_inaug <- read.csv(paste0(path_data, "/csv/inaugCorpus.csv"))
names(dat_inaug)
```

```
## [1] "texts"     "Year"      "President" "FirstName"
```

`names(dat_inaug)` lists the columns in the data frame: the speech text is stored in the `texts` column, and the rest, such as `Year` and `President`, will become document-level variables once we build the corpus. We tell `corpus()` which column holds the text with the `text_field` argument.


``` r
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

``` r
summary(corp_inaug, 5)
```

```
## Corpus consisting of 5 documents, showing 5 documents:
## 
##   Text Types Tokens Sentences Year  President FirstName
##  text1   625   1538        23 1789 Washington    George
##  text2    96    147         4 1793 Washington    George
##  text3   826   2578        37 1797      Adams      John
##  text4   717   1927        41 1801  Jefferson    Thomas
##  text5   804   2381        45 1805  Jefferson    Thomas
```

By default, **quanteda** names each document `text1`, `text2` and so on. These generic labels are hard to work with once you have more than a handful of documents, so you can replace them with something meaningful using `docnames()`.


``` r
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

If you are coming to **quanteda** from the older **tm** package, you do not need to start from scratch. **quanteda** can convert a **tm** `VCorpus` object directly into its own corpus format. Your existing texts stay intact.


``` r
corp_tm <- tm::VCorpus(tm::VectorSource(data_char_ukimmig2010))
corp_quanteda <- corpus(corp_tm)
```
