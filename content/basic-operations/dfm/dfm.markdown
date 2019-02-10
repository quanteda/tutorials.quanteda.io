---
title: Construct a DFM
weight: 10
draft: false
---


```r
require(quanteda)
```

`dfm()` constructs a document-feature matrix (DFM) from a tokens object.


```r
toks_irish <- tokens(data_corpus_irishbudget2010, remove_punct = TRUE)
dfmat_irish <- dfm(toks_irish)
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
## [1] 5127
```

You can also obtain the names of documents and features by `docnames()` and `featnames()`.


```r
head(docnames(dfmat_irish), 20)
```

```
##  [1] "Lenihan, Brian (FF)"       "Bruton, Richard (FG)"     
##  [3] "Burton, Joan (LAB)"        "Morgan, Arthur (SF)"      
##  [5] "Cowen, Brian (FF)"         "Kenny, Enda (FG)"         
##  [7] "ODonnell, Kieran (FG)"     "Gilmore, Eamon (LAB)"     
##  [9] "Higgins, Michael (LAB)"    "Quinn, Ruairi (LAB)"      
## [11] "Gormley, John (Green)"     "Ryan, Eamon (Green)"      
## [13] "Cuffe, Ciaran (Green)"     "OCaolain, Caoimhghin (SF)"
```

```r
head(featnames(dfmat_irish), 20)
```

```
##  [1] "when"          "i"             "presented"     "the"          
##  [5] "supplementary" "budget"        "to"            "this"         
##  [9] "house"         "last"          "april"         "said"         
## [13] "we"            "could"         "work"          "our"          
## [17] "way"           "through"       "period"        "of"
```

Just like normal matrices, you can use`rowSums()` and `colSums()` to calculate marginals. 


```r
head(rowSums(dfmat_irish), 10)
```

```
##    Lenihan, Brian (FF)   Bruton, Richard (FG)     Burton, Joan (LAB) 
##                   7916                   4086                   5790 
##    Morgan, Arthur (SF)      Cowen, Brian (FF)       Kenny, Enda (FG) 
##                   6510                   5964                   3896 
##  ODonnell, Kieran (FG)   Gilmore, Eamon (LAB) Higgins, Michael (LAB) 
##                   2086                   3807                   1149 
##    Quinn, Ruairi (LAB) 
##                   1181
```

```r
head(colSums(dfmat_irish), 10)
```

```
##          when             i     presented           the supplementary 
##            90           272             3          3598            10 
##        budget            to          this         house          last 
##           260          1633           559            49            47
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
# check topfeatures in first document
topfeatures(dfmat_irish_prop[1,])
```

```
##        the         to         of         in        and          a 
## 0.06808994 0.03852956 0.03688732 0.02892875 0.02475998 0.01806468 
##       will        for       this         we 
## 0.01755937 0.01364325 0.01250632 0.01237999
```

{{% notice tip %}}
`textstat_frequency()`, desribed in chapter 4 offers more advanced functionalities than `topfeatures()` and returns a `data.frame` object, making it easier to use the output for further analyses.
{{% /notice %}}


You can also weight frequency count by uniqueness of the features across documents using `dfm_tfidf()`.


```r
dfmat_irish_tfidf <- dfm_tfidf(dfmat_irish)
# check topfeatures in first document
topfeatures(dfmat_irish_tfidf[1,])
```

```
##    details     review   measures reductions    shortly      level 
##  11.831373   8.943161   7.525750   7.072885   6.876768   6.707370 
##     scheme       body       2010    summary 
##   6.321630   5.915686   5.832913   5.730640
```

{{% notice warning %}}
Even after applying  `dfm_weight()` or `dfm_tfidf()`, `topfeatures()` works on a document-feature matrix, but it can be misleading if applied to more than one document.
{{% /notice %}}
