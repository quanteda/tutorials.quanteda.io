---
title: Getting texts into R
weight: 10
chapter: false
draft: true
---



# Introduction: Getting texts into R

In this section we will show how to load texts from different sources and create a `corpus` object in **quanteda**.

## Creating a `corpus` object

**quanteda can construct a `corpus` object** from several input sources:

## a character vector object  

```r
require(quanteda, warn.conflicts = FALSE, quietly = TRUE)
myCorpus <- corpus(data_char_ukimmig2010, notes = "My first corpus")
## Warning in corpus.character(data_char_ukimmig2010, notes = "My first
## corpus"): Argument notes not used.
summary(myCorpus)
## Corpus consisting of 9 documents:
## 
##          Text Types Tokens Sentences
##           BNP  1125   3280        88
##     Coalition   142    260         4
##  Conservative   251    499        15
##        Greens   322    679        21
##        Labour   298    683        29
##        LibDem   251    483        14
##            PC    77    114         5
##           SNP    88    134         4
##          UKIP   346    723        27
## 
## Source:  /Users/stefan/GitHub/quanteda_tutorials/content/import-data/* on x86_64 by stefan
## Created: Wed Jan 24 02:01:26 2018
## Notes:
```
    
## a `VCorpus` object from the **tm** package, and


```r
data(crude, package = "tm")
myTmCorpus <- corpus(crude)
summary(myTmCorpus, 5)
## Corpus consisting of 20 documents, showing 5 documents:
## 
##  Text Types Tokens Sentences       datetimestamp description
##   127    62    103         5 1987-02-26 17:00:56            
##   144   238    495        19 1987-02-26 17:34:11            
##   191    47     62         4 1987-02-26 18:18:00            
##   194    55     74         5 1987-02-26 18:21:01            
##   211    67     97         4 1987-02-26 19:00:57            
##                                          heading  id language
##         DIAMOND SHAMROCK (DIA) CUTS CRUDE PRICES 127       en
##  OPEC MAY HAVE TO MEET TO FIRM PRICES - ANALYSTS 144       en
##        TEXACO CANADA <TXC> LOWERS CRUDE POSTINGS 191       en
##        MARATHON PETROLEUM REDUCES CRUDE POSTINGS 194       en
##        HOUSTON OIL <HO> RESERVES STUDY COMPLETED 211       en
##             origin topics lewissplit     cgisplit oldid places
##  Reuters-21578 XML    YES      TRAIN TRAINING-SET  5670    usa
##  Reuters-21578 XML    YES      TRAIN TRAINING-SET  5687    usa
##  Reuters-21578 XML    YES      TRAIN TRAINING-SET  5734 canada
##  Reuters-21578 XML    YES      TRAIN TRAINING-SET  5737    usa
##  Reuters-21578 XML    YES      TRAIN TRAINING-SET  5754    usa
##                      author orgs people exchanges
##                        <NA> <NA>   <NA>      <NA>
##  BY TED D'AFFLISIO, Reuters opec   <NA>      <NA>
##                        <NA> <NA>   <NA>      <NA>
##                        <NA> <NA>   <NA>      <NA>
##                        <NA> <NA>   <NA>      <NA>
## 
## Source:  Converted from tm Corpus 'crude'
## Created: Wed Jan 24 02:01:26 2018
## Notes:
```

## using `readtext()` to import texts

This chapter walks you through importing a variety of different text files into R using the **readtext** package. Currently, **readtext** supports plain text files (.txt), data in some form of JavaScript Object Notation (.json), comma-or tab-separated values (.csv, .tab, .tsv), XML documents (.xml), as well as PDF and Microsoft Word formatted files (.pdf, .doc, .docx).  **readtext** is a one-function package that does exactly what it says on the tin: It reads files containing text, along with any associated document-level metadata, which we call "docvars", for document variables.  

**readtext** accepts filemasks, so that you can specify a pattern to load multiple texts from one or more folders, and these texts can even be of multiple types.  **readtext** is smart enough to process them correctly, returning a data.frame with a primary field "text" containing a character vector of the texts, and additional columns of the data.frame as found in the document variables from the source files.

As encoding can also be a challenging issue for those reading in texts, we include functions for diagnosing encodings on a file-by-file basis, and allow you to specify vectorized input encodings to read in file types with individually set (and different) encodings.  (All encoding functions are handled by the **stringi** package.) Because **quanteda**'s corpus constructor recognizes the data.frame format returned by `readtext()`, it can construct a corpus directly from a `readtext` object, preserving all docvars and other meta-data.

**readtext** also handles multiple files and file types using for instance a "glob" expression, files from a URL or an archive file (.zip, .tar, .tar.gz, .tar.bz). Usually, you do not have to determine the format of the files explicitly - **readtext** takes this information from the file ending.

The **readtext** package comes with a data directory called `extdata` that contains examples of all files listed above. In the vignette, we use this data directory.


```r
# Load the readtext package
library(readtext)

# Get the data directory from readtext
DATA_DIR <- system.file("extdata/", package = "readtext")
```

The `extdata` directory contains several subfolders that include different text files. In the following examples, we load one or more files stored in each of these folders. The `paste0` command is used to concatenate the `extdata` folder from the 
**readtext** package with the subfolders. When reading in custom text files, you will need to determine your own data directory (see `?setwd()`). Alternatively, if you use RStudio you can create a Project (File -> New Project) and put the textual raw data in a separate folder in the Project folder. Then you can simply start the .Rproj file and you do not have to specify the full working directory. 
