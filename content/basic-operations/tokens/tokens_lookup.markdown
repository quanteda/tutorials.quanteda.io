---
title: Look up dictionary
weight: 50
draft: false
---


```r
require(quanteda)
```


```r
toks <- tokens(data_char_ukimmig2010)
```

`tokens_lookup()` is the most flexible dictionary lookup function in **quanteda**. We use [geographical dictionary](https://raw.githubusercontent.com/quanteda/tutorials.quanteda.io/master/content/dictionary/newsmap.yml) from the [**newsmap**](https://cran.r-project.org/web/packages/newsmap/index.html) package as an example. Using `dictionary()`, you can import dictionary files in the Wordstat, LIWC, Yoshicoder, Lexicoder and YAML formats.


```r
newsmap_dict <- dictionary(file = "content/dictionary/newsmap.yml")
```



The geographical dictionary comprises of names of countries and cities (and their demonyms) in a hierachical structure ( which countries are nested in world regions and sub-regions).


```r
length(newsmap_dict)
```

```
## [1] 5
```

```r
names(newsmap_dict)
```

```
## [1] "AFRICA"  "AMERICA" "ASIA"    "EUROPE"  "OCEANIA"
```

```r
names(newsmap_dict[['AFRICA']])
```

```
## [1] "EAST"   "MIDDLE" "NORTH"  "SOUTH"  "WEST"
```

```r
newsmap_dict[['AFRICA']][['NORTH']]
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
## - [TN]:
##   - tunisia, tunisian*, tunis
## - [EH]:
##   - western sahara, western saharan*, el aaiun
```

The `levels` argument determines the keys to be recored in a resulting tokens object.


```r
region_toks <- tokens_lookup(toks, newsmap_dict, levels = 1)
head(region_toks)
```

```
## tokens from 6 documents.
## BNP :
##  [1] "EUROPE"  "EUROPE"  "EUROPE"  "EUROPE"  "EUROPE"  "EUROPE"  "EUROPE" 
##  [8] "EUROPE"  "EUROPE"  "EUROPE"  "EUROPE"  "EUROPE"  "EUROPE"  "ASIA"   
## [15] "ASIA"    "EUROPE"  "EUROPE"  "EUROPE"  "EUROPE"  "EUROPE"  "EUROPE" 
## [22] "EUROPE"  "EUROPE"  "EUROPE"  "EUROPE"  "EUROPE"  "EUROPE"  "EUROPE" 
## [29] "EUROPE"  "EUROPE"  "EUROPE"  "EUROPE"  "EUROPE"  "EUROPE"  "EUROPE" 
## [36] "EUROPE"  "EUROPE"  "EUROPE"  "EUROPE"  "EUROPE"  "EUROPE"  "EUROPE" 
## [43] "EUROPE"  "EUROPE"  "EUROPE"  "EUROPE"  "EUROPE"  "EUROPE"  "EUROPE" 
## [50] "EUROPE"  "EUROPE"  "EUROPE"  "EUROPE"  "EUROPE"  "EUROPE"  "EUROPE" 
## [57] "EUROPE"  "EUROPE"  "EUROPE"  "EUROPE"  "EUROPE"  "EUROPE"  "EUROPE" 
## [64] "EUROPE"  "EUROPE"  "EUROPE"  "AFRICA"  "AFRICA"  "ASIA"    "OCEANIA"
## [71] "ASIA"    "ASIA"    "EUROPE"  "EUROPE" 
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
##  [1] "EUROPE"  "OCEANIA" "OCEANIA" "OCEANIA" "EUROPE"  "OCEANIA" "EUROPE" 
##  [8] "EUROPE"  "EUROPE"  "EUROPE"  "EUROPE" 
## 
## LibDem :
## [1] "EUROPE"  "EUROPE"  "EUROPE"  "EUROPE"  "EUROPE"  "EUROPE"  "AMERICA"
```

```r
dfm(region_toks)
```

```
## Document-feature matrix of: 9 documents, 5 features (62.2% sparse).
## 9 x 5 sparse Matrix of class "dfm"
##               features
## docs           africa america asia europe oceania
##   BNP               2       0    5     66       1
##   Coalition         0       0    0      1       0
##   Conservative      0       0    0      5       0
##   Greens            0       0    0      2       0
##   Labour            0       0    0      7       4
##   LibDem            0       1    0      6       0
##   PC                0       0    0      1       0
##   SNP               0       1    0      0       1
##   UKIP              0       1    0     20       2
```


```r
country_toks <- tokens_lookup(toks, newsmap_dict, levels = 3)
head(country_toks)
```

```
## tokens from 6 documents.
## BNP :
##  [1] "GB" "GB" "GB" "GB" "GB" "GB" "GB" "GB" "GB" "GB" "GB" "GB" "GB" "BD"
## [15] "PK" "GB" "GB" "GB" "GB" "GB" "GB" "GB" "GB" "GB" "GB" "GB" "GB" "GB"
## [29] "GB" "GB" "GB" "GB" "GB" "GB" "GB" "GB" "GB" "GB" "GB" "GB" "GB" "GB"
## [43] "GB" "GB" "GB" "GB" "GB" "GB" "GB" "GB" "GB" "GB" "GB" "GB" "GB" "GB"
## [57] "GB" "GB" "GB" "GB" "IM" "GB" "GB" "GB" "IE" "IT" "EG" "RW" "ID" "FJ"
## [71] "LK" "IQ" "GB" "GB"
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
```

```r
dfm(country_toks)
```

```
## Document-feature matrix of: 9 documents, 241 features (98.8% sparse).
```


```r
kwic(toks, newsmap_dict['AFRICA'])
```

```
##                                                
##  [BNP, 3116] , with Rome and Ancient | Egypt  |
##  [BNP, 3149]   purpose. The Balkans, | Rwanda |
##                              
##  being well known examples of
##  , Indonesia, Ulster,
```

You can define your own dictionary by passing a named list of characters to `dictionary()`.


```r
dict <- dictionary(list(refugee = c('refugee*', 'asylum*'),
                        worker = c('worker*', 'employee*')))
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
dict_toks <- tokens_lookup(toks, dict)
head(dict_toks)
```

```
## tokens from 6 documents.
## BNP :
##  [1] "refugee" "worker"  "refugee" "refugee" "refugee" "refugee" "refugee"
##  [8] "refugee" "refugee" "refugee" "refugee" "refugee" "refugee" "worker" 
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
## [1] "refugee" "refugee" "refugee" "refugee" "worker"  "worker"  "worker" 
## [8] "worker" 
## 
## LibDem :
## [1] "refugee" "refugee" "refugee" "refugee" "refugee" "refugee"
```

```r
dfm(dict_toks)
```

```
## Document-feature matrix of: 9 documents, 2 features (38.9% sparse).
## 9 x 2 sparse Matrix of class "dfm"
##               features
## docs           refugee worker
##   BNP               12      2
##   Coalition          1      0
##   Conservative       0      0
##   Greens             4      1
##   Labour             4      4
##   LibDem             6      0
##   PC                 3      0
##   SNP                1      0
##   UKIP               6      0
```

{{% notice tip %}}
`tokens_lookup()` ignores multiple matches of dictionary values for the same key with the same token to avoide double counting. For example, if `US = c('United States of America', 'United States')` is in your dictionary, you get 'US' only once for a sequence of tokens `'United' 'States' 'of' 'America'`.
{{% /notice %}}
