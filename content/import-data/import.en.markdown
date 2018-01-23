---
title: Data import
weight: 10
chapter: false
draft: true
---



## Getting texts into R

In this section we will show how to load texts from different sources and create a `corpus` object in **quanteda**.

## Creating a `corpus` object

**quanteda can construct a `corpus` object** from several input sources:

###  a character vector object  

```r
require(quanteda, warn.conflicts = FALSE, quietly = TRUE)
myCorpus <- corpus(data_char_ukimmig2010, notes = "My first corpus")
## Warning in corpus.character(data_char_ukimmig2010, notes = "My first
## corpus"): Argument notes not used.
summary(myCorpus)
## Corpus consisting of 9 documents:
## 
##          Text Types Tokens Sentences
##           BNP  1125   3280        88
##     Coalition   142    260         4
##  Conservative   251    499        15
##        Greens   322    679        21
##        Labour   298    683        29
##        LibDem   251    483        14
##            PC    77    114         5
##           SNP    88    134         4
##          UKIP   346    723        27
## 
## Source:  /home/kohei/packages/quanteda_tutorials/content/import-data/* on x86_64 by kohei
## Created: Tue Jan 23 19:40:14 2018
## Notes:
```
    
###  a `VCorpus` object from the **tm** package, and

```r
data(crude, package = "tm")
myTmCorpus <- corpus(crude)
summary(myTmCorpus, 5)
## Corpus consisting of 20 documents, showing 5 documents:
## 
##  Text Types Tokens Sentences       datetimestamp description
##   127    62    103         5 1987-02-26 17:00:56            
##   144   238    495        19 1987-02-26 17:34:11            
##   191    47     62         4 1987-02-26 18:18:00            
##   194    55     74         5 1987-02-26 18:21:01            
##   211    67     97         4 1987-02-26 19:00:57            
##                                          heading  id language
##         DIAMOND SHAMROCK (DIA) CUTS CRUDE PRICES 127       en
##  OPEC MAY HAVE TO MEET TO FIRM PRICES - ANALYSTS 144       en
##        TEXACO CANADA <TXC> LOWERS CRUDE POSTINGS 191       en
##        MARATHON PETROLEUM REDUCES CRUDE POSTINGS 194       en
##        HOUSTON OIL <HO> RESERVES STUDY COMPLETED 211       en
##             origin topics lewissplit     cgisplit oldid places
##  Reuters-21578 XML    YES      TRAIN TRAINING-SET  5670    usa
##  Reuters-21578 XML    YES      TRAIN TRAINING-SET  5687    usa
##  Reuters-21578 XML    YES      TRAIN TRAINING-SET  5734 canada
##  Reuters-21578 XML    YES      TRAIN TRAINING-SET  5737    usa
##  Reuters-21578 XML    YES      TRAIN TRAINING-SET  5754    usa
##                      author orgs people exchanges
##                        <NA> <NA>   <NA>      <NA>
##  BY TED D'AFFLISIO, Reuters opec   <NA>      <NA>
##                        <NA> <NA>   <NA>      <NA>
##                        <NA> <NA>   <NA>      <NA>
##                        <NA> <NA>   <NA>      <NA>
## 
## Source:  Converted from tm Corpus 'crude'
## Created: Tue Jan 23 19:40:14 2018
## Notes:
```

### using `readtext()` to import texts

The vignette walks you through importing a variety of different text files into R using the **readtext** package. Currently, **readtext** supports plain text files (.txt), data in some form of JavaScript Object Notation (.json), comma-or tab-separated values (.csv, .tab, .tsv), XML documents (.xml), as well as PDF and Microsoft Word formatted files (.pdf, .doc, .docx). The **quanteda** package works nicely with a companion package we have written named, descriptively, [**readtext**](https://github.com/kbenoit/readtext).  **readtext** is a one-function package that does exactly what it says on the tin: It reads files containing text, along with any associated document-level metadata, which we call "docvars", for document variables.  

**readtext** accepts filemasks, so that you can specify a pattern to load multiple texts from one or more folders, and these texts can even be of multiple types.  **readtext** is smart enough to process them correctly, returning a data.frame with a primary field "text" containing a character vector of the texts, and additional columns of the data.frame as found in the document variables from the source files.

As encoding can also be a challenging issue for those reading in texts, we include functions for diagnosing encodings on a file-by-file basis, and allow you to specify vectorized input encodings to read in file types with individually set (and different) encodings.  (All encoding functions are handled by the **stringi** package.) Because **quanteda**'s corpus constructor recognizes the data.frame format returned by `readtext()`, it can construct a corpus directly from a `readtext` object, preserving all docvars and other meta-data.

**readtext** also handles multiple files and file types using for instance a "glob" expression, files from a URL or an archive file (.zip, .tar, .tar.gz, .tar.bz). Usually, you do not have to determine the format of the files explicitly - **readtext** takes this information from the file ending.

The **readtext** package comes with a data directory called `extdata` that contains examples of all files listed above. In the vignette, we use this data directory.


```r
# Load the readtext package
library(readtext)

# Get the data directory from readtext
DATA_DIR <- system.file("extdata/", package = "readtext")
```

The `extdata` directory contains several subfolders that include different text files. In the following examples, we load one or more files stored in each of these folders. The `paste0` command is used to concatenate the `extdata` folder from the 
**readtext** package with the subfolders. When reading in custom text files, you will need to determine your own data directory (see `?setwd()`). Alternatively, if you use RStudio you can create a Project (File -> New Project) and put the textual raw data in a separate folder in the Project folder. Then you can simply start the .Rproj file and you do not have to specify the full working directory. 

# 2. Reading one or more text files

## 2.1 Plain text files (.txt)

The folder "txt" contains a subfolder named UDHR with .txt files of the Universal Declaration of Human Rights in 13 languages. 


```r
# Read in all files from a folder
readtext(paste0(DATA_DIR, "/txt/UDHR/*"))
## readtext object consisting of 13 documents and 0 docvars.
## # data.frame [13 × 2]
##   doc_id            text                         
##   <chr>             <chr>                        
## 1 UDHR_chinese.txt  "\"世界人权宣言\n联合国\"..."
## 2 UDHR_czech.txt    "\"VŠEOBECNÁ \"..."          
## 3 UDHR_danish.txt   "\"Den 10. de\"..."          
## 4 UDHR_english.txt  "\"Universal \"..."          
## 5 UDHR_french.txt   "\"Déclaratio\"..."          
## 6 UDHR_georgian.txt "\"FLFVBFYBC \"..."          
## # ... with 7 more rows
```

We can specify document-level metadata (`docvars`) based on the file names or on a separate data.frame. Below we take the docvars from the filenames (`docvarsfrom = "filenames"`) and set the names for each variable (`docvarnames = c("unit", "context", "year", "language", "party")`). The command `dvsep = "_"` determines the separator (a regular expression character string) included in the filenames to delimit the `docvar` elements. `encoding = "ISO-8859-1"` specifies the encoding of the text. Below we describe how you can find out how your text is encoded.


```r
# Manifestos with docvars from filenames
readtext(paste0(DATA_DIR, "/txt/EU_manifestos/*.txt"),
         docvarsfrom = "filenames", 
         docvarnames = c("unit", "context", "year", "language", "party"),
         dvsep = "_", 
         encoding = "ISO-8859-1")
## readtext object consisting of 17 documents and 5 docvars.
## # data.frame [17 × 7]
##   doc_id                  text            unit  context  year langu… party
##   <chr>                   <chr>           <chr> <chr>   <int> <chr>  <chr>
## 1 EU_euro_2004_de_PSE.txt "\"PES · PSE \… EU    euro     2004 de     PSE  
## 2 EU_euro_2004_de_V.txt   "\"Gemeinsame\… EU    euro     2004 de     V    
## 3 EU_euro_2004_en_PSE.txt "\"PES · PSE \… EU    euro     2004 en     PSE  
## 4 EU_euro_2004_en_V.txt   "\"Manifesto\n… EU    euro     2004 en     V    
## 5 EU_euro_2004_es_PSE.txt "\"PES · PSE \… EU    euro     2004 es     PSE  
## 6 EU_euro_2004_es_V.txt   "\"Manifesto\n… EU    euro     2004 es     V    
## # ... with 11 more rows
```

**readtext** can also curse through subdirectories. In our example, the folder `txt/movie_reviews` contains two subfolders (called `neg` and `pos`). We can load all texts included in both folders. 


```r
# Recurse through subdirectories
readtext(paste0(DATA_DIR, "/txt/movie_reviews/*"))
## readtext object consisting of 10 documents and 0 docvars.
## # data.frame [10 × 2]
##   doc_id                  text                
##   <chr>                   <chr>               
## 1 neg/neg_cv000_29416.txt "\"plot : two\"..." 
## 2 neg/neg_cv001_19502.txt "\"the happy \"..." 
## 3 neg/neg_cv002_17424.txt "\"it is movi\"..." 
## 4 neg/neg_cv003_12683.txt "\" \" quest f\"..."
## 5 neg/neg_cv004_12641.txt "\"synopsis :\"..." 
## 6 pos/pos_cv000_29590.txt "\"films adap\"..." 
## # ... with 4 more rows
```

## 2.2 Comma- or tab-separated values (.csv, .tab, .tsv)

Read in comma separted values (.csv files) that contain textual data. We determine the `texts` variable in our .csv file as the `text_field`. This is the column that contains the actual text. The other columns of the original csv file (`Year`, `President`, `FirstName`) are by default treated as document-level variables. 


```r
# Read in comma-separated values
readtext(paste0(DATA_DIR, "/csv/inaugCorpus.csv"), text_field = "texts")
## readtext object consisting of 5 documents and 3 docvars.
## # data.frame [5 × 5]
##   doc_id            text                 Year President  FirstName
##   <chr>             <chr>               <int> <chr>      <chr>    
## 1 inaugCorpus.csv.1 "\"Fellow-Cit\"..."  1789 Washington George   
## 2 inaugCorpus.csv.2 "\"Fellow cit\"..."  1793 Washington George   
## 3 inaugCorpus.csv.3 "\"When it wa\"..."  1797 Adams      John     
## 4 inaugCorpus.csv.4 "\"Friends an\"..."  1801 Jefferson  Thomas   
## 5 inaugCorpus.csv.5 "\"Proceeding\"..."  1805 Jefferson  Thomas
```

The same procedure applies to tab-separated values.


```r
# Read in tab-separated values
readtext(paste0(DATA_DIR, "/tsv/dailsample.tsv"), text_field = "speech")
## readtext object consisting of 33 documents and 9 docvars.
## # data.frame [33 × 11]
##   doc_id text   speec… membe… party… cons… title  date  membe… part… cons…
##   <chr>  <chr>   <int>  <int>  <int> <int> <chr>  <chr> <chr>  <chr> <chr>
## 1 dails… "\"Mo…      1    977     22   158 1. CE… 1919… Count… Sinn… Rosc…
## 2 dails… "\"Is…      2   1603     22   103 1. CE… 1919… Mr. P… Sinn… Galw…
## 3 dails… "\"' …      3    116     22   178 1. CE… 1919… Mr. C… Sinn… Wate…
## 4 dails… "\"Tá…      4    116     22   178 2. CL… 1919… Mr. C… Sinn… Wate…
## 5 dails… "\"Lé…      5    116     22   178 3. AN… 1919… Mr. C… Sinn… Wate…
## 6 dails… "\"-B…      6    116     22   178 3. AN… 1919… Mr. C… Sinn… Wate…
## # ... with 27 more rows
```

## 2.3 JSON data (.json)

You can also read .json data. Again you need to specify the `text_field`. 


```r
## Read in JSON data
readtext(paste0(DATA_DIR, "/json/inaugural_sample.json"), text_field = "texts")
## Warning in doTryCatch(return(expr), name, parentenv, handler): Doesn't look
## like Tweets json file, trying general JSON
## readtext object consisting of 3 documents and 3 docvars.
## # data.frame [3 × 5]
##   doc_id                  text                 Year President  FirstName
##   <chr>                   <chr>               <int> <chr>      <chr>    
## 1 inaugural_sample.json.1 "\"Fellow-Cit\"..."  1789 Washington George   
## 2 inaugural_sample.json.2 "\"Fellow cit\"..."  1793 Washington George   
## 3 inaugural_sample.json.3 "\"When it wa\"..."  1797 Adams      John
```

## 2.4 PDF files

**readtext** can also read in and convert .pdf files. In the example below we load all .pdf files stored in the `UDHR` folder, and determine that the `docvars` shall be taken from the filenames. We call the document-level variables `document` and `language`, and specify the delimiter (`dvsep`).


```r
## Read in Universal Declaration of Human Rights pdf files
(rt_pdf <- readtext(paste0(DATA_DIR, "/pdf/UDHR/*.pdf"), 
                    docvarsfrom = "filenames", 
                    docvarnames = c("document", "language"),
                    sep = "_"))
## readtext object consisting of 11 documents and 2 docvars.
## # data.frame [11 × 4]
##   doc_id           text                          document language
##   <chr>            <chr>                         <chr>    <chr>   
## 1 UDHR_chinese.pdf "\"世界人权宣言\n联合国\"..." UDHR     chinese 
## 2 UDHR_czech.pdf   "\"VŠEOBECNÁ \"..."           UDHR     czech   
## 3 UDHR_danish.pdf  "\"Den 10. de\"..."           UDHR     danish  
## 4 UDHR_english.pdf "\"Universal \"..."           UDHR     english 
## 5 UDHR_french.pdf  "\"Déclaratio\"..."           UDHR     french  
## 6 UDHR_greek.pdf   "\"ΟΙΚΟΥΜΕΝΙΚ\"..."           UDHR     greek   
## # ... with 5 more rows
```


## 2.5 Microsoft Word files (.doc, .docx)

Microsoft Word formatted files are converted through the package **antiword** for older `.doc` files, and using **XML** for newer `.docx` files.


```r
## Read in Word data (.docx)
readtext(paste0(DATA_DIR, "/word/*.docx"))
## readtext object consisting of 2 documents and 0 docvars.
## # data.frame [2 × 2]
##   doc_id                      text               
##   <chr>                       <chr>              
## 1 UK_2015_EccentricParty.docx "\"The Eccent\"..."
## 2 UK_2015_LoonyParty.docx     "\"The Offici\"..."
```

## 2.6 Text from URLs

You can also read in text directly from a URL.


```r
# Note: Example required: which URL should we use?
```

## 2.7 Text from archive files (.zip, .tar, .tar.gz, .tar.bz)

Finally, it is possible to inclue text from archives. In the repository for this tutorial page, we include a zip archive of all State of the Union


```r
# Note: Archive file required. The only zip archive included in readtext has 
# different encodings and is difficult to import (see section 4.2).
#sotu <- readtext("content/basic/sotu.zip")
```

# 3. Inter-operability with quanteda

**readtext** was originally developed in early versions of the [**quanteda**](http://github.com/quanteda/quanteda) package for the quantitative analysis of textual data.  It was spawned from the `textfile()` function from that package, and now lives exclusively in **readtext**. Because **quanteda**'s corpus constructor recognizes the data.frame format returned by `readtext()`, it can construct a corpus directly from a `readtext` object, preserving all docvars and other meta-data.


```r
require(quanteda)
```

You can easily contruct a corpus from a **readtext** object.


```r
# read in comma-separated values with readtext
rt_csv <- readtext(paste0(DATA_DIR, "/csv/inaugCorpus.csv"), text_field = "texts")

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
## Source:  /home/kohei/packages/quanteda_tutorials/content/import-data/* on x86_64 by kohei
## Created: Tue Jan 23 19:40:16 2018
## Notes:
```

## 4. Read files with different encodings

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
txts <- readtext(paste0(DATA_DIR, "/data_files_encodedtexts.zip"), 
                 encoding = fileencodings,
                 docvarsfrom = "filenames", 
                 docvarnames = c("document", "language", "input_encoding"))
print(txts, n = 50)
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
## Source:  /home/kohei/packages/quanteda_tutorials/content/import-data/* on x86_64 by kohei
## Created: Tue Jan 23 19:40:16 2018
## Notes:
```

