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
##  [1] "said"         "the"          "Airports"     "Commission"   "had"         
##  [6] "disagreed"    "with"         "Khan"         "adding"       "that"        
## [11] "Brexit"       "loaded"       "the"          "dice"         "further"     
## [16] "in"           "favour"       "of"           "the"          "west"        
## [21] "London"       "needs"        "more"         "easily"       "and"         
## [26] "quickly"      "than"         "any"          "other"        "option"      
## [31] "|"            "Brexit"       "makes"        "the"          "commission's"
## [36] "conclusion"   "that"         "with"         "Heathrow"     "expansion"   
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
## 1           brexit 229894.37532 0.000000e+00      785           0
## 2             vote   1891.62084 0.000000e+00      134        2330
## 3      uncertainty    799.47432 0.000000e+00       32         308
## 5         sterling    679.88650 0.000000e+00       19         122
## 9            pound    598.46888 0.000000e+00       25         249
## 10      referendum    563.93230 0.000000e+00       49        1007
## 11              eu    549.91324 0.000000e+00       89        3172
## 17           leave    351.85631 0.000000e+00       49        1545
## 28         related    318.65019 0.000000e+00       73        3399
## 32     campaigners    269.24265 0.000000e+00       19         312
## 33            gove    258.97976 0.000000e+00       15         201
## 39         theresa    202.82343 0.000000e+00       13         190
## 41           boris    198.02561 0.000000e+00       18         372
## 42         johnson    194.44056 0.000000e+00       25         710
## 44              uk    173.08977 0.000000e+00       70        4804
## 45           fears    169.11152 0.000000e+00       18         428
## 56          impact    157.55949 0.000000e+00       27         973
## 58           risks    142.42737 0.000000e+00       17         445
## 59            live    140.82039 0.000000e+00       30        1284
## 61            hard    136.79331 0.000000e+00       26        1015
## 62         economy    130.06450 0.000000e+00       35        1830
## 68        economic    125.64096 0.000000e+00       33        1696
## 72           could    112.73220 0.000000e+00       71        6307
## 76           would    108.63886 0.000000e+00      111       12516
## 86        campaign     95.99631 0.000000e+00       41        2910
## 88         britain     94.36604 0.000000e+00       31        1859
## 98               |     91.18313 0.000000e+00      601      120130
## 179        cameron     68.33349 1.110223e-16       29        2050
## 181            gmt     64.28041 1.110223e-15       38        3260
## 182       treasury     63.91020 1.332268e-15       12         445
## 193           camp     60.47722 7.438494e-15       11         395
## 196           2016     59.11420 1.487699e-14       27        1996
## 197           june     58.41829 2.120526e-14       15         715
## 200        markets     56.83420 4.740652e-14       18        1004
## 207        ireland     52.47619 4.355405e-13       11         441
## 211      britain's     52.23175 4.932721e-13       16         867
## 212           uk's     51.42839 7.425172e-13       13         607
## 214       brussels     50.09087 1.467937e-12       14         706
## 221   negotiations     47.71244 4.935496e-12       12         555
## 225    immigration     46.25186 1.039879e-11       15         842
## 227 published-time     45.50154 1.525191e-11       41        4361
## 228           mean     44.78105 2.203449e-11       13         670
## 235          david     42.07700 8.774914e-11       22        1761
## 239       february     41.25922 1.333217e-10       18        1236
## 241       european     40.80110 1.685379e-10       24        2053
## 243          talks     40.74769 1.732079e-10       15         916
## 245            may     39.77453 2.850377e-10       36        3837
## 255        osborne     36.39767 1.608958e-09       14         876
## 291     block-time     34.73817 3.771632e-09       46        5785
## 293          after     34.23121 4.893790e-09       62        8771
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
## 1             trump 236264.9805 0     3269           0
## 2            donald  45437.9647 0      842         275
## 3              cruz   6872.3087 0      322         703
## 4               ted   3977.7783 0      168         318
## 5        republican   3963.4525 0      283        1033
## 6  @realdonaldtrump   2420.8551 0       53          28
## 7           nominee   1247.4609 0       64         154
## 8                 j   1181.4025 0       55         116
## 9           clinton   1165.7984 0      201        1739
## 10      frontrunner   1120.8047 0       52         109
## 11            rubio    972.0780 0       97         505
## 12           carson    891.2443 0       61         208
## 13     presidential    794.7356 0      109         770
## 14         campaign    759.4932 0      214        2737
## 15      lewandowski    727.0025 0       34          71
## 16          hillary    719.4515 0       93         622
## 17            rally    712.3820 0       77         434
## 18            1,237    677.8517 0       23          29
## 19          melania    675.4415 0       18          14
## 20           kasich    663.9480 0       60         277
## 21        delegates    608.4481 0       60         303
## 22          sanders    563.2274 0      117        1182
## 23      republicans    533.7360 0       61         363
## 24        candidate    513.6818 0       96         889
## 25              his    506.5779 0      568       16257
## 26             2016    488.6755 0      143        1880
## 27              gop    479.4694 0       31          97
## 28       nomination    449.4473 0       43         209
## 29             poll    437.7463 0       72         598
## 32            corey    402.4573 0       18          34
## 33           voters    399.9152 0       98        1128
## 34             iowa    366.4544 0       43         256
## 36            mogul    359.6800 0       14          21
## 37           staten    356.9420 0       16          30
## 38          crooked    351.0478 0       11          11
## 39          primary    344.9800 0       64         589
## 40         delegate    332.9506 0       20          56
## 41              fox    330.5653 0       32         156
## 42       supporters    321.0663 0       65         643
## 43             race    317.5000 0       61         578
## 44          indiana    310.2145 0       21          68
## 45                |    307.7001 0     2347      118384
## 46           cruz's    307.3922 0       24          92
## 48            tower    289.7423 0       25         107
## 49            marco    287.9564 0       31         168
## 50      presumptive    287.8714 0       14          29
## 51          related    285.8652 0      163        3309
## 53          muslims    280.2579 0       43         336
## 54              jeb    272.8384 0       25         114
## 56             ohio    269.5243 0       24         106
```
