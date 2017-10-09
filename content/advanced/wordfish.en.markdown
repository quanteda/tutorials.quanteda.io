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
## 2010_BUDGET_01_Brian_Lenihan_FF        1.8209539 0.02032346  1.78111990
## 2010_BUDGET_02_Richard_Bruton_FG      -0.5932777 0.02818835 -0.64852686
## 2010_BUDGET_03_Joan_Burton_LAB        -1.1136749 0.01540255 -1.14386390
## 2010_BUDGET_04_Arthur_Morgan_SF       -0.1219328 0.02846318 -0.17772066
## 2010_BUDGET_05_Brian_Cowen_FF          1.7724227 0.02364098  1.72608638
## 2010_BUDGET_06_Enda_Kenny_FG          -0.7145783 0.02650252 -0.76652321
## 2010_BUDGET_07_Kieran_ODonnell_FG     -0.4844819 0.04171474 -0.56624283
## 2010_BUDGET_08_Eamon_Gilmore_LAB      -0.5616716 0.02967349 -0.61983165
## 2010_BUDGET_09_Michael_Higgins_LAB    -0.9703104 0.03850539 -1.04578092
## 2010_BUDGET_10_Ruairi_Quinn_LAB       -0.9589227 0.03892370 -1.03521315
## 2010_BUDGET_11_John_Gormley_Green      1.1807220 0.07221466  1.03918129
## 2010_BUDGET_12_Eamon_Ryan_Green        0.1866455 0.06294118  0.06328074
## 2010_BUDGET_13_Ciaran_Cuffe_Green      0.7421891 0.07245438  0.60017849
## 2010_BUDGET_14_Caoimhghin_OCaolain_SF -0.1840828 0.03666254 -0.25594142
##                                             upper
## 2010_BUDGET_01_Brian_Lenihan_FF        1.86078788
## 2010_BUDGET_02_Richard_Bruton_FG      -0.53802855
## 2010_BUDGET_03_Joan_Burton_LAB        -1.08348590
## 2010_BUDGET_04_Arthur_Morgan_SF       -0.06614501
## 2010_BUDGET_05_Brian_Cowen_FF          1.81875902
## 2010_BUDGET_06_Enda_Kenny_FG          -0.66263331
## 2010_BUDGET_07_Kieran_ODonnell_FG     -0.40272105
## 2010_BUDGET_08_Eamon_Gilmore_LAB      -0.50351157
## 2010_BUDGET_09_Michael_Higgins_LAB    -0.89483980
## 2010_BUDGET_10_Ruairi_Quinn_LAB       -0.88263224
## 2010_BUDGET_11_John_Gormley_Green      1.32226274
## 2010_BUDGET_12_Eamon_Ryan_Green        0.31001017
## 2010_BUDGET_13_Ciaran_Cuffe_Green      0.88419965
## 2010_BUDGET_14_Caoimhghin_OCaolain_SF -0.11222425
```


```r
knitr::kable(summary(ieWF))
```

Call:
	textmodel_wordfish.dfm(x = ., dir = c(6, 5))

Estimated document positions:
                                           theta         SE       lower
2010_BUDGET_01_Brian_Lenihan_FF        1.8209539 0.02032346  1.78111990
2010_BUDGET_02_Richard_Bruton_FG      -0.5932777 0.02818835 -0.64852686
2010_BUDGET_03_Joan_Burton_LAB        -1.1136749 0.01540255 -1.14386390
2010_BUDGET_04_Arthur_Morgan_SF       -0.1219328 0.02846318 -0.17772066
2010_BUDGET_05_Brian_Cowen_FF          1.7724227 0.02364098  1.72608638
2010_BUDGET_06_Enda_Kenny_FG          -0.7145783 0.02650252 -0.76652321
2010_BUDGET_07_Kieran_ODonnell_FG     -0.4844819 0.04171474 -0.56624283
2010_BUDGET_08_Eamon_Gilmore_LAB      -0.5616716 0.02967349 -0.61983165
2010_BUDGET_09_Michael_Higgins_LAB    -0.9703104 0.03850539 -1.04578092
2010_BUDGET_10_Ruairi_Quinn_LAB       -0.9589227 0.03892370 -1.03521315
2010_BUDGET_11_John_Gormley_Green      1.1807220 0.07221466  1.03918129
2010_BUDGET_12_Eamon_Ryan_Green        0.1866455 0.06294118  0.06328074
2010_BUDGET_13_Ciaran_Cuffe_Green      0.7421891 0.07245438  0.60017849
2010_BUDGET_14_Caoimhghin_OCaolain_SF -0.1840828 0.03666254 -0.25594142
                                            upper
2010_BUDGET_01_Brian_Lenihan_FF        1.86078788
2010_BUDGET_02_Richard_Bruton_FG      -0.53802855
2010_BUDGET_03_Joan_Burton_LAB        -1.08348590
2010_BUDGET_04_Arthur_Morgan_SF       -0.06614501
2010_BUDGET_05_Brian_Cowen_FF          1.81875902
2010_BUDGET_06_Enda_Kenny_FG          -0.66263331
2010_BUDGET_07_Kieran_ODonnell_FG     -0.40272105
2010_BUDGET_08_Eamon_Gilmore_LAB      -0.50351157
2010_BUDGET_09_Michael_Higgins_LAB    -0.89483980
2010_BUDGET_10_Ruairi_Quinn_LAB       -0.88263224
2010_BUDGET_11_John_Gormley_Green      1.32226274
2010_BUDGET_12_Eamon_Ryan_Green        0.31001017
2010_BUDGET_13_Ciaran_Cuffe_Green      0.88419965
2010_BUDGET_14_Caoimhghin_OCaolain_SF -0.11222425


|                                      |      theta|        SE|      lower|      upper|
|:-------------------------------------|----------:|---------:|----------:|----------:|
|2010_BUDGET_01_Brian_Lenihan_FF       |  1.8209539| 0.0203235|  1.7811199|  1.8607879|
|2010_BUDGET_02_Richard_Bruton_FG      | -0.5932777| 0.0281883| -0.6485269| -0.5380285|
|2010_BUDGET_03_Joan_Burton_LAB        | -1.1136749| 0.0154026| -1.1438639| -1.0834859|
|2010_BUDGET_04_Arthur_Morgan_SF       | -0.1219328| 0.0284632| -0.1777207| -0.0661450|
|2010_BUDGET_05_Brian_Cowen_FF         |  1.7724227| 0.0236410|  1.7260864|  1.8187590|
|2010_BUDGET_06_Enda_Kenny_FG          | -0.7145783| 0.0265025| -0.7665232| -0.6626333|
|2010_BUDGET_07_Kieran_ODonnell_FG     | -0.4844819| 0.0417147| -0.5662428| -0.4027210|
|2010_BUDGET_08_Eamon_Gilmore_LAB      | -0.5616716| 0.0296735| -0.6198317| -0.5035116|
|2010_BUDGET_09_Michael_Higgins_LAB    | -0.9703104| 0.0385054| -1.0457809| -0.8948398|
|2010_BUDGET_10_Ruairi_Quinn_LAB       | -0.9589227| 0.0389237| -1.0352132| -0.8826322|
|2010_BUDGET_11_John_Gormley_Green     |  1.1807220| 0.0722147|  1.0391813|  1.3222627|
|2010_BUDGET_12_Eamon_Ryan_Green       |  0.1866455| 0.0629412|  0.0632807|  0.3100102|
|2010_BUDGET_13_Ciaran_Cuffe_Green     |  0.7421891| 0.0724544|  0.6001785|  0.8841997|
|2010_BUDGET_14_Caoimhghin_OCaolain_SF | -0.1840828| 0.0366625| -0.2559414| -0.1122243|
