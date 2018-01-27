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
ie_toks <- tokens(data_corpus_irishbudget2010, remove_punct = TRUE)
ie_dfm <- dfm(ie_toks)
```

If corpus is given to `dfm()`, it tokenize texts internally with the same level of control through the remove_* options. Therefore, the code above and below are equivalent:


```r
ie_dfm <- tokens(ie_corp, remove_punct = TRUE)
```

You can get the number of documents and features `ndoc()` and `nfeat()` 

```r
ndoc(ie_dfm)
```

```
## [1] 14
```

```r
nfeat(ie_dfm)
```

```
## [1] 5127
```

You can also get names of the documents and features by `docnames()` and `featnames()`.


```r
head(docnames(ie_dfm), 20)
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
head(featnames(ie_dfm), 20)
```

```
##  [1] "when"          "i"             "presented"     "the"          
##  [5] "supplementary" "budget"        "to"            "this"         
##  [9] "house"         "last"          "april"         "said"         
## [13] "we"            "could"         "work"          "our"          
## [17] "way"           "through"       "period"        "of"
```

You can use`rowSums()` and `colSums()` to calculate marginals. 


```r
head(rowSums(ie_dfm), 10)
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
head(colSums(ie_dfm), 10)
```

```
##          when             i     presented           the supplementary 
##            90           272             3          3598            10 
##        budget            to          this         house          last 
##           260          1633           559            49            47
```

The most frequent features can be found using `topfeatures()`.


```r
topfeatures(ie_dfm, 10)
```

```
##  the   to   of  and   in    a   is that   we  for 
## 3598 1633 1537 1359 1231 1013  868  804  618  578
```

