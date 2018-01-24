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
##       Topic 1   Topic 2    Topic 3   Topic 4    Topic 5    Topic 6     
##  [1,] "world"   "new"      "change"  "freedom"  "must"     "government"
##  [2,] "free"    "world"    "must"    "every"    "make"     "must"      
##  [3,] "peace"   "must"     "man"     "must"     "world"    "believe"   
##  [4,] "nations" "together" "union"   "country"  "time"     "world"     
##  [5,] "freedom" "country"  "world"   "liberty"  "peace"    "one"       
##  [6,] "may"     "one"      "old"     "time"     "together" "time"      
##  [7,] "must"    "strength" "land"    "world"    "new"      "freedom"   
##  [8,] "upon"    "now"      "every"   "citizens" "now"      "work"      
##  [9,] "shall"   "great"    "liberty" "work"     "know"     "man"       
## [10,] "faith"   "never"    "justice" "new"      "let"      "let"       
##       Topic 7  Topic 8   Topic 9      Topic 10    
##  [1,] "world"  "let"     "world"      "new"       
##  [2,] "must"   "world"   "new"        "let"       
##  [3,] "today"  "new"     "must"       "world"     
##  [4,] "new"    "sides"   "freedom"    "century"   
##  [5,] "change" "shall"   "government" "government"
##  [6,] "let"    "pledge"  "time"       "peace"     
##  [7,] "time"   "nations" "one"        "time"      
##  [8,] "work"   "power"   "great"      "every"     
##  [9,] "fellow" "ask"     "work"       "one"       
## [10,] "idea"   "free"    "history"    "great"
```


```r
knitr::kable(terms(lda, 10))
```



|Topic 1 |Topic 2  |Topic 3 |Topic 4  |Topic 5  |Topic 6    |Topic 7 |Topic 8 |Topic 9    |Topic 10   |
|:-------|:--------|:-------|:--------|:--------|:----------|:-------|:-------|:----------|:----------|
|world   |new      |change  |freedom  |must     |government |world   |let     |world      |new        |
|free    |world    |must    |every    |make     |must       |must    |world   |new        |let        |
|peace   |must     |man     |must     |world    |believe    |today   |new     |must       |world      |
|nations |together |union   |country  |time     |world      |new     |sides   |freedom    |century    |
|freedom |country  |world   |liberty  |peace    |one        |change  |shall   |government |government |
|may     |one      |old     |time     |together |time       |let     |pledge  |time       |peace      |
|must    |strength |land    |world    |new      |freedom    |time    |nations |one        |time       |
|upon    |now      |every   |citizens |now      |work       |work    |power   |great      |every      |
|shall   |great    |liberty |work     |know     |man        |fellow  |ask     |work       |one        |
|faith   |never    |justice |new      |let      |let        |idea    |free    |history    |great      |
