---
title: Different encodings
weight: 40
draft: false
---

Even if files are not saved in UTF8, you can can extract information on character encoding from the file names and import the texts correctly.


```r
require(quanteda)
require(readtext)
```

`temp_dir` contains the example files in various character encodings.


```r
temp_dir <- tempdir()
unzip(system.file("extdata", "data_files_encodedtexts.zip", package = "readtext"), exdir = temp_dir)
```

`list.files()` returns names of all the text files (.txt) in the directory


```r
filename <- list.files(temp_dir, "^(Indian|UDHR_).*\\.txt$")
head(filename)
```

```
## [1] "IndianTreaty_English_UTF-16LE.txt" 
## [2] "IndianTreaty_English_UTF-8-BOM.txt"
## [3] "UDHR_Arabic_ISO-8859-6.txt"        
## [4] "UDHR_Arabic_UTF-8.txt"             
## [5] "UDHR_Arabic_WINDOWS-1256.txt"      
## [6] "UDHR_Chinese_GB2312.txt"
```

You can extract character encoding information from the file names using R's basic commands. 


```r
filename <- gsub(".txt$", "", filename)
encoding <- sapply(strsplit(filename, "_"), "[", 3)
head(encoding)
```

```
## [1] "UTF-16LE"     "UTF-8-BOM"    "ISO-8859-6"   "UTF-8"       
## [5] "WINDOWS-1256" "GB2312"
```

There is a character encoding not supported by R.


```r
setdiff(encoding, iconvlist())
```

```
## [1] "UTF-8-BOM"
```

You then pass `encoding` to `readtext()` to convert various character encodings into UTF8.


```r
data_dir <- system.file("extdata/", package = "readtext")
txt_data <- readtext(paste0(data_dir, "/data_files_encodedtexts.zip"), 
                     encoding = encoding,
                     docvarsfrom = "filenames", 
                     docvarnames = c("document", "language", "input_encoding"))
print(txt_data, n = 50)
```

```
## readtext object consisting of 36 documents and 3 docvars.
## # data.frame [36 × 5]
##    doc_id                             text          docume… langu… input_…
##    <chr>                              <chr>         <chr>   <chr>  <chr>  
##  1 IndianTreaty_English_UTF-16LE.txt  "\"WHEREAS, … Indian… Engli… UTF-16…
##  2 IndianTreaty_English_UTF-8-BOM.txt "\"ARTICLE 1… Indian… Engli… UTF-8-…
##  3 UDHR_Arabic_ISO-8859-6.txt         "\"الديباجة\… UDHR    Arabic ISO-88…
##  4 UDHR_Arabic_UTF-8.txt              "\"الديباجة\… UDHR    Arabic UTF-8  
##  5 UDHR_Arabic_WINDOWS-1256.txt       "\"الديباجة\… UDHR    Arabic WINDOW…
##  6 UDHR_Chinese_GB2312.txt            "\"世界人权宣言\n联… UDHR    Chine… GB2312 
##  7 UDHR_Chinese_GBK.txt               "\"世界人权宣言\n联… UDHR    Chine… GBK    
##  8 UDHR_Chinese_UTF-8.txt             "\"世界人权宣言\n联… UDHR    Chine… UTF-8  
##  9 UDHR_English_UTF-16BE.txt          "\"Universal… UDHR    Engli… UTF-16…
## 10 UDHR_English_UTF-16LE.txt          "\"Universal… UDHR    Engli… UTF-16…
## 11 UDHR_English_UTF-8.txt             "\"Universal… UDHR    Engli… UTF-8  
## 12 UDHR_English_WINDOWS-1252.txt      "\"Universal… UDHR    Engli… WINDOW…
## 13 UDHR_French_ISO-8859-1.txt         "\"Déclarati… UDHR    French ISO-88…
## 14 UDHR_French_UTF-8.txt              "\"Déclarati… UDHR    French UTF-8  
## 15 UDHR_French_WINDOWS-1252.txt       "\"Déclarati… UDHR    French WINDOW…
## 16 UDHR_German_ISO-8859-1.txt         "\"Die Allge… UDHR    German ISO-88…
## 17 UDHR_German_UTF-8.txt              "\"Die Allge… UDHR    German UTF-8  
## 18 UDHR_German_WINDOWS-1252.txt       "\"Die Allge… UDHR    German WINDOW…
## 19 UDHR_Greek_CP1253.txt              "\"ΟΙΚΟΥΜΕΝΙ… UDHR    Greek  CP1253 
## 20 UDHR_Greek_ISO-8859-7.txt          "\"ΟΙΚΟΥΜΕΝΙ… UDHR    Greek  ISO-88…
## 21 UDHR_Greek_UTF-8.txt               "\"ΟΙΚΟΥΜΕΝΙ… UDHR    Greek  UTF-8  
## 22 UDHR_Hindi_UTF-8.txt               "\"मानव अधिक… UDHR    Hindi  UTF-8  
## 23 UDHR_Icelandic_ISO-8859-1.txt      "\"Mannrétti… UDHR    Icela… ISO-88…
## 24 UDHR_Icelandic_UTF-8.txt           "\"Mannrétti… UDHR    Icela… UTF-8  
## 25 UDHR_Icelandic_WINDOWS-1252.txt    "\"Mannrétti… UDHR    Icela… WINDOW…
## 26 UDHR_Japanese_CP932.txt            "\"『世界人権宣言』\… UDHR    Japan… CP932  
## 27 UDHR_Japanese_ISO-2022-JP.txt      "\"『世界人権宣言』\… UDHR    Japan… ISO-20…
## 28 UDHR_Japanese_UTF-8.txt            "\"『世界人権宣言』\… UDHR    Japan… UTF-8  
## 29 UDHR_Japanese_WINDOWS-936.txt      "\"『世界人権宣言』\… UDHR    Japan… WINDOW…
## 30 UDHR_Korean_ISO-2022-KR.txt        "\"세 계 인 권 선… UDHR    Korean ISO-20…
## 31 UDHR_Korean_UTF-8.txt              "\"세 계 인 권 선… UDHR    Korean UTF-8  
## 32 UDHR_Russian_ISO-8859-5.txt        "\"Всеобщая … UDHR    Russi… ISO-88…
## 33 UDHR_Russian_KOI8-R.txt            "\"Всеобщая … UDHR    Russi… KOI8-R 
## 34 UDHR_Russian_UTF-8.txt             "\"Всеобщая … UDHR    Russi… UTF-8  
## 35 UDHR_Russian_WINDOWS-1251.txt      "\"Всеобщая … UDHR    Russi… WINDOW…
## 36 UDHR_Thai_UTF-8.txt                "\"ปฏิญญาสาก…  UDHR    Thai   UTF-8
```
