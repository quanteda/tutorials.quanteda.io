---
title: Compound multi-word expressions
weight: 20
draft: false
---

We can compound multi-word expressions through collocation analysis. In this example, we will identify sequences of capitalized words and compound them as proper names, which are important linguistic features of newspaper articles.


```r
require(quanteda)
require(quanteda.textstats)
require(quanteda.corpora)
options(width = 110)
```

This corpus contains 6,000 Guardian news articles from 2012 to 2016.


```r
corp_news <- download("data_corpus_guardian")
```





We remove punctuation marks and symbols in `tokens()` and stopwords in `tokens_remove()` with `padding = TRUE` to keep the original positions of tokens. 


```r
toks_news <- tokens(corp_news, remove_punct = TRUE, remove_symbols = TRUE, padding = TRUE) %>% 
    tokens_remove(stopwords("en"), padding = TRUE)
```

One of the most common type of multi-word expressions is proper names, which we can select simply based on capitalization in English texts.


```r
toks_news_cap <- tokens_select(toks_news, 
                               pattern = "^[A-Z]",
                               valuetype = "regex",
                               case_insensitive = FALSE, 
                               padding = TRUE)

tstat_col_cap <- textstat_collocations(toks_news_cap, min_count = 10, tolower = FALSE)
head(tstat_col_cap, 20)
```

```
##           collocation count count_nested length    lambda         z
## 1       David Cameron   860            0      2  8.288932 149.63137
## 2        Donald Trump   774            0      2  8.459635 124.65110
## 3      George Osborne   362            0      2  8.780452 109.09755
## 4     Hillary Clinton   525            0      2  9.226408 104.05605
## 5            New York  1016            0      2 10.580500 101.53958
## 6       Islamic State   330            0      2  9.934794  99.47335
## 7         White House   479            0      2 10.054592  97.46159
## 8      European Union   348            0      2  8.371449  96.20098
## 9       Jeremy Corbyn   244            0      2  8.862147  92.17368
## 10      Boris Johnson   245            0      2  9.796771  85.93115
## 11     Bernie Sanders   394            0      2 10.034026  85.75118
## 12 Guardian Australia   237            0      2  6.460533  85.44272
## 13   Northern Ireland   205            0      2 10.015792  84.37474
## 14        Home Office   216            0      2  9.823115  79.74718
## 15        Ed Miliband   173            0      2  9.984852  79.42691
## 16       Barack Obama   343            0      2  9.892183  78.99228
## 17       South Africa   172            0      2  7.701286  78.97229
## 18           Ted Cruz   417            0      2 10.888985  78.83869
## 19       Black Friday   190            0      2  8.591620  78.00359
## 20     South Carolina   271            0      2  9.537382  77.88448
```

We will only compound strongly associated multi-word expressions here by subsetting `tstat_col_cap` with the z-score (`z > 3`).


```r
toks_comp <- tokens_compound(toks_news, pattern = tstat_col_cap[tstat_col_cap$z > 3,], 
                             case_insensitive = FALSE)
kw_comp <- kwic(toks_comp, pattern = c("London_*", "British_*"))
head(kw_comp, 10)
```

```
## Keyword-in-context with 10 matches.                                                                                               
##     [text9204, 398] researchers publishing | British_Medical_Journal | found drop heart        
##   [text150582, 373]      including Bermuda | British_Virgin_Islands  | Cayman_Islands          
##   [text150582, 663]        included Panama | British_Virgin_Islands  | published               
##  [text120395, 1117]       director general |    British_Chambers     | Commerce said businesses
##    [text64192, 300]   Guardian 90 York_Way |        London_N1        | 9GU Please include      
##  [text145860, 1814]            Association |    British_Insurers     | ABI says insurers       
##   [text148174, 435] EZY5258 Rome Fiumicino |     London_Gatwick      | 29 March delayed        
##    [text109224, 17]            Coast range |    British_Columbia     | Hanging                 
##   [text109224, 115]                  coast |    British_Columbia     | Today however           
##   [text109224, 220]          Alberta coast |    British_Columbia     | plan
```
