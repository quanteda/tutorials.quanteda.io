---
title: Different encodings
weight: 40
chapter: false
draft: false
---


```r
knitr::opts_chunk$set(collapse = TRUE)
library(quanteda)

# Load the readtext package
require(readtext)

# Get the data directory from readtext
data_dir <- system.file("extdata/", package = "readtext")
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
## [1] "UTF-8-BOM"
```

If we read the text files without specifying the encoding, we get erroneously formatted text. To avoid this, we determine the `encoding` using the character object `fileencoding` created above. 

We can also add `docvars` based on the filenames.


```r
data_txts <- readtext(paste0(data_dir, "/data_files_encodedtexts.zip"), 
                 encoding = fileencodings,
                 docvarsfrom = "filenames", 
                 docvarnames = c("document", "language", "input_encoding"))
print(data_txts, n = 50)
## readtext object consisting of 36 documents and 3 docvars.
## # data.frame [36 x 5]
##                                doc_id                          text
##                                 <chr>                         <chr>
##  1  IndianTreaty_English_UTF-16LE.txt           "\"WHEREAS, t\"..."
##  2 IndianTreaty_English_UTF-8-BOM.txt           "\"ARTICLE 1.\"..."
##  3         UDHR_Arabic_ISO-8859-6.txt          "\"الديباجة\nل\"..."
##  4              UDHR_Arabic_UTF-8.txt          "\"الديباجة\nل\"..."
##  5       UDHR_Arabic_WINDOWS-1256.txt          "\"الديباجة\nل\"..."
##  6            UDHR_Chinese_GB2312.txt "\"世界人权宣言\n联合国\"..."
##  7               UDHR_Chinese_GBK.txt "\"世界人权宣言\n联合国\"..."
##  8             UDHR_Chinese_UTF-8.txt "\"世界人权宣言\n联合国\"..."
##  9          UDHR_English_UTF-16BE.txt           "\"Universal \"..."
## 10          UDHR_English_UTF-16LE.txt           "\"Universal \"..."
## 11             UDHR_English_UTF-8.txt           "\"Universal \"..."
## 12      UDHR_English_WINDOWS-1252.txt           "\"Universal \"..."
## 13         UDHR_French_ISO-8859-1.txt           "\"Déclaratio\"..."
## 14              UDHR_French_UTF-8.txt           "\"Déclaratio\"..."
## 15       UDHR_French_WINDOWS-1252.txt           "\"Déclaratio\"..."
## 16         UDHR_German_ISO-8859-1.txt           "\"Die Allgem\"..."
## 17              UDHR_German_UTF-8.txt           "\"Die Allgem\"..."
## 18       UDHR_German_WINDOWS-1252.txt           "\"Die Allgem\"..."
## 19              UDHR_Greek_CP1253.txt           "\"ΟΙΚΟΥΜΕΝΙΚ\"..."
## 20          UDHR_Greek_ISO-8859-7.txt           "\"ΟΙΚΟΥΜΕΝΙΚ\"..."
## 21               UDHR_Greek_UTF-8.txt           "\"ΟΙΚΟΥΜΕΝΙΚ\"..."
## 22               UDHR_Hindi_UTF-8.txt           "\"मानव अधिका\"..."
## 23      UDHR_Icelandic_ISO-8859-1.txt           "\"Mannréttin\"..."
## 24           UDHR_Icelandic_UTF-8.txt           "\"Mannréttin\"..."
## 25    UDHR_Icelandic_WINDOWS-1252.txt           "\"Mannréttin\"..."
## 26            UDHR_Japanese_CP932.txt  "\"『世界人権宣言』\n \"..."
## 27      UDHR_Japanese_ISO-2022-JP.txt  "\"『世界人権宣言』\n \"..."
## 28            UDHR_Japanese_UTF-8.txt  "\"『世界人権宣言』\n \"..."
## 29      UDHR_Japanese_WINDOWS-936.txt  "\"『世界人権宣言』\n \"..."
## 30        UDHR_Korean_ISO-2022-KR.txt      "\"세 계 인 권 선 \"..."
## 31              UDHR_Korean_UTF-8.txt      "\"세 계 인 권 선 \"..."
## 32        UDHR_Russian_ISO-8859-5.txt           "\"Всеобщая д\"..."
## 33            UDHR_Russian_KOI8-R.txt           "\"Всеобщая д\"..."
## 34             UDHR_Russian_UTF-8.txt           "\"Всеобщая д\"..."
## 35      UDHR_Russian_WINDOWS-1251.txt           "\"Всеобщая д\"..."
## 36                UDHR_Thai_UTF-8.txt            "\"ปฏิญญาสากล\"..."
## # ... with 3 more variables: document <chr>, language <chr>,
## #   input_encoding <chr>
```

From this file we can easily create a **quanteda** `corpus` object.


```r
corpus_txts <- corpus(data_txts)
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
## Source:  /Users/stefan/GitHub/quanteda_tutorials/content/import-data/* on x86_64 by stefan
## Created: Wed Jan 24 17:49:45 2018
## Notes:
```
