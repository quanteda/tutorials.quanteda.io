---
title: Wordfish
draft: true
---



Poisson scaling


```r
ieWF <- dfm(data_corpus_irishbudget2010, removePunct = TRUE) %>%
    textmodel_wordfish(dir = c(6,5))
## Warning: Argument removePunct not used.
textplot_scale1d(ieWF)
```

<img src="/advanced/wordfish.en_files/figure-html/unnamed-chunk-2-1.svg" width="768" />

```r
summary(ieWF)
## Call:
## 	textmodel_wordfish.dfm(x = ., dir = c(6, 5))
## 
## Estimated document positions:
##                                            theta         SE       lower
## 2010_BUDGET_01_Brian_Lenihan_FF        1.8209724 0.02032394  1.78113753
## 2010_BUDGET_02_Richard_Bruton_FG      -0.5932640 0.02818781 -0.64851211
## 2010_BUDGET_03_Joan_Burton_LAB        -1.1136557 0.01540208 -1.14384383
## 2010_BUDGET_04_Arthur_Morgan_SF       -0.1219514 0.02846263 -0.17773818
## 2010_BUDGET_05_Brian_Cowen_FF          1.7724352 0.02364156  1.72609773
## 2010_BUDGET_06_Enda_Kenny_FG          -0.7145712 0.02650180 -0.76651473
## 2010_BUDGET_07_Kieran_ODonnell_FG     -0.4844752 0.04171391 -0.56623446
## 2010_BUDGET_08_Eamon_Gilmore_LAB      -0.5616870 0.02967240 -0.61984489
## 2010_BUDGET_09_Michael_Higgins_LAB    -0.9702997 0.03850410 -1.04576772
## 2010_BUDGET_10_Ruairi_Quinn_LAB       -0.9589125 0.03892240 -1.03520039
## 2010_BUDGET_11_John_Gormley_Green      1.1807216 0.07221584  1.03917850
## 2010_BUDGET_12_Eamon_Ryan_Green        0.1866368 0.06294078  0.06327288
## 2010_BUDGET_13_Ciaran_Cuffe_Green      0.7421707 0.07245504  0.60015887
## 2010_BUDGET_14_Caoimhghin_OCaolain_SF -0.1841200 0.03666148 -0.25597654
##                                             upper
## 2010_BUDGET_01_Brian_Lenihan_FF        1.86080737
## 2010_BUDGET_02_Richard_Bruton_FG      -0.53801588
## 2010_BUDGET_03_Joan_Burton_LAB        -1.08346766
## 2010_BUDGET_04_Arthur_Morgan_SF       -0.06616465
## 2010_BUDGET_05_Brian_Cowen_FF          1.81877266
## 2010_BUDGET_06_Enda_Kenny_FG          -0.66262769
## 2010_BUDGET_07_Kieran_ODonnell_FG     -0.40271595
## 2010_BUDGET_08_Eamon_Gilmore_LAB      -0.50352908
## 2010_BUDGET_09_Michael_Higgins_LAB    -0.89483164
## 2010_BUDGET_10_Ruairi_Quinn_LAB       -0.88262458
## 2010_BUDGET_11_John_Gormley_Green      1.32226461
## 2010_BUDGET_12_Eamon_Ryan_Green        0.31000074
## 2010_BUDGET_13_Ciaran_Cuffe_Green      0.88418261
## 2010_BUDGET_14_Caoimhghin_OCaolain_SF -0.11226352
```


```r
knitr::kable(summary(ieWF))
```

Call:
	textmodel_wordfish.dfm(x = ., dir = c(6, 5))

Estimated document positions:
                                           theta         SE       lower
2010_BUDGET_01_Brian_Lenihan_FF        1.8209724 0.02032394  1.78113753
2010_BUDGET_02_Richard_Bruton_FG      -0.5932640 0.02818781 -0.64851211
2010_BUDGET_03_Joan_Burton_LAB        -1.1136557 0.01540208 -1.14384383
2010_BUDGET_04_Arthur_Morgan_SF       -0.1219514 0.02846263 -0.17773818
2010_BUDGET_05_Brian_Cowen_FF          1.7724352 0.02364156  1.72609773
2010_BUDGET_06_Enda_Kenny_FG          -0.7145712 0.02650180 -0.76651473
2010_BUDGET_07_Kieran_ODonnell_FG     -0.4844752 0.04171391 -0.56623446
2010_BUDGET_08_Eamon_Gilmore_LAB      -0.5616870 0.02967240 -0.61984489
2010_BUDGET_09_Michael_Higgins_LAB    -0.9702997 0.03850410 -1.04576772
2010_BUDGET_10_Ruairi_Quinn_LAB       -0.9589125 0.03892240 -1.03520039
2010_BUDGET_11_John_Gormley_Green      1.1807216 0.07221584  1.03917850
2010_BUDGET_12_Eamon_Ryan_Green        0.1866368 0.06294078  0.06327288
2010_BUDGET_13_Ciaran_Cuffe_Green      0.7421707 0.07245504  0.60015887
2010_BUDGET_14_Caoimhghin_OCaolain_SF -0.1841200 0.03666148 -0.25597654
                                            upper
2010_BUDGET_01_Brian_Lenihan_FF        1.86080737
2010_BUDGET_02_Richard_Bruton_FG      -0.53801588
2010_BUDGET_03_Joan_Burton_LAB        -1.08346766
2010_BUDGET_04_Arthur_Morgan_SF       -0.06616465
2010_BUDGET_05_Brian_Cowen_FF          1.81877266
2010_BUDGET_06_Enda_Kenny_FG          -0.66262769
2010_BUDGET_07_Kieran_ODonnell_FG     -0.40271595
2010_BUDGET_08_Eamon_Gilmore_LAB      -0.50352908
2010_BUDGET_09_Michael_Higgins_LAB    -0.89483164
2010_BUDGET_10_Ruairi_Quinn_LAB       -0.88262458
2010_BUDGET_11_John_Gormley_Green      1.32226461
2010_BUDGET_12_Eamon_Ryan_Green        0.31000074
2010_BUDGET_13_Ciaran_Cuffe_Green      0.88418261
2010_BUDGET_14_Caoimhghin_OCaolain_SF -0.11226352


|                                      |      theta|        SE|      lower|      upper|
|:-------------------------------------|----------:|---------:|----------:|----------:|
|2010_BUDGET_01_Brian_Lenihan_FF       |  1.8209724| 0.0203239|  1.7811375|  1.8608074|
|2010_BUDGET_02_Richard_Bruton_FG      | -0.5932640| 0.0281878| -0.6485121| -0.5380159|
|2010_BUDGET_03_Joan_Burton_LAB        | -1.1136557| 0.0154021| -1.1438438| -1.0834677|
|2010_BUDGET_04_Arthur_Morgan_SF       | -0.1219514| 0.0284626| -0.1777382| -0.0661647|
|2010_BUDGET_05_Brian_Cowen_FF         |  1.7724352| 0.0236416|  1.7260977|  1.8187727|
|2010_BUDGET_06_Enda_Kenny_FG          | -0.7145712| 0.0265018| -0.7665147| -0.6626277|
|2010_BUDGET_07_Kieran_ODonnell_FG     | -0.4844752| 0.0417139| -0.5662345| -0.4027159|
|2010_BUDGET_08_Eamon_Gilmore_LAB      | -0.5616870| 0.0296724| -0.6198449| -0.5035291|
|2010_BUDGET_09_Michael_Higgins_LAB    | -0.9702997| 0.0385041| -1.0457677| -0.8948316|
|2010_BUDGET_10_Ruairi_Quinn_LAB       | -0.9589125| 0.0389224| -1.0352004| -0.8826246|
|2010_BUDGET_11_John_Gormley_Green     |  1.1807216| 0.0722158|  1.0391785|  1.3222646|
|2010_BUDGET_12_Eamon_Ryan_Green       |  0.1866368| 0.0629408|  0.0632729|  0.3100007|
|2010_BUDGET_13_Ciaran_Cuffe_Green     |  0.7421707| 0.0724550|  0.6001589|  0.8841826|
|2010_BUDGET_14_Caoimhghin_OCaolain_SF | -0.1841200| 0.0366615| -0.2559765| -0.1122635|
