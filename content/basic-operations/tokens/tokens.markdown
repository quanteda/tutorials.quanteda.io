---
title: Construct a tokens
weight: 10
draft: false
---


```r
require(quanteda)
```

`tokens()` segments texts in a corpus into tokens (words or sentences) by word boundaries. 


```r
immig_corp <- corpus(data_char_ukimmig2010)
toks <- tokens(immig_corp)
```

A corpus is passed to `tokens()` in the code above, but it works with a character string.


```r
toks <- tokens(data_char_ukimmig2010)
head(toks[[1]], 50)
```

```
##  [1] "IMMIGRATION"  ":"            "AN"           "UNPARALLELED"
##  [5] "CRISIS"       "WHICH"        "ONLY"         "THE"         
##  [9] "BNP"          "CAN"          "SOLVE"        "."           
## [13] "-"            "At"           "current"      "immigration" 
## [17] "and"          "birth"        "rates"        ","           
## [21] "indigenous"   "British"      "people"       "are"         
## [25] "set"          "to"           "become"       "a"           
## [29] "minority"     "well"         "within"       "50"          
## [33] "years"        "."            "-"            "This"        
## [37] "will"         "result"       "in"           "the"         
## [41] "extinction"   "of"           "the"          "British"     
## [45] "people"       ","            "culture"      ","           
## [49] "heritage"     "and"
```

{{% notice tip %}}
`tokens()` can tokenize texts in Asia languages such as Japanese or Chinese without additional tools thanks to the **stringi** package. See an example of in [documentation website](http://docs.quanteda.io/articles/pkgdown/examples/chinese.html).
{{% /notice %}}

By default, `tokens()` only removes separators (typically whitespaces), but you can remove punctuation and numbers.


```r
nopunct_toks <- tokens(data_char_ukimmig2010, remove_punct = TRUE)
head(nopunct_toks[[1]], 50)
```

```
##  [1] "IMMIGRATION"  "AN"           "UNPARALLELED" "CRISIS"      
##  [5] "WHICH"        "ONLY"         "THE"          "BNP"         
##  [9] "CAN"          "SOLVE"        "At"           "current"     
## [13] "immigration"  "and"          "birth"        "rates"       
## [17] "indigenous"   "British"      "people"       "are"         
## [21] "set"          "to"           "become"       "a"           
## [25] "minority"     "well"         "within"       "50"          
## [29] "years"        "This"         "will"         "result"      
## [33] "in"           "the"          "extinction"   "of"          
## [37] "the"          "British"      "people"       "culture"     
## [41] "heritage"     "and"          "identity"     "The"         
## [45] "BNP"          "will"         "take"         "all"         
## [49] "steps"        "necessary"
```


{{% notice tip %}}
**stringi**'s word boundary detection splits Twitter hashtags into # and following keyword, but `tokens()` preserves these unless you set `remove_twitter = TRUE`.
{{% /notice %}}



