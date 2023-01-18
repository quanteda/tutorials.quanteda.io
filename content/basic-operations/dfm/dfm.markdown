---
title: Construct a DFM
weight: 10
draft: false
---


```r
require(quanteda)
require(quanteda.textstats)
options(width = 110)
```

`dfm()` constructs a document-feature matrix (DFM) from a tokens object.


```r
toks_inaug <- tokens(data_corpus_inaugural, remove_punct = TRUE)
dfmat_inaug <- dfm(toks_inaug)
print(dfmat_inaug)
```

```
## Document-feature matrix of: 59 documents, 9,423 features (91.89% sparse) and 4 docvars.
##                  features
## docs              fellow-citizens  of the senate and house representatives among vicissitudes incident
##   1789-Washington               1  71 116      1  48     2               2     1            1        1
##   1793-Washington               0  11  13      0   2     0               0     0            0        0
##   1797-Adams                    3 140 163      1 130     0               2     4            0        0
##   1801-Jefferson                2 104 130      0  81     0               0     1            0        0
##   1805-Jefferson                0 101 143      0  93     0               0     7            0        0
##   1809-Madison                  1  69 104      0  43     0               0     0            0        0
## [ reached max_ndoc ... 53 more documents, reached max_nfeat ... 9,413 more features ]
```

You can get the number of documents and features `ndoc()` and `nfeat()`.


```r
ndoc(dfmat_inaug)
```

```
## [1] 59
```

```r
nfeat(dfmat_inaug)
```

```
## [1] 9423
```

You can also obtain the names of documents and features by `docnames()` and `featnames()`.


```r
head(docnames(dfmat_inaug), 20)
```

```
##  [1] "1789-Washington" "1793-Washington" "1797-Adams"      "1801-Jefferson"  "1805-Jefferson" 
##  [6] "1809-Madison"    "1813-Madison"    "1817-Monroe"     "1821-Monroe"     "1825-Adams"     
## [11] "1829-Jackson"    "1833-Jackson"    "1837-VanBuren"   "1841-Harrison"   "1845-Polk"      
## [16] "1849-Taylor"     "1853-Pierce"     "1857-Buchanan"   "1861-Lincoln"    "1865-Lincoln"
```

```r
head(featnames(dfmat_inaug), 20)
```

```
##  [1] "fellow-citizens" "of"              "the"             "senate"          "and"            
##  [6] "house"           "representatives" "among"           "vicissitudes"    "incident"       
## [11] "to"              "life"            "no"              "event"           "could"          
## [16] "have"            "filled"          "me"              "with"            "greater"
```

Just like normal matrices, you can use`rowSums()` and `colSums()` to calculate marginals. 


```r
head(rowSums(dfmat_inaug), 10)
```

```
## 1789-Washington 1793-Washington      1797-Adams  1801-Jefferson  1805-Jefferson    1809-Madison 
##            1430             135            2318            1726            2166            1175 
##    1813-Madison     1817-Monroe     1821-Monroe      1825-Adams 
##            1210            3370            4472            2915
```

```r
head(colSums(dfmat_inaug), 10)
```

```
## fellow-citizens              of             the          senate             and           house 
##              39            7180           10183              15            5406              11 
## representatives           among    vicissitudes        incident 
##              19             108               5               8
```

The most frequent features can be found using `topfeatures()`.


```r
topfeatures(dfmat_inaug, 10)
```

```
##   the    of   and    to    in     a   our    we  that    be 
## 10183  7180  5406  4591  2827  2292  2224  1827  1813  1502
```

If you want to convert the frequency count to a proportion within documents, use `dfm_weight(scheme  = "prop")`.


```r
dfmat_inaug_prop <- dfm_weight(dfmat_inaug, scheme  = "prop")
print(dfmat_inaug_prop)
```

```
## Document-feature matrix of: 59 documents, 9,423 features (91.89% sparse) and 4 docvars.
##                  features
## docs              fellow-citizens         of        the       senate        and       house representatives
##   1789-Washington    0.0006993007 0.04965035 0.08111888 0.0006993007 0.03356643 0.001398601    0.0013986014
##   1793-Washington    0            0.08148148 0.09629630 0            0.01481481 0              0           
##   1797-Adams         0.0012942192 0.06039689 0.07031924 0.0004314064 0.05608283 0              0.0008628128
##   1801-Jefferson     0.0011587486 0.06025492 0.07531866 0            0.04692932 0              0           
##   1805-Jefferson     0            0.04662973 0.06602031 0            0.04293629 0              0           
##   1809-Madison       0.0008510638 0.05872340 0.08851064 0            0.03659574 0              0           
##                  features
## docs                     among vicissitudes     incident
##   1789-Washington 0.0006993007 0.0006993007 0.0006993007
##   1793-Washington 0            0            0           
##   1797-Adams      0.0017256255 0            0           
##   1801-Jefferson  0.0005793743 0            0           
##   1805-Jefferson  0.0032317636 0            0           
##   1809-Madison    0            0            0           
## [ reached max_ndoc ... 53 more documents, reached max_nfeat ... 9,413 more features ]
```

{{% notice tip %}}
`textstat_frequency()`, described in Chapter 4, offers more advanced functionalities than `topfeatures()` and returns a `data.frame` object, making it easier to use the output for further analyses.
{{% /notice %}}


You can also weight the frequency count by uniqueness of the features across documents using `dfm_tfidf()`.


```r
dfmat_inaug_tfidf <- dfm_tfidf(dfmat_inaug)
print(dfmat_inaug_tfidf)
```

```
## Document-feature matrix of: 59 documents, 9,423 features (91.89% sparse) and 4 docvars.
##                  features
## docs              fellow-citizens of the    senate and    house representatives     among vicissitudes
##   1789-Washington       0.4920984  0   0 0.8166095   0 1.735524        1.249448 0.1373836     1.071882
##   1793-Washington       0          0   0 0           0 0               0        0             0       
##   1797-Adams            1.4762952  0   0 0.8166095   0 0               1.249448 0.5495342     0       
##   1801-Jefferson        0.9841968  0   0 0           0 0               0        0.1373836     0       
##   1805-Jefferson        0          0   0 0           0 0               0        0.9616849     0       
##   1809-Madison          0.4920984  0   0 0           0 0               0        0             0       
##                  features
## docs               incident
##   1789-Washington 0.9927008
##   1793-Washington 0        
##   1797-Adams      0        
##   1801-Jefferson  0        
##   1805-Jefferson  0        
##   1809-Madison    0        
## [ reached max_ndoc ... 53 more documents, reached max_nfeat ... 9,413 more features ]
```

{{% notice warning %}}
Even after applying  `dfm_weight()` or `dfm_tfidf()`, `topfeatures()` works on a document-feature matrix, but it can be misleading if applied to more than one document.
{{% /notice %}}
