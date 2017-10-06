---
title: Topic models
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
##       Topic 1    Topic 2      Topic 3   Topic 4   Topic 5      Topic 6   
##  [1,] "let"      "freedom"    "new"     "may"     "new"        "free"    
##  [2,] "world"    "liberty"    "world"   "world"   "world"      "world"   
##  [3,] "new"      "government" "great"   "nations" "must"       "faith"   
##  [4,] "peace"    "one"        "free"    "peace"   "century"    "peace"   
##  [5,] "together" "world"      "must"    "freedom" "time"       "shall"   
##  [6,] "know"     "must"       "friends" "seek"    "let"        "upon"    
##  [7,] "now"      "time"       "good"    "must"    "today"      "freedom" 
##  [8,] "earth"    "work"       "things"  "upon"    "government" "must"    
##  [9,] "man"      "every"      "hand"    "help"    "work"       "strength"
## [10,] "spirit"   "believe"    "work"    "justice" "every"      "country" 
##       Topic 7      Topic 8    Topic 9    Topic 10
##  [1,] "world"      "country"  "must"     "new"   
##  [2,] "let"        "citizens" "change"   "must"  
##  [3,] "peace"      "every"    "time"     "every" 
##  [4,] "new"        "new"      "every"    "less"  
##  [5,] "government" "never"    "one"      "let"   
##  [6,] "one"        "many"     "new"      "work"  
##  [7,] "freedom"    "world"    "liberty"  "world" 
##  [8,] "history"    "one"      "world"    "time"  
##  [9,] "time"       "great"    "now"      "today" 
## [10,] "great"      "must"     "together" "common"
```


```r
knitr::kable(terms(lda, 10))
```



|Topic 1  |Topic 2    |Topic 3 |Topic 4 |Topic 5    |Topic 6  |Topic 7    |Topic 8  |Topic 9  |Topic 10 |
|:--------|:----------|:-------|:-------|:----------|:--------|:----------|:--------|:--------|:--------|
|let      |freedom    |new     |may     |new        |free     |world      |country  |must     |new      |
|world    |liberty    |world   |world   |world      |world    |let        |citizens |change   |must     |
|new      |government |great   |nations |must       |faith    |peace      |every    |time     |every    |
|peace    |one        |free    |peace   |century    |peace    |new        |new      |every    |less     |
|together |world      |must    |freedom |time       |shall    |government |never    |one      |let      |
|know     |must       |friends |seek    |let        |upon     |one        |many     |new      |work     |
|now      |time       |good    |must    |today      |freedom  |freedom    |world    |liberty  |world    |
|earth    |work       |things  |upon    |government |must     |history    |one      |world    |time     |
|man      |every      |hand    |help    |work       |strength |time       |great    |now      |today    |
|spirit   |believe    |work    |justice |every      |country  |great      |must     |together |common   |
