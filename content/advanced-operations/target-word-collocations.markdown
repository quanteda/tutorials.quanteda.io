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
range(corp_news$date)
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
## 1           brexit 229903.00100 0.000000e+00      785           0
## 2             vote   1891.70074 0.000000e+00      134        2330
## 3      uncertainty    799.50640 0.000000e+00       32         308
## 5         sterling    679.91319 0.000000e+00       19         122
## 9            pound    598.49295 0.000000e+00       25         249
## 10      referendum    563.95668 0.000000e+00       49        1007
## 11              eu    549.93957 0.000000e+00       89        3172
## 17           leave    351.87269 0.000000e+00       49        1545
## 28         related    318.66665 0.000000e+00       73        3399
## 32     campaigners    269.25398 0.000000e+00       19         312
## 33            gove    258.99044 0.000000e+00       15         201
## 39         theresa    202.83188 0.000000e+00       13         190
## 41           boris    198.03421 0.000000e+00       18         372
## 42         johnson    194.44946 0.000000e+00       25         710
## 44              uk    173.10023 0.000000e+00       70        4804
## 45           fears    169.11902 0.000000e+00       18         428
## 56          impact    157.56709 0.000000e+00       27         973
## 58           risks    142.43380 0.000000e+00       17         445
## 59            live    140.82752 0.000000e+00       30        1284
## 61            hard    136.80005 0.000000e+00       26        1015
## 62         economy    130.07150 0.000000e+00       35        1830
## 68        economic    125.64768 0.000000e+00       33        1696
## 72           could    112.74010 0.000000e+00       71        6307
## 76           would    108.64802 0.000000e+00      111       12516
## 86        campaign     96.00221 0.000000e+00       41        2910
## 88         britain     94.37140 0.000000e+00       31        1859
## 98               |     91.20112 0.000000e+00      601      120130
## 179        cameron     68.33768 1.110223e-16       29        2050
## 181            gmt     64.28482 1.110223e-15       38        3260
## 182       treasury     63.91333 1.332268e-15       12         445
## 193           camp     60.48016 7.438494e-15       11         395
## 196           2016     59.11791 1.487699e-14       27        1996
## 197           june     58.42136 2.120526e-14       15         715
## 200        markets     56.83737 4.729550e-14       18        1004
## 207        ireland     52.47882 4.348744e-13       11         441
## 211      britain's     52.23463 4.924949e-13       16         867
## 212           uk's     51.43108 7.415180e-13       13         607
## 214       brussels     50.09356 1.465827e-12       14         706
## 221   negotiations     47.71493 4.929279e-12       12         555
## 225    immigration     46.25445 1.038503e-11       15         842
## 227 published-time     45.50519 1.522349e-11       41        4361
## 228           mean     44.78349 2.200706e-11       13         670
## 235          david     42.07977 8.762524e-11       22        1761
## 239       february     41.26175 1.331492e-10       18        1236
## 241       european     40.80390 1.682973e-10       24        2053
## 243          talks     40.75006 1.729981e-10       15         916
## 245            may     39.77773 2.845716e-10       36        3837
## 255        osborne     36.39981 1.607190e-09       14         876
## 291     block-time     34.74143 3.765321e-09       46        5785
## 293          after     34.23488 4.884550e-09       62        8771
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
## 1             trump 236270.1842 0     3269           0
## 2            donald  45438.9753 0      842         275
## 3              cruz   6872.4696 0      322         703
## 4               ted   3977.8707 0      168         318
## 5        republican   3963.5491 0      283        1033
## 6  @realdonaldtrump   2420.9093 0       53          28
## 7           nominee   1247.4904 0       64         154
## 8                 j   1181.4302 0       55         116
## 9           clinton   1165.8311 0      201        1739
## 10      frontrunner   1120.8309 0       52         109
## 11            rubio    972.1028 0       97         505
## 12           carson    891.2659 0       61         208
## 13     presidential    794.7569 0      109         770
## 14         campaign    759.5172 0      214        2737
## 15      lewandowski    727.0195 0       34          71
## 16          hillary    719.4706 0       93         622
## 17            rally    712.4003 0       77         434
## 18            1,237    677.8672 0       23          29
## 19          melania    675.4567 0       18          14
## 20           kasich    663.9647 0       60         277
## 21        delegates    608.4636 0       60         303
## 22          sanders    563.2439 0      117        1182
## 23      republicans    533.7499 0       61         363
## 24        candidate    513.6965 0       96         889
## 25              his    506.6036 0      568       16257
## 26             2016    488.6911 0      143        1880
## 27              gop    479.4809 0       31          97
## 28       nomination    449.4586 0       43         209
## 29             poll    437.7584 0       72         598
## 32            corey    402.4667 0       18          34
## 33           voters    399.9273 0       98        1128
## 34             iowa    366.4640 0       43         256
## 36            mogul    359.6883 0       14          21
## 37           staten    356.9503 0       16          30
## 38          crooked    351.0558 0       11          11
## 39          primary    344.9898 0       64         589
## 40         delegate    332.9586 0       20          56
## 41              fox    330.5737 0       32         156
## 42       supporters    321.0756 0       65         643
## 43             race    317.5091 0       61         578
## 44          indiana    310.2220 0       21          68
## 45                |    307.7380 0     2347      118384
## 46           cruz's    307.3998 0       24          92
## 48            tower    289.7495 0       25         107
## 49            marco    287.9638 0       31         168
## 50      presumptive    287.8781 0       14          29
## 51          related    285.8764 0      163        3309
## 53          muslims    280.2655 0       43         336
## 54              jeb    272.8453 0       25         114
## 56             ohio    269.5311 0       24         106
```
