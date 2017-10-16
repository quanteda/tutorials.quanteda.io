---
title: Topic models
draft: true
---




```r
require(topicmodels)
## Loading required package: topicmodels
mycorpus <- corpus_subset(data_corpus_inaugural, Year > 1950)
quantdfm <- dfm(mycorpus, verbose = FALSE, remove_punct = TRUE,
                remove = c(stopwords('english'), 'will', 'us', 'nation', 'can', 'peopl*', 'americ*'))
ldadfm <- convert(quantdfm, to = "topicmodels")
lda <- LDA(ldadfm, control = list(alpha = 0.1), k = 10)
terms(lda, 10)
##       Topic 1      Topic 2    Topic 3          Topic 4      Topic 5   
##  [1,] "must"       "world"    "let"            "new"        "must"    
##  [2,] "government" "now"      "new"            "world"      "time"    
##  [3,] "world"      "make"     "world"          "must"       "make"    
##  [4,] "time"       "one"      "peace"          "century"    "every"   
##  [5,] "new"        "new"      "government"     "time"       "together"
##  [6,] "let"        "let"      "together"       "let"        "citizens"
##  [7,] "work"       "peace"    "responsibility" "every"      "country" 
##  [8,] "today"      "together" "great"          "today"      "one"     
##  [9,] "freedom"    "earth"    "home"           "government" "new"     
## [10,] "now"        "great"    "must"           "work"       "freedom" 
##       Topic 6      Topic 7  Topic 8   Topic 9    Topic 10  
##  [1,] "world"      "new"    "may"     "freedom"  "let"     
##  [2,] "free"       "must"   "world"   "liberty"  "citizens"
##  [3,] "freedom"    "world"  "nations" "every"    "country" 
##  [4,] "must"       "great"  "peace"   "one"      "new"     
##  [5,] "peace"      "man"    "freedom" "country"  "world"   
##  [6,] "one"        "old"    "seek"    "world"    "power"   
##  [7,] "faith"      "change" "must"    "history"  "freedom" 
##  [8,] "history"    "time"   "upon"    "free"     "pledge"  
##  [9,] "government" "work"   "help"    "time"     "story"   
## [10,] "time"       "day"    "justice" "citizens" "know"
```


```r
knitr::kable(terms(lda, 10))
```



|Topic 1    |Topic 2  |Topic 3        |Topic 4    |Topic 5  |Topic 6    |Topic 7 |Topic 8 |Topic 9  |Topic 10 |
|:----------|:--------|:--------------|:----------|:--------|:----------|:-------|:-------|:--------|:--------|
|must       |world    |let            |new        |must     |world      |new     |may     |freedom  |let      |
|government |now      |new            |world      |time     |free       |must    |world   |liberty  |citizens |
|world      |make     |world          |must       |make     |freedom    |world   |nations |every    |country  |
|time       |one      |peace          |century    |every    |must       |great   |peace   |one      |new      |
|new        |new      |government     |time       |together |peace      |man     |freedom |country  |world    |
|let        |let      |together       |let        |citizens |one        |old     |seek    |world    |power    |
|work       |peace    |responsibility |every      |country  |faith      |change  |must    |history  |freedom  |
|today      |together |great          |today      |one      |history    |time    |upon    |free     |pledge   |
|freedom    |earth    |home           |government |new      |government |work    |help    |time     |story    |
|now        |great    |must           |work       |freedom  |time       |day     |justice |citizens |know     |
