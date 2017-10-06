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

<img src="/_advanced/wordfish.en_files/figure-html/unnamed-chunk-2-1.svg" width="768" />

```r
summary(ieWF)
## Call:
## 	textmodel_wordfish.dfm(x = ., dir = c(6, 5))
## 
## Estimated document positions:
##                                            theta         SE       lower
## 2010_BUDGET_01_Brian_Lenihan_FF        1.8209522 0.02032342  1.78111829
## 2010_BUDGET_02_Richard_Bruton_FG      -0.5932789 0.02818839 -0.64852817
## 2010_BUDGET_03_Joan_Burton_LAB        -1.1136766 0.01540259 -1.14386571
## 2010_BUDGET_04_Arthur_Morgan_SF       -0.1219311 0.02846323 -0.17771906
## 2010_BUDGET_05_Brian_Cowen_FF          1.7724216 0.02364093  1.72608535
## 2010_BUDGET_06_Enda_Kenny_FG          -0.7145789 0.02650259 -0.76652400
## 2010_BUDGET_07_Kieran_ODonnell_FG     -0.4844826 0.04171482 -0.56624360
## 2010_BUDGET_08_Eamon_Gilmore_LAB      -0.5616702 0.02967359 -0.61983042
## 2010_BUDGET_09_Michael_Higgins_LAB    -0.9703114 0.03850550 -1.04578214
## 2010_BUDGET_10_Ruairi_Quinn_LAB       -0.9589236 0.03892382 -1.03521433
## 2010_BUDGET_11_John_Gormley_Green      1.1807221 0.07221455  1.03918154
## 2010_BUDGET_12_Eamon_Ryan_Green        0.1866462 0.06294122  0.06328145
## 2010_BUDGET_13_Ciaran_Cuffe_Green      0.7421907 0.07245432  0.60018028
## 2010_BUDGET_14_Caoimhghin_OCaolain_SF -0.1840794 0.03666264 -0.25593821
##                                             upper
## 2010_BUDGET_01_Brian_Lenihan_FF        1.86078610
## 2010_BUDGET_02_Richard_Bruton_FG      -0.53802967
## 2010_BUDGET_03_Joan_Burton_LAB        -1.08348755
## 2010_BUDGET_04_Arthur_Morgan_SF       -0.06614321
## 2010_BUDGET_05_Brian_Cowen_FF          1.81875778
## 2010_BUDGET_06_Enda_Kenny_FG          -0.66263385
## 2010_BUDGET_07_Kieran_ODonnell_FG     -0.40272152
## 2010_BUDGET_08_Eamon_Gilmore_LAB      -0.50350994
## 2010_BUDGET_09_Michael_Higgins_LAB    -0.89484057
## 2010_BUDGET_10_Ruairi_Quinn_LAB       -0.88263296
## 2010_BUDGET_11_John_Gormley_Green      1.32226257
## 2010_BUDGET_12_Eamon_Ryan_Green        0.31001102
## 2010_BUDGET_13_Ciaran_Cuffe_Green      0.88420121
## 2010_BUDGET_14_Caoimhghin_OCaolain_SF -0.11222067
```


```r
knitr::kable(summary(ieWF))
```

Call:
	textmodel_wordfish.dfm(x = ., dir = c(6, 5))

Estimated document positions:
                                           theta         SE       lower
2010_BUDGET_01_Brian_Lenihan_FF        1.8209522 0.02032342  1.78111829
2010_BUDGET_02_Richard_Bruton_FG      -0.5932789 0.02818839 -0.64852817
2010_BUDGET_03_Joan_Burton_LAB        -1.1136766 0.01540259 -1.14386571
2010_BUDGET_04_Arthur_Morgan_SF       -0.1219311 0.02846323 -0.17771906
2010_BUDGET_05_Brian_Cowen_FF          1.7724216 0.02364093  1.72608535
2010_BUDGET_06_Enda_Kenny_FG          -0.7145789 0.02650259 -0.76652400
2010_BUDGET_07_Kieran_ODonnell_FG     -0.4844826 0.04171482 -0.56624360
2010_BUDGET_08_Eamon_Gilmore_LAB      -0.5616702 0.02967359 -0.61983042
2010_BUDGET_09_Michael_Higgins_LAB    -0.9703114 0.03850550 -1.04578214
2010_BUDGET_10_Ruairi_Quinn_LAB       -0.9589236 0.03892382 -1.03521433
2010_BUDGET_11_John_Gormley_Green      1.1807221 0.07221455  1.03918154
2010_BUDGET_12_Eamon_Ryan_Green        0.1866462 0.06294122  0.06328145
2010_BUDGET_13_Ciaran_Cuffe_Green      0.7421907 0.07245432  0.60018028
2010_BUDGET_14_Caoimhghin_OCaolain_SF -0.1840794 0.03666264 -0.25593821
                                            upper
2010_BUDGET_01_Brian_Lenihan_FF        1.86078610
2010_BUDGET_02_Richard_Bruton_FG      -0.53802967
2010_BUDGET_03_Joan_Burton_LAB        -1.08348755
2010_BUDGET_04_Arthur_Morgan_SF       -0.06614321
2010_BUDGET_05_Brian_Cowen_FF          1.81875778
2010_BUDGET_06_Enda_Kenny_FG          -0.66263385
2010_BUDGET_07_Kieran_ODonnell_FG     -0.40272152
2010_BUDGET_08_Eamon_Gilmore_LAB      -0.50350994
2010_BUDGET_09_Michael_Higgins_LAB    -0.89484057
2010_BUDGET_10_Ruairi_Quinn_LAB       -0.88263296
2010_BUDGET_11_John_Gormley_Green      1.32226257
2010_BUDGET_12_Eamon_Ryan_Green        0.31001102
2010_BUDGET_13_Ciaran_Cuffe_Green      0.88420121
2010_BUDGET_14_Caoimhghin_OCaolain_SF -0.11222067


|                                      |      theta|        SE|      lower|      upper|
|:-------------------------------------|----------:|---------:|----------:|----------:|
|2010_BUDGET_01_Brian_Lenihan_FF       |  1.8209522| 0.0203234|  1.7811183|  1.8607861|
|2010_BUDGET_02_Richard_Bruton_FG      | -0.5932789| 0.0281884| -0.6485282| -0.5380297|
|2010_BUDGET_03_Joan_Burton_LAB        | -1.1136766| 0.0154026| -1.1438657| -1.0834875|
|2010_BUDGET_04_Arthur_Morgan_SF       | -0.1219311| 0.0284632| -0.1777191| -0.0661432|
|2010_BUDGET_05_Brian_Cowen_FF         |  1.7724216| 0.0236409|  1.7260853|  1.8187578|
|2010_BUDGET_06_Enda_Kenny_FG          | -0.7145789| 0.0265026| -0.7665240| -0.6626338|
|2010_BUDGET_07_Kieran_ODonnell_FG     | -0.4844826| 0.0417148| -0.5662436| -0.4027215|
|2010_BUDGET_08_Eamon_Gilmore_LAB      | -0.5616702| 0.0296736| -0.6198304| -0.5035099|
|2010_BUDGET_09_Michael_Higgins_LAB    | -0.9703114| 0.0385055| -1.0457821| -0.8948406|
|2010_BUDGET_10_Ruairi_Quinn_LAB       | -0.9589236| 0.0389238| -1.0352143| -0.8826330|
|2010_BUDGET_11_John_Gormley_Green     |  1.1807221| 0.0722145|  1.0391815|  1.3222626|
|2010_BUDGET_12_Eamon_Ryan_Green       |  0.1866462| 0.0629412|  0.0632815|  0.3100110|
|2010_BUDGET_13_Ciaran_Cuffe_Green     |  0.7421907| 0.0724543|  0.6001803|  0.8842012|
|2010_BUDGET_14_Caoimhghin_OCaolain_SF | -0.1840794| 0.0366626| -0.2559382| -0.1122207|
