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
dict_lg <- dictionary(file = "../../dictionary/laver-garry.cat")
```

`dfm_lookup()` translates dictionary values to keys in a DFM.


```r
toks_irish <- dfm(data_corpus_irishbudget2010, remove_punct = TRUE)
dfmat_irish <- dfm(toks_irish)
dfmat_irish_lg <- dfm_lookup(dfmat_irish, dictionary = dict_lg)
head(dfmat_irish_lg)
```

```
## Document-feature matrix of: 6 documents, 20 features (30.0% sparse) and 6 docvars.
##                       features
## docs                   CULTURE CULTURE.CULTURE-HIGH CULTURE.CULTURE-POPULAR
##   Lenihan, Brian (FF)        8                    1                       0
##   Bruton, Richard (FG)      35                    0                       0
##   Burton, Joan (LAB)        31                    1                       1
##   Morgan, Arthur (SF)       53                    0                       3
##   Cowen, Brian (FF)         15                    1                       0
##   Kenny, Enda (FG)          25                    1                       0
##                       features
## docs                   CULTURE.SPORT ECONOMY.+STATE+ ECONOMY.=STATE=
##   Lenihan, Brian (FF)              0             115             355
##   Bruton, Richard (FG)             0              35             131
##   Burton, Joan (LAB)               0             134             206
##   Morgan, Arthur (SF)              0              92             286
##   Cowen, Brian (FF)                0              92             244
##   Kenny, Enda (FG)                 0              46             138
##                       features
## docs                   ECONOMY.-STATE- ENVIRONMENT.CON ENVIRONMENT
##   Lenihan, Brian (FF)              113                           4
##   Bruton, Richard (FG)              35                           3
##   Burton, Joan (LAB)                61                           0
##   Morgan, Arthur (SF)               49                           1
##   Cowen, Brian (FF)                 81                           7
##   Kenny, Enda (FG)                  27                           2
##                       features
## docs                   ENVIRONMENT.PRO ENVIRONMENT GROUPS.ETHNIC
##   Lenihan, Brian (FF)                           17             0
##   Bruton, Richard (FG)                           2             0
##   Burton, Joan (LAB)                             6             0
##   Morgan, Arthur (SF)                            9             0
##   Cowen, Brian (FF)                             17             0
##   Kenny, Enda (FG)                               6             0
## [ reached max_nfeat ... 10 more features ]
```

You can also pass a dictionary to `dfm()` to simplify your code. Therefore, the code above and below are equivalent.


```r
dfmat_irish_lg <- dfm(data_corpus_irishbudget2010, dictionary = dict_lg, remove_punct = TRUE)
```

{{% notice note %}}
`dfm_lookup()` cannot find multi-word expressions since a document-feature matrix does not store information about positions of words. Yet, `dfm()` can handle multi-word expressions because dictionary lookup is performed internally on tokens using `tokens_lookup()`. Afterwards, you can create a document-feature matrix from this object.
{{% /notice %}}

