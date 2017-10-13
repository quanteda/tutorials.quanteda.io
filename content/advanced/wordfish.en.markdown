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
## 2010_BUDGET_01_Brian_Lenihan_FF        1.8209392 0.02032309  1.78110595
## 2010_BUDGET_02_Richard_Bruton_FG      -0.5932887 0.02818876 -0.64853867
## 2010_BUDGET_03_Joan_Burton_LAB        -1.1136900 0.01540292 -1.14387977
## 2010_BUDGET_04_Arthur_Morgan_SF       -0.1219181 0.02846361 -0.17770674
## 2010_BUDGET_05_Brian_Cowen_FF          1.7724128 0.02364052  1.72607740
## 2010_BUDGET_06_Enda_Kenny_FG          -0.7145839 0.02650310 -0.76652995
## 2010_BUDGET_07_Kieran_ODonnell_FG     -0.4844873 0.04171540 -0.56624947
## 2010_BUDGET_08_Eamon_Gilmore_LAB      -0.5616593 0.02967436 -0.61982104
## 2010_BUDGET_09_Michael_Higgins_LAB    -0.9703189 0.03850640 -1.04579142
## 2010_BUDGET_10_Ruairi_Quinn_LAB       -0.9589308 0.03892473 -1.03522326
## 2010_BUDGET_11_John_Gormley_Green      1.1807224 0.07221372  1.03918351
## 2010_BUDGET_12_Eamon_Ryan_Green        0.1866522 0.06294150  0.06328684
## 2010_BUDGET_13_Ciaran_Cuffe_Green      0.7422035 0.07245386  0.60019397
## 2010_BUDGET_14_Caoimhghin_OCaolain_SF -0.1840532 0.03666338 -0.25591343
##                                             upper
## 2010_BUDGET_01_Brian_Lenihan_FF        1.86077246
## 2010_BUDGET_02_Richard_Bruton_FG      -0.53803872
## 2010_BUDGET_03_Joan_Burton_LAB        -1.08350032
## 2010_BUDGET_04_Arthur_Morgan_SF       -0.06612939
## 2010_BUDGET_05_Brian_Cowen_FF          1.81874822
## 2010_BUDGET_06_Enda_Kenny_FG          -0.66263780
## 2010_BUDGET_07_Kieran_ODonnell_FG     -0.40272510
## 2010_BUDGET_08_Eamon_Gilmore_LAB      -0.50349756
## 2010_BUDGET_09_Michael_Higgins_LAB    -0.89484631
## 2010_BUDGET_10_Ruairi_Quinn_LAB       -0.88263831
## 2010_BUDGET_11_John_Gormley_Green      1.32226128
## 2010_BUDGET_12_Eamon_Ryan_Green        0.31001751
## 2010_BUDGET_13_Ciaran_Cuffe_Green      0.88421309
## 2010_BUDGET_14_Caoimhghin_OCaolain_SF -0.11219297
```


```r
knitr::kable(summary(ieWF))
```

Call:
	textmodel_wordfish.dfm(x = ., dir = c(6, 5))

Estimated document positions:
                                           theta         SE       lower
2010_BUDGET_01_Brian_Lenihan_FF        1.8209392 0.02032309  1.78110595
2010_BUDGET_02_Richard_Bruton_FG      -0.5932887 0.02818876 -0.64853867
2010_BUDGET_03_Joan_Burton_LAB        -1.1136900 0.01540292 -1.14387977
2010_BUDGET_04_Arthur_Morgan_SF       -0.1219181 0.02846361 -0.17770674
2010_BUDGET_05_Brian_Cowen_FF          1.7724128 0.02364052  1.72607740
2010_BUDGET_06_Enda_Kenny_FG          -0.7145839 0.02650310 -0.76652995
2010_BUDGET_07_Kieran_ODonnell_FG     -0.4844873 0.04171540 -0.56624947
2010_BUDGET_08_Eamon_Gilmore_LAB      -0.5616593 0.02967436 -0.61982104
2010_BUDGET_09_Michael_Higgins_LAB    -0.9703189 0.03850640 -1.04579142
2010_BUDGET_10_Ruairi_Quinn_LAB       -0.9589308 0.03892473 -1.03522326
2010_BUDGET_11_John_Gormley_Green      1.1807224 0.07221372  1.03918351
2010_BUDGET_12_Eamon_Ryan_Green        0.1866522 0.06294150  0.06328684
2010_BUDGET_13_Ciaran_Cuffe_Green      0.7422035 0.07245386  0.60019397
2010_BUDGET_14_Caoimhghin_OCaolain_SF -0.1840532 0.03666338 -0.25591343
                                            upper
2010_BUDGET_01_Brian_Lenihan_FF        1.86077246
2010_BUDGET_02_Richard_Bruton_FG      -0.53803872
2010_BUDGET_03_Joan_Burton_LAB        -1.08350032
2010_BUDGET_04_Arthur_Morgan_SF       -0.06612939
2010_BUDGET_05_Brian_Cowen_FF          1.81874822
2010_BUDGET_06_Enda_Kenny_FG          -0.66263780
2010_BUDGET_07_Kieran_ODonnell_FG     -0.40272510
2010_BUDGET_08_Eamon_Gilmore_LAB      -0.50349756
2010_BUDGET_09_Michael_Higgins_LAB    -0.89484631
2010_BUDGET_10_Ruairi_Quinn_LAB       -0.88263831
2010_BUDGET_11_John_Gormley_Green      1.32226128
2010_BUDGET_12_Eamon_Ryan_Green        0.31001751
2010_BUDGET_13_Ciaran_Cuffe_Green      0.88421309
2010_BUDGET_14_Caoimhghin_OCaolain_SF -0.11219297


|                                      |      theta|        SE|      lower|      upper|
|:-------------------------------------|----------:|---------:|----------:|----------:|
|2010_BUDGET_01_Brian_Lenihan_FF       |  1.8209392| 0.0203231|  1.7811059|  1.8607725|
|2010_BUDGET_02_Richard_Bruton_FG      | -0.5932887| 0.0281888| -0.6485387| -0.5380387|
|2010_BUDGET_03_Joan_Burton_LAB        | -1.1136900| 0.0154029| -1.1438798| -1.0835003|
|2010_BUDGET_04_Arthur_Morgan_SF       | -0.1219181| 0.0284636| -0.1777067| -0.0661294|
|2010_BUDGET_05_Brian_Cowen_FF         |  1.7724128| 0.0236405|  1.7260774|  1.8187482|
|2010_BUDGET_06_Enda_Kenny_FG          | -0.7145839| 0.0265031| -0.7665299| -0.6626378|
|2010_BUDGET_07_Kieran_ODonnell_FG     | -0.4844873| 0.0417154| -0.5662495| -0.4027251|
|2010_BUDGET_08_Eamon_Gilmore_LAB      | -0.5616593| 0.0296744| -0.6198210| -0.5034976|
|2010_BUDGET_09_Michael_Higgins_LAB    | -0.9703189| 0.0385064| -1.0457914| -0.8948463|
|2010_BUDGET_10_Ruairi_Quinn_LAB       | -0.9589308| 0.0389247| -1.0352233| -0.8826383|
|2010_BUDGET_11_John_Gormley_Green     |  1.1807224| 0.0722137|  1.0391835|  1.3222613|
|2010_BUDGET_12_Eamon_Ryan_Green       |  0.1866522| 0.0629415|  0.0632868|  0.3100175|
|2010_BUDGET_13_Ciaran_Cuffe_Green     |  0.7422035| 0.0724539|  0.6001940|  0.8842131|
|2010_BUDGET_14_Caoimhghin_OCaolain_SF | -0.1840532| 0.0366634| -0.2559134| -0.1121930|
