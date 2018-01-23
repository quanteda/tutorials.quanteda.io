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
##       Topic 1   Topic 2      Topic 3  Topic 4    Topic 5     
##  [1,] "may"     "free"       "must"   "world"    "new"       
##  [2,] "nations" "world"      "world"  "peace"    "must"      
##  [3,] "world"   "must"       "change" "let"      "time"      
##  [4,] "peace"   "freedom"    "new"    "know"     "century"   
##  [5,] "freedom" "government" "today"  "make"     "every"     
##  [6,] "seek"    "faith"      "time"   "earth"    "one"       
##  [7,] "must"    "strength"   "let"    "now"      "government"
##  [8,] "upon"    "upon"       "now"    "new"      "citizens"  
##  [9,] "help"    "peace"      "fellow" "together" "together"  
## [10,] "justice" "one"        "work"   "voices"   "world"     
##       Topic 6          Topic 7    Topic 8    Topic 9      Topic 10 
##  [1,] "let"            "new"      "citizens" "world"      "freedom"
##  [2,] "peace"          "world"    "country"  "new"        "let"    
##  [3,] "world"          "must"     "story"    "must"       "world"  
##  [4,] "new"            "together" "must"     "freedom"    "every"  
##  [5,] "responsibility" "country"  "every"    "government" "new"    
##  [6,] "government"     "one"      "new"      "time"       "liberty"
##  [7,] "great"          "strength" "know"     "one"        "time"   
##  [8,] "home"           "now"      "freedom"  "great"      "must"   
##  [9,] "abroad"         "great"    "promise"  "work"       "country"
## [10,] "shall"          "never"    "common"   "history"    "free"
```


```r
knitr::kable(terms(lda, 10))
```



|Topic 1 |Topic 2    |Topic 3 |Topic 4  |Topic 5    |Topic 6        |Topic 7  |Topic 8  |Topic 9    |Topic 10 |
|:-------|:----------|:-------|:--------|:----------|:--------------|:--------|:--------|:----------|:--------|
|may     |free       |must    |world    |new        |let            |new      |citizens |world      |freedom  |
|nations |world      |world   |peace    |must       |peace          |world    |country  |new        |let      |
|world   |must       |change  |let      |time       |world          |must     |story    |must       |world    |
|peace   |freedom    |new     |know     |century    |new            |together |must     |freedom    |every    |
|freedom |government |today   |make     |every      |responsibility |country  |every    |government |new      |
|seek    |faith      |time    |earth    |one        |government     |one      |new      |time       |liberty  |
|must    |strength   |let     |now      |government |great          |strength |know     |one        |time     |
|upon    |upon       |now     |new      |citizens   |home           |now      |freedom  |great      |must     |
|help    |peace      |fellow  |together |together   |abroad         |great    |promise  |work       |country  |
|justice |one        |work    |voices   |world      |shall          |never    |common   |history    |free     |
