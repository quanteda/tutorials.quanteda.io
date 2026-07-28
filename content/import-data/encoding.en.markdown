---
title: Different encodings
weight: 40
draft: false
---

Text files are stored on disk as raw bytes, and "character encoding" translates those bytes back into readable characters. Most modern files use UTF-8, which R handles automatically. Older files, especially ones exported from older software or from non-English systems, are sometimes saved in a different encoding. If R assumes the wrong encoding, accented letters and non-Latin scripts turn into garbled symbols instead of the correct text. Here is one way to recover from that. Even if files are not saved in UTF-8, you can sometimes extract the correct encoding from the file names themselves and import the texts correctly.


``` r
library(quanteda)
library(readtext)
```

`path_temp` will hold a folder of example files, each deliberately saved in a different character encoding, so that we can practise detecting and fixing it.


``` r
path_temp <- tempdir()
unzip(system.file("extdata", "data_files_encodedtexts.zip", package = "readtext"), exdir = path_temp)
```

`list.files()` returns the names of all the text files (`.txt`) in the directory that match the given pattern.


``` r
filename <- list.files(path_temp, "^(Indian|UDHR_).*\\.txt$")
head(filename)
```

```
## [1] "IndianTreaty_English_UTF-16LE.txt"  "IndianTreaty_English_UTF-8-BOM.txt"
## [3] "UDHR_Arabic_ISO-8859-6.txt"         "UDHR_Arabic_UTF-8.txt"             
## [5] "UDHR_Arabic_WINDOWS-1256.txt"       "UDHR_Chinese_GB2312.txt"
```

In this example dataset, each file name encodes its own character encoding as the third underscore-separated part. We can extract it using ordinary R string functions, without needing to open and inspect each file by hand.


``` r
filename <- gsub(".txt$", "", filename)
encoding <- sapply(strsplit(filename, "_"), "[", 3)
head(encoding)
```

```
## [1] "UTF-16LE"     "UTF-8-BOM"    "ISO-8859-6"   "UTF-8"        "WINDOWS-1256"
## [6] "GB2312"
```

Before using these encodings, check that R recognises all of them. `iconvlist()` returns every character encoding your machine's R installation supports, so comparing it against the encodings we extracted tells us whether any are unsupported.


``` r
setdiff(encoding, iconvlist())
```

```
## [1] "UTF-8-BOM"
```

One of our example files, labelled `"UTF-8-BOM"`, is therefore not a genuine encoding that R can convert from directly. It's UTF-8 text with an extra marker (a "byte order mark") at the start of the file. `readtext()` handles this as a special case even though it does not appear in `iconvlist()`. For every other encoding, you can pass the `encoding` vector straight to `readtext()`, which converts each file from its original encoding into UTF-8 as it reads it in.


``` r
path_data <- system.file("extdata/", package = "readtext")
dat_txt <- readtext(paste0(path_data, "/data_files_encodedtexts.zip"),
                     encoding = encoding,
                     docvarsfrom = "filenames",
                     docvarnames = c("document", "language", "input_encoding"))
print(dat_txt, n = 50)
```

```
## readtext object consisting of 36 documents and 3 docvars.
## $text
##  [1] "# A data frame: 36 × 5"                                                                  
##  [2] "   doc_id                             text      document language input_encoding"        
##  [3] "   <chr>                              <chr>     <chr>    <chr>    <chr>         "        
##  [4] " 1 IndianTreaty_English_UTF-16LE.txt  \"\\\"WHERE… IndianT… English  UTF-16LE      "     
##  [5] " 2 IndianTreaty_English_UTF-8-BOM.txt \"\\\"ARTIC… IndianT… English  UTF-8-BOM     "     
##  [6] " 3 UDHR_Arabic_ISO-8859-6.txt         \"\\\"الديب… UDHR     Arabic   ISO-8859-6    "     
##  [7] " 4 UDHR_Arabic_UTF-8.txt              \"\\\"الديب… UDHR     Arabic   UTF-8         "     
##  [8] " 5 UDHR_Arabic_WINDOWS-1256.txt       \"\\\"الديب… UDHR     Arabic   WINDOWS-1256  "     
##  [9] " 6 UDHR_Chinese_GB2312.txt            \"\\\"世界人权宣… UDHR     Chinese  GB2312        "
## [10] " 7 UDHR_Chinese_GBK.txt               \"\\\"世界人权宣… UDHR     Chinese  GBK           "
## [11] " 8 UDHR_Chinese_UTF-8.txt             \"\\\"世界人权宣… UDHR     Chinese  UTF-8         "
## [12] " 9 UDHR_English_UTF-16BE.txt          \"\\\"Unive… UDHR     English  UTF-16BE      "     
## [13] "10 UDHR_English_UTF-16LE.txt          \"\\\"Unive… UDHR     English  UTF-16LE      "     
## [14] "11 UDHR_English_UTF-8.txt             \"\\\"Unive… UDHR     English  UTF-8         "     
## [15] "12 UDHR_English_WINDOWS-1252.txt      \"\\\"Unive… UDHR     English  WINDOWS-1252  "     
## [16] "13 UDHR_French_ISO-8859-1.txt         \"\\\"Décla… UDHR     French   ISO-8859-1    "     
## [17] "14 UDHR_French_UTF-8.txt              \"\\\"Décla… UDHR     French   UTF-8         "     
## [18] "15 UDHR_French_WINDOWS-1252.txt       \"\\\"Décla… UDHR     French   WINDOWS-1252  "     
## [19] "16 UDHR_German_ISO-8859-1.txt         \"\\\"Die A… UDHR     German   ISO-8859-1    "     
## [20] "17 UDHR_German_UTF-8.txt              \"\\\"Die A… UDHR     German   UTF-8         "     
## [21] "18 UDHR_German_WINDOWS-1252.txt       \"\\\"Die A… UDHR     German   WINDOWS-1252  "     
## [22] "19 UDHR_Greek_CP1253.txt              \"\\\"ΟΙΚΟΥ… UDHR     Greek    CP1253        "     
## [23] "20 UDHR_Greek_ISO-8859-7.txt          \"\\\"ΟΙΚΟΥ… UDHR     Greek    ISO-8859-7    "     
## [24] "21 UDHR_Greek_UTF-8.txt               \"\\\"ΟΙΚΟΥ… UDHR     Greek    UTF-8         "     
## [25] "22 UDHR_Hindi_UTF-8.txt               \"\\\"मानव अ… UDHR     Hindi    UTF-8         "    
## [26] "23 UDHR_Icelandic_ISO-8859-1.txt      \"\\\"Mannr… UDHR     Iceland… ISO-8859-1    "     
## [27] "24 UDHR_Icelandic_UTF-8.txt           \"\\\"Mannr… UDHR     Iceland… UTF-8         "     
## [28] "25 UDHR_Icelandic_WINDOWS-1252.txt    \"\\\"Mannr… UDHR     Iceland… WINDOWS-1252  "     
## [29] "26 UDHR_Japanese_CP932.txt            \"\\\"『世界人権… UDHR     Japanese CP932         "
## [30] "27 UDHR_Japanese_ISO-2022-JP.txt      \"\\\"『世界人権… UDHR     Japanese ISO-2022-JP   "
## [31] "28 UDHR_Japanese_UTF-8.txt            \"\\\"『世界人権… UDHR     Japanese UTF-8         "
## [32] "29 UDHR_Japanese_WINDOWS-936.txt      \"\\\"『世界人権… UDHR     Japanese WINDOWS-936   "
## [33] "30 UDHR_Korean_ISO-2022-KR.txt        \"\\\"세 계 인… UDHR     Korean   ISO-2022-KR   "  
## [34] "31 UDHR_Korean_UTF-8.txt              \"\\\"세 계 인… UDHR     Korean   UTF-8         "  
## [35] "32 UDHR_Russian_ISO-8859-5.txt        \"\\\"Всеоб… UDHR     Russian  ISO-8859-5    "     
## [36] "33 UDHR_Russian_KOI8-R.txt            \"\\\"Всеоб… UDHR     Russian  KOI8-R        "     
## [37] "34 UDHR_Russian_UTF-8.txt             \"\\\"Всеоб… UDHR     Russian  UTF-8         "     
## [38] "35 UDHR_Russian_WINDOWS-1251.txt      \"\\\"Всеоб… UDHR     Russian  WINDOWS-1251  "     
## [39] "36 UDHR_Thai_UTF-8.txt                \"\\\"ปฏิญญา… UDHR     Thai     UTF-8         "     
## 
## $summary
## $summary[[1]]
## NULL
## 
## 
## attr(,"class")
## [1] "trunc_mat"
```

Every file, regardless of its original encoding, has now been read into `dat_txt` as correctly displayed UTF-8 text, ready to be passed to `corpus()` exactly like any of the other data sources in this chapter.
