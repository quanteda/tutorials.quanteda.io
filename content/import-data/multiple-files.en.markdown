---
title: Multiple text files
weight: 20
draft: false
---


```r
require(quanteda)
require(readtext)
```

`data_dir`is the location of sample files in your computer.


```r
data_dir <- system.file("extdata/", package = "readtext")
```

Unlike the pre-formatted files, individual text files usually do not contain document-level variables. However, you can created document variables using the **readtext** package.


The directory "/txt/UDHR" contains text files (.txt) of the Universal Declaration of Human Rights in 13 languages. 


```r
udhr_data <- readtext(paste0(data_dir, "/txt/UDHR/*"))
```

You can generate document-level variables based on the file names using the `docvarnames` and `docvarsfrom` argument. `dvsep = "_"` specifies the value separator in the filenames.`encoding = "ISO-8859-1"` determines character encodings of the texts.


```r
eu_data <- readtext(paste0(data_dir, "/txt/EU_manifestos/*.txt"),
                    docvarsfrom = "filenames", 
                    docvarnames = c("unit", "context", "year", "language", "party"),
                    dvsep = "_", 
                    encoding = "ISO-8859-1")
str(eu_data)
```

```
## Classes 'readtext' and 'data.frame':	17 obs. of  7 variables:
##  $ doc_id  : chr  "EU_euro_2004_de_PSE.txt" "EU_euro_2004_de_V.txt" "EU_euro_2004_en_PSE.txt" "EU_euro_2004_en_V.txt" ...
##  $ text    : chr  "PES · PSE · SPE European Parliament rue Wiertz B 1047 Brussels\n\nGEMEINSAM WERDEN WIR STÄRKER Fünf Verpflichtu"| __truncated__ "Gemeinsames Manifest\nGemeinsames Manifest zur Europawahl 2004 Europäischen Föderation Grüner Parteien (EFGP) \"| __truncated__ "PES · PSE · SPE European Parliament rue Wiertz B 1047 Brussels\n\nGROWING STRONGER TOGETHER Five commitments fo"| __truncated__ "Manifesto\nEuropean Elections Manifesto 2004\nCOMMON PREAMBLE\nAs adopted at 15th EFGP Council, Luxembourg, 8th"| __truncated__ ...
##  $ unit    : chr  "EU" "EU" "EU" "EU" ...
##  $ context : chr  "euro" "euro" "euro" "euro" ...
##  $ year    : int  2004 2004 2004 2004 2004 2004 2004 2004 2004 2004 ...
##  $ language: chr  "de" "de" "en" "en" ...
##  $ party   : chr  "PSE" "V" "PSE" "V" ...
```

`readtext()` can also curse through sub-directories. The directory `txt/movie_reviews` contains two directories `neg` and `pos`.


```r
data_reviews <- readtext(paste0(data_dir, "/txt/movie_reviews/*"))
```

### JSON

You can also read JSON files (.json) downloaded from the Twititer stream API. [twitter.json](https://raw.githubusercontent.com/quanteda/quanteda_tutorials/master/content/data/twitter.json) is located in data directory of this tutorial package.


```r
twitter_data <- readtext("content/data/twitter.json")
```



The file comes with several metadata for each tweet, such as the number of retweets and likes, the username, time and time zone. 


```r
names(twitter_data)
```

```
##  [1] "doc_id"                    "text"                     
##  [3] "retweet_count"             "favorited"                
##  [5] "truncated"                 "id_str"                   
##  [7] "in_reply_to_screen_name"   "source"                   
##  [9] "retweeted"                 "created_at"               
## [11] "in_reply_to_status_id_str" "in_reply_to_user_id_str"  
## [13] "lang"                      "listed_count"             
## [15] "verified"                  "location"                 
## [17] "user_id_str"               "description"              
## [19] "geo_enabled"               "user_created_at"          
## [21] "statuses_count"            "followers_count"          
## [23] "favourites_count"          "protected"                
## [25] "user_url"                  "name"                     
## [27] "time_zone"                 "user_lang"                
## [29] "utc_offset"                "friends_count"            
## [31] "screen_name"               "country_code"             
## [33] "country"                   "place_type"               
## [35] "full_name"                 "place_name"               
## [37] "place_id"                  "place_lat"                
## [39] "place_lon"                 "lat"                      
## [41] "lon"                       "expanded_url"             
## [43] "url"
```

### PDF

`readtext()` can also read and convert PDF (.pdf) files. 


```r
udhr_data <- readtext(paste0(data_dir, "/pdf/UDHR/*.pdf"), 
                      docvarsfrom = "filenames", 
                      docvarnames = c("document", "language"),
                      sep = "_")
```

### Microsoft Word

`readtext()` can import Microsoft Word ('.doc' and '.docx') files through the **antiword** package.


```r
word_data <- readtext(paste0(data_dir, "/word/*.docx"))
```
