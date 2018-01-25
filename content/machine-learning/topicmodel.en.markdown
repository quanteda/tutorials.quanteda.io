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
##       Topic 1   Topic 2    Topic 3      Topic 4   Topic 5      Topic 6   
##  [1,] "world"   "free"     "government" "freedom" "world"      "new"     
##  [2,] "one"     "world"    "world"      "every"   "must"       "let"     
##  [3,] "must"    "faith"    "let"        "liberty" "new"        "century" 
##  [4,] "every"   "peace"    "one"        "world"   "government" "world"   
##  [5,] "change"  "shall"    "peace"      "must"    "time"       "time"    
##  [6,] "great"   "upon"     "now"        "time"    "freedom"    "every"   
##  [7,] "new"     "freedom"  "time"       "work"    "let"        "land"    
##  [8,] "now"     "must"     "man"        "new"     "one"        "citizens"
##  [9,] "country" "strength" "make"       "day"     "today"      "fellow"  
## [10,] "man"     "country"  "earth"      "one"     "now"        "must"    
##       Topic 7   Topic 8          Topic 9    Topic 10    
##  [1,] "world"   "let"            "must"     "new"       
##  [2,] "new"     "peace"          "time"     "must"      
##  [3,] "must"    "world"          "make"     "country"   
##  [4,] "may"     "new"            "every"    "citizens"  
##  [5,] "freedom" "responsibility" "together" "freedom"   
##  [6,] "nations" "government"     "citizens" "spirit"    
##  [7,] "great"   "great"          "country"  "story"     
##  [8,] "free"    "home"           "one"      "government"
##  [9,] "peace"   "abroad"         "new"      "world"     
## [10,] "time"    "years"          "freedom"  "know"
```


```r
knitr::kable(terms(lda, 10))
```



|Topic 1 |Topic 2  |Topic 3    |Topic 4 |Topic 5    |Topic 6  |Topic 7 |Topic 8        |Topic 9  |Topic 10   |
|:-------|:--------|:----------|:-------|:----------|:--------|:-------|:--------------|:--------|:----------|
|world   |free     |government |freedom |world      |new      |world   |let            |must     |new        |
|one     |world    |world      |every   |must       |let      |new     |peace          |time     |must       |
|must    |faith    |let        |liberty |new        |century  |must    |world          |make     |country    |
|every   |peace    |one        |world   |government |world    |may     |new            |every    |citizens   |
|change  |shall    |peace      |must    |time       |time     |freedom |responsibility |together |freedom    |
|great   |upon     |now        |time    |freedom    |every    |nations |government     |citizens |spirit     |
|new     |freedom  |time       |work    |let        |land     |great   |great          |country  |story      |
|now     |must     |man        |new     |one        |citizens |free    |home           |one      |government |
|country |strength |make       |day     |today      |fellow   |peace   |abroad         |new      |world      |
|man     |country  |earth      |one     |now        |must     |time    |years          |freedom  |know       |
