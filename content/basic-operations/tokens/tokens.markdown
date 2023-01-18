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

{{% notice tip %}}
`tokens()` can tokenize texts in Asian languages such as Japanese or Chinese without additional tools thanks to the **stringi** package. See an example of in [documentation website](https://quanteda.io/articles/pkgdown/examples/chinese.html).
{{% /notice %}}

By default, `tokens()` only removes separators (typically white spaces), but you can also remove punctuation and numbers.


```r
toks_nopunct <- tokens(data_char_ukimmig2010, remove_punct = TRUE)
print(toks_nopunct)
```

```
## Tokens consisting of 9 documents.
## BNP :
##  [1] "IMMIGRATION"  "AN"           "UNPARALLELED" "CRISIS"       "WHICH"        "ONLY"         "THE"         
##  [8] "BNP"          "CAN"          "SOLVE"        "At"           "current"     
## [ ... and 2,839 more ]
## 
## Coalition :
##  [1] "IMMIGRATION"  "The"          "Government"   "believes"     "that"         "immigration"  "has"         
##  [8] "enriched"     "our"          "culture"      "and"          "strengthened"
## [ ... and 219 more ]
## 
## Conservative :
##  [1] "Attract"     "the"         "brightest"   "and"         "best"        "to"          "our"        
##  [8] "country"     "Immigration" "has"         "enriched"    "our"        
## [ ... and 440 more ]
## 
## Greens :
##  [1] "Immigration" "Migration"   "is"          "a"           "fact"        "of"          "life"       
##  [8] "People"      "have"        "always"      "moved"       "from"       
## [ ... and 598 more ]
## 
## Labour :
##  [1] "Crime"       "and"         "immigration" "The"         "challenge"   "for"         "Britain"    
##  [8] "We"          "will"        "control"     "immigration" "with"       
## [ ... and 608 more ]
## 
## LibDem :
##  [1] "firm"        "but"         "fair"        "immigration" "system"      "Britain"     "has"        
##  [8] "always"      "been"        "an"          "open"        "welcoming"  
## [ ... and 423 more ]
## 
## [ reached max_ndoc ... 3 more documents ]
```

