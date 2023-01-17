---
title: Different encodings
weight: 40
draft: false
---

Even if files are not saved in UTF-8, you can extract information on character encoding from the file names and import the texts correctly.


```r
require(quanteda)
require(readtext)
```

`temp_dir` contains the example files in various character encodings.


```r
path_temp <- tempdir()
unzip(system.file("extdata", "data_files_encodedtexts.zip", package = "readtext"), exdir = path_temp)
```

`list.files()` returns names of all the text files (".txt") in the directory


```r
filename <- list.files(path_temp, "^(Indian|UDHR_).*\\.txt$")
head(filename)
```

```
## [1] "IndianTreaty_English_UTF-16LE.txt"  "IndianTreaty_English_UTF-8-BOM.txt"
## [3] "UDHR_Arabic_ISO-8859-6.txt"         "UDHR_Arabic_UTF-8.txt"             
## [5] "UDHR_Arabic_WINDOWS-1256.txt"       "UDHR_Chinese_GB2312.txt"
```

You can extract character encoding information from the file names using R's basic commands. 


```r
filename <- gsub(".txt$", "", filename)
encoding <- sapply(strsplit(filename, "_"), "[", 3)
head(encoding)
```

```
## [1] "UTF-16LE"     "UTF-8-BOM"    "ISO-8859-6"   "UTF-8"        "WINDOWS-1256"
## [6] "GB2312"
```

There is a character encoding not supported by R.


```r
setdiff(encoding, iconvlist())
```

```
## [1] "UTF-8-BOM"
```

You then pass `encoding` to `readtext()` to convert various character encodings into UTF-8.


```r
path_data <- system.file("extdata/", package = "readtext")
dat_txt <- readtext(paste0(path_data, "/data_files_encodedtexts.zip"), 
                     encoding = encoding,
                     docvarsfrom = "filenames", 
                     docvarnames = c("document", "language", "input_encoding"))
print(dat_txt, n = 50)
```

```
## readtext object consisting of 36 documents and 3 docvars.
## # Description: df [36 × 5]
##    doc_id                             text           document  langua… input_en…
##    <chr>                              <chr>          <chr>     <chr>   <chr>    
##  1 IndianTreaty_English_UTF-16LE.txt  "\"WHEREAS, t… IndianTr… English UTF-16LE 
##  2 IndianTreaty_English_UTF-8-BOM.txt "\"ARTICLE 1.… IndianTr… English UTF-8-BOM
##  3 UDHR_Arabic_ISO-8859-6.txt         "\"الديباجة\n… UDHR      Arabic  ISO-8859…
##  4 UDHR_Arabic_UTF-8.txt              "\"الديباجة\n… UDHR      Arabic  UTF-8    
##  5 UDHR_Arabic_WINDOWS-1256.txt       "\"الديباجة\n… UDHR      Arabic  WINDOWS-…
##  6 UDHR_Chinese_GB2312.txt            "\"世界人权宣… UDHR      Chinese GB2312   
##  7 UDHR_Chinese_GBK.txt               "\"世界人权宣… UDHR      Chinese GBK      
##  8 UDHR_Chinese_UTF-8.txt             "\"世界人权宣… UDHR      Chinese UTF-8    
##  9 UDHR_English_UTF-16BE.txt          "\"Universal … UDHR      English UTF-16BE 
## 10 UDHR_English_UTF-16LE.txt          "\"Universal … UDHR      English UTF-16LE 
## 11 UDHR_English_UTF-8.txt             "\"Universal … UDHR      English UTF-8    
## 12 UDHR_English_WINDOWS-1252.txt      "\"Universal … UDHR      English WINDOWS-…
## 13 UDHR_French_ISO-8859-1.txt         "\"Déclaratio… UDHR      French  ISO-8859…
## 14 UDHR_French_UTF-8.txt              "\"Déclaratio… UDHR      French  UTF-8    
## 15 UDHR_French_WINDOWS-1252.txt       "\"Déclaratio… UDHR      French  WINDOWS-…
## 16 UDHR_German_ISO-8859-1.txt         "\"Die Allgem… UDHR      German  ISO-8859…
## 17 UDHR_German_UTF-8.txt              "\"Die Allgem… UDHR      German  UTF-8    
## 18 UDHR_German_WINDOWS-1252.txt       "\"Die Allgem… UDHR      German  WINDOWS-…
## 19 UDHR_Greek_CP1253.txt              "\"ΟΙΚΟΥΜΕΝΙΚ… UDHR      Greek   CP1253   
## 20 UDHR_Greek_ISO-8859-7.txt          "\"ΟΙΚΟΥΜΕΝΙΚ… UDHR      Greek   ISO-8859…
## 21 UDHR_Greek_UTF-8.txt               "\"ΟΙΚΟΥΜΕΝΙΚ… UDHR      Greek   UTF-8    
## 22 UDHR_Hindi_UTF-8.txt               "\"मानव अधिका… UDHR      Hindi   UTF-8    
## 23 UDHR_Icelandic_ISO-8859-1.txt      "\"Mannréttin… UDHR      Icelan… ISO-8859…
## 24 UDHR_Icelandic_UTF-8.txt           "\"Mannréttin… UDHR      Icelan… UTF-8    
## 25 UDHR_Icelandic_WINDOWS-1252.txt    "\"Mannréttin… UDHR      Icelan… WINDOWS-…
## 26 UDHR_Japanese_CP932.txt            "\"『世界人権… UDHR      Japane… CP932    
## 27 UDHR_Japanese_ISO-2022-JP.txt      "\"『世界人権… UDHR      Japane… ISO-2022…
## 28 UDHR_Japanese_UTF-8.txt            "\"『世界人権… UDHR      Japane… UTF-8    
## 29 UDHR_Japanese_WINDOWS-936.txt      "\"『世界人権… UDHR      Japane… WINDOWS-…
## 30 UDHR_Korean_ISO-2022-KR.txt        "\"세 계 인 …  UDHR      Korean  ISO-2022…
## 31 UDHR_Korean_UTF-8.txt              "\"세 계 인 …  UDHR      Korean  UTF-8    
## 32 UDHR_Russian_ISO-8859-5.txt        "\"Всеобщая д… UDHR      Russian ISO-8859…
## 33 UDHR_Russian_KOI8-R.txt            "\"Всеобщая д… UDHR      Russian KOI8-R   
## 34 UDHR_Russian_UTF-8.txt             "\"Всеобщая д… UDHR      Russian UTF-8    
## 35 UDHR_Russian_WINDOWS-1251.txt      "\"Всеобщая д… UDHR      Russian WINDOWS-…
## 36 UDHR_Thai_UTF-8.txt                "\"ปฏิญญาสากล\… UDHR      Thai    UTF-8
```
