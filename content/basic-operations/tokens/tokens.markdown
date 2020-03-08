---
title: Construct a tokens object
weight: 10
draft: false
---


```r
require(quanteda)
```

`tokens()` segments texts in a corpus into tokens (words or sentences) by word boundaries. 


```r
corp_immig <- corpus(data_char_ukimmig2010)
toks_immig <- tokens(corp_immig)
```

A corpus is passed to `tokens()` in the code above, but it works with a character string too.


```r
toks <- tokens(data_char_ukimmig2010)
head(toks[[1]], 50)
```

```
##  [1] "IMMIGRATION"  ":"            "AN"           "UNPARALLELED" "CRISIS"      
##  [6] "WHICH"        "ONLY"         "THE"          "BNP"          "CAN"         
## [11] "SOLVE"        "."            "-"            "At"           "current"     
## [16] "immigration"  "and"          "birth"        "rates"        ","           
## [21] "indigenous"   "British"      "people"       "are"          "set"         
## [26] "to"           "become"       "a"            "minority"     "well"        
## [31] "within"       "50"           "years"        "."            "-"           
## [36] "This"         "will"         "result"       "in"           "the"         
## [41] "extinction"   "of"           "the"          "British"      "people"      
## [46] ","            "culture"      ","            "heritage"     "and"
```

{{% notice tip %}}
`tokens()` can tokenize texts in Asian languages such as Japanese or Chinese without additional tools thanks to the **stringi** package. See an example of in [documentation website](https://quanteda.io/articles/pkgdown/examples/chinese.html).
{{% /notice %}}

By default, `tokens()` only removes separators (typically whitespaces), but you can remove punctuation and numbers.


```r
toks_nopunct <- tokens(data_char_ukimmig2010, remove_punct = TRUE)
head(toks_nopunct[[1]], 50)
```

```
##  [1] "IMMIGRATION"  "AN"           "UNPARALLELED" "CRISIS"       "WHICH"       
##  [6] "ONLY"         "THE"          "BNP"          "CAN"          "SOLVE"       
## [11] "At"           "current"      "immigration"  "and"          "birth"       
## [16] "rates"        "indigenous"   "British"      "people"       "are"         
## [21] "set"          "to"           "become"       "a"            "minority"    
## [26] "well"         "within"       "50"           "years"        "This"        
## [31] "will"         "result"       "in"           "the"          "extinction"  
## [36] "of"           "the"          "British"      "people"       "culture"     
## [41] "heritage"     "and"          "identity"     "The"          "BNP"         
## [46] "will"         "take"         "all"          "steps"        "necessary"
```


{{% notice tip %}}
**stringi**'s word boundary detection splits Twitter hashtags into # and following keyword, but `tokens()` preserves these unless you set `remove_twitter = TRUE`.
{{% /notice %}}



