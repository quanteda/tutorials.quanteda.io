---
title: Compound multi-word expressions
weight: 20
draft: false
---

[Earlier in this tutorial](/basic-operations/tokens/tokens_compound), you compounded multi-word expressions that you already knew about, such as "asylum seeker", by naming them directly. In real text, though, you rarely know every meaningful phrase in advance. Here we discover multi-word expressions automatically, through collocation analysis, before compounding them. In this example, we will identify sequences of capitalised words and compound them as proper names, a common type of multi-word expression in newspaper articles.


``` r
library(quanteda)
library(quanteda.textstats)
library(quanteda.corpora)
```



The corpus contains 6,000 Guardian news articles from 2012 to 2016. As in the [fcm chapter](/basic-operations/fcm/fcm), it is normally retrieved with `download()`, but here we load an already-downloaded copy for this website.


``` r
corp_news <- download("data_corpus_guardian")
```



We remove punctuation marks and symbols in `tokens()` and stopwords in `tokens_remove()`, but this time with `padding = TRUE`, which you met in the [Basic Operations chapter](/basic-operations/tokens/tokens_select). Padding matters here because collocation analysis needs to know which words were originally next to each other; without it, two words that were never actually adjacent could end up looking adjacent once the words between them are deleted.


``` r
toks_news <- tokens(corp_news, remove_punct = TRUE, remove_symbols = TRUE, padding = TRUE) |> 
    tokens_remove(stopwords("en"), padding = TRUE)
```

One of the most common types of multi-word expression is proper names, which we can select based on capitalisation in English texts. `tokens_select()` here keeps only tokens starting with a capital letter, and `textstat_collocations()` then measures, for every pair of adjacent capitalised words, how much more often they occur together than you would expect if they occurred independently.


``` r
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
## 1       David Cameron   860            0      2  8.289184 149.63592
## 2        Donald Trump   774            0      2  8.459887 124.65481
## 3      George Osborne   362            0      2  8.780704 109.10068
## 4     Hillary Clinton   525            0      2  9.226660 104.05889
## 5            New York  1016            0      2 10.580752 101.54199
## 6       Islamic State   330            0      2  9.935046  99.47587
## 7         White House   479            0      2 10.054844  97.46403
## 8      European Union   348            0      2  8.371700  96.20388
## 9       Jeremy Corbyn   244            0      2  8.862399  92.17630
## 10      Boris Johnson   245            0      2  9.797023  85.93335
## 11     Bernie Sanders   394            0      2 10.034278  85.75333
## 12 Guardian Australia   237            0      2  6.460785  85.44605
## 13   Northern Ireland   205            0      2 10.016044  84.37686
## 14        Home Office   216            0      2  9.823367  79.74922
## 15        Ed Miliband   173            0      2  9.985104  79.42891
## 16       Barack Obama   343            0      2  9.892435  78.99429
## 17       South Africa   172            0      2  7.701538  78.97487
## 18           Ted Cruz   417            0      2 10.889237  78.84051
## 19       Black Friday   190            0      2  8.591872  78.00588
## 20     South Carolina   271            0      2  9.537634  77.88654
```

The `z` column is a statistical score: the higher it is, the more confident we can be that the two words form a genuine collocation rather than appearing together by chance. We will only compound strongly associated multi-word expressions here by subsetting `tstat_col_cap` to keep just those with `z > 3`, a common threshold for a strong association.


``` r
toks_comp <- tokens_compound(toks_news, pattern = tstat_col_cap[tstat_col_cap$z > 3,],
                             case_insensitive = FALSE)
kw_comp <- kwic(toks_comp, pattern = c("London_*", "British_*"))
head(kw_comp, 10)
```

```
## Keyword-in-context with 10 matches.                                                                                                 
##     [text9204, 398]  researchers publishing | British_Medical_Journal | found drop heart         
##   [text150582, 373]       including Bermuda | British_Virgin_Islands  |  Cayman_Islands          
##   [text150582, 663]         included Panama | British_Virgin_Islands  |  published               
##  [text120395, 1117]        director general |    British_Chambers     |  Commerce said businesses
##    [text64192, 300]    Guardian 90 York_Way |        London_N1        | 9GU Please include       
##  [text145860, 1814]             Association |    British_Insurers     |  ABI says insurers       
##   [text148174, 435]  EZY5258 Rome Fiumicino |     London_Gatwick      |  29 March delayed        
##    [text109224, 17]             Coast range |    British_Columbia     |  Hanging                 
##   [text109224, 115]                   coast |    British_Columbia     |  Today however           
##   [text109224, 220]           Alberta coast |    British_Columbia     |  plan
```

Notice that `pattern` here is the `tstat_col_cap` collocations table itself, not a character vector wrapped in `phrase()`, as it was when you [compounded known phrases by hand earlier in this tutorial](/basic-operations/tokens/tokens_compound). `textstat_collocations()` already records each candidate as a multi-word sequence, so `tokens_compound()` recognises it as one directly; `phrase()` is only needed when you type a multi-word pattern yourself as plain text.

The keyword-in-context output confirms that phrases such as "London Gatwick" and "British Columbia" have been compounded into single tokens, joined by an underscore, just as before. The difference is that this time, `textstat_collocations()` found the phrases for us, based purely on how often capitalised words occurred next to each other in the corpus.
