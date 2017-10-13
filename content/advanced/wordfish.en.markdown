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
## 2010_BUDGET_01_Brian_Lenihan_FF        1.8204853 0.02031136  1.78067506
## 2010_BUDGET_02_Richard_Bruton_FG      -0.5935941 0.02820181 -0.64886962
## 2010_BUDGET_03_Joan_Burton_LAB        -1.1141263 0.01541454 -1.14433884
## 2010_BUDGET_04_Arthur_Morgan_SF       -0.1214490 0.02847670 -0.17726333
## 2010_BUDGET_05_Brian_Cowen_FF          1.7721035 0.02362608  1.72579641
## 2010_BUDGET_06_Enda_Kenny_FG          -0.7147287 0.02652093 -0.76670976
## 2010_BUDGET_07_Kieran_ODonnell_FG     -0.4846416 0.04173533 -0.56644287
## 2010_BUDGET_08_Eamon_Gilmore_LAB      -0.5612695 0.02970077 -0.61948300
## 2010_BUDGET_09_Michael_Higgins_LAB    -0.9706527 0.03853464 -1.04618057
## 2010_BUDGET_10_Ruairi_Quinn_LAB       -0.9592196 0.03895446 -1.03557031
## 2010_BUDGET_11_John_Gormley_Green      1.1807301 0.07218433  1.03924887
## 2010_BUDGET_12_Eamon_Ryan_Green        0.1868450 0.06295052  0.06346197
## 2010_BUDGET_13_Ciaran_Cuffe_Green      0.7426461 0.07243733  0.60066894
## 2010_BUDGET_14_Caoimhghin_OCaolain_SF -0.1831286 0.03668897 -0.25503899
##                                             upper
## 2010_BUDGET_01_Brian_Lenihan_FF        1.86029558
## 2010_BUDGET_02_Richard_Bruton_FG      -0.53831852
## 2010_BUDGET_03_Joan_Burton_LAB        -1.08391383
## 2010_BUDGET_04_Arthur_Morgan_SF       -0.06563465
## 2010_BUDGET_05_Brian_Cowen_FF          1.81841066
## 2010_BUDGET_06_Enda_Kenny_FG          -0.66274772
## 2010_BUDGET_07_Kieran_ODonnell_FG     -0.40284036
## 2010_BUDGET_08_Eamon_Gilmore_LAB      -0.50305599
## 2010_BUDGET_09_Michael_Higgins_LAB    -0.89512477
## 2010_BUDGET_10_Ruairi_Quinn_LAB       -0.88286883
## 2010_BUDGET_11_John_Gormley_Green      1.32221143
## 2010_BUDGET_12_Eamon_Ryan_Green        0.31022801
## 2010_BUDGET_13_Ciaran_Cuffe_Green      0.88462329
## 2010_BUDGET_14_Caoimhghin_OCaolain_SF -0.11121823
```


```r
knitr::kable(summary(ieWF))
```

Call:
	textmodel_wordfish.dfm(x = ., dir = c(6, 5))

Estimated document positions:
                                           theta         SE       lower
2010_BUDGET_01_Brian_Lenihan_FF        1.8204853 0.02031136  1.78067506
2010_BUDGET_02_Richard_Bruton_FG      -0.5935941 0.02820181 -0.64886962
2010_BUDGET_03_Joan_Burton_LAB        -1.1141263 0.01541454 -1.14433884
2010_BUDGET_04_Arthur_Morgan_SF       -0.1214490 0.02847670 -0.17726333
2010_BUDGET_05_Brian_Cowen_FF          1.7721035 0.02362608  1.72579641
2010_BUDGET_06_Enda_Kenny_FG          -0.7147287 0.02652093 -0.76670976
2010_BUDGET_07_Kieran_ODonnell_FG     -0.4846416 0.04173533 -0.56644287
2010_BUDGET_08_Eamon_Gilmore_LAB      -0.5612695 0.02970077 -0.61948300
2010_BUDGET_09_Michael_Higgins_LAB    -0.9706527 0.03853464 -1.04618057
2010_BUDGET_10_Ruairi_Quinn_LAB       -0.9592196 0.03895446 -1.03557031
2010_BUDGET_11_John_Gormley_Green      1.1807301 0.07218433  1.03924887
2010_BUDGET_12_Eamon_Ryan_Green        0.1868450 0.06295052  0.06346197
2010_BUDGET_13_Ciaran_Cuffe_Green      0.7426461 0.07243733  0.60066894
2010_BUDGET_14_Caoimhghin_OCaolain_SF -0.1831286 0.03668897 -0.25503899
                                            upper
2010_BUDGET_01_Brian_Lenihan_FF        1.86029558
2010_BUDGET_02_Richard_Bruton_FG      -0.53831852
2010_BUDGET_03_Joan_Burton_LAB        -1.08391383
2010_BUDGET_04_Arthur_Morgan_SF       -0.06563465
2010_BUDGET_05_Brian_Cowen_FF          1.81841066
2010_BUDGET_06_Enda_Kenny_FG          -0.66274772
2010_BUDGET_07_Kieran_ODonnell_FG     -0.40284036
2010_BUDGET_08_Eamon_Gilmore_LAB      -0.50305599
2010_BUDGET_09_Michael_Higgins_LAB    -0.89512477
2010_BUDGET_10_Ruairi_Quinn_LAB       -0.88286883
2010_BUDGET_11_John_Gormley_Green      1.32221143
2010_BUDGET_12_Eamon_Ryan_Green        0.31022801
2010_BUDGET_13_Ciaran_Cuffe_Green      0.88462329
2010_BUDGET_14_Caoimhghin_OCaolain_SF -0.11121823


|                                      |      theta|        SE|      lower|      upper|
|:-------------------------------------|----------:|---------:|----------:|----------:|
|2010_BUDGET_01_Brian_Lenihan_FF       |  1.8204853| 0.0203114|  1.7806751|  1.8602956|
|2010_BUDGET_02_Richard_Bruton_FG      | -0.5935941| 0.0282018| -0.6488696| -0.5383185|
|2010_BUDGET_03_Joan_Burton_LAB        | -1.1141263| 0.0154145| -1.1443388| -1.0839138|
|2010_BUDGET_04_Arthur_Morgan_SF       | -0.1214490| 0.0284767| -0.1772633| -0.0656346|
|2010_BUDGET_05_Brian_Cowen_FF         |  1.7721035| 0.0236261|  1.7257964|  1.8184107|
|2010_BUDGET_06_Enda_Kenny_FG          | -0.7147287| 0.0265209| -0.7667098| -0.6627477|
|2010_BUDGET_07_Kieran_ODonnell_FG     | -0.4846416| 0.0417353| -0.5664429| -0.4028404|
|2010_BUDGET_08_Eamon_Gilmore_LAB      | -0.5612695| 0.0297008| -0.6194830| -0.5030560|
|2010_BUDGET_09_Michael_Higgins_LAB    | -0.9706527| 0.0385346| -1.0461806| -0.8951248|
|2010_BUDGET_10_Ruairi_Quinn_LAB       | -0.9592196| 0.0389545| -1.0355703| -0.8828688|
|2010_BUDGET_11_John_Gormley_Green     |  1.1807301| 0.0721843|  1.0392489|  1.3222114|
|2010_BUDGET_12_Eamon_Ryan_Green       |  0.1868450| 0.0629505|  0.0634620|  0.3102280|
|2010_BUDGET_13_Ciaran_Cuffe_Green     |  0.7426461| 0.0724373|  0.6006689|  0.8846233|
|2010_BUDGET_14_Caoimhghin_OCaolain_SF | -0.1831286| 0.0366890| -0.2550390| -0.1112182|
