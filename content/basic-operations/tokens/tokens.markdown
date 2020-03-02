---
title: Construct a tokens object
weight: 10
draft: false
---


```r
require(quanteda)
options(width = 110)
```

`tokens()` segments texts in a corpus into tokens (words or sentences) by word boundaries. 


```r
corp_immig <- corpus(data_char_ukimmig2010)
toks_immig <- tokens(corp_immig)
```

A corpus is passed to `tokens()` in the code above, but it works with a character string too.


```r
toks <- tokens(data_char_ukimmig2010)
print(toks)
```

```
## Tokens consisting of 9 documents.
## BNP :
##  [1] "IMMIGRATION"  ":"            "AN"           "UNPARALLELED" "CRISIS"       "WHICH"        "ONLY"        
##  [8] "THE"          "BNP"          "CAN"          "SOLVE"        "."           
## [ ... and 3,268 more ]
## 
## Coalition :
##  [1] "IMMIGRATION" "."           "The"         "Government"  "believes"    "that"        "immigration"
##  [8] "has"         "enriched"    "our"         "culture"     "and"        
## [ ... and 248 more ]
## 
## Conservative :
##  [1] "Attract"     "the"         "brightest"   "and"         "best"        "to"          "our"        
##  [8] "country"     "."           "Immigration" "has"         "enriched"   
## [ ... and 487 more ]
## 
## Greens :
##  [1] "Immigration" "."           "Migration"   "is"          "a"           "fact"        "of"         
##  [8] "life"        "."           "People"      "have"        "always"     
## [ ... and 665 more ]
## 
## Labour :
##  [1] "Crime"       "and"         "immigration" "The"         "challenge"   "for"         "Britain"    
##  [8] "We"          "will"        "control"     "immigration" "with"       
## [ ... and 668 more ]
## 
## LibDem :
##  [1] "firm"        "but"         "fair"        "immigration" "system"      "Britain"     "has"        
##  [8] "always"      "been"        "an"          "open"        ","          
## [ ... and 471 more ]
## 
## [ reached max_ndoc ... 3 more documents ]
```

{{% notice tip %}}
`tokens()` can tokenize texts in Asian languages such as Japanese or Chinese without additional tools thanks to the **stringi** package. See an example of in [documentation website](https://quanteda.io/articles/pkgdown/examples/chinese.html).
{{% /notice %}}

By default, `tokens()` only removes separators (typically whitespaces), but you can remove punctuation and numbers.


```r
toks_nopunct <- tokens(data_char_ukimmig2010, remove_punct = TRUE)
print(toks)
```

```
## Tokens consisting of 9 documents.
## BNP :
##  [1] "IMMIGRATION"  ":"            "AN"           "UNPARALLELED" "CRISIS"       "WHICH"        "ONLY"        
##  [8] "THE"          "BNP"          "CAN"          "SOLVE"        "."           
## [ ... and 3,268 more ]
## 
## Coalition :
##  [1] "IMMIGRATION" "."           "The"         "Government"  "believes"    "that"        "immigration"
##  [8] "has"         "enriched"    "our"         "culture"     "and"        
## [ ... and 248 more ]
## 
## Conservative :
##  [1] "Attract"     "the"         "brightest"   "and"         "best"        "to"          "our"        
##  [8] "country"     "."           "Immigration" "has"         "enriched"   
## [ ... and 487 more ]
## 
## Greens :
##  [1] "Immigration" "."           "Migration"   "is"          "a"           "fact"        "of"         
##  [8] "life"        "."           "People"      "have"        "always"     
## [ ... and 665 more ]
## 
## Labour :
##  [1] "Crime"       "and"         "immigration" "The"         "challenge"   "for"         "Britain"    
##  [8] "We"          "will"        "control"     "immigration" "with"       
## [ ... and 668 more ]
## 
## LibDem :
##  [1] "firm"        "but"         "fair"        "immigration" "system"      "Britain"     "has"        
##  [8] "always"      "been"        "an"          "open"        ","          
## [ ... and 471 more ]
## 
## [ reached max_ndoc ... 3 more documents ]
```


{{% notice tip %}}
**stringi**'s word boundary detection splits Twitter hashtags into # and following keyword, but `tokens()` preserves these unless you set `remove_twitter = TRUE`.
{{% /notice %}}



