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
##       Topic 1   Topic 2      Topic 3          Topic 4   Topic 5 
##  [1,] "new"     "world"      "let"            "world"   "world" 
##  [2,] "century" "let"        "new"            "peace"   "must"  
##  [3,] "must"    "freedom"    "world"          "nations" "today" 
##  [4,] "land"    "new"        "peace"          "may"     "new"   
##  [5,] "world"   "one"        "government"     "know"    "change"
##  [6,] "every"   "government" "together"       "earth"   "let"   
##  [7,] "time"    "must"       "responsibility" "freedom" "time"  
##  [8,] "one"     "time"       "great"          "seek"    "work"  
##  [9,] "change"  "peace"      "home"           "new"     "fellow"
## [10,] "union"   "human"      "must"           "now"     "idea"  
##       Topic 6      Topic 7  Topic 8   Topic 9   Topic 10  
##  [1,] "must"       "new"    "freedom" "country" "free"    
##  [2,] "government" "must"   "world"   "one"     "world"   
##  [3,] "time"       "every"  "free"    "every"   "country" 
##  [4,] "believe"    "less"   "liberty" "world"   "faith"   
##  [5,] "one"        "let"    "new"     "great"   "must"    
##  [6,] "freedom"    "work"   "must"    "new"     "freedom" 
##  [7,] "work"       "world"  "time"    "never"   "peace"   
##  [8,] "world"      "time"   "great"   "now"     "citizens"
##  [9,] "make"       "today"  "work"    "back"    "know"    
## [10,] "act"        "common" "day"     "make"    "shall"
```


```r
knitr::kable(terms(lda, 10))
```



|Topic 1 |Topic 2    |Topic 3        |Topic 4 |Topic 5 |Topic 6    |Topic 7 |Topic 8 |Topic 9 |Topic 10 |
|:-------|:----------|:--------------|:-------|:-------|:----------|:-------|:-------|:-------|:--------|
|new     |world      |let            |world   |world   |must       |new     |freedom |country |free     |
|century |let        |new            |peace   |must    |government |must    |world   |one     |world    |
|must    |freedom    |world          |nations |today   |time       |every   |free    |every   |country  |
|land    |new        |peace          |may     |new     |believe    |less    |liberty |world   |faith    |
|world   |one        |government     |know    |change  |one        |let     |new     |great   |must     |
|every   |government |together       |earth   |let     |freedom    |work    |must    |new     |freedom  |
|time    |must       |responsibility |freedom |time    |work       |world   |time    |never   |peace    |
|one     |time       |great          |seek    |work    |world      |time    |great   |now     |citizens |
|change  |peace      |home           |new     |fellow  |make       |today   |work    |back    |know     |
|union   |human      |must           |now     |idea    |act        |common  |day     |make    |shall    |
