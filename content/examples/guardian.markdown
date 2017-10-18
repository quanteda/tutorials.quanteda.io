---
title: Newspapers
draft: true
---

# Analyzing Media Text

### Kohei Watanabe
### 24 April 2017



### Overview

This section explains specific techniques useful in analyzing news articles:

* Constructing subject specific `tokens` object using `kwic`
* Identifying and concatenating multi-word expressions using `textstat_collocations()`

### Characteristics of news articles/transcripts

It is common to construct a corpus of news articles (or transcripts) from databases with keyword queries, but such a corpus still contains a lot of noises, because a news article is a set sentences and paragraphs on various subjects. To focus our analysis on particular subjects, we can construct a `tokens` by `kwic()`.

1. Construct a `tokens` as usual from a corpus
2. Run `kwic()` on the `tokens` to create a `kwic` 
3. Convert `kiwc` into `tokens` using `as.tokens`
4. Apply whatever analysis we want on `tokens`

### Constructing tokens from kwic


```r
require(quanteda)
## Loading required package: quanteda
## quanteda version 0.99.14
## Using 3 of 4 threads for parallel computing
## 
## Attaching package: 'quanteda'
## The following object is masked from 'package:utils':
## 
##     View
load('guardianSample.RData')
toks <- tokens(guardianSample, remove_punct = TRUE)
toks <- tokens_remove(toks, stopwords('english'))

europe <- kwic(toks, c('Europe*', 'European Union', 'EU'))
head(europe)
##                                                                          
##  [text166545, 128]    Sunday master's degree Russian Eastern | European |
##  [text166549, 460]      also agnostic possibility UK leaving | European |
##  [text166551, 128]    Sunday master's degree Russian Eastern | European |
##  [text166552, 345]     protests human rights groups scrutiny | European |
##  [text166552, 416]       LGBTI people least legal protection |    EU    |
##  [text166556, 231] attending Bilderberg 2016 senior director | European |
##                                            
##  Eurasian studies rather making campus     
##  Union refused say whether potential       
##  Eurasian studies rather making campus     
##  bodies Last month government scrapped     
##  activists say impression attacks increased
##  affairs Aspen Institute sounds awful

britain <- kwic(toks, c('Brit*', 'United Kingdom', 'UK'))
head(britain)
##                                                                    
##  [text166549, 458]         said Johnson also agnostic possibility |
##  [text166549, 470]                potential Brexit hurt US assume |
##  [text166549, 489]      condemned Barack Obama's presumption urge |
##  [text166549, 496]        think far removed decision-making occur |
##    [text166557, 4]                      German government written |
##   [text166557, 15] undercover policing extended covert operations |
##                                                  
##    UK    | leaving European Union refused say    
##  Britain | acting best interests best interests  
##    UK    | exit think far removed decision-making
##  Britain | said                                  
##  British | Home Office asking Pitchford inquiry  
##  British | police Germany inquiry set following

toks_europe <- as.tokens(europe)
toks_britain <- as.tokens(britain)
```

### Relative frequency analysis

```r
dfm_europe <- dfm(toks_europe)
dfm_britain <- dfm(toks_britain)
kwds <- textstat_keyness(rbind(dfm_europe, dfm_britain), target = seq_along(toks_europe))
head(kwds, 20)
##                   chi2 p n_target n_reference
## eu          4442.10046 0    12743        3967
## european    2051.37224 0     4554        1066
## europe      1498.64867 0     3377         801
## referendum   381.17637 0     1833         783
## union        298.20806 0     1417         601
## leaving      199.74942 0      955         406
## uk's         190.52331 0      374          73
## commission   144.58534 0      387         109
## countries    126.19164 0      575         238
## membership   118.63619 0      613         271
## turkey       107.58729 0      233          52
## member       107.13918 0      499         209
## eastern      106.61401 0      173          24
## leave         97.28235 0     1866        1252
## council       91.82762 0      305         103
## europeans     80.90518 0      177          40
## europe's      79.23348 0      227          68
## jean-claude   71.21817 0      133          24
## leaders       70.74838 0      485         243
## president     70.32435 0      318         131
tail(kwds, 20)
##                    chi2            p n_target n_reference
## territories   -50.38892 1.260991e-12        4          59
## retail        -51.95931 5.666578e-13        8          70
## great         -51.97641 5.616618e-13      164         311
## shouted       -52.99678 3.340661e-13        3          59
## recession     -53.67940 2.360334e-13       35         124
## brits         -54.00779 1.997291e-13       20          97
## virgin        -64.81069 7.771561e-16        4          73
## people        -65.31925 6.661338e-16      716        1015
## shouting      -68.84903 1.110223e-16        0          66
## overseas      -69.99963 1.110223e-16       23         120
## britons       -74.88527 0.000000e+00       97         251
## rating        -82.54807 0.000000e+00        1          82
## tax           -89.08758 0.000000e+00       42         177
## companies     -95.67059 0.000000e+00       55         208
## government   -115.00713 0.000000e+00      363         688
## first        -144.03459 0.000000e+00      196         497
## britain's    -451.27557 0.000000e+00      607        1544
## britain     -1516.90395 0.000000e+00     2082        5225
## british     -2177.23832 0.000000e+00      712        3700
## uk          -3569.67255 0.000000e+00     2406        8266
```

### Targetted dictionary analysis 

```r
dict <- dictionary(list(people = c('people', 'citizen*'), 
                        migrant = c('immigra*', 'migra*'),
                        economy = c('econom*', 'financ*', 'business*'),
                        crime = c('crim*', 'polic*', 'terror*')))

dfm_dict_europe <- dfm(toks_europe, dictionary = dict)
dfm_dict_britain <- dfm(toks_britain, dictionary = dict)

freq_dict_europe <- colSums(dfm_dict_europe) / sum(ntoken(toks_europe))
freq_dict_britain <- colSums(dfm_dict_britain) / sum(ntoken(toks_britain))


plot(freq_dict_europe, ylab = 'Frequency', xlab = 'Category', xaxt = 'n', 
     ylim = c(0, 0.01), xlim = c(0.5, 4.5), main = "Frequency of keywords")
axis(1, at = 1:4, labels = names(freq_dict_europe))
grid()
points(freq_dict_britain, col = 'red', pch = 2)
legend('topright', legend = c('Europe', 'Britain'), col = c('black', 'red'), pch = 1:2)
```

<img src="/examples/guardian_files/figure-html/unnamed-chunk-4-1.svg" width="768" />

### Identifying and concatenating multi-word expressions


```r
load('guardianSample.RData')
toks <- tokens(guardianSample, remove_punct = FALSE)
toks <- tokens_remove(toks, stopwords('english'), padding = TRUE)
toks <- tokens_select(toks, "^[A-Z][A-Za-z0-9]+$", valuetype = "regex", padding = TRUE)
col <- textstat_collocations(toks, method = 'lambda', size = 2, min_count = 100)
head(col)
toks_multi <- tokens_compound(toks, col)
head(toks[['text166563']], 50)
head(toks_multi[['text166563']], 50)

```
