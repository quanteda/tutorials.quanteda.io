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
## 2010_BUDGET_01_Brian_Lenihan_FF        1.8209533 0.02032345  1.78111930
## 2010_BUDGET_02_Richard_Bruton_FG      -0.5932779 0.02818837 -0.64852713
## 2010_BUDGET_03_Joan_Burton_LAB        -1.1136755 0.01540257 -1.14386454
## 2010_BUDGET_04_Arthur_Morgan_SF       -0.1219323 0.02846320 -0.17772014
## 2010_BUDGET_05_Brian_Cowen_FF          1.7724223 0.02364096  1.72608601
## 2010_BUDGET_06_Enda_Kenny_FG          -0.7145785 0.02650255 -0.76652348
## 2010_BUDGET_07_Kieran_ODonnell_FG     -0.4844821 0.04171477 -0.56624309
## 2010_BUDGET_08_Eamon_Gilmore_LAB      -0.5616712 0.02967352 -0.61983132
## 2010_BUDGET_09_Michael_Higgins_LAB    -0.9703107 0.03850543 -1.04578136
## 2010_BUDGET_10_Ruairi_Quinn_LAB       -0.9589231 0.03892374 -1.03521360
## 2010_BUDGET_11_John_Gormley_Green      1.1807220 0.07221462  1.03918136
## 2010_BUDGET_12_Eamon_Ryan_Green        0.1866459 0.06294119  0.06328111
## 2010_BUDGET_13_Ciaran_Cuffe_Green      0.7421897 0.07245435  0.60017920
## 2010_BUDGET_14_Caoimhghin_OCaolain_SF -0.1840818 0.03666257 -0.25594045
##                                             upper
## 2010_BUDGET_01_Brian_Lenihan_FF        1.86078722
## 2010_BUDGET_02_Richard_Bruton_FG      -0.53802874
## 2010_BUDGET_03_Joan_Burton_LAB        -1.08348648
## 2010_BUDGET_04_Arthur_Morgan_SF       -0.06614441
## 2010_BUDGET_05_Brian_Cowen_FF          1.81875857
## 2010_BUDGET_06_Enda_Kenny_FG          -0.66263350
## 2010_BUDGET_07_Kieran_ODonnell_FG     -0.40272120
## 2010_BUDGET_08_Eamon_Gilmore_LAB      -0.50351110
## 2010_BUDGET_09_Michael_Higgins_LAB    -0.89484008
## 2010_BUDGET_10_Ruairi_Quinn_LAB       -0.88263253
## 2010_BUDGET_11_John_Gormley_Green      1.32226266
## 2010_BUDGET_12_Eamon_Ryan_Green        0.31001059
## 2010_BUDGET_13_Ciaran_Cuffe_Green      0.88420027
## 2010_BUDGET_14_Caoimhghin_OCaolain_SF -0.11222316
```


```r
knitr::kable(summary(ieWF))
```

Call:
	textmodel_wordfish.dfm(x = ., dir = c(6, 5))

Estimated document positions:
                                           theta         SE       lower
2010_BUDGET_01_Brian_Lenihan_FF        1.8209533 0.02032345  1.78111930
2010_BUDGET_02_Richard_Bruton_FG      -0.5932779 0.02818837 -0.64852713
2010_BUDGET_03_Joan_Burton_LAB        -1.1136755 0.01540257 -1.14386454
2010_BUDGET_04_Arthur_Morgan_SF       -0.1219323 0.02846320 -0.17772014
2010_BUDGET_05_Brian_Cowen_FF          1.7724223 0.02364096  1.72608601
2010_BUDGET_06_Enda_Kenny_FG          -0.7145785 0.02650255 -0.76652348
2010_BUDGET_07_Kieran_ODonnell_FG     -0.4844821 0.04171477 -0.56624309
2010_BUDGET_08_Eamon_Gilmore_LAB      -0.5616712 0.02967352 -0.61983132
2010_BUDGET_09_Michael_Higgins_LAB    -0.9703107 0.03850543 -1.04578136
2010_BUDGET_10_Ruairi_Quinn_LAB       -0.9589231 0.03892374 -1.03521360
2010_BUDGET_11_John_Gormley_Green      1.1807220 0.07221462  1.03918136
2010_BUDGET_12_Eamon_Ryan_Green        0.1866459 0.06294119  0.06328111
2010_BUDGET_13_Ciaran_Cuffe_Green      0.7421897 0.07245435  0.60017920
2010_BUDGET_14_Caoimhghin_OCaolain_SF -0.1840818 0.03666257 -0.25594045
                                            upper
2010_BUDGET_01_Brian_Lenihan_FF        1.86078722
2010_BUDGET_02_Richard_Bruton_FG      -0.53802874
2010_BUDGET_03_Joan_Burton_LAB        -1.08348648
2010_BUDGET_04_Arthur_Morgan_SF       -0.06614441
2010_BUDGET_05_Brian_Cowen_FF          1.81875857
2010_BUDGET_06_Enda_Kenny_FG          -0.66263350
2010_BUDGET_07_Kieran_ODonnell_FG     -0.40272120
2010_BUDGET_08_Eamon_Gilmore_LAB      -0.50351110
2010_BUDGET_09_Michael_Higgins_LAB    -0.89484008
2010_BUDGET_10_Ruairi_Quinn_LAB       -0.88263253
2010_BUDGET_11_John_Gormley_Green      1.32226266
2010_BUDGET_12_Eamon_Ryan_Green        0.31001059
2010_BUDGET_13_Ciaran_Cuffe_Green      0.88420027
2010_BUDGET_14_Caoimhghin_OCaolain_SF -0.11222316


|                                      |      theta|        SE|      lower|      upper|
|:-------------------------------------|----------:|---------:|----------:|----------:|
|2010_BUDGET_01_Brian_Lenihan_FF       |  1.8209533| 0.0203234|  1.7811193|  1.8607872|
|2010_BUDGET_02_Richard_Bruton_FG      | -0.5932779| 0.0281884| -0.6485271| -0.5380287|
|2010_BUDGET_03_Joan_Burton_LAB        | -1.1136755| 0.0154026| -1.1438645| -1.0834865|
|2010_BUDGET_04_Arthur_Morgan_SF       | -0.1219323| 0.0284632| -0.1777201| -0.0661444|
|2010_BUDGET_05_Brian_Cowen_FF         |  1.7724223| 0.0236410|  1.7260860|  1.8187586|
|2010_BUDGET_06_Enda_Kenny_FG          | -0.7145785| 0.0265025| -0.7665235| -0.6626335|
|2010_BUDGET_07_Kieran_ODonnell_FG     | -0.4844821| 0.0417148| -0.5662431| -0.4027212|
|2010_BUDGET_08_Eamon_Gilmore_LAB      | -0.5616712| 0.0296735| -0.6198313| -0.5035111|
|2010_BUDGET_09_Michael_Higgins_LAB    | -0.9703107| 0.0385054| -1.0457814| -0.8948401|
|2010_BUDGET_10_Ruairi_Quinn_LAB       | -0.9589231| 0.0389237| -1.0352136| -0.8826325|
|2010_BUDGET_11_John_Gormley_Green     |  1.1807220| 0.0722146|  1.0391814|  1.3222627|
|2010_BUDGET_12_Eamon_Ryan_Green       |  0.1866459| 0.0629412|  0.0632811|  0.3100106|
|2010_BUDGET_13_Ciaran_Cuffe_Green     |  0.7421897| 0.0724544|  0.6001792|  0.8842003|
|2010_BUDGET_14_Caoimhghin_OCaolain_SF | -0.1840818| 0.0366626| -0.2559404| -0.1122232|
