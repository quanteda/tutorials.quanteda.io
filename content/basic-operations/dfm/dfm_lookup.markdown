---
title: Look up dictionary
weight: 30
draft: false
---


```r
require(quanteda)
```

[laver-garry.cat](https://raw.githubusercontent.com/quanteda/tutorials.quanteda.io/master/content/dictionary/laver-garry.cat) is a Wordstat dictionary that contain political left-right ideology keywords (Laver and Garry 2000). 


```r
dict_lg <- dictionary(file = "content/dictionary/laver-garry.cat")
```



`dfm_lookup()` translates dictionary values to keys in a DFM.


```r
toks_irish <- dfm(data_corpus_irishbudget2010, remove_punct = TRUE)
dfmat_irish <- dfm(toks_irish)
dfmat_irish_lg <- dfm_lookup(dfmat_irish, dictionary = dict_lg)
head(dfmat_irish_lg)
```

```
## Document-feature matrix of: 6 documents, 20 features (30.0% sparse).
## 6 x 20 sparse Matrix of class "dfm"
##                       features
## docs                   CULTURE CULTURE.CULTURE-HIGH
##   Lenihan, Brian (FF)        8                    1
##   Bruton, Richard (FG)      35                    0
##   Burton, Joan (LAB)        31                    1
##   Morgan, Arthur (SF)       53                    0
##   Cowen, Brian (FF)         15                    1
##   Kenny, Enda (FG)          25                    1
##                       features
## docs                   CULTURE.CULTURE-POPULAR CULTURE.SPORT
##   Lenihan, Brian (FF)                        0             0
##   Bruton, Richard (FG)                       0             0
##   Burton, Joan (LAB)                         1             0
##   Morgan, Arthur (SF)                        3             0
##   Cowen, Brian (FF)                          0             0
##   Kenny, Enda (FG)                           0             0
##                       features
## docs                   ECONOMY.+STATE+ ECONOMY.=STATE= ECONOMY.-STATE-
##   Lenihan, Brian (FF)              115             355             113
##   Bruton, Richard (FG)              35             131              35
##   Burton, Joan (LAB)               134             206              61
##   Morgan, Arthur (SF)               92             286              49
##   Cowen, Brian (FF)                 92             244              81
##   Kenny, Enda (FG)                  46             138              27
##                       features
## docs                   ENVIRONMENT.CON ENVIRONMENT
##   Lenihan, Brian (FF)                            4
##   Bruton, Richard (FG)                           3
##   Burton, Joan (LAB)                             0
##   Morgan, Arthur (SF)                            1
##   Cowen, Brian (FF)                              7
##   Kenny, Enda (FG)                               2
##                       features
## docs                   ENVIRONMENT.PRO ENVIRONMENT GROUPS.ETHNIC
##   Lenihan, Brian (FF)                           17             0
##   Bruton, Richard (FG)                           2             0
##   Burton, Joan (LAB)                             6             0
##   Morgan, Arthur (SF)                            9             0
##   Cowen, Brian (FF)                             17             0
##   Kenny, Enda (FG)                               6             0
##                       features
## docs                   GROUPS.WOMEN INSTITUTIONS.CONSERVATIVE
##   Lenihan, Brian (FF)             0                        13
##   Bruton, Richard (FG)            0                         6
##   Burton, Joan (LAB)              3                         5
##   Morgan, Arthur (SF)             0                         6
##   Cowen, Brian (FF)               0                        19
##   Kenny, Enda (FG)                1                        10
##                       features
## docs                   INSTITUTIONS.NEUTRAL INSTITUTIONS.RADICAL
##   Lenihan, Brian (FF)                    63                   17
##   Bruton, Richard (FG)                   63                   26
##   Burton, Joan (LAB)                     68                   11
##   Morgan, Arthur (SF)                    48                    9
##   Cowen, Brian (FF)                      34                   10
##   Kenny, Enda (FG)                       34                    9
##                       features
## docs                   LAW_AND_ORDER.LAW-CONSERVATIVE
##   Lenihan, Brian (FF)                              11
##   Bruton, Richard (FG)                             14
##   Burton, Joan (LAB)                                6
##   Morgan, Arthur (SF)                              22
##   Cowen, Brian (FF)                                 4
##   Kenny, Enda (FG)                                 18
##                       features
## docs                   LAW_AND_ORDER.LAW-LIBERAL RURAL URBAN
##   Lenihan, Brian (FF)                          0     9     0
##   Bruton, Richard (FG)                         0     0     0
##   Burton, Joan (LAB)                           0     2     3
##   Morgan, Arthur (SF)                          0     2     1
##   Cowen, Brian (FF)                            0     8     1
##   Kenny, Enda (FG)                             0     0     2
##                       features
## docs                   VALUES.CONSERVATIVE VALUES.LIBERAL
##   Lenihan, Brian (FF)                   19              0
##   Bruton, Richard (FG)                  14              0
##   Burton, Joan (LAB)                     5              1
##   Morgan, Arthur (SF)                   16              2
##   Cowen, Brian (FF)                     13              0
##   Kenny, Enda (FG)                       7              1
```

You can also pass a dictionary to `dfm()` to simplify your code. Therefore, the code above and below are equivalent.


```r
dfmat_irish_lg <- dfm(data_corpus_irishbudget2010, dictionary = dict_lg, remove_punct = TRUE)
```

{{% notice note %}}
`dfm_lookup()` cannot find multi-word expressions since a document-feature matrix does not store information about positions of words. Yet, `dfm()` can handle multi-word expressions because dictionary lookup is performed internally on tokens using `tokens_lookup()`. Afterwards, you can create a document-feature matrix from this object.
{{% /notice %}}

