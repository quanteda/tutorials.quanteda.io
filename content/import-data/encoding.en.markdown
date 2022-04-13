---
title: Different encodings
weight: 40
draft: false
---

Even if files are not saved in UTF-8, you can can extract information on character encoding from the file names and import the texts correctly.


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
##    doc_id                             text      document language input_encoding
##    <chr>                              <chr>     <chr>    <chr>    <chr>         
##  1 IndianTreaty_English_UTF-16LE.txt  "\"WHERE… IndianT… English  UTF-16LE      
##  2 IndianTreaty_English_UTF-8-BOM.txt "\"ARTIC… IndianT… English  UTF-8-BOM     
##  3 UDHR_Arabic_ISO-8859-6.txt         "\"الديب… UDHR     Arabic   ISO-8859-6    
##  4 UDHR_Arabic_UTF-8.txt              "\"الديب… UDHR     Arabic   UTF-8         
##  5 UDHR_Arabic_WINDOWS-1256.txt       "\"الديب… UDHR     Arabic   WINDOWS-1256  
##  6 UDHR_Chinese_GB2312.txt            "\"世界…  UDHR     Chinese  GB2312        
##  7 UDHR_Chinese_GBK.txt               "\"世界…  UDHR     Chinese  GBK           
##  8 UDHR_Chinese_UTF-8.txt             "\"世界…  UDHR     Chinese  UTF-8         
##  9 UDHR_English_UTF-16BE.txt          "\"Unive… UDHR     English  UTF-16BE      
## 10 UDHR_English_UTF-16LE.txt          "\"Unive… UDHR     English  UTF-16LE      
## 11 UDHR_English_UTF-8.txt             "\"Unive… UDHR     English  UTF-8         
## 12 UDHR_English_WINDOWS-1252.txt      "\"Unive… UDHR     English  WINDOWS-1252  
## 13 UDHR_French_ISO-8859-1.txt         "\"Décla… UDHR     French   ISO-8859-1    
## 14 UDHR_French_UTF-8.txt              "\"Décla… UDHR     French   UTF-8         
## 15 UDHR_French_WINDOWS-1252.txt       "\"Décla… UDHR     French   WINDOWS-1252  
## 16 UDHR_German_ISO-8859-1.txt         "\"Die A… UDHR     German   ISO-8859-1    
## 17 UDHR_German_UTF-8.txt              "\"Die A… UDHR     German   UTF-8         
## 18 UDHR_German_WINDOWS-1252.txt       "\"Die A… UDHR     German   WINDOWS-1252  
## 19 UDHR_Greek_CP1253.txt              "\"ΟΙΚΟΥ… UDHR     Greek    CP1253        
## 20 UDHR_Greek_ISO-8859-7.txt          "\"ΟΙΚΟΥ… UDHR     Greek    ISO-8859-7    
## 21 UDHR_Greek_UTF-8.txt               "\"ΟΙΚΟΥ… UDHR     Greek    UTF-8         
## 22 UDHR_Hindi_UTF-8.txt               "\"मानव … UDHR     Hindi    UTF-8         
## 23 UDHR_Icelandic_ISO-8859-1.txt      "\"Mannr… UDHR     Iceland… ISO-8859-1    
## 24 UDHR_Icelandic_UTF-8.txt           "\"Mannr… UDHR     Iceland… UTF-8         
## 25 UDHR_Icelandic_WINDOWS-1252.txt    "\"Mannr… UDHR     Iceland… WINDOWS-1252  
## 26 UDHR_Japanese_CP932.txt            "\"『世…  UDHR     Japanese CP932         
## 27 UDHR_Japanese_ISO-2022-JP.txt      "\"『世…  UDHR     Japanese ISO-2022-JP   
## 28 UDHR_Japanese_UTF-8.txt            "\"『世…  UDHR     Japanese UTF-8         
## 29 UDHR_Japanese_WINDOWS-936.txt      "\"『世…  UDHR     Japanese WINDOWS-936   
## 30 UDHR_Korean_ISO-2022-KR.txt        "\"세 계… UDHR     Korean   ISO-2022-KR   
## 31 UDHR_Korean_UTF-8.txt              "\"세 계… UDHR     Korean   UTF-8         
## 32 UDHR_Russian_ISO-8859-5.txt        "\"Всеоб… UDHR     Russian  ISO-8859-5    
## 33 UDHR_Russian_KOI8-R.txt            "\"Всеоб… UDHR     Russian  KOI8-R        
## 34 UDHR_Russian_UTF-8.txt             "\"Всеоб… UDHR     Russian  UTF-8         
## 35 UDHR_Russian_WINDOWS-1251.txt      "\"Всеоб… UDHR     Russian  WINDOWS-1251  
## 36 UDHR_Thai_UTF-8.txt                "\"ปฏิญญา… UDHR     Thai     UTF-8
```
