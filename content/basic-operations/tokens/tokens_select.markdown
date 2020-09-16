---
title: Select tokens
weight: 30
draft: false
---


```r
require(quanteda)
options(width = 110)
```


```r
toks <- tokens(data_char_ukimmig2010)
```

You can remove tokens that you are not interested in using `tokens_select()`. Usually we remove function words (grammatical words) that have little or no substantive meaning in pre-processing. `stopwords()` returns a pre-defined list of function words.


```r
toks_nostop <- tokens_select(toks, pattern = stopwords("en"), selection = "remove")
print(toks_nostop)
```

```
## Tokens consisting of 9 documents.
## BNP :
##  [1] "IMMIGRATION"  ":"            "UNPARALLELED" "CRISIS"       "BNP"          "CAN"          "SOLVE"       
##  [8] "."            "-"            "current"      "immigration"  "birth"       
## [ ... and 2,109 more ]
## 
## Coalition :
##  [1] "IMMIGRATION"  "."            "Government"   "believes"     "immigration"  "enriched"     "culture"     
##  [8] "strengthened" "economy"      ","            "must"         "controlled"  
## [ ... and 146 more ]
## 
## Conservative :
##  [1] "Attract"     "brightest"   "best"        "country"     "."           "Immigration" "enriched"   
##  [8] "nation"      "years"       "want"        "attract"     "brightest"  
## [ ... and 277 more ]
## 
## Greens :
##  [1] "Immigration" "."           "Migration"   "fact"        "life"        "."           "People"     
##  [8] "always"      "moved"       "one"         "country"     "another"    
## [ ... and 376 more ]
## 
## Labour :
##  [1] "Crime"            "immigration"      "challenge"        "Britain"          "control"         
##  [6] "immigration"      "new"              "Australian-style" "points-based"     "system"          
## [11] "-"                "unlike"          
## [ ... and 388 more ]
## 
## LibDem :
##  [1] "firm"        "fair"        "immigration" "system"      "Britain"     "always"      "open"       
##  [8] ","           "welcoming"   "country"     ","           "thousands"  
## [ ... and 285 more ]
## 
## [ reached max_ndoc ... 3 more documents ]
```

`tokens_remove()` is an alias to `tokens_select(selection = "remove")`. Therefore, the code above and below are equivalent.


```r
toks_nostop2 <- tokens_remove(toks, pattern = stopwords("en"))
print(toks_nostop2)
```

```
## Tokens consisting of 9 documents.
## BNP :
##  [1] "IMMIGRATION"  ":"            "UNPARALLELED" "CRISIS"       "BNP"          "CAN"          "SOLVE"       
##  [8] "."            "-"            "current"      "immigration"  "birth"       
## [ ... and 2,109 more ]
## 
## Coalition :
##  [1] "IMMIGRATION"  "."            "Government"   "believes"     "immigration"  "enriched"     "culture"     
##  [8] "strengthened" "economy"      ","            "must"         "controlled"  
## [ ... and 146 more ]
## 
## Conservative :
##  [1] "Attract"     "brightest"   "best"        "country"     "."           "Immigration" "enriched"   
##  [8] "nation"      "years"       "want"        "attract"     "brightest"  
## [ ... and 277 more ]
## 
## Greens :
##  [1] "Immigration" "."           "Migration"   "fact"        "life"        "."           "People"     
##  [8] "always"      "moved"       "one"         "country"     "another"    
## [ ... and 376 more ]
## 
## Labour :
##  [1] "Crime"            "immigration"      "challenge"        "Britain"          "control"         
##  [6] "immigration"      "new"              "Australian-style" "points-based"     "system"          
## [11] "-"                "unlike"          
## [ ... and 388 more ]
## 
## LibDem :
##  [1] "firm"        "fair"        "immigration" "system"      "Britain"     "always"      "open"       
##  [8] ","           "welcoming"   "country"     ","           "thousands"  
## [ ... and 285 more ]
## 
## [ reached max_ndoc ... 3 more documents ]
```

Removal of tokens changes the lengths of documents, but they remain the same if you set `padding = TRUE`. This option is useful especially when you perform positional analysis.


```r
toks_nostop_pad <- tokens_remove(toks, pattern = stopwords("en"), padding = TRUE)
print(toks_nostop_pad)
```

```
## Tokens consisting of 9 documents.
## BNP :
##  [1] "IMMIGRATION"  ":"            ""             "UNPARALLELED" "CRISIS"       ""             ""            
##  [8] ""             "BNP"          "CAN"          "SOLVE"        "."           
## [ ... and 3,268 more ]
## 
## Coalition :
##  [1] "IMMIGRATION" "."           ""            "Government"  "believes"    ""            "immigration"
##  [8] ""            "enriched"    ""            "culture"     ""           
## [ ... and 248 more ]
## 
## Conservative :
##  [1] "Attract"     ""            "brightest"   ""            "best"        ""            ""           
##  [8] "country"     "."           "Immigration" ""            "enriched"   
## [ ... and 487 more ]
## 
## Greens :
##  [1] "Immigration" "."           "Migration"   ""            ""            "fact"        ""           
##  [8] "life"        "."           "People"      ""            "always"     
## [ ... and 665 more ]
## 
## Labour :
##  [1] "Crime"       ""            "immigration" ""            "challenge"   ""            "Britain"    
##  [8] ""            ""            "control"     "immigration" ""           
## [ ... and 668 more ]
## 
## LibDem :
##  [1] "firm"        ""            "fair"        "immigration" "system"      "Britain"     ""           
##  [8] "always"      ""            ""            "open"        ","          
## [ ... and 471 more ]
## 
## [ reached max_ndoc ... 3 more documents ]
```

If you are only interested in certain words, you can keep these and remove others.


```r
toks_immig <- tokens_select(toks, pattern = c("immig*", "migra*"), padding = TRUE)
print(toks_immig)
```

```
## Tokens consisting of 9 documents.
## BNP :
##  [1] "IMMIGRATION" ""            ""            ""            ""            ""            ""           
##  [8] ""            ""            ""            ""            ""           
## [ ... and 3,268 more ]
## 
## Coalition :
##  [1] "IMMIGRATION" ""            ""            ""            ""            ""            "immigration"
##  [8] ""            ""            ""            ""            ""           
## [ ... and 248 more ]
## 
## Conservative :
##  [1] ""            ""            ""            ""            ""            ""            ""           
##  [8] ""            ""            "Immigration" ""            ""           
## [ ... and 487 more ]
## 
## Greens :
##  [1] "Immigration" ""            "Migration"   ""            ""            ""            ""           
##  [8] ""            ""            ""            ""            ""           
## [ ... and 665 more ]
## 
## Labour :
##  [1] ""            ""            "immigration" ""            ""            ""            ""           
##  [8] ""            ""            ""            "immigration" ""           
## [ ... and 668 more ]
## 
## LibDem :
##  [1] ""            ""            ""            "immigration" ""            ""            ""           
##  [8] ""            ""            ""            ""            ""           
## [ ... and 471 more ]
## 
## [ reached max_ndoc ... 3 more documents ]
```

If you want to analyze words that appear around keywords, use the `window` argument.


```r
toks_immig_window <- tokens_select(toks, pattern = c("immig*", "migra*"), padding = TRUE, window = 5)
print(toks_immig_window)
```

```
## Tokens consisting of 9 documents.
## BNP :
##  [1] "IMMIGRATION"  ":"            "AN"           "UNPARALLELED" "CRISIS"       "WHICH"        ""            
##  [8] ""             ""             ""             "SOLVE"        "."           
## [ ... and 3,268 more ]
## 
## Coalition :
##  [1] "IMMIGRATION" "."           "The"         "Government"  "believes"    "that"        "immigration"
##  [8] "has"         "enriched"    "our"         "culture"     "and"        
## [ ... and 248 more ]
## 
## Conservative :
##  [1] ""            ""            ""            ""            "best"        "to"          "our"        
##  [8] "country"     "."           "Immigration" "has"         "enriched"   
## [ ... and 487 more ]
## 
## Greens :
##  [1] "Immigration" "."           "Migration"   "is"          "a"           "fact"        "of"         
##  [8] "life"        ""            ""            ""            ""           
## [ ... and 665 more ]
## 
## Labour :
##  [1] "Crime"       "and"         "immigration" "The"         "challenge"   "for"         "Britain"    
##  [8] "We"          "will"        "control"     "immigration" "with"       
## [ ... and 668 more ]
## 
## LibDem :
##  [1] "firm"        "but"         "fair"        "immigration" "system"      "Britain"     "has"        
##  [8] "always"      "been"        ""            ""            ""           
## [ ... and 471 more ]
## 
## [ reached max_ndoc ... 3 more documents ]
```

{{% notice tip %}}
We keep using *glob* patterns in this tutorials, but almost all the functions in **quanteda** support *regular expression* patterns. See `?pattern` for details and this [cheatsheet](https://www.cheatography.com/davechild/cheat-sheets/regular-expressions/pdf_bw/) for a concise introduction to frequently used regular expressions.
{{% /notice %}}

