---
title: Look up dictionary
weight: 50
draft: false
---


```r
require(quanteda)
options(width = 110)
```


```r
toks <- tokens(data_char_ukimmig2010)
```

`tokens_lookup()` is the most flexible dictionary look up function in **quanteda**. We use [geographical dictionary](https://raw.githubusercontent.com/quanteda/tutorials.quanteda.io/master/content/dictionary/newsmap.yml) from the [**newsmap**](https://cran.r-project.org/web/packages/newsmap/index.html) package as an example. Using `dictionary()`, you can import dictionary files in the Wordstat, LIWC, Yoshicoder, Lexicoder and YAML formats.


```r
dict_newsmap <- dictionary(file = "../../dictionary/newsmap.yml")
```

Note that you can access the dictionary in various languages (currently English, German, Japanese, Russian, and Spanish) with the **newsmap** package.

The geographical dictionary comprises of names of countries and cities (and their demonyms) in a hierarchical structure (countries are nested in world regions and sub-regions).


```r
length(dict_newsmap)
```

```
## [1] 5
```

```r
names(dict_newsmap)
```

```
## [1] "AFRICA"  "AMERICA" "ASIA"    "EUROPE"  "OCEANIA"
```

```r
names(dict_newsmap[["AFRICA"]])
```

```
## [1] "EAST"   "MIDDLE" "NORTH"  "SOUTH"  "WEST"
```

```r
dict_newsmap[["AFRICA"]][["NORTH"]]
```

```
## Dictionary object with 8 key entries.
## - [DZ]:
##   - algeria, algerian*, algiers
## - [EG]:
##   - egypt, egyptian*, cairo
## - [LY]:
##   - libya, libyan*, tripoli
## - [MA]:
##   - morocco, moroccan*, rabat
## - [SD]:
##   - sudan, sudanese, khartoum
## - [SS]:
##   - south sudan, s sudan, s sudanese, juba
## [ reached max_nkey ... 2 more keys ]
```

The `levels` argument determines the keys to be recorded in a resulting tokens object.


```r
# use level of continents
toks_region <- tokens_lookup(toks, dictionary = dict_newsmap, levels = 1)
print(toks_region)
```

```
## Tokens consisting of 9 documents.
## BNP :
##  [1] "EUROPE" "EUROPE" "EUROPE" "EUROPE" "EUROPE" "EUROPE" "EUROPE" "EUROPE" "EUROPE" "EUROPE" "EUROPE"
## [12] "EUROPE"
## [ ... and 62 more ]
## 
## Coalition :
## [1] "EUROPE"
## 
## Conservative :
## [1] "EUROPE" "EUROPE" "EUROPE" "EUROPE" "EUROPE"
## 
## Greens :
## [1] "EUROPE" "EUROPE"
## 
## Labour :
##  [1] "EUROPE"  "OCEANIA" "OCEANIA" "OCEANIA" "EUROPE"  "OCEANIA" "EUROPE"  "EUROPE"  "EUROPE"  "EUROPE" 
## [11] "EUROPE" 
## 
## LibDem :
## [1] "EUROPE"  "EUROPE"  "EUROPE"  "EUROPE"  "EUROPE"  "EUROPE"  "AMERICA"
## 
## [ reached max_ndoc ... 3 more documents ]
```


```r
# use level of countries
toks_country <- tokens_lookup(toks, dictionary = dict_newsmap, levels = 3)
print(toks_country)
```

```
## Tokens consisting of 9 documents.
## BNP :
##  [1] "GB" "GB" "GB" "GB" "GB" "GB" "GB" "GB" "GB" "GB" "GB" "GB"
## [ ... and 62 more ]
## 
## Coalition :
## [1] "GB"
## 
## Conservative :
## [1] "GB" "GB" "GB" "GB" "GB"
## 
## Greens :
## [1] "GB" "GB"
## 
## Labour :
##  [1] "GB" "AU" "AU" "AU" "GB" "AU" "GB" "GB" "GB" "GB" "GB"
## 
## LibDem :
## [1] "GB" "GB" "GB" "GB" "GB" "GB" "CA"
## 
## [ reached max_ndoc ... 3 more documents ]
```

You can also use run a keyword-in-context analysis by looking up the mentions of all African countries and the words surrounding the countries.


```r
kwic(toks, dict_newsmap["AFRICA"])
```

```
## Keyword-in-context with 2 matches.                                                                            
##  [BNP, 3116] , with Rome and Ancient | Egypt  | being well known examples of
##  [BNP, 3149]   purpose. The Balkans, | Rwanda | , Indonesia, Ulster,
```

You can define your own dictionary by passing a named list of characters to `dictionary()`.


```r
dict <- dictionary(list(refugee = c("refugee*", "asylum*"),
                        worker = c("worker*", "employee*")))
print(dict)
```

```
## Dictionary object with 2 key entries.
## - [refugee]:
##   - refugee*, asylum*
## - [worker]:
##   - worker*, employee*
```

```r
dict_toks <- tokens_lookup(toks, dictionary = dict)
print(dict_toks)
```

```
## Tokens consisting of 9 documents.
## BNP :
##  [1] "refugee" "worker"  "refugee" "refugee" "refugee" "refugee" "refugee" "refugee" "refugee" "refugee"
## [11] "refugee" "refugee"
## [ ... and 2 more ]
## 
## Coalition :
## [1] "refugee"
## 
## Conservative :
## character(0)
## 
## Greens :
## [1] "worker"  "refugee" "refugee" "refugee" "refugee"
## 
## Labour :
## [1] "refugee" "refugee" "refugee" "refugee" "worker"  "worker"  "worker"  "worker" 
## 
## LibDem :
## [1] "refugee" "refugee" "refugee" "refugee" "refugee" "refugee"
## 
## [ reached max_ndoc ... 3 more documents ]
```

```r
dfm(dict_toks)
```

```
## Document-feature matrix of: 9 documents, 2 features (38.89% sparse) and 0 docvars.
##               features
## docs           refugee worker
##   BNP               12      2
##   Coalition          1      0
##   Conservative       0      0
##   Greens             4      1
##   Labour             4      4
##   LibDem             6      0
## [ reached max_ndoc ... 3 more documents ]
```

{{% notice tip %}}
`tokens_lookup()` ignores multiple matches of dictionary values for the same key with the same token to avoid double counting. For example, if `US = c("United States of America", "United States")` is in your dictionary, you get "US" only once for a sequence of tokens `"United" "States" "of" "America"`.
{{% /notice %}}
