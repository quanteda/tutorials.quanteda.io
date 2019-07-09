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
## 1           brexit 224316.52938 0.000000e+00      785           0
## 2             vote   1899.11052 0.000000e+00      136        2328
## 3      uncertainty    778.73467 0.000000e+00       32         308
## 4         sterling    738.25814 0.000000e+00       20         121
## 6            pound    633.46621 0.000000e+00       26         248
## 11      referendum    548.16556 0.000000e+00       49        1007
## 12              eu    546.70707 0.000000e+00       90        3171
## 18           leave    357.15640 0.000000e+00       50        1544
## 30         related    308.01075 0.000000e+00       73        3399
## 35     campaigners    261.91114 0.000000e+00       19         312
## 36            gove    252.07039 0.000000e+00       15         201
## 37         theresa    232.25304 0.000000e+00       14         189
## 39           boris    217.05540 0.000000e+00       19         371
## 45         johnson    206.28106 0.000000e+00       26         709
## 47              uk    185.63070 0.000000e+00       73        4802
## 48           fears    185.50867 0.000000e+00       19         427
## 60          impact    152.64341 0.000000e+00       27         973
## 61           risks    138.27118 0.000000e+00       17         445
## 62            live    136.21465 0.000000e+00       30        1284
## 64            hard    132.43304 0.000000e+00       26        1015
## 70         economy    125.54318 0.000000e+00       35        1830
## 72        economic    121.30054 0.000000e+00       33        1696
## 76           could    112.09311 0.000000e+00       72        6306
## 78             gmt    110.23137 0.000000e+00       47        3251
## 82         britain    106.40534 0.000000e+00       33        1857
## 92           would    102.74056 0.000000e+00      111       12516
## 95        campaign     98.28543 0.000000e+00       42        2909
## 108           2016     81.73680 0.000000e+00       31        1992
## 188        cameron     65.62650 5.551115e-16       29        2050
## 190     block-time     63.00248 2.109424e-15       56        5775
## 192       treasury     61.88810 3.663736e-15       12         445
## 204      britain's     58.80260 1.743050e-14       17         866
## 205           camp     58.57787 1.953993e-14       11         395
## 207 published-time     57.64793 3.130829e-14       45        4357
## 211           june     56.42857 5.828671e-14       15         715
## 215        markets     54.78585 1.344480e-13       18        1004
## 226        ireland     50.77735 1.034617e-12       11         441
## 227           uk's     49.68581 1.804445e-12       13         607
## 229       european     48.86004 2.748912e-12       26        2051
## 230        england     48.38577 3.500977e-12       18        1090
## 231       brussels     48.34870 3.567813e-12       14         706
## 239   negotiations     46.09909 1.124212e-11       12         555
## 243    immigration     44.57656 2.446021e-11       15         842
## 244            mps     43.98891 3.302414e-11       19        1268
## 250           mean     43.20920 4.918888e-11       13         670
## 253     businesses     42.21266 8.186840e-11       14         775
## 256          david     40.29297 2.185938e-10       22        1761
## 261       february     39.62495 3.077264e-10       18        1236
## 263          talks     39.21740 3.791457e-10       15         916
## 265        morning     38.86246 4.547429e-10       19        1367
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
## 1             trump 231497.5787 0     3269           0
## 2            donald  44727.9183 0      844         273
## 3              cruz   7259.6266 0      334         691
## 4        republican   3992.9287 0      287        1029
## 5               ted   3941.5710 0      169         317
## 6  @realdonaldtrump   2464.4377 0       54          27
## 7           nominee   1302.5925 0       66         152
## 8           clinton   1215.5674 0      207        1733
## 9                 j   1156.0882 0       55         116
## 10      frontrunner   1096.7963 0       52         109
## 11            rubio   1037.0487 0      101         501
## 12           carson    871.4424 0       61         208
## 13      lewandowski    803.0243 0       36          69
## 14           kasich    798.5029 0       66         271
## 15            1,237    791.1629 0       25          27
## 16     presidential    775.2120 0      109         770
## 17         campaign    763.4001 0      217        2734
## 18            rally    756.5670 0       80         431
## 19          hillary    753.5990 0       96         619
## 20        delegates    673.1636 0       63         300
## 21          melania    661.4833 0       18          14
## 22          sanders    581.9227 0      120        1179
## 23             2016    569.6936 0      154        1869
## 24              gop    536.4770 0       33          95
## 25      republicans    521.0235 0       61         363
## 26        candidate    512.4976 0       97         888
## 27              his    512.4509 0      578       16247
## 28             poll    468.4377 0       75         595
## 29       nomination    461.8521 0       44         208
## 32           voters    408.2410 0      100        1126
## 33            corey    393.8731 0       18          34
## 34             race    371.3462 0       66         573
## 35             iowa    357.7054 0       43         256
## 37            mogul    352.0882 0       14          21
## 38           staten    349.3293 0       16          30
## 39          crooked    343.7395 0       11          11
## 40          primary    335.9819 0       64         589
## 41          indiana    335.6594 0       22          67
## 42           cruz's    328.5569 0       25          91
## 43         delegate    325.6644 0       20          56
## 44               ad    325.2917 0       39         231
## 45       supporters    323.9679 0       66         642
## 46              fox    322.9037 0       32         156
## 47          muslims    318.4175 0       46         333
## 48              jeb    315.9177 0       27         112
## 50          trump's    288.5554 0       58         558
## 51             bush    283.6846 0       43         325
## 52            tower    283.1341 0       25         107
## 53      presumptive    281.6947 0       14          29
## 54            marco    281.1784 0       31         168
```
