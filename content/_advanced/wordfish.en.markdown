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
## 2010_BUDGET_01_Brian_Lenihan_FF        1.8209520 0.02032342  1.78111810
## 2010_BUDGET_02_Richard_Bruton_FG      -0.5932790 0.02818840 -0.64852830
## 2010_BUDGET_03_Joan_Burton_LAB        -1.1136768 0.01540260 -1.14386592
## 2010_BUDGET_04_Arthur_Morgan_SF       -0.1219309 0.02846323 -0.17771888
## 2010_BUDGET_05_Brian_Cowen_FF          1.7724214 0.02364092  1.72608523
## 2010_BUDGET_06_Enda_Kenny_FG          -0.7145790 0.02650260 -0.76652408
## 2010_BUDGET_07_Kieran_ODonnell_FG     -0.4844826 0.04171482 -0.56624368
## 2010_BUDGET_08_Eamon_Gilmore_LAB      -0.5616700 0.02967360 -0.61983030
## 2010_BUDGET_09_Michael_Higgins_LAB    -0.9703115 0.03850552 -1.04578229
## 2010_BUDGET_10_Ruairi_Quinn_LAB       -0.9589238 0.03892383 -1.03521447
## 2010_BUDGET_11_John_Gormley_Green      1.1807221 0.07221454  1.03918156
## 2010_BUDGET_12_Eamon_Ryan_Green        0.1866463 0.06294122  0.06328155
## 2010_BUDGET_13_Ciaran_Cuffe_Green      0.7421910 0.07245431  0.60018050
## 2010_BUDGET_14_Caoimhghin_OCaolain_SF -0.1840791 0.03666265 -0.25593787
##                                             upper
## 2010_BUDGET_01_Brian_Lenihan_FF        1.86078589
## 2010_BUDGET_02_Richard_Bruton_FG      -0.53802977
## 2010_BUDGET_03_Joan_Burton_LAB        -1.08348774
## 2010_BUDGET_04_Arthur_Morgan_SF       -0.06614301
## 2010_BUDGET_05_Brian_Cowen_FF          1.81875763
## 2010_BUDGET_06_Enda_Kenny_FG          -0.66263389
## 2010_BUDGET_07_Kieran_ODonnell_FG     -0.40272157
## 2010_BUDGET_08_Eamon_Gilmore_LAB      -0.50350978
## 2010_BUDGET_09_Michael_Higgins_LAB    -0.89484067
## 2010_BUDGET_10_Ruairi_Quinn_LAB       -0.88263305
## 2010_BUDGET_11_John_Gormley_Green      1.32226255
## 2010_BUDGET_12_Eamon_Ryan_Green        0.31001114
## 2010_BUDGET_13_Ciaran_Cuffe_Green      0.88420140
## 2010_BUDGET_14_Caoimhghin_OCaolain_SF -0.11222028
```

```r
textplot_scale1d(ieWF)
```

<img src="/_advanced/wordfish.en_files/figure-html/unnamed-chunk-1-1.svg" width="768" />
