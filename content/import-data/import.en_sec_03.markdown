---
title: Inter-operability
weight: 30
chapter: false
draft: false
---

**readtext** was originally developed in early versions of the [**quanteda**](http://github.com/quanteda/quanteda) package for the quantitative analysis of textual data.  It was spawned from the `textfile()` function from that package, and now lives exclusively in **readtext**. Because **quanteda**'s corpus constructor recognizes the data.frame format returned by `readtext()`, it can construct a corpus directly from a `readtext` object, preserving all docvars and other meta-data.


```r
knitr::opts_chunk$set(collapse = TRUE)
require(quanteda)

# Load the readtext package
require(readtext)

# Get the data directory from readtext
data_dir <- system.file("extdata/", package = "readtext")
```

You can easily contruct a corpus from a **readtext** object.


```r
# read in comma-separated values with readtext
rt_csv <- readtext(paste0(data_dir, "/csv/inaugCorpus.csv"), text_field = "texts")

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
## Source:  C:/Users/Kohei/Documents/R/quanteda_tutorials/content/import-data/* on x86-64 by Kohei
## Created: Wed Jan 24 16:08:30 2018
## Notes:
```

## Read files with different encodings

Sometimes files of the same type have different encodings. If the encoding of a file is included in the file name, we can extract this information and import the texts correctly. 


```r
# create a temporary directory to extract the .zip file
FILEDIR <- tempdir()
# unzip file
unzip(system.file("extdata", "data_files_encodedtexts.zip", package = "readtext"), exdir = FILEDIR)
```

Here, we will get the encoding from the filenames themselves.

```r
# get encoding from filename
filenames <- list.files(FILEDIR, "^(Indian|UDHR_).*\\.txt$")

head(filenames)
## [1] "IndianTreaty_English_UTF-16LE.txt" 
## [2] "IndianTreaty_English_UTF-8-BOM.txt"
## [3] "UDHR_Arabic_ISO-8859-6.txt"        
## [4] "UDHR_Arabic_UTF-8.txt"             
## [5] "UDHR_Arabic_WINDOWS-1256.txt"      
## [6] "UDHR_Chinese_GB2312.txt"

# Strip the extension
filenames <- gsub(".txt$", "", filenames)
parts <- strsplit(filenames, "_")
fileencodings <- sapply(parts, "[", 3)

head(fileencodings)
## [1] "UTF-16LE"     "UTF-8-BOM"    "ISO-8859-6"   "UTF-8"       
## [5] "WINDOWS-1256" "GB2312"

# Check whether certain file encodings are not supported
notAvailableIndex <- which(!(fileencodings %in% iconvlist()))
fileencodings[notAvailableIndex]
##  [1] "UTF-8-BOM"    "ISO-8859-6"   "WINDOWS-1256" "GB2312"      
##  [5] "WINDOWS-1252" "WINDOWS-1252" "WINDOWS-1252" "ISO-8859-7"  
##  [9] "WINDOWS-1252" "ISO-2022-KR"  "ISO-8859-5"   "KOI8-R"      
## [13] "WINDOWS-1251"
```

If we read the text files without specifying the encoding, we get erroneously formatted text. To avoid this, we determine the `encoding` using the character object `fileencoding` created above. 

We can also add `docvars` based on the filenames.

```r
txts <- readtext(paste0(data_dir, "/data_files_encodedtexts.zip"), 
                 encoding = fileencodings,
                 docvarsfrom = "filenames", 
                 docvarnames = c("document", "language", "input_encoding"))
print(txts, n = 50)
## readtext object consisting of 36 documents and 3 docvars.
## # data.frame [36 x 5]
##    doc_id                             text            docume~ lang~ input~
##    <chr>                              <chr>           <chr>   <chr> <chr> 
##  1 IndianTreaty_English_UTF-16LE.txt  "\"WHEREAS, t\~ Indian~ Engl~ UTF-1~
##  2 IndianTreaty_English_UTF-8-BOM.txt "\"ARTICLE 1.\~ Indian~ Engl~ UTF-8~
##  3 UDHR_Arabic_ISO-8859-6.txt         "\"\u0627\u064~ UDHR    Arab~ ISO-8~
##  4 UDHR_Arabic_UTF-8.txt              "\"\u0627\u064~ UDHR    Arab~ UTF-8 
##  5 UDHR_Arabic_WINDOWS-1256.txt       "\"\u0627\u064~ UDHR    Arab~ WINDO~
##  6 UDHR_Chinese_GB2312.txt            "\"\u4e16\u754~ UDHR    Chin~ GB2312
##  7 UDHR_Chinese_GBK.txt               "\"\u4e16\u754~ UDHR    Chin~ GBK   
##  8 UDHR_Chinese_UTF-8.txt             "\"\u4e16\u754~ UDHR    Chin~ UTF-8 
##  9 UDHR_English_UTF-16BE.txt          "\"Universal \~ UDHR    Engl~ UTF-1~
## 10 UDHR_English_UTF-16LE.txt          "\"Universal \~ UDHR    Engl~ UTF-1~
## 11 UDHR_English_UTF-8.txt             "\"Universal \~ UDHR    Engl~ UTF-8 
## 12 UDHR_English_WINDOWS-1252.txt      "\"Universal \~ UDHR    Engl~ WINDO~
## 13 UDHR_French_ISO-8859-1.txt         "\"D\u00e9clar~ UDHR    Fren~ ISO-8~
## 14 UDHR_French_UTF-8.txt              "\"D\u00e9clar~ UDHR    Fren~ UTF-8 
## 15 UDHR_French_WINDOWS-1252.txt       "\"D\u00e9clar~ UDHR    Fren~ WINDO~
## 16 UDHR_German_ISO-8859-1.txt         "\"Die Allgem\~ UDHR    Germ~ ISO-8~
## 17 UDHR_German_UTF-8.txt              "\"Die Allgem\~ UDHR    Germ~ UTF-8 
## 18 UDHR_German_WINDOWS-1252.txt       "\"Die Allgem\~ UDHR    Germ~ WINDO~
## 19 UDHR_Greek_CP1253.txt              "\"\u039f\u039~ UDHR    Greek CP1253
## 20 UDHR_Greek_ISO-8859-7.txt          "\"\u039f\u039~ UDHR    Greek ISO-8~
## 21 UDHR_Greek_UTF-8.txt               "\"\u039f\u039~ UDHR    Greek UTF-8 
## 22 UDHR_Hindi_UTF-8.txt               "\"\u092e\u093~ UDHR    Hindi UTF-8 
## 23 UDHR_Icelandic_ISO-8859-1.txt      "\"Mannr\u00e9~ UDHR    Icel~ ISO-8~
## 24 UDHR_Icelandic_UTF-8.txt           "\"Mannr\u00e9~ UDHR    Icel~ UTF-8 
## 25 UDHR_Icelandic_WINDOWS-1252.txt    "\"Mannr\u00e9~ UDHR    Icel~ WINDO~
## 26 UDHR_Japanese_CP932.txt            "\"\u300e\u4e1~ UDHR    Japa~ CP932 
## 27 UDHR_Japanese_ISO-2022-JP.txt      "\"\u300e\u4e1~ UDHR    Japa~ ISO-2~
## 28 UDHR_Japanese_UTF-8.txt            "\"\u300e\u4e1~ UDHR    Japa~ UTF-8 
## 29 UDHR_Japanese_WINDOWS-936.txt      "\"\u300e\u4e1~ UDHR    Japa~ WINDO~
## 30 UDHR_Korean_ISO-2022-KR.txt        "\"\xe3\"..."   UDHR    Kore~ ISO-2~
## 31 UDHR_Korean_UTF-8.txt              "\"\uc138 \uac~ UDHR    Kore~ UTF-8 
## 32 UDHR_Russian_ISO-8859-5.txt        "\"\u0412\u044~ UDHR    Russ~ ISO-8~
## 33 UDHR_Russian_KOI8-R.txt            "\"\u0412\u044~ UDHR    Russ~ KOI8-R
## 34 UDHR_Russian_UTF-8.txt             "\"\u0412\u044~ UDHR    Russ~ UTF-8 
## 35 UDHR_Russian_WINDOWS-1251.txt      "\"\u0412\u044~ UDHR    Russ~ WINDO~
## 36 UDHR_Thai_UTF-8.txt                "\"\u0e1b\u0e0~ UDHR    Thai  UTF-8
```

From this file we can easily create a **quanteda** `corpus` object.


```r
corpus_txts <- corpus(txts)
summary(corpus_txts, 5)
## Corpus consisting of 36 documents, showing 5 documents:
## 
##   Text Types Tokens Sentences                             doc_id
##  text1   618   2578       155  IndianTreaty_English_UTF-16LE.txt
##  text2   646   3090       154 IndianTreaty_English_UTF-8-BOM.txt
##  text3   753   1555        86         UDHR_Arabic_ISO-8859-6.txt
##  text4   753   1555        86              UDHR_Arabic_UTF-8.txt
##  text5   753   1555        86       UDHR_Arabic_WINDOWS-1256.txt
##      document language input_encoding
##  IndianTreaty  English       UTF-16LE
##  IndianTreaty  English      UTF-8-BOM
##          UDHR   Arabic     ISO-8859-6
##          UDHR   Arabic          UTF-8
##          UDHR   Arabic   WINDOWS-1256
## 
## Source:  C:/Users/Kohei/Documents/R/quanteda_tutorials/content/import-data/* on x86-64 by Kohei
## Created: Wed Jan 24 16:08:31 2018
## Notes:
```

