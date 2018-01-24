---
title: Reading multiple text files
weight: 10
chapter: false
draft: true
---




# 2. Reading one or more text files

## 2.1 Plain text files (.txt)

The folder "txt" contains a subfolder named UDHR with .txt files of the Universal Declaration of Human Rights in 13 languages. 


```r
# Read in all files from a folder
readtext(paste0(DATA_DIR, "/txt/UDHR/*"))
## readtext object consisting of 13 documents and 0 docvars.
## # data.frame [13 x 2]
##              doc_id                          text
##               <chr>                         <chr>
## 1  UDHR_chinese.txt "\"世界人权宣言\n联合国\"..."
## 2    UDHR_czech.txt           "\"VŠEOBECNÁ \"..."
## 3   UDHR_danish.txt           "\"Den 10. de\"..."
## 4  UDHR_english.txt           "\"Universal \"..."
## 5   UDHR_french.txt           "\"Déclaratio\"..."
## 6 UDHR_georgian.txt           "\"FLFVBFYBC \"..."
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
## # data.frame [17 x 7]
##                    doc_id                 text  unit context  year
##                     <chr>                <chr> <chr>   <chr> <int>
## 1 EU_euro_2004_de_PSE.txt  "\"PES · PSE \"..."    EU    euro  2004
## 2   EU_euro_2004_de_V.txt  "\"Gemeinsame\"..."    EU    euro  2004
## 3 EU_euro_2004_en_PSE.txt  "\"PES · PSE \"..."    EU    euro  2004
## 4   EU_euro_2004_en_V.txt "\"Manifesto\n\"..."    EU    euro  2004
## 5 EU_euro_2004_es_PSE.txt  "\"PES · PSE \"..."    EU    euro  2004
## 6   EU_euro_2004_es_V.txt "\"Manifesto\n\"..."    EU    euro  2004
## # ... with 11 more rows, and 2 more variables: language <chr>, party <chr>
```

**readtext** can also curse through subdirectories. In our example, the folder `txt/movie_reviews` contains two subfolders (called `neg` and `pos`). We can load all texts included in both folders. 


```r
# Recurse through subdirectories
readtext(paste0(DATA_DIR, "/txt/movie_reviews/*"))
## readtext object consisting of 10 documents and 0 docvars.
## # data.frame [10 x 2]
##                    doc_id                 text
##                     <chr>                <chr>
## 1 neg/neg_cv000_29416.txt  "\"plot : two\"..."
## 2 neg/neg_cv001_19502.txt  "\"the happy \"..."
## 3 neg/neg_cv002_17424.txt  "\"it is movi\"..."
## 4 neg/neg_cv003_12683.txt "\" \" quest f\"..."
## 5 neg/neg_cv004_12641.txt  "\"synopsis :\"..."
## 6 pos/pos_cv000_29590.txt  "\"films adap\"..."
## # ... with 4 more rows
```

## 2.2 Comma- or tab-separated values (.csv, .tab, .tsv)

Read in comma separted values (.csv files) that contain textual data. We determine the `texts` variable in our .csv file as the `text_field`. This is the column that contains the actual text. The other columns of the original csv file (`Year`, `President`, `FirstName`) are by default treated as document-level variables. 


```r
# Read in comma-separated values
readtext(paste0(DATA_DIR, "/csv/inaugCorpus.csv"), text_field = "texts")
## readtext object consisting of 5 documents and 3 docvars.
## # data.frame [5 x 5]
##              doc_id                text  Year  President FirstName
##               <chr>               <chr> <int>      <chr>     <chr>
## 1 inaugCorpus.csv.1 "\"Fellow-Cit\"..."  1789 Washington    George
## 2 inaugCorpus.csv.2 "\"Fellow cit\"..."  1793 Washington    George
## 3 inaugCorpus.csv.3 "\"When it wa\"..."  1797      Adams      John
## 4 inaugCorpus.csv.4 "\"Friends an\"..."  1801  Jefferson    Thomas
## 5 inaugCorpus.csv.5 "\"Proceeding\"..."  1805  Jefferson    Thomas
```

The same procedure applies to tab-separated values.


```r
# Read in tab-separated values
readtext(paste0(DATA_DIR, "/tsv/dailsample.tsv"), text_field = "speech")
## readtext object consisting of 33 documents and 9 docvars.
## # data.frame [33 x 11]
##             doc_id                text speechID memberID partyID constID
##              <chr>               <chr>    <int>    <int>   <int>   <int>
## 1 dailsample.tsv.1 "\"Molaimse d\"..."        1      977      22     158
## 2 dailsample.tsv.2 "\"Is bród mó\"..."        2     1603      22     103
## 3 dailsample.tsv.3 "\"' A cháird\"..."        3      116      22     178
## 4 dailsample.tsv.4 "\"Tá ceathra\"..."        4      116      22     178
## 5 dailsample.tsv.5 "\"Léighfead \"..."        5      116      22     178
## 6 dailsample.tsv.6 "\"-Braithean\"..."        6      116      22     178
## # ... with 27 more rows, and 5 more variables: title <chr>, date <chr>,
## #   member_name <chr>, party_name <chr>, const_name <chr>
```

## 2.3 JSON data (.json)

You can also read .json data. Again you need to specify the `text_field`. 


```r
## Read in JSON data
readtext(paste0(DATA_DIR, "/json/inaugural_sample.json"), text_field = "texts")
## Warning in doTryCatch(return(expr), name, parentenv, handler): Doesn't look
## like Tweets json file, trying general JSON
## readtext object consisting of 3 documents and 3 docvars.
## # data.frame [3 x 5]
##                    doc_id                text  Year  President FirstName
##                     <chr>               <chr> <int>      <chr>     <chr>
## 1 inaugural_sample.json.1 "\"Fellow-Cit\"..."  1789 Washington    George
## 2 inaugural_sample.json.2 "\"Fellow cit\"..."  1793 Washington    George
## 3 inaugural_sample.json.3 "\"When it wa\"..."  1797      Adams      John
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
## # data.frame [11 x 4]
##             doc_id                          text document language
##              <chr>                         <chr>    <chr>    <chr>
## 1 UDHR_chinese.pdf "\"世界人权宣言\n联合国\"..."     UDHR  chinese
## 2   UDHR_czech.pdf           "\"VŠEOBECNÁ \"..."     UDHR    czech
## 3  UDHR_danish.pdf           "\"Den 10. de\"..."     UDHR   danish
## 4 UDHR_english.pdf           "\"Universal \"..."     UDHR  english
## 5  UDHR_french.pdf           "\"Déclaratio\"..."     UDHR   french
## 6   UDHR_greek.pdf           "\"ΟΙΚΟΥΜΕΝΙΚ\"..."     UDHR    greek
## # ... with 5 more rows
```


## 2.5 Microsoft Word files (.doc, .docx)

Microsoft Word formatted files are converted through the package **antiword** for older `.doc` files, and using **XML** for newer `.docx` files.


```r
## Read in Word data (.docx)
readtext(paste0(DATA_DIR, "/word/*.docx"))
## readtext object consisting of 2 documents and 0 docvars.
## # data.frame [2 x 2]
##                        doc_id                text
##                         <chr>               <chr>
## 1 UK_2015_EccentricParty.docx "\"The Eccent\"..."
## 2     UK_2015_LoonyParty.docx "\"The Offici\"..."
```
