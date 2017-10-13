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
## 2010_BUDGET_01_Brian_Lenihan_FF        1.8209708 0.02032390  1.78113592
## 2010_BUDGET_02_Richard_Bruton_FG      -0.5932653 0.02818786 -0.64851349
## 2010_BUDGET_03_Joan_Burton_LAB        -1.1136575 0.01540213 -1.14384566
## 2010_BUDGET_04_Arthur_Morgan_SF       -0.1219497 0.02846268 -0.17773657
## 2010_BUDGET_05_Brian_Cowen_FF          1.7724341 0.02364151  1.72609669
## 2010_BUDGET_06_Enda_Kenny_FG          -0.7145718 0.02650186 -0.76651548
## 2010_BUDGET_07_Kieran_ODonnell_FG     -0.4844758 0.04171398 -0.56623522
## 2010_BUDGET_08_Eamon_Gilmore_LAB      -0.5616856 0.02967250 -0.61984368
## 2010_BUDGET_09_Michael_Higgins_LAB    -0.9703007 0.03850422 -1.04576894
## 2010_BUDGET_10_Ruairi_Quinn_LAB       -0.9589134 0.03892252 -1.03520156
## 2010_BUDGET_11_John_Gormley_Green      1.1807216 0.07221574  1.03917876
## 2010_BUDGET_12_Eamon_Ryan_Green        0.1866376 0.06294082  0.06327359
## 2010_BUDGET_13_Ciaran_Cuffe_Green      0.7421724 0.07245498  0.60016065
## 2010_BUDGET_14_Caoimhghin_OCaolain_SF -0.1841166 0.03666158 -0.25597331
##                                             upper
## 2010_BUDGET_01_Brian_Lenihan_FF        1.86080559
## 2010_BUDGET_02_Richard_Bruton_FG      -0.53801707
## 2010_BUDGET_03_Joan_Burton_LAB        -1.08346932
## 2010_BUDGET_04_Arthur_Morgan_SF       -0.06616285
## 2010_BUDGET_05_Brian_Cowen_FF          1.81877141
## 2010_BUDGET_06_Enda_Kenny_FG          -0.66262817
## 2010_BUDGET_07_Kieran_ODonnell_FG     -0.40271640
## 2010_BUDGET_08_Eamon_Gilmore_LAB      -0.50352748
## 2010_BUDGET_09_Michael_Higgins_LAB    -0.89483241
## 2010_BUDGET_10_Ruairi_Quinn_LAB       -0.88262528
## 2010_BUDGET_11_John_Gormley_Green      1.32226444
## 2010_BUDGET_12_Eamon_Ryan_Green        0.31000159
## 2010_BUDGET_13_Ciaran_Cuffe_Green      0.88418415
## 2010_BUDGET_14_Caoimhghin_OCaolain_SF -0.11225991
```


```r
knitr::kable(summary(ieWF))
```

Call:
	textmodel_wordfish.dfm(x = ., dir = c(6, 5))

Estimated document positions:
                                           theta         SE       lower
2010_BUDGET_01_Brian_Lenihan_FF        1.8209708 0.02032390  1.78113592
2010_BUDGET_02_Richard_Bruton_FG      -0.5932653 0.02818786 -0.64851349
2010_BUDGET_03_Joan_Burton_LAB        -1.1136575 0.01540213 -1.14384566
2010_BUDGET_04_Arthur_Morgan_SF       -0.1219497 0.02846268 -0.17773657
2010_BUDGET_05_Brian_Cowen_FF          1.7724341 0.02364151  1.72609669
2010_BUDGET_06_Enda_Kenny_FG          -0.7145718 0.02650186 -0.76651548
2010_BUDGET_07_Kieran_ODonnell_FG     -0.4844758 0.04171398 -0.56623522
2010_BUDGET_08_Eamon_Gilmore_LAB      -0.5616856 0.02967250 -0.61984368
2010_BUDGET_09_Michael_Higgins_LAB    -0.9703007 0.03850422 -1.04576894
2010_BUDGET_10_Ruairi_Quinn_LAB       -0.9589134 0.03892252 -1.03520156
2010_BUDGET_11_John_Gormley_Green      1.1807216 0.07221574  1.03917876
2010_BUDGET_12_Eamon_Ryan_Green        0.1866376 0.06294082  0.06327359
2010_BUDGET_13_Ciaran_Cuffe_Green      0.7421724 0.07245498  0.60016065
2010_BUDGET_14_Caoimhghin_OCaolain_SF -0.1841166 0.03666158 -0.25597331
                                            upper
2010_BUDGET_01_Brian_Lenihan_FF        1.86080559
2010_BUDGET_02_Richard_Bruton_FG      -0.53801707
2010_BUDGET_03_Joan_Burton_LAB        -1.08346932
2010_BUDGET_04_Arthur_Morgan_SF       -0.06616285
2010_BUDGET_05_Brian_Cowen_FF          1.81877141
2010_BUDGET_06_Enda_Kenny_FG          -0.66262817
2010_BUDGET_07_Kieran_ODonnell_FG     -0.40271640
2010_BUDGET_08_Eamon_Gilmore_LAB      -0.50352748
2010_BUDGET_09_Michael_Higgins_LAB    -0.89483241
2010_BUDGET_10_Ruairi_Quinn_LAB       -0.88262528
2010_BUDGET_11_John_Gormley_Green      1.32226444
2010_BUDGET_12_Eamon_Ryan_Green        0.31000159
2010_BUDGET_13_Ciaran_Cuffe_Green      0.88418415
2010_BUDGET_14_Caoimhghin_OCaolain_SF -0.11225991


|                                      |      theta|        SE|      lower|      upper|
|:-------------------------------------|----------:|---------:|----------:|----------:|
|2010_BUDGET_01_Brian_Lenihan_FF       |  1.8209708| 0.0203239|  1.7811359|  1.8608056|
|2010_BUDGET_02_Richard_Bruton_FG      | -0.5932653| 0.0281879| -0.6485135| -0.5380171|
|2010_BUDGET_03_Joan_Burton_LAB        | -1.1136575| 0.0154021| -1.1438457| -1.0834693|
|2010_BUDGET_04_Arthur_Morgan_SF       | -0.1219497| 0.0284627| -0.1777366| -0.0661628|
|2010_BUDGET_05_Brian_Cowen_FF         |  1.7724341| 0.0236415|  1.7260967|  1.8187714|
|2010_BUDGET_06_Enda_Kenny_FG          | -0.7145718| 0.0265019| -0.7665155| -0.6626282|
|2010_BUDGET_07_Kieran_ODonnell_FG     | -0.4844758| 0.0417140| -0.5662352| -0.4027164|
|2010_BUDGET_08_Eamon_Gilmore_LAB      | -0.5616856| 0.0296725| -0.6198437| -0.5035275|
|2010_BUDGET_09_Michael_Higgins_LAB    | -0.9703007| 0.0385042| -1.0457689| -0.8948324|
|2010_BUDGET_10_Ruairi_Quinn_LAB       | -0.9589134| 0.0389225| -1.0352016| -0.8826253|
|2010_BUDGET_11_John_Gormley_Green     |  1.1807216| 0.0722157|  1.0391788|  1.3222644|
|2010_BUDGET_12_Eamon_Ryan_Green       |  0.1866376| 0.0629408|  0.0632736|  0.3100016|
|2010_BUDGET_13_Ciaran_Cuffe_Green     |  0.7421724| 0.0724550|  0.6001607|  0.8841842|
|2010_BUDGET_14_Caoimhghin_OCaolain_SF | -0.1841166| 0.0366616| -0.2559733| -0.1122599|
