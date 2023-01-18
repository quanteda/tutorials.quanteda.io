---
title: Look up dictionary
weight: 30
draft: false
---


```r
require(quanteda)
require(quanteda.textmodels)
options(width = 110)
```

[laver-garry.cat](https://raw.githubusercontent.com/quanteda/tutorials.quanteda.io/master/content/dictionary/laver-garry.cat) is a Wordstat dictionary that contain political left-right ideology keywords (Laver and Garry 2000). 


```r
dict_lg <- dictionary(file = "../../dictionary/laver-garry.cat", encoding = "UTF-8")
```

`dfm_lookup()` translates dictionary values to keys in a DFM.


```r
toks_irish <- tokens(data_corpus_irishbudget2010, remove_punct = TRUE)
dfmat_irish <- dfm(toks_irish)
print(dfmat_irish)
```

```
## Document-feature matrix of: 14 documents, 5,129 features (81.27% sparse) and 6 docvars.
##                       features
## docs                   when  i presented the supplementary budget  to this house last
##   Lenihan, Brian (FF)     5 73         1 539             7     23 305   99    10    6
##   Bruton, Richard (FG)    2  6         0 305             0     27 172   55     0    5
##   Burton, Joan (LAB)     11 40         0 428             0     37 157   53     6    4
##   Morgan, Arthur (SF)    21 26         0 501             1     26 204   85     5    4
##   Cowen, Brian (FF)       4 17         0 394             0     21 209   43     4    6
##   Kenny, Enda (FG)       12 25         1 304             1     23 119   47     0    4
## [ reached max_ndoc ... 8 more documents, reached max_nfeat ... 5,119 more features ]
```

```r
dfmat_irish_lg <- dfm_lookup(dfmat_irish, dictionary = dict_lg, levels = 1)
print(dfmat_irish_lg)
```

```
## Document-feature matrix of: 14 documents, 9 features (19.84% sparse) and 6 docvars.
##                       features
## docs                   CULTURE ECONOMY ENVIRONMENT GROUPS INSTITUTIONS LAW_AND_ORDER RURAL URBAN VALUES
##   Lenihan, Brian (FF)        9     583          21      0           93            11     9     0     19
##   Bruton, Richard (FG)      35     201           5      0           95            14     0     0     14
##   Burton, Joan (LAB)        33     400           6      3           84             6     2     3      6
##   Morgan, Arthur (SF)       56     427          10      0           63            22     2     1     18
##   Cowen, Brian (FF)         16     416          24      0           63             4     8     1     13
##   Kenny, Enda (FG)          26     211           8      1           53            18     0     2      8
## [ reached max_ndoc ... 8 more documents ]
```


{{% notice note %}}
`dfm_lookup()` cannot detect multi-word expressions since a document-feature matrix does not store information about positions of words. We recommend using `tokens_lookup()` to detect or `tokens_copound()` to compound multi-word expressions before creating a DFM.
{{% /notice %}}

