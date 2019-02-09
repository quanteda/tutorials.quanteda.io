---
title: Target-word collocations
weight: 40
draft: false
---


```r
require(quanteda)
```

This corpus contains 6,000 Guardian news articles from 2012 to 2016.


```r
corp_news <- download('data_corpus_guardian')
```




```r
ndoc(corp_news)
```

```
## [1] 6000
```

```r
range(docvars(corp_news, 'date'))
```

```
## [1] "2012-01-02" "2016-12-31"
```

```r
toks_news <- tokens(corp_news, remove_punct = TRUE)
```

### Keywords for Brexit

We can also find words associated with target words using the `window` argument of `tokens_select()`. 


```r
toks_brexit <- tokens_keep(toks_news, pattern = 'brexit', window = 10) # equivalent to tokens_select(selection = 'keep')
toks_nobrexit <- tokens_remove(toks_news, pattern = 'brexit', window = 10) # equivalent to tokens_select(selection = 'remove')
print(toks_brexit[['text173244']])
```

```
##  [1] "said"         "the"          "Airports"     "Commission"  
##  [5] "had"          "disagreed"    "with"         "Khan"        
##  [9] "adding"       "that"         "Brexit"       "loaded"      
## [13] "the"          "dice"         "further"      "in"          
## [17] "favour"       "of"           "the"          "west"        
## [21] "London"       "UK"           "needs"        "more"        
## [25] "easily"       "and"          "quickly"      "than"        
## [29] "any"          "other"        "option"       "Brexit"      
## [33] "makes"        "the"          "commission's" "conclusion"  
## [37] "that"         "with"         "Heathrow"     "expansion"   
## [41] "the"          "benefits"
```

```r
dfmat_brexit <- dfm(toks_brexit)
dfmat_nobrexit <- dfm(toks_nobrexit)

tstat_key_brexit <- textstat_keyness(rbind(dfmat_brexit, dfmat_nobrexit), seq_len(ndoc(dfmat_brexit)))
tstat_key_brexit_subset <- tstat_key_brexit[tstat_key_brexit$n_target > 10, ]
head(tstat_key_brexit_subset, 50)
```

```
##            feature         chi2            p n_target n_reference
## 1           brexit 224316.28238 0.000000e+00      785           0
## 2             vote   1899.10816 0.000000e+00      136        2328
## 3      uncertainty    778.73375 0.000000e+00       32         308
## 4         sterling    738.25729 0.000000e+00       20         121
## 6            pound    633.46547 0.000000e+00       26         248
## 11      referendum    548.16486 0.000000e+00       49        1007
## 12              eu    546.70630 0.000000e+00       90        3171
## 18           leave    357.15591 0.000000e+00       50        1544
## 30         related    308.01028 0.000000e+00       73        3399
## 35     campaigners    261.91081 0.000000e+00       19         312
## 36            gove    252.07009 0.000000e+00       15         201
## 37         theresa    232.25275 0.000000e+00       14         189
## 39           boris    217.05512 0.000000e+00       19         371
## 45         johnson    206.28078 0.000000e+00       26         709
## 47              uk    185.63037 0.000000e+00       73        4802
## 48           fears    185.50843 0.000000e+00       19         427
## 60          impact    152.64320 0.000000e+00       27         973
## 61           risks    138.27100 0.000000e+00       17         445
## 62            live    136.21445 0.000000e+00       30        1284
## 64            hard    132.43285 0.000000e+00       26        1015
## 70         economy    125.54298 0.000000e+00       35        1830
## 72        economic    121.30035 0.000000e+00       33        1696
## 76           could    112.09288 0.000000e+00       72        6306
## 78             gmt    110.23117 0.000000e+00       47        3251
## 82         britain    106.40517 0.000000e+00       33        1857
## 92           would    102.74030 0.000000e+00      111       12516
## 95        campaign     98.28525 0.000000e+00       42        2909
## 108           2016     81.73665 0.000000e+00       31        1992
## 188        cameron     65.62638 5.551115e-16       29        2050
## 190     block-time     63.00233 2.109424e-15       56        5775
## 192       treasury     61.88801 3.663736e-15       12         445
## 204      britain's     58.80251 1.743050e-14       17         866
## 205           camp     58.57778 1.953993e-14       11         395
## 207 published-time     57.64780 3.130829e-14       45        4357
## 211           june     56.42848 5.828671e-14       15         715
## 215        markets     54.78576 1.344480e-13       18        1004
## 226        ireland     50.77727 1.034617e-12       11         441
## 227           uk's     49.68573 1.804557e-12       13         607
## 229       european     48.85995 2.749023e-12       26        2051
## 230        england     48.38569 3.501199e-12       18        1090
## 231       brussels     48.34862 3.567924e-12       14         706
## 239   negotiations     46.09902 1.124245e-11       12         555
## 243    immigration     44.57649 2.446121e-11       15         842
## 244            mps     43.98883 3.302547e-11       19        1268
## 250           mean     43.20913 4.919065e-11       13         670
## 253     businesses     42.21259 8.187129e-11       14         775
## 256          david     40.29289 2.186027e-10       22        1761
## 261       february     39.62488 3.077378e-10       18        1236
## 263          talks     39.21734 3.791588e-10       15         916
## 265        morning     38.86238 4.547599e-10       19        1367
```

### Keywords for Trump

Targeted frequency analysis might look complex, but can be done in five lines.


```r
trump <- c('donald trump', 'trump')
dfmat_trump <- tokens_keep(toks_news, pattern = phrase(trump), window = 10) %>% 
    dfm()

dfmat_no_trump <- tokens_remove(toks_news, pattern = phrase(trump), window = 10) %>%
    dfm()
tstat_key_trump <- textstat_keyness(rbind(dfmat_trump, dfmat_no_trump), seq_len(ndoc(dfmat_trump)))

tstat_key_trump_subset <- tstat_key_trump[tstat_key_trump$n_target > 10, ]
head(tstat_key_trump_subset, 50)
```

```
##             feature        chi2 p n_target n_reference
## 1             trump 231497.3213 0     3269           0
## 2            donald  44727.8680 0      844         273
## 3              cruz   7259.6180 0      334         691
## 4        republican   3992.9237 0      287        1029
## 5               ted   3941.5664 0      169         317
## 6  @realdonaldtrump   2464.4349 0       54          27
## 7           nominee   1302.5910 0       66         152
## 8           clinton   1215.5657 0      207        1733
## 9                 j   1156.0868 0       55         116
## 10      frontrunner   1096.7950 0       52         109
## 11            rubio   1037.0474 0      101         501
## 12           carson    871.4414 0       61         208
## 13      lewandowski    803.0234 0       36          69
## 14           kasich    798.5019 0       66         271
## 15            1,237    791.1620 0       25          27
## 16     presidential    775.2109 0      109         770
## 17         campaign    763.3989 0      217        2734
## 18            rally    756.5660 0       80         431
## 19          hillary    753.5980 0       96         619
## 20        delegates    673.1628 0       63         300
## 21          melania    661.4825 0       18          14
## 22          sanders    581.9218 0      120        1179
## 23             2016    569.6927 0      154        1869
## 24              gop    536.4764 0       33          95
## 25      republicans    521.0228 0       61         363
## 26        candidate    512.4968 0       97         888
## 27              his    512.4496 0      578       16247
## 28             poll    468.4371 0       75         595
## 29       nomination    461.8515 0       44         208
## 32           voters    408.2404 0      100        1126
## 33            corey    393.8726 0       18          34
## 34             race    371.3457 0       66         573
## 35             iowa    357.7049 0       43         256
## 37            mogul    352.0878 0       14          21
## 38           staten    349.3289 0       16          30
## 39          crooked    343.7391 0       11          11
## 40          primary    335.9814 0       64         589
## 41          indiana    335.6590 0       22          67
## 42           cruz's    328.5565 0       25          91
## 43         delegate    325.6640 0       20          56
## 44               ad    325.2913 0       39         231
## 45       supporters    323.9674 0       66         642
## 46              fox    322.9033 0       32         156
## 47          muslims    318.4170 0       46         333
## 48              jeb    315.9173 0       27         112
## 50          trump's    288.5550 0       58         558
## 51             bush    283.6842 0       43         325
## 52            tower    283.1338 0       25         107
## 53      presumptive    281.6943 0       14          29
## 54            marco    281.1780 0       31         168
```
