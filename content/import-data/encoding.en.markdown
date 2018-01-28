---
title: Different encodings
weight: 40
draft: false
---

Even if files are not saved in UTF8, you can can extract encoding information from the file names and import the texts correctly.


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

You can extract chracter encoding from the file using the folloing command. 


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

You then pass `encoding` to `readtext()` to correctly convert character strings into UTF8.


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
##    doc_id              text             document   language input_encoding
##    <chr>               <chr>            <chr>      <chr>    <chr>         
##  1 IndianTreaty_Engli… "\"WHEREAS, t\"… IndianTre… English  UTF-16LE      
##  2 IndianTreaty_Engli… "\"ARTICLE 1.\"… IndianTre… English  UTF-8-BOM     
##  3 UDHR_Arabic_ISO-88… "\"الديباجة\nل\… UDHR       Arabic   ISO-8859-6    
##  4 UDHR_Arabic_UTF-8.… "\"الديباجة\nل\… UDHR       Arabic   UTF-8         
##  5 UDHR_Arabic_WINDOW… "\"الديباجة\nل\… UDHR       Arabic   WINDOWS-1256  
##  6 UDHR_Chinese_GB231… "\"世界人权宣言\n联合国\… UDHR       Chinese  GB2312        
##  7 UDHR_Chinese_GBK.t… "\"世界人权宣言\n联合国\… UDHR       Chinese  GBK           
##  8 UDHR_Chinese_UTF-8… "\"世界人权宣言\n联合国\… UDHR       Chinese  UTF-8         
##  9 UDHR_English_UTF-1… "\"Universal \"… UDHR       English  UTF-16BE      
## 10 UDHR_English_UTF-1… "\"Universal \"… UDHR       English  UTF-16LE      
## 11 UDHR_English_UTF-8… "\"Universal \"… UDHR       English  UTF-8         
## 12 UDHR_English_WINDO… "\"Universal \"… UDHR       English  WINDOWS-1252  
## 13 UDHR_French_ISO-88… "\"Déclaratio\"… UDHR       French   ISO-8859-1    
## 14 UDHR_French_UTF-8.… "\"Déclaratio\"… UDHR       French   UTF-8         
## 15 UDHR_French_WINDOW… "\"Déclaratio\"… UDHR       French   WINDOWS-1252  
## 16 UDHR_German_ISO-88… "\"Die Allgem\"… UDHR       German   ISO-8859-1    
## 17 UDHR_German_UTF-8.… "\"Die Allgem\"… UDHR       German   UTF-8         
## 18 UDHR_German_WINDOW… "\"Die Allgem\"… UDHR       German   WINDOWS-1252  
## 19 UDHR_Greek_CP1253.… "\"ΟΙΚΟΥΜΕΝΙΚ\"… UDHR       Greek    CP1253        
## 20 UDHR_Greek_ISO-885… "\"ΟΙΚΟΥΜΕΝΙΚ\"… UDHR       Greek    ISO-8859-7    
## 21 UDHR_Greek_UTF-8.t… "\"ΟΙΚΟΥΜΕΝΙΚ\"… UDHR       Greek    UTF-8         
## 22 UDHR_Hindi_UTF-8.t… "\"मानव अधिका\"… UDHR       Hindi    UTF-8         
## 23 UDHR_Icelandic_ISO… "\"Mannréttin\"… UDHR       Iceland… ISO-8859-1    
## 24 UDHR_Icelandic_UTF… "\"Mannréttin\"… UDHR       Iceland… UTF-8         
## 25 UDHR_Icelandic_WIN… "\"Mannréttin\"… UDHR       Iceland… WINDOWS-1252  
## 26 UDHR_Japanese_CP93… "\"『世界人権宣言』\n \… UDHR       Japanese CP932         
## 27 UDHR_Japanese_ISO-… "\"『世界人権宣言』\n \… UDHR       Japanese ISO-2022-JP   
## 28 UDHR_Japanese_UTF-… "\"『世界人権宣言』\n \… UDHR       Japanese UTF-8         
## 29 UDHR_Japanese_WIND… "\"『世界人権宣言』\n \… UDHR       Japanese WINDOWS-936   
## 30 UDHR_Korean_ISO-20… "\"세 계 인 권 선 \"… UDHR       Korean   ISO-2022-KR   
## 31 UDHR_Korean_UTF-8.… "\"세 계 인 권 선 \"… UDHR       Korean   UTF-8         
## 32 UDHR_Russian_ISO-8… "\"Всеобщая д\"… UDHR       Russian  ISO-8859-5    
## 33 UDHR_Russian_KOI8-… "\"Всеобщая д\"… UDHR       Russian  KOI8-R        
## 34 UDHR_Russian_UTF-8… "\"Всеобщая д\"… UDHR       Russian  UTF-8         
## 35 UDHR_Russian_WINDO… "\"Всеобщая д\"… UDHR       Russian  WINDOWS-1251  
## 36 UDHR_Thai_UTF-8.txt "\"ปฏิญญาสากล\"…  UDHR       Thai     UTF-8
```
