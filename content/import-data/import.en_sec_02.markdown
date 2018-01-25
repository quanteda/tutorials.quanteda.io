---
title: Multiple text files
weight: 20
chapter: false
draft: false
---

In this section we show you how to import multiple text files from one or more folders. In contrast to the pre-formatted files from the previous sections, these files usually do not contain document-level variables (`docvars`). However, you can use the filenames as `docvars` or use a data frame that contains document-level variables and merge them with the text data. 


```r
knitr::opts_chunk$set(collapse = TRUE)
require(quanteda)

# load the readtext package
library(readtext)

# get the data directory from readtext
data_dir <- system.file("extdata/", package = "readtext")
```

## Plain text files (.txt)

The folder "txt" contains a subfolder named UDHR with .txt files of the Universal Declaration of Human Rights in 13 languages. 


```r
# read in all files from a folder
data_udhr <- readtext(paste0(data_dir, "/txt/UDHR/*"))
```

We can specify document-level metadata (`docvars`) based on the file names or on a separate data.frame. Below we take the docvars from the filenames (`docvarsfrom = "filenames"`) and set the names for each variable (`docvarnames = c("unit", "context", "year", "language", "party")`). The command `dvsep = "_"` determines the separator (a regular expression character string) included in the filenames to delimit the `docvar` elements. `encoding = "ISO-8859-1"` specifies the encoding of the text. Below we describe how you can find out how your text is encoded.


```r
# manifestos with docvars from filenames
data_eu <- readtext(paste0(data_dir, "/txt/EU_manifestos/*.txt"),
         docvarsfrom = "filenames", 
         docvarnames = c("unit", "context", "year", "language", "party"),
         dvsep = "_", 
         encoding = "ISO-8859-1")
str(data_eu)
## Classes 'readtext' and 'data.frame':	17 obs. of  7 variables:
##  $ doc_id  : chr  "EU_euro_2004_de_PSE.txt" "EU_euro_2004_de_V.txt" "EU_euro_2004_en_PSE.txt" "EU_euro_2004_en_V.txt" ...
##  $ text    : chr  "PES · PSE · SPE European Parliament rue Wiertz B 1047 Brussels\n\nGEMEINSAM WERDEN WIR STÄRKER Fünf Verpflichtu"| __truncated__ "Gemeinsames Manifest\nGemeinsames Manifest zur Europawahl 2004 Europäischen Föderation Grüner Parteien (EFGP) \"| __truncated__ "PES · PSE · SPE European Parliament rue Wiertz B 1047 Brussels\n\nGROWING STRONGER TOGETHER Five commitments fo"| __truncated__ "Manifesto\nEuropean Elections Manifesto 2004\nCOMMON PREAMBLE\nAs adopted at 15th EFGP Council, Luxembourg, 8th"| __truncated__ ...
##  $ unit    : chr  "EU" "EU" "EU" "EU" ...
##  $ context : chr  "euro" "euro" "euro" "euro" ...
##  $ year    : int  2004 2004 2004 2004 2004 2004 2004 2004 2004 2004 ...
##  $ language: chr  "de" "de" "en" "en" ...
##  $ party   : chr  "PSE" "V" "PSE" "V" ...
```

**readtext** can also curse through subdirectories. In our example, the folder `txt/movie_reviews` contains two subfolders (called `neg` and `pos`). We can load all texts included in both folders. 


```r
# recurse through subdirectories
data_reviews <- readtext(paste0(data_dir, "/txt/movie_reviews/*"))
```

## JSON data (.json)

You can also read .json data. Again you need to specify the `text_field`. We use Twitter data stored in a .json file format.


```r
twitter_data <- readtext("content/data/twitter.json")
```

The file comes with several metadata for each tweet, such as the number of retweets and likes, the username, time and time zone. 


```r
names(twitter_data)
```

## PDF files

**readtext** can also read in and convert .pdf files. In the example below we load all .pdf files stored in the `UDHR` folder, and determine that the `docvars` shall be taken from the filenames. We call the document-level variables `document` and `language`, and specify the delimiter (`dvsep`).



```r
# read in Universal Declaration of Human Rights pdf files
data_udhr <- readtext(paste0(data_dir, "/pdf/UDHR/*.pdf"), 
                    docvarsfrom = "filenames", 
                    docvarnames = c("document", "language"),
                    sep = "_")
```


## Microsoft Word files (.doc, .docx)

Microsoft Word formatted files are converted through the package **antiword** for older `.doc` files, and using **XML** for newer `.docx` files.


```r
# read in Word data (.docx)
data_word <- readtext(paste0(data_dir, "/word/*.docx"))
```
