---
title: Construct a corpus
weight: 10
chapter: true
draft: false
---


```
## Warning: package 'tm' was built under R version 3.4.3
```

```
## Warning: package 'NLP' was built under R version 3.4.1
```

Having shown how to read various text files, we turn to constructing a text corpus from these text data. You can create a corpus object in **quanteda** from various available sources:

1. a `character` vector, consisting of one document per element; if the elements are named, these names will be used as document names.

2. a `data.frame` (or a **tibble** `tbl_df`), whose default document id is a variable identified by `docid_field`; the text of the document is a variable identified by `textid_field`; other variables are imported as document-level meta-data. This matches the format of data.frames constructed by the the **readtext** package.

3. a **tm** `VCorpus` or `SimpleCorpus` class object, with the fixed metadata fields imported as docvars and corpus-level metadata imported as metacorpus information.

4. (a keywords-in-context object constructed by `kwic`).

## Construct a corpus from a character vector

The file `data_char_ukimmig2010` is a named character vector of extracts of election manifestos from UK political parties on immigration and asylum-seekers.


```r
# Transform data_char_ukimmig2010 to corpus and specify document-level variables

data_corpus_ukimmig2010 <- corpus(data_char_ukimmig2010, 
                                  docvars = data.frame(party = names(data_char_ukimmig2010)))

summary(data_corpus_ukimmig2010)
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
## Source:  /Users/stefan/GitHub/quanteda_tutorials/content/basic-operations/corpus/* on x86_64 by stefan
## Created: Wed Jan 24 17:49:09 2018
## Notes:
```

## Construct a corpus from a data frame

To construct a corpus from a data frame or a **readtext** object.


```r
# get the data directory from readtext
data_dir <- system.file("extdata/", package = "readtext")
```


```r
# read in comma-separated values with readtext
data_inaugural <- readtext(paste0(data_dir, "/csv/inaugCorpus.csv"), text_field = "texts")

# create quanteda corpus
data_corpus_inaugural <- corpus(data_inaugural)

summary(data_corpus_inaugural, 5)
## Corpus consisting of 5 documents:
## 
##   Text Types Tokens Sentences            doc_id Year  President FirstName
##  text1   625   1540        23 inaugCorpus.csv.1 1789 Washington    George
##  text2    96    147         4 inaugCorpus.csv.2 1793 Washington    George
##  text3   826   2578        37 inaugCorpus.csv.3 1797      Adams      John
##  text4   717   1927        41 inaugCorpus.csv.4 1801  Jefferson    Thomas
##  text5   804   2381        45 inaugCorpus.csv.5 1805  Jefferson    Thomas
## 
## Source:  /Users/stefan/GitHub/quanteda_tutorials/content/basic-operations/corpus/* on x86_64 by stefan
## Created: Wed Jan 24 17:49:09 2018
## Notes:
```

You can change the `docnames` for a corpus to change them from `text1`, `text2` etc to a meaningful identifier. 


```r

doc_id <- paste(data_inaugural$Year, 
                data_inaugural$FirstName, 
                data_inaugural$President, sep = " ")

docnames(data_corpus_inaugural) <- doc_id

summary(data_corpus_inaugural, 5)
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
## Source:  /Users/stefan/GitHub/quanteda_tutorials/content/basic-operations/corpus/* on x86_64 by stefan
## Created: Wed Jan 24 17:49:09 2018
## Notes:
```

## Import a VM corpus

**quanteda** also allows to import a **tm** `VCorpus` object


```r
# load in a tm example VCorpus
data(crude, package = "tm")

tm_corpus <- corpus(crude)

corpus_tm <- tm::VCorpus(tm::VectorSource(data_char_ukimmig2010))

corpus_quanteda <- corpus(corpus_tm)
```
