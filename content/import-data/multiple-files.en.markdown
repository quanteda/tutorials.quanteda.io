---
title: Multiple text files
weight: 20
draft: false
---


```r
require(quanteda)
require(readtext)
```

A second option to import data is to load multiple text files at once that are stored in the same folder or subfolders. Again, `path_data` is the location of sample files on your computer.


```r
path_data <- system.file("extdata/", package = "readtext")
```

Unlike the pre-formatted files, individual text files usually do not contain document-level variables. However, you can create document-level variables using the **readtext** package.

The directory `/txt/UDHR` contains text files (".txt") of the Universal Declaration of Human Rights in 13 languages. 


```r
dat_udhr <- readtext(paste0(path_data, "/txt/UDHR/*"))
```

{{% notice note %}}
If you are using Windows, you need might need to specify the encoding of the file by adding `encoding = "utf-8"`. In this case, imported texts might appear like `<U+4E16><U+754C><U+4EBA><U+6743>` but they indicate that Unicode charactes are imported correctly.
{{% /notice %}}

You can generate document-level variables based on the file names using the `docvarnames` and `docvarsfrom` argument. `dvsep = "_"` specifies the value separator in the filenames.`encoding = "ISO-8859-1"` determines character encodings of the texts.


```r
dat_eu <- readtext(paste0(path_data, "/txt/EU_manifestos/*.txt"),
                    docvarsfrom = "filenames", 
                    docvarnames = c("unit", "context", "year", "language", "party"),
                    dvsep = "_", 
                    encoding = "ISO-8859-1")
str(dat_eu)
```

```
## Classes 'readtext' and 'data.frame':	17 obs. of  7 variables:
##  $ doc_id  : chr  "EU_euro_2004_de_PSE.txt" "EU_euro_2004_de_V.txt" "EU_euro_2004_en_PSE.txt" "EU_euro_2004_en_V.txt" ...
##  $ text    : chr  "PES · PSE · SPE European Parliament rue Wiertz B 1047 Brussels\n\nGEMEINSAM WERDEN WIR STÄRKER Fünf Verpflichtu"| __truncated__ "Gemeinsames Manifest\nGemeinsames Manifest zur Europawahl 2004 Europäischen Föderation Grüner Parteien (EFGP) \"| __truncated__ "PES · PSE · SPE European Parliament rue Wiertz B 1047 Brussels\n\nGROWING STRONGER TOGETHER Five commitments fo"| __truncated__ "Manifesto\nEuropean Elections Manifesto 2004\nCOMMON PREAMBLE\nAs adopted at 15th EFGP Council, Luxembourg, 8th"| __truncated__ ...
##  $ unit    : chr  "EU" "EU" "EU" "EU" ...
##  $ context : chr  "euro" "euro" "euro" "euro" ...
##  $ year    : int  2004 2004 2004 2004 2004 2004 2004 2004 2004 2004 ...
##  $ language: chr  "de" "de" "en" "en" ...
##  $ party   : chr  "PSE" "V" "PSE" "V" ...
```

### JSON

You can also read JSON files (.json) downloaded from the Twititer stream API. [twitter.json](https://raw.githubusercontent.com/quanteda/tutorials.quanteda.io/master/content/data/twitter.json) is located in data directory of this tutorial package.


```r
dat_twitter <- readtext("../data/twitter.json", source = "twitter")
```

The file comes with several metadata for each tweet, such as the number of retweets and likes, the username, time and time zone. 


```r
head(names(dat_twitter))
```

```
## [1] "doc_id"         "text"           "retweet_count"  "favorite_count"
## [5] "favorited"      "truncated"
```

### PDF

`readtext()` can also convert and read PDF (".pdf") files. 


```r
dat_udhr <- readtext(paste0(path_data, "/pdf/UDHR/*.pdf"), 
                      docvarsfrom = "filenames", 
                      docvarnames = c("document", "language"),
                      sep = "_")
```

### Microsoft Word

Finally, `readtext()` can import Microsoft Word (".doc" and ".docx") files.


```r
dat_word <- readtext(paste0(path_data, "/word/*.docx"))
```
