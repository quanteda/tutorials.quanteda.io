---
title: Wordfish
---
## Poisson scaling:

```r
require(quanteda)
```

```
## Loading required package: quanteda
```

```
## quanteda version 0.99.9
```

```
## Using 3 of 4 threads for parallel computing
```

```
## 
## Attaching package: 'quanteda'
```

```
## The following object is masked from 'package:utils':
## 
##     View
```

```r
ieWF <- dfm(data_corpus_irishbudget2010, removePunct = TRUE) %>%
    textmodel_wordfish(dir = c(6,5))
```

```
## Warning: Argument removePunct not used.
```

```r
summary(ieWF)
```

```
## Call:
## 	textmodel_wordfish.dfm(x = ., dir = c(6, 5))
## 
## Estimated document positions:
##                                            theta         SE       lower
## 2010_BUDGET_01_Brian_Lenihan_FF        1.8209524 0.02032343  1.78111849
## 2010_BUDGET_02_Richard_Bruton_FG      -0.5932787 0.02818839 -0.64852798
## 2010_BUDGET_03_Joan_Burton_LAB        -1.1136764 0.01540259 -1.14386548
## 2010_BUDGET_04_Arthur_Morgan_SF       -0.1219314 0.02846322 -0.17771927
## 2010_BUDGET_05_Brian_Cowen_FF          1.7724217 0.02364093  1.72608548
## 2010_BUDGET_06_Enda_Kenny_FG          -0.7145788 0.02650258 -0.76652389
## 2010_BUDGET_07_Kieran_ODonnell_FG     -0.4844825 0.04171481 -0.56624349
## 2010_BUDGET_08_Eamon_Gilmore_LAB      -0.5616704 0.02967358 -0.61983060
## 2010_BUDGET_09_Michael_Higgins_LAB    -0.9703112 0.03850549 -1.04578199
## 2010_BUDGET_10_Ruairi_Quinn_LAB       -0.9589235 0.03892380 -1.03521418
## 2010_BUDGET_11_John_Gormley_Green      1.1807220 0.07221456  1.03918150
## 2010_BUDGET_12_Eamon_Ryan_Green        0.1866462 0.06294121  0.06328138
## 2010_BUDGET_13_Ciaran_Cuffe_Green      0.7421905 0.07245432  0.60018007
## 2010_BUDGET_14_Caoimhghin_OCaolain_SF -0.1840799 0.03666263 -0.25593865
##                                             upper
## 2010_BUDGET_01_Brian_Lenihan_FF        1.86078632
## 2010_BUDGET_02_Richard_Bruton_FG      -0.53802950
## 2010_BUDGET_03_Joan_Burton_LAB        -1.08348734
## 2010_BUDGET_04_Arthur_Morgan_SF       -0.06614345
## 2010_BUDGET_05_Brian_Cowen_FF          1.81875793
## 2010_BUDGET_06_Enda_Kenny_FG          -0.66263377
## 2010_BUDGET_07_Kieran_ODonnell_FG     -0.40272145
## 2010_BUDGET_08_Eamon_Gilmore_LAB      -0.50351017
## 2010_BUDGET_09_Michael_Higgins_LAB    -0.89484047
## 2010_BUDGET_10_Ruairi_Quinn_LAB       -0.88263287
## 2010_BUDGET_11_John_Gormley_Green      1.32226259
## 2010_BUDGET_12_Eamon_Ryan_Green        0.31001093
## 2010_BUDGET_13_Ciaran_Cuffe_Green      0.88420102
## 2010_BUDGET_14_Caoimhghin_OCaolain_SF -0.11222115
```

```r
textplot_scale1d(ieWF)
```

<img src="/_advanced/wordfish.en_files/figure-html/unnamed-chunk-1-1.svg" width="768" />
