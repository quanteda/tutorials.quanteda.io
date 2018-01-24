---
title: Multiple text files
weight: 20
chapter: false
draft: false
---


```r
knitr::opts_chunk$set(collapse = TRUE)
require(quanteda)

# Load the readtext package
require(readtext)

# Get the data directory from readtext
data_dir <- system.file("extdata/", package = "readtext")
```

## Plain text files (.txt)

The folder "txt" contains a subfolder named UDHR with .txt files of the Universal Declaration of Human Rights in 13 languages. 


```r
# Read in all files from a folder
readtext(paste0(data_dir, "/txt/UDHR/*"))
## readtext object consisting of 13 documents and 0 docvars.
## # data.frame [13 x 2]
##   doc_id            text                                                  
##   <chr>             <chr>                                                 
## 1 UDHR_chinese.txt  "\"\u00e4\u00b8\u2013\u00e7\u2022\u0152\u00e4\u00ba\u~
## 2 UDHR_czech.txt    "\"V\u00c5\u00a0EOBECN\u00c3\"..."                    
## 3 UDHR_danish.txt   "\"Den 10. de\"..."                                   
## 4 UDHR_english.txt  "\"Universal \"..."                                   
## 5 UDHR_french.txt   "\"D\u00c3\u00a9clarati\"..."                         
## 6 UDHR_georgian.txt "\"FLFVBFYBC \"..."                                   
## # ... with 7 more rows
```

We can specify document-level metadata (`docvars`) based on the file names or on a separate data.frame. Below we take the docvars from the filenames (`docvarsfrom = "filenames"`) and set the names for each variable (`docvarnames = c("unit", "context", "year", "language", "party")`). The command `dvsep = "_"` determines the separator (a regular expression character string) included in the filenames to delimit the `docvar` elements. `encoding = "ISO-8859-1"` specifies the encoding of the text. Below we describe how you can find out how your text is encoded.


```r
# Manifestos with docvars from filenames
readtext(paste0(data_dir, "/txt/EU_manifestos/*.txt"),
         docvarsfrom = "filenames", 
         docvarnames = c("unit", "context", "year", "language", "party"),
         dvsep = "_", 
         encoding = "ISO-8859-1")
## readtext object consisting of 17 documents and 5 docvars.
## # data.frame [17 x 7]
##   doc_id                  text             unit  conte~  year langu~ party
##   <chr>                   <chr>            <chr> <chr>  <int> <chr>  <chr>
## 1 EU_euro_2004_de_PSE.txt "\"PES \u00b7 P~ EU    euro    2004 de     PSE  
## 2 EU_euro_2004_de_V.txt   "\"Gemeinsame\"~ EU    euro    2004 de     V    
## 3 EU_euro_2004_en_PSE.txt "\"PES \u00b7 P~ EU    euro    2004 en     PSE  
## 4 EU_euro_2004_en_V.txt   "\"Manifesto\n\~ EU    euro    2004 en     V    
## 5 EU_euro_2004_es_PSE.txt "\"PES \u00b7 P~ EU    euro    2004 es     PSE  
## 6 EU_euro_2004_es_V.txt   "\"Manifesto\n\~ EU    euro    2004 es     V    
## # ... with 11 more rows
```

**readtext** can also curse through subdirectories. In our example, the folder `txt/movie_reviews` contains two subfolders (called `neg` and `pos`). We can load all texts included in both folders. 


```r
# Recurse through subdirectories
readtext(paste0(data_dir, "/txt/movie_reviews/*"))
## readtext object consisting of 10 documents and 0 docvars.
## # data.frame [10 x 2]
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

## Comma- or tab-separated values (.csv, .tab, .tsv)

Read in comma separted values (.csv files) that contain textual data. We determine the `texts` variable in our .csv file as the `text_field`. This is the column that contains the actual text. The other columns of the original csv file (`Year`, `President`, `FirstName`) are by default treated as document-level variables. 


```r
# Read in comma-separated values
readtext(paste0(data_dir, "/csv/inaugCorpus.csv"), text_field = "texts")
## readtext object consisting of 5 documents and 3 docvars.
## # data.frame [5 x 5]
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
readtext(paste0(data_dir, "/tsv/dailsample.tsv"), text_field = "speech")
## readtext object consisting of 33 documents and 9 docvars.
## # data.frame [33 x 11]
##   doc_id text    speec~ memb~ part~ cons~ title  date  member~ part~ cons~
##   <chr>  <chr>    <int> <int> <int> <int> <chr>  <chr> <chr>   <chr> <chr>
## 1 dails~ "\"Mol~      1   977    22   158 1. CE~ 1919~ Count ~ "Sin~ Rosc~
## 2 dails~ "\"Is ~      2  1603    22   103 1. CE~ 1919~ "Mr. P~ "Sin~ Galw~
## 3 dails~ "\"' A~      3   116    22   178 1. CE~ 1919~ Mr. Ca~ "Sin~ Wate~
## 4 dails~ "\"T\u~      4   116    22   178 2. CL~ 1919~ Mr. Ca~ "Sin~ Wate~
## 5 dails~ "\"L\u~      5   116    22   178 3. AN~ 1919~ Mr. Ca~ "Sin~ Wate~
## 6 dails~ "\"-Br~      6   116    22   178 3. AN~ 1919~ Mr. Ca~ "Sin~ Wate~
## # ... with 27 more rows
```

## JSON data (.json)

You can also read .json data. Again you need to specify the `text_field`. 


```r
## Read in JSON data
readtext(paste0(data_dir, "/json/inaugural_sample.json"), text_field = "texts")
## Warning in doTryCatch(return(expr), name, parentenv, handler): Doesn't look
## like Tweets json file, trying general JSON
## readtext object consisting of 3 documents and 3 docvars.
## # data.frame [3 x 5]
##   doc_id                  text                 Year President  FirstName
##   <chr>                   <chr>               <int> <chr>      <chr>    
## 1 inaugural_sample.json.1 "\"Fellow-Cit\"..."  1789 Washington George   
## 2 inaugural_sample.json.2 "\"Fellow cit\"..."  1793 Washington George   
## 3 inaugural_sample.json.3 "\"When it wa\"..."  1797 Adams      John
```

## PDF files

**readtext** can also read in and convert .pdf files. In the example below we load all .pdf files stored in the `UDHR` folder, and determine that the `docvars` shall be taken from the filenames. We call the document-level variables `document` and `language`, and specify the delimiter (`dvsep`).


```r
## Read in Universal Declaration of Human Rights pdf files
(rt_pdf <- readtext(paste0(data_dir, "/pdf/UDHR/*.pdf"), 
                    docvarsfrom = "filenames", 
                    docvarnames = c("document", "language"),
                    sep = "_"))
## readtext object consisting of 11 documents and 2 docvars.
## # data.frame [11 x 4]
##   doc_id           text                                     docume~ langu~
##   <chr>            <chr>                                    <chr>   <chr> 
## 1 UDHR_chinese.pdf "\"\u4e16\u754c\u4eba\u6743\u5ba3\u8a00~ UDHR    chine~
## 2 UDHR_czech.pdf   "\"V\u0160EOBECN\u00c1 \"..."            UDHR    czech 
## 3 UDHR_danish.pdf  "\"Den 10. de\"..."                      UDHR    danish
## 4 UDHR_english.pdf "\"Universal \"..."                      UDHR    engli~
## 5 UDHR_french.pdf  "\"D\u00e9claratio\"..."                 UDHR    french
## 6 UDHR_greek.pdf   "\"\u039f\u0399\u039a\u039f\u03a5\u039c~ UDHR    greek 
## # ... with 5 more rows
```


## Microsoft Word files (.doc, .docx)

Microsoft Word formatted files are converted through the package **antiword** for older `.doc` files, and using **XML** for newer `.docx` files.


```r
## Read in Word data (.docx)
readtext(paste0(data_dir, "/word/*.docx"))
## readtext object consisting of 2 documents and 0 docvars.
## # data.frame [2 x 2]
##   doc_id                      text               
##   <chr>                       <chr>              
## 1 UK_2015_EccentricParty.docx "\"The Eccent\"..."
## 2 UK_2015_LoonyParty.docx     "\"The Offici\"..."
```
