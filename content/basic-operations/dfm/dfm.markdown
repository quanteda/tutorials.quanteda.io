---
title: Construct a DFM
weight: 10
draft: false
---


```r
require(quanteda)
```

`dfm()` constructs a document-feature matrix (DFM) from a tokens.


```r
irish_toks <- tokens(data_corpus_irishbudget2010, remove_punct = TRUE)
irish_dfm <- dfm(irish_toks)
```

If corpus is given to `dfm()`, it tokenizes texts internally with the same level of control through the `remove_*` arguments of `tokens()`. Therefore, the code above and below are equivalent.


```r
irish_dfm <- tokens(data_corpus_irishbudget2010, remove_punct = TRUE) %>% 
  dfm()
```

You can get the number of documents and features `ndoc()` and `nfeat()`.


```r
ndoc(irish_dfm)
```

```
## [1] 14
```

```r
nfeat(irish_dfm)
```

```
## [1] 5127
```

You can also obtain the names of documents and features by `docnames()` and `featnames()`.


```r
head(docnames(irish_dfm), 20)
```

```
##  [1] "2010_BUDGET_01_Brian_Lenihan_FF"      
##  [2] "2010_BUDGET_02_Richard_Bruton_FG"     
##  [3] "2010_BUDGET_03_Joan_Burton_LAB"       
##  [4] "2010_BUDGET_04_Arthur_Morgan_SF"      
##  [5] "2010_BUDGET_05_Brian_Cowen_FF"        
##  [6] "2010_BUDGET_06_Enda_Kenny_FG"         
##  [7] "2010_BUDGET_07_Kieran_ODonnell_FG"    
##  [8] "2010_BUDGET_08_Eamon_Gilmore_LAB"     
##  [9] "2010_BUDGET_09_Michael_Higgins_LAB"   
## [10] "2010_BUDGET_10_Ruairi_Quinn_LAB"      
## [11] "2010_BUDGET_11_John_Gormley_Green"    
## [12] "2010_BUDGET_12_Eamon_Ryan_Green"      
## [13] "2010_BUDGET_13_Ciaran_Cuffe_Green"    
## [14] "2010_BUDGET_14_Caoimhghin_OCaolain_SF"
```

```r
head(featnames(irish_dfm), 20)
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
head(rowSums(irish_dfm), 10)
```

```
##    2010_BUDGET_01_Brian_Lenihan_FF   2010_BUDGET_02_Richard_Bruton_FG 
##                               7916                               4086 
##     2010_BUDGET_03_Joan_Burton_LAB    2010_BUDGET_04_Arthur_Morgan_SF 
##                               5790                               6510 
##      2010_BUDGET_05_Brian_Cowen_FF       2010_BUDGET_06_Enda_Kenny_FG 
##                               5964                               3896 
##  2010_BUDGET_07_Kieran_ODonnell_FG   2010_BUDGET_08_Eamon_Gilmore_LAB 
##                               2086                               3807 
## 2010_BUDGET_09_Michael_Higgins_LAB    2010_BUDGET_10_Ruairi_Quinn_LAB 
##                               1149                               1181
```

```r
head(colSums(irish_dfm), 10)
```

```
##          when             i     presented           the supplementary 
##            90           272             3          3598            10 
##        budget            to          this         house          last 
##           260          1633           559            49            47
```

The most frequent features can be found using `topfeatures()`.


```r
topfeatures(irish_dfm, 10)
```

```
##  the   to   of  and   in    a   is that   we  for 
## 3598 1633 1537 1359 1231 1013  868  804  618  578
```

If you want to convert frequency count to proportion with in documents, use `dfm_weight(scheme  = "prop")`.


```r
prop_irish_dfm <- dfm_weight(irish_dfm, scheme  = "prop")
topfeatures(prop_irish_dfm[1,])
```

```
##        the         to         of         in        and          a 
## 0.06808994 0.03852956 0.03688732 0.02892875 0.02475998 0.01806468 
##       will        for       this         we 
## 0.01755937 0.01364325 0.01250632 0.01237999
```

You can also weight frequency count by uniqueness of the features across docuemnt using `dfm_tfidf()`.


```r
tfidf_irish_dfm <- dfm_tfidf(irish_dfm)
topfeatures(tfidf_irish_dfm[1,])
```

```
##    details     review   measures reductions    shortly      level 
##  11.831373   8.943161   7.525750   7.072885   6.876768   6.707370 
##     scheme       body       2010    summary 
##   6.321630   5.915686   5.832913   5.730640
```

{{% notice warning %}}
Even after applying  `dfm_weight()` or `dfm_tfidf()`, `topfeatures()` works on a DFM, but it can be misleading if applied to more than one document.
{{% /notice %}}
