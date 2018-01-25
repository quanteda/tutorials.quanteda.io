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
##       Topic 1   Topic 2      Topic 3   Topic 4    Topic 5 Topic 6     
##  [1,] "must"    "government" "freedom" "free"     "world" "must"      
##  [2,] "may"     "world"      "liberty" "world"    "new"   "world"     
##  [3,] "world"   "must"       "must"    "faith"    "know"  "new"       
##  [4,] "freedom" "one"        "every"   "peace"    "make"  "let"       
##  [5,] "nations" "freedom"    "world"   "shall"    "great" "today"     
##  [6,] "time"    "time"       "one"     "upon"     "peace" "time"      
##  [7,] "peace"   "now"        "change"  "freedom"  "need"  "change"    
##  [8,] "new"     "history"    "justice" "must"     "let"   "together"  
##  [9,] "seek"    "let"        "time"    "strength" "time"  "government"
## [10,] "one"     "believe"    "man"     "country"  "today" "work"      
##       Topic 7    Topic 8    Topic 9          Topic 10
##  [1,] "new"      "country"  "let"            "new"   
##  [2,] "let"      "citizens" "peace"          "must"  
##  [3,] "century"  "every"    "world"          "every" 
##  [4,] "world"    "new"      "new"            "less"  
##  [5,] "time"     "never"    "responsibility" "let"   
##  [6,] "every"    "many"     "government"     "work"  
##  [7,] "citizens" "world"    "great"          "world" 
##  [8,] "land"     "one"      "home"           "time"  
##  [9,] "fellow"   "great"    "abroad"         "today" 
## [10,] "one"      "must"     "better"         "common"
```


```r
knitr::kable(terms(lda, 10))
```



|Topic 1 |Topic 2    |Topic 3 |Topic 4  |Topic 5 |Topic 6    |Topic 7  |Topic 8  |Topic 9        |Topic 10 |
|:-------|:----------|:-------|:--------|:-------|:----------|:--------|:--------|:--------------|:--------|
|must    |government |freedom |free     |world   |must       |new      |country  |let            |new      |
|may     |world      |liberty |world    |new     |world      |let      |citizens |peace          |must     |
|world   |must       |must    |faith    |know    |new        |century  |every    |world          |every    |
|freedom |one        |every   |peace    |make    |let        |world    |new      |new            |less     |
|nations |freedom    |world   |shall    |great   |today      |time     |never    |responsibility |let      |
|time    |time       |one     |upon     |peace   |time       |every    |many     |government     |work     |
|peace   |now        |change  |freedom  |need    |change     |citizens |world    |great          |world    |
|new     |history    |justice |must     |let     |together   |land     |one      |home           |time     |
|seek    |let        |time    |strength |time    |government |fellow   |great    |abroad         |today    |
|one     |believe    |man     |country  |today   |work       |one      |must     |better         |common   |
