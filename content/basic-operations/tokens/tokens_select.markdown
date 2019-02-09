---
title: Select tokens
weight: 30
draft: false
---


```r
require(quanteda)
```


```r
toks <- tokens(data_char_ukimmig2010)
```

You can remove tokens that you are not interested in using `tokens_select()`. Usually we remove function words (grammatical words) that have little or no substantive meaning in pre-processing. `stopwords()` returns a pre-defined list of function words.


```r
toks_nostop <- tokens_select(toks, pattern = stopwords('en'), selection = 'remove')
head(toks_nostop[[1]], 50)
```

```
##  [1] "IMMIGRATION"  ":"            "UNPARALLELED" "CRISIS"      
##  [5] "BNP"          "CAN"          "SOLVE"        "."           
##  [9] "-"            "current"      "immigration"  "birth"       
## [13] "rates"        ","            "indigenous"   "British"     
## [17] "people"       "set"          "become"       "minority"    
## [21] "well"         "within"       "50"           "years"       
## [25] "."            "-"            "result"       "extinction"  
## [29] "British"      "people"       ","            "culture"     
## [33] ","            "heritage"     "identity"     "."           
## [37] "-"            "BNP"          "take"         "steps"       
## [41] "necessary"    "halt"         "reverse"      "process"     
## [45] "."            "-"            "steps"        "include"     
## [49] "halt"         "immigration"
```

`tokens_remove()` is an alias to `tokens_select(selection = 'remove')`. Therefore, the code above and below are equivalent.


```r
tooks_nostop2 <- tokens_remove(toks, pattern = stopwords('en'))
head(tooks_nostop2[[1]], 50)
```

```
##  [1] "IMMIGRATION"  ":"            "UNPARALLELED" "CRISIS"      
##  [5] "BNP"          "CAN"          "SOLVE"        "."           
##  [9] "-"            "current"      "immigration"  "birth"       
## [13] "rates"        ","            "indigenous"   "British"     
## [17] "people"       "set"          "become"       "minority"    
## [21] "well"         "within"       "50"           "years"       
## [25] "."            "-"            "result"       "extinction"  
## [29] "British"      "people"       ","            "culture"     
## [33] ","            "heritage"     "identity"     "."           
## [37] "-"            "BNP"          "take"         "steps"       
## [41] "necessary"    "halt"         "reverse"      "process"     
## [45] "."            "-"            "steps"        "include"     
## [49] "halt"         "immigration"
```

Removal of tokens changes the lengths of documents, but they remain the same if you set `padding = TRUE`. This option is useful especially when you perform positional analysis.


```r
toks_nostop_pad <- tokens_remove(toks, pattern = stopwords('en'), padding = TRUE)
head(toks_nostop_pad[[1]], 50)
```

```
##  [1] "IMMIGRATION"  ":"            ""             "UNPARALLELED"
##  [5] "CRISIS"       ""             ""             ""            
##  [9] "BNP"          "CAN"          "SOLVE"        "."           
## [13] "-"            ""             "current"      "immigration" 
## [17] ""             "birth"        "rates"        ","           
## [21] "indigenous"   "British"      "people"       ""            
## [25] "set"          ""             "become"       ""            
## [29] "minority"     "well"         "within"       "50"          
## [33] "years"        "."            "-"            ""            
## [37] ""             "result"       ""             ""            
## [41] "extinction"   ""             ""             "British"     
## [45] "people"       ","            "culture"      ","           
## [49] "heritage"     ""
```

If you are only interested in certain words, you can keep these and remove others.


```r
toks_immig <- tokens_select(toks, pattern = c('immig*', 'migra*'), padding = TRUE)
head(toks_immig[[1]], 50)
```

```
##  [1] "IMMIGRATION" ""            ""            ""            ""           
##  [6] ""            ""            ""            ""            ""           
## [11] ""            ""            ""            ""            ""           
## [16] "immigration" ""            ""            ""            ""           
## [21] ""            ""            ""            ""            ""           
## [26] ""            ""            ""            ""            ""           
## [31] ""            ""            ""            ""            ""           
## [36] ""            ""            ""            ""            ""           
## [41] ""            ""            ""            ""            ""           
## [46] ""            ""            ""            ""            ""
```

If you want to analyze words that appear around keywords, use the `window` argument.


```r
toks_immig_window <- tokens_select(toks, pattern = c('immig*', 'migra*'), padding = TRUE, window = 5)
head(toks_immig_window[[1]], 50)
```

```
##  [1] "IMMIGRATION"  ":"            "AN"           "UNPARALLELED"
##  [5] "CRISIS"       "WHICH"        ""             ""            
##  [9] ""             ""             "SOLVE"        "."           
## [13] "-"            "At"           "current"      "immigration" 
## [17] "and"          "birth"        "rates"        ","           
## [21] "indigenous"   ""             ""             ""            
## [25] ""             ""             ""             ""            
## [29] ""             ""             ""             ""            
## [33] ""             ""             ""             ""            
## [37] ""             ""             ""             ""            
## [41] ""             ""             ""             ""            
## [45] ""             ""             ""             ""            
## [49] ""             ""
```

{{% notice tip %}}
We keep using *glob* patterns in this tutorials, but almost all the functions in **quanteda** support *regular expression* patterns. See `?pattern` for details and this [cheatsheet](https://www.cheatography.com/davechild/cheat-sheets/regular-expressions/pdf_bw/) for a concise introduction to frequently used regular expressions.
{{% /notice %}}

