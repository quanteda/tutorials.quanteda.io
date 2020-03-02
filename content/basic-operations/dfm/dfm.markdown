---
title: Construct a DFM
weight: 10
draft: false
---


```r
require(quanteda)
options(width = 110)
```

`dfm()` constructs a document-feature matrix (DFM) from a tokens object.


```r
toks_irish <- tokens(data_corpus_irishbudget2010, remove_punct = TRUE)
dfmat_irish <- dfm(toks_irish)
print(dfmat_irish)
```

```
## Document-feature matrix of: 14 documents, 5,129 features (81.3% sparse) and 6 docvars.
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

If corpus is given to `dfm()`, it tokenizes texts internally with the same level of control through the `remove_*` arguments of `tokens()`. Therefore, the code above and below are equivalent.


```r
dfmat_irish <- data_corpus_irishbudget2010 %>% 
    tokens(remove_punct = TRUE) %>% 
    dfm()
```

You can get the number of documents and features `ndoc()` and `nfeat()`.


```r
ndoc(dfmat_irish)
```

```
## [1] 14
```

```r
nfeat(dfmat_irish)
```

```
## [1] 5129
```

You can also obtain the names of documents and features by `docnames()` and `featnames()`.


```r
head(docnames(dfmat_irish), 20)
```

```
##  [1] "Lenihan, Brian (FF)"       "Bruton, Richard (FG)"      "Burton, Joan (LAB)"       
##  [4] "Morgan, Arthur (SF)"       "Cowen, Brian (FF)"         "Kenny, Enda (FG)"         
##  [7] "ODonnell, Kieran (FG)"     "Gilmore, Eamon (LAB)"      "Higgins, Michael (LAB)"   
## [10] "Quinn, Ruairi (LAB)"       "Gormley, John (Green)"     "Ryan, Eamon (Green)"      
## [13] "Cuffe, Ciaran (Green)"     "OCaolain, Caoimhghin (SF)"
```

```r
head(featnames(dfmat_irish), 20)
```

```
##  [1] "when"          "i"             "presented"     "the"           "supplementary" "budget"       
##  [7] "to"            "this"          "house"         "last"          "april"         "said"         
## [13] "we"            "could"         "work"          "our"           "way"           "through"      
## [19] "period"        "of"
```

Just like normal matrices, you can use`rowSums()` and `colSums()` to calculate marginals. 


```r
head(rowSums(dfmat_irish), 10)
```

```
##    Lenihan, Brian (FF)   Bruton, Richard (FG)     Burton, Joan (LAB)    Morgan, Arthur (SF) 
##                   7991                   4104                   5839                   6552 
##      Cowen, Brian (FF)       Kenny, Enda (FG)  ODonnell, Kieran (FG)   Gilmore, Eamon (LAB) 
##                   6002                   3916                   2103                   3832 
## Higgins, Michael (LAB)    Quinn, Ruairi (LAB) 
##                   1153                   1181
```

```r
head(colSums(dfmat_irish), 10)
```

```
##          when             i     presented           the supplementary        budget            to 
##            90           272             3          3598            10           260          1633 
##          this         house          last 
##           559            49            47
```

The most frequent features can be found using `topfeatures()`.


```r
topfeatures(dfmat_irish, 10)
```

```
##  the   to   of  and   in    a   is that   we  for 
## 3598 1633 1537 1359 1231 1013  868  804  618  578
```

If you want to convert the frequency count to a proportion within documents, use `dfm_weight(scheme  = "prop")`.


```r
dfmat_irish_prop <- dfm_weight(dfmat_irish, scheme  = "prop")
print(dfmat_irish_prop)
```

```
## Document-feature matrix of: 14 documents, 5,129 features (81.3% sparse) and 6 docvars.
##                       features
## docs                           when           i    presented        the supplementary      budget         to
##   Lenihan, Brian (FF)  0.0006257039 0.009135277 0.0001251408 0.06745088  0.0008759855 0.002878238 0.03816794
##   Bruton, Richard (FG) 0.0004873294 0.001461988 0            0.07431774  0            0.006578947 0.04191033
##   Burton, Joan (LAB)   0.0018838842 0.006850488 0            0.07330022  0            0.006336701 0.02688817
##   Morgan, Arthur (SF)  0.0032051282 0.003968254 0            0.07646520  0.0001526252 0.003968254 0.03113553
##   Cowen, Brian (FF)    0.0006664445 0.002832389 0            0.06564479  0            0.003498834 0.03482173
##   Kenny, Enda (FG)     0.0030643514 0.006384065 0.0002553626 0.07763023  0.0002553626 0.005873340 0.03038815
##                       features
## docs                          this        house         last
##   Lenihan, Brian (FF)  0.012388938 0.0012514078 0.0007508447
##   Bruton, Richard (FG) 0.013401559 0            0.0012183236
##   Burton, Joan (LAB)   0.009076897 0.0010275732 0.0006850488
##   Morgan, Arthur (SF)  0.012973138 0.0007631258 0.0006105006
##   Cowen, Brian (FF)    0.007164279 0.0006664445 0.0009996668
##   Kenny, Enda (FG)     0.012002043 0            0.0010214505
## [ reached max_ndoc ... 8 more documents, reached max_nfeat ... 5,119 more features ]
```

{{% notice tip %}}
`textstat_frequency()`, desribed in chapter 4 offers more advanced functionalities than `topfeatures()` and returns a `data.frame` object, making it easier to use the output for further analyses.
{{% /notice %}}


You can also weight frequency count by uniqueness of the features across documents using `dfm_tfidf()`.


```r
dfmat_irish_tfidf <- dfm_tfidf(dfmat_irish)
print(dfmat_irish_tfidf)
```

```
## Document-feature matrix of: 14 documents, 5,129 features (81.3% sparse) and 6 docvars.
##                       features
## docs                         when i presented the supplementary budget to this     house      last
##   Lenihan, Brian (FF)  0.16092342 0 0.6690068   0      3.808476      0  0    0 1.0473535 0.1931081
##   Bruton, Richard (FG) 0.06436937 0 0           0      0             0  0    0 0         0.1609234
##   Burton, Joan (LAB)   0.35403152 0 0           0      0             0  0    0 0.6284121 0.1287387
##   Morgan, Arthur (SF)  0.67587835 0 0           0      0.544068      0  0    0 0.5236768 0.1287387
##   Cowen, Brian (FF)    0.12873873 0 0           0      0             0  0    0 0.4189414 0.1931081
##   Kenny, Enda (FG)     0.38621620 0 0.6690068   0      0.544068      0  0    0 0         0.1287387
## [ reached max_ndoc ... 8 more documents, reached max_nfeat ... 5,119 more features ]
```

{{% notice warning %}}
Even after applying  `dfm_weight()` or `dfm_tfidf()`, `topfeatures()` works on a document-feature matrix, but it can be misleading if applied to more than one document.
{{% /notice %}}
