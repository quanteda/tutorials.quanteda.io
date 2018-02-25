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
##  [1] "UTF-8-BOM"    "ISO-8859-6"   "WINDOWS-1256" "GB2312"      
##  [5] "WINDOWS-1252" "ISO-8859-7"   "ISO-2022-KR"  "ISO-8859-5"  
##  [9] "KOI8-R"       "WINDOWS-1251"
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
## # data.frame [36 x 5]
##    doc_id              text             document   language input_encoding
##    <chr>               <chr>            <chr>      <chr>    <chr>         
##  1 IndianTreaty_Engli~ "\"WHEREAS, t\"~ IndianTre~ English  UTF-16LE      
##  2 IndianTreaty_Engli~ "\"ARTICLE 1.\"~ IndianTre~ English  UTF-8-BOM     
##  3 UDHR_Arabic_ISO-88~ "\"<U+0627><U+0644><U+062F><U+064A><U+0628><U+0627><U+062C><U+0629>\n<U+0644>\~ UDHR       Arabic   ISO-8859-6    
##  4 UDHR_Arabic_UTF-8.~ "\"<U+0627><U+0644><U+062F><U+064A><U+0628><U+0627><U+062C><U+0629>\n<U+0644>\~ UDHR       Arabic   UTF-8         
##  5 UDHR_Arabic_WINDOW~ "\"<U+0627><U+0644><U+062F><U+064A><U+0628><U+0627><U+062C><U+0629>\n<U+0644>\~ UDHR       Arabic   WINDOWS-1256  
##  6 UDHR_Chinese_GB231~ "\"<U+4E16><U+754C><U+4EBA><U+6743><U+5BA3><U+8A00>\n<U+8054><U+5408><U+56FD>\~ UDHR       Chinese  GB2312        
##  7 UDHR_Chinese_GBK.t~ "\"<U+4E16><U+754C><U+4EBA><U+6743><U+5BA3><U+8A00>\n<U+8054><U+5408><U+56FD>\~ UDHR       Chinese  GBK           
##  8 UDHR_Chinese_UTF-8~ "\"<U+4E16><U+754C><U+4EBA><U+6743><U+5BA3><U+8A00>\n<U+8054><U+5408><U+56FD>\~ UDHR       Chinese  UTF-8         
##  9 UDHR_English_UTF-1~ "\"Universal \"~ UDHR       English  UTF-16BE      
## 10 UDHR_English_UTF-1~ "\"Universal \"~ UDHR       English  UTF-16LE      
## 11 UDHR_English_UTF-8~ "\"Universal \"~ UDHR       English  UTF-8         
## 12 UDHR_English_WINDO~ "\"Universal \"~ UDHR       English  WINDOWS-1252  
## 13 UDHR_French_ISO-88~ "\"Déclaratio\"~ UDHR       French   ISO-8859-1    
## 14 UDHR_French_UTF-8.~ "\"Déclaratio\"~ UDHR       French   UTF-8         
## 15 UDHR_French_WINDOW~ "\"Déclaratio\"~ UDHR       French   WINDOWS-1252  
## 16 UDHR_German_ISO-88~ "\"Die Allgem\"~ UDHR       German   ISO-8859-1    
## 17 UDHR_German_UTF-8.~ "\"Die Allgem\"~ UDHR       German   UTF-8         
## 18 UDHR_German_WINDOW~ "\"Die Allgem\"~ UDHR       German   WINDOWS-1252  
## 19 UDHR_Greek_CP1253.~ "\"<U+039F><U+0399><U+039A><U+039F><U+03A5><U+039C><U+0395><U+039D><U+0399><U+039A>\"~ UDHR       Greek    CP1253        
## 20 UDHR_Greek_ISO-885~ "\"<U+039F><U+0399><U+039A><U+039F><U+03A5><U+039C><U+0395><U+039D><U+0399><U+039A>\"~ UDHR       Greek    ISO-8859-7    
## 21 UDHR_Greek_UTF-8.t~ "\"<U+039F><U+0399><U+039A><U+039F><U+03A5><U+039C><U+0395><U+039D><U+0399><U+039A>\"~ UDHR       Greek    UTF-8         
## 22 UDHR_Hindi_UTF-8.t~ "\"<U+092E><U+093E><U+0928><U+0935> <U+0905><U+0927><U+093F><U+0915><U+093E>\"~ UDHR       Hindi    UTF-8         
## 23 UDHR_Icelandic_ISO~ "\"Mannréttin\"~ UDHR       Iceland~ ISO-8859-1    
## 24 UDHR_Icelandic_UTF~ "\"Mannréttin\"~ UDHR       Iceland~ UTF-8         
## 25 UDHR_Icelandic_WIN~ "\"Mannréttin\"~ UDHR       Iceland~ WINDOWS-1252  
## 26 UDHR_Japanese_CP93~ "\"<U+300E><U+4E16><U+754C><U+4EBA><U+6A29><U+5BA3><U+8A00><U+300F>\n \~ UDHR       Japanese CP932         
## 27 UDHR_Japanese_ISO-~ "\"<U+300E><U+4E16><U+754C><U+4EBA><U+6A29><U+5BA3><U+8A00><U+300F>\n \~ UDHR       Japanese ISO-2022-JP   
## 28 UDHR_Japanese_UTF-~ "\"<U+300E><U+4E16><U+754C><U+4EBA><U+6A29><U+5BA3><U+8A00><U+300F>\n \~ UDHR       Japanese UTF-8         
## 29 UDHR_Japanese_WIND~ "\"<U+300E><U+4E16><U+754C><U+4EBA><U+6A29><U+5BA3><U+8A00><U+300F>\n \~ UDHR       Japanese WINDOWS-936   
## 30 UDHR_Korean_ISO-20~ "\"n\"..."       UDHR       Korean   ISO-2022-KR   
## 31 UDHR_Korean_UTF-8.~ "\"<U+C138> <U+ACC4> <U+C778> <U+AD8C> <U+C120> \"~ UDHR       Korean   UTF-8         
## 32 UDHR_Russian_ISO-8~ "\"<U+0412><U+0441><U+0435><U+043E><U+0431><U+0449><U+0430><U+044F> <U+0434>\"~ UDHR       Russian  ISO-8859-5    
## 33 UDHR_Russian_KOI8-~ "\"<U+0412><U+0441><U+0435><U+043E><U+0431><U+0449><U+0430><U+044F> <U+0434>\"~ UDHR       Russian  KOI8-R        
## 34 UDHR_Russian_UTF-8~ "\"<U+0412><U+0441><U+0435><U+043E><U+0431><U+0449><U+0430><U+044F> <U+0434>\"~ UDHR       Russian  UTF-8         
## 35 UDHR_Russian_WINDO~ "\"<U+0412><U+0441><U+0435><U+043E><U+0431><U+0449><U+0430><U+044F> <U+0434>\"~ UDHR       Russian  WINDOWS-1251  
## 36 UDHR_Thai_UTF-8.txt "\"<U+0E1B><U+0E0F><U+0E34><U+0E0D><U+0E0D><U+0E32><U+0E2A><U+0E32><U+0E01><U+0E25>\"~  UDHR       Thai     UTF-8
```
