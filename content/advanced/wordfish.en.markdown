---
title: Wordfish
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
## 2010_BUDGET_01_Brian_Lenihan_FF        1.8209554 0.02032350  1.78112134
## 2010_BUDGET_02_Richard_Bruton_FG      -0.5932763 0.02818830 -0.64852541
## 2010_BUDGET_03_Joan_Burton_LAB        -1.1136733 0.01540251 -1.14386220
## 2010_BUDGET_04_Arthur_Morgan_SF       -0.1219344 0.02846313 -0.17772215
## 2010_BUDGET_05_Brian_Cowen_FF          1.7724237 0.02364103  1.72608732
## 2010_BUDGET_06_Enda_Kenny_FG          -0.7145776 0.02650246 -0.76652247
## 2010_BUDGET_07_Kieran_ODonnell_FG     -0.4844814 0.04171467 -0.56624211
## 2010_BUDGET_08_Eamon_Gilmore_LAB      -0.5616730 0.02967340 -0.61983285
## 2010_BUDGET_09_Michael_Higgins_LAB    -0.9703095 0.03850528 -1.04577988
## 2010_BUDGET_10_Ruairi_Quinn_LAB       -0.9589219 0.03892359 -1.03521215
## 2010_BUDGET_11_John_Gormley_Green      1.1807220 0.07221476  1.03918103
## 2010_BUDGET_12_Eamon_Ryan_Green        0.1866448 0.06294115  0.06328019
## 2010_BUDGET_13_Ciaran_Cuffe_Green      0.7421876 0.07245443  0.60017693
## 2010_BUDGET_14_Caoimhghin_OCaolain_SF -0.1840861 0.03666245 -0.25594450
##                                             upper
## 2010_BUDGET_01_Brian_Lenihan_FF        1.86078947
## 2010_BUDGET_02_Richard_Bruton_FG      -0.53802726
## 2010_BUDGET_03_Joan_Burton_LAB        -1.08348435
## 2010_BUDGET_04_Arthur_Morgan_SF       -0.06614667
## 2010_BUDGET_05_Brian_Cowen_FF          1.81876014
## 2010_BUDGET_06_Enda_Kenny_FG          -0.66263281
## 2010_BUDGET_07_Kieran_ODonnell_FG     -0.40272060
## 2010_BUDGET_08_Eamon_Gilmore_LAB      -0.50351312
## 2010_BUDGET_09_Michael_Higgins_LAB    -0.89483920
## 2010_BUDGET_10_Ruairi_Quinn_LAB       -0.88263168
## 2010_BUDGET_11_John_Gormley_Green      1.32226288
## 2010_BUDGET_12_Eamon_Ryan_Green        0.31000950
## 2010_BUDGET_13_Ciaran_Cuffe_Green      0.88419830
## 2010_BUDGET_14_Caoimhghin_OCaolain_SF -0.11222769
```


```r
knitr::kable(summary(ieWF))
```

Call:
	textmodel_wordfish.dfm(x = ., dir = c(6, 5))

Estimated document positions:
                                           theta         SE       lower
2010_BUDGET_01_Brian_Lenihan_FF        1.8209554 0.02032350  1.78112134
2010_BUDGET_02_Richard_Bruton_FG      -0.5932763 0.02818830 -0.64852541
2010_BUDGET_03_Joan_Burton_LAB        -1.1136733 0.01540251 -1.14386220
2010_BUDGET_04_Arthur_Morgan_SF       -0.1219344 0.02846313 -0.17772215
2010_BUDGET_05_Brian_Cowen_FF          1.7724237 0.02364103  1.72608732
2010_BUDGET_06_Enda_Kenny_FG          -0.7145776 0.02650246 -0.76652247
2010_BUDGET_07_Kieran_ODonnell_FG     -0.4844814 0.04171467 -0.56624211
2010_BUDGET_08_Eamon_Gilmore_LAB      -0.5616730 0.02967340 -0.61983285
2010_BUDGET_09_Michael_Higgins_LAB    -0.9703095 0.03850528 -1.04577988
2010_BUDGET_10_Ruairi_Quinn_LAB       -0.9589219 0.03892359 -1.03521215
2010_BUDGET_11_John_Gormley_Green      1.1807220 0.07221476  1.03918103
2010_BUDGET_12_Eamon_Ryan_Green        0.1866448 0.06294115  0.06328019
2010_BUDGET_13_Ciaran_Cuffe_Green      0.7421876 0.07245443  0.60017693
2010_BUDGET_14_Caoimhghin_OCaolain_SF -0.1840861 0.03666245 -0.25594450
                                            upper
2010_BUDGET_01_Brian_Lenihan_FF        1.86078947
2010_BUDGET_02_Richard_Bruton_FG      -0.53802726
2010_BUDGET_03_Joan_Burton_LAB        -1.08348435
2010_BUDGET_04_Arthur_Morgan_SF       -0.06614667
2010_BUDGET_05_Brian_Cowen_FF          1.81876014
2010_BUDGET_06_Enda_Kenny_FG          -0.66263281
2010_BUDGET_07_Kieran_ODonnell_FG     -0.40272060
2010_BUDGET_08_Eamon_Gilmore_LAB      -0.50351312
2010_BUDGET_09_Michael_Higgins_LAB    -0.89483920
2010_BUDGET_10_Ruairi_Quinn_LAB       -0.88263168
2010_BUDGET_11_John_Gormley_Green      1.32226288
2010_BUDGET_12_Eamon_Ryan_Green        0.31000950
2010_BUDGET_13_Ciaran_Cuffe_Green      0.88419830
2010_BUDGET_14_Caoimhghin_OCaolain_SF -0.11222769


|                                      |      theta|        SE|      lower|      upper|
|:-------------------------------------|----------:|---------:|----------:|----------:|
|2010_BUDGET_01_Brian_Lenihan_FF       |  1.8209554| 0.0203235|  1.7811213|  1.8607895|
|2010_BUDGET_02_Richard_Bruton_FG      | -0.5932763| 0.0281883| -0.6485254| -0.5380273|
|2010_BUDGET_03_Joan_Burton_LAB        | -1.1136733| 0.0154025| -1.1438622| -1.0834844|
|2010_BUDGET_04_Arthur_Morgan_SF       | -0.1219344| 0.0284631| -0.1777222| -0.0661467|
|2010_BUDGET_05_Brian_Cowen_FF         |  1.7724237| 0.0236410|  1.7260873|  1.8187601|
|2010_BUDGET_06_Enda_Kenny_FG          | -0.7145776| 0.0265025| -0.7665225| -0.6626328|
|2010_BUDGET_07_Kieran_ODonnell_FG     | -0.4844814| 0.0417147| -0.5662421| -0.4027206|
|2010_BUDGET_08_Eamon_Gilmore_LAB      | -0.5616730| 0.0296734| -0.6198328| -0.5035131|
|2010_BUDGET_09_Michael_Higgins_LAB    | -0.9703095| 0.0385053| -1.0457799| -0.8948392|
|2010_BUDGET_10_Ruairi_Quinn_LAB       | -0.9589219| 0.0389236| -1.0352122| -0.8826317|
|2010_BUDGET_11_John_Gormley_Green     |  1.1807220| 0.0722148|  1.0391810|  1.3222629|
|2010_BUDGET_12_Eamon_Ryan_Green       |  0.1866448| 0.0629411|  0.0632802|  0.3100095|
|2010_BUDGET_13_Ciaran_Cuffe_Green     |  0.7421876| 0.0724544|  0.6001769|  0.8841983|
|2010_BUDGET_14_Caoimhghin_OCaolain_SF | -0.1840861| 0.0366625| -0.2559445| -0.1122277|
