---
title: Preparing texts
weight: 20
chapter: false
draft: true
---



Here we will step through the basic elements of preparing a text for analysis.  These are tokenization, conversion to lower case, stemming, removing or selecting features, and defining equivalency classes for features, including the use of dictionaries.

### 1. Tokenization

Tokenization in quanteda is very *conservative*: by default, it only removes separator characters.


```r
require(quanteda, quietly = TRUE, warn.conflicts = FALSE)
txt <- c(text1 = "This is $10 in 999 different ways,\n up and down; left and right!",
         text2 = "@kenbenoit working: on #quanteda 2day\t4ever, http://textasdata.com?page=123.")
tokens(txt)
## tokens from 2 documents.
## text1 :
##  [1] "This"      "is"        "$"         "10"        "in"       
##  [6] "999"       "different" "ways"      ","         "up"       
## [11] "and"       "down"      ";"         "left"      "and"      
## [16] "right"     "!"        
## 
## text2 :
##  [1] "@kenbenoit"     "working"        ":"              "on"            
##  [5] "#quanteda"      "2day"           "4ever"          ","             
##  [9] "http"           ":"              "/"              "/"             
## [13] "textasdata.com" "?"              "page"           "="             
## [17] "123"            "."
tokens(txt, verbose = TRUE)
## Starting tokenization...
## ...tokenizing 1 of 1 blocks
## ...preserving hyphens
## ...preserving Twitter characters (#, @)
## ...serializing tokens 34 unique types
## ...total elapsed:  0.13 seconds.
## Finished tokenizing and cleaning 2 texts.
## tokens from 2 documents.
## text1 :
##  [1] "This"      "is"        "$"         "10"        "in"       
##  [6] "999"       "different" "ways"      ","         "up"       
## [11] "and"       "down"      ";"         "left"      "and"      
## [16] "right"     "!"        
## 
## text2 :
##  [1] "@kenbenoit"     "working"        ":"              "on"            
##  [5] "#quanteda"      "2day"           "4ever"          ","             
##  [9] "http"           ":"              "/"              "/"             
## [13] "textasdata.com" "?"              "page"           "="             
## [17] "123"            "."
tokens(txt, remove_numbers = TRUE,  remove_punct = TRUE)
## tokens from 2 documents.
## text1 :
##  [1] "This"      "is"        "in"        "different" "ways"     
##  [6] "up"        "and"       "down"      "left"      "and"      
## [11] "right"    
## 
## text2 :
## [1] "@kenbenoit"     "working"        "on"             "#quanteda"     
## [5] "2day"           "4ever"          "http"           "textasdata.com"
## [9] "page"
tokens(txt, remove_numbers = FALSE, remove_punct = TRUE)
## tokens from 2 documents.
## text1 :
##  [1] "This"      "is"        "10"        "in"        "999"      
##  [6] "different" "ways"      "up"        "and"       "down"     
## [11] "left"      "and"       "right"    
## 
## text2 :
##  [1] "@kenbenoit"     "working"        "on"             "#quanteda"     
##  [5] "2day"           "4ever"          "http"           "textasdata.com"
##  [9] "page"           "123"
tokens(txt, remove_numbers = TRUE,  remove_punct = FALSE)
## tokens from 2 documents.
## text1 :
##  [1] "This"      "is"        "$"         "in"        "different"
##  [6] "ways"      ","         "up"        "and"       "down"     
## [11] ";"         "left"      "and"       "right"     "!"        
## 
## text2 :
##  [1] "@kenbenoit"     "working"        ":"              "on"            
##  [5] "#quanteda"      "2day"           "4ever"          ","             
##  [9] "http"           ":"              "/"              "/"             
## [13] "textasdata.com" "?"              "page"           "="             
## [17] "."
tokens(txt, remove_numbers = FALSE, remove_punct = FALSE)
## tokens from 2 documents.
## text1 :
##  [1] "This"      "is"        "$"         "10"        "in"       
##  [6] "999"       "different" "ways"      ","         "up"       
## [11] "and"       "down"      ";"         "left"      "and"      
## [16] "right"     "!"        
## 
## text2 :
##  [1] "@kenbenoit"     "working"        ":"              "on"            
##  [5] "#quanteda"      "2day"           "4ever"          ","             
##  [9] "http"           ":"              "/"              "/"             
## [13] "textasdata.com" "?"              "page"           "="             
## [17] "123"            "."
tokens(txt, remove_numbers = FALSE, remove_punct = FALSE, remove_separators = FALSE)
## tokens from 2 documents.
## text1 :
##  [1] "This"      " "         "is"        " "         "$"        
##  [6] "10"        " "         "in"        " "         "999"      
## [11] " "         "different" " "         "ways"      ","        
## [16] "\n"        " "         "up"        " "         "and"      
## [21] " "         "down"      ";"         " "         "left"     
## [26] " "         "and"       " "         "right"     "!"        
## 
## text2 :
##  [1] "@kenbenoit"     " "              "working"        ":"             
##  [5] " "              "on"             " "              "#quanteda"     
##  [9] " "              "2day"           "\t"              "4ever"         
## [13] ","              " "              "http"           ":"             
## [17] "/"              "/"              "textasdata.com" "?"             
## [21] "page"           "="              "123"            "."
```

There are several options to the `what` argument: 

```r
# sentence level
tokens(c("Kurt Vongeut said; only assholes use semi-colons.",
         "Today is Thursday in Canberra:  It is yesterday in London.",
         "Today is Thursday in Canberra:  \nIt is yesterday in London.",
         "To be?  Or\nnot to be?"),
       what = "sentence")
## tokens from 4 documents.
## text1 :
## [1] "Kurt Vongeut said; only assholes use semi-colons."
## 
## text2 :
## [1] "Today is Thursday in Canberra:  It is yesterday in London."
## 
## text3 :
## [1] "Today is Thursday in Canberra:   It is yesterday in London."
## 
## text4 :
## [1] "To be?"        "Or not to be?"
tokens(data_corpus_inaugural[2], what = "sentence")
## tokens from 1 document.
## 1793-Washington :
## [1] "Fellow citizens, I am again called upon by the voice of my country to execute the functions of its Chief Magistrate."                                                                                                                                                                                                                                       
## [2] "When the occasion proper for it shall arrive, I shall endeavor to express the high sense I entertain of this distinguished honor, and of the confidence which has been reposed in me by the people of united America."                                                                                                                                      
## [3] "Previous to the execution of any official act of the President the Constitution requires an oath of office."                                                                                                                                                                                                                                                
## [4] "This oath I am now about to take, and in your presence: That if it shall be found during my administration of the Government I have in any instance violated willingly or knowingly the injunctions thereof, I may (besides incurring constitutional punishment) be subject to the upbraidings of all who are now witnesses of the present solemn ceremony."

# character level
tokens("My big fat text package.", what = "character")
## tokens from 1 document.
## text1 :
##  [1] "M" "y" "b" "i" "g" "f" "a" "t" "t" "e" "x" "t" "p" "a" "c" "k" "a"
## [18] "g" "e" "."
tokens("My big fat text package.", what = "character", remove_separators = FALSE)
## tokens from 1 document.
## text1 :
##  [1] "M" "y" " " "b" "i" "g" " " "f" "a" "t" " " "t" "e" "x" "t" " " "p"
## [18] "a" "c" "k" "a" "g" "e" "."
```

Two other options, for really fast and simple tokenization are `"fastestword"` and `"fasterword"`, if performance is a key issue.  These are less intelligent than the boundary detection used in the default `"word"` method, which is based on stringi/ICU boundary detection.

### 2. Conversion to lower case

This is a tricky one in our workflow, since it is a form of equivalency declaration, rather than a tokenization step.  It turns out that it is more efficient to perform at the pre-tokenization stage. 

The **quanteda** functions `*_tolower` include options designed to preserve acronyms.

```r
test1 <- c(text1 = "England and France are members of NATO and UNESCO",
           text2 = "NASA sent a rocket into space.")
char_tolower(test1)
##                                               text1 
## "england and france are members of nato and unesco" 
##                                               text2 
##                    "nasa sent a rocket into space."
char_tolower(test1, keep_acronyms = TRUE)
##                                               text1 
## "england and france are members of NATO and UNESCO" 
##                                               text2 
##                    "NASA sent a rocket into space."

test2 <- tokens(test1, remove_punct = TRUE)
tokens_tolower(test2)
## tokens from 2 documents.
## text1 :
## [1] "england" "and"     "france"  "are"     "members" "of"      "nato"   
## [8] "and"     "unesco" 
## 
## text2 :
## [1] "nasa"   "sent"   "a"      "rocket" "into"   "space"
tokens_tolower(test2, keep_acronyms = TRUE)
## tokens from 2 documents.
## text1 :
## [1] "england" "and"     "france"  "are"     "members" "of"      "NATO"   
## [8] "and"     "UNESCO" 
## 
## text2 :
## [1] "NASA"   "sent"   "a"      "rocket" "into"   "space"
```

The **quanteda** `*_tolower` functions are based on `stringi::stri_trans_tolower()`, and is therefore nicely Unicode compliant.

```r
data(data_char_encodedtexts, package = "readtext")

# Russian
cat(iconv(data_char_encodedtexts[8], "windows-1251", "UTF-8"))
## "8-<U+0431><U+0438><U+0442><U+043D><U+044B><U+0435>" o
