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
##       Topic 1    Topic 2      Topic 3      Topic 4    Topic 5     
##  [1,] "citizens" "let"        "government" "freedom"  "new"       
##  [2,] "country"  "peace"      "must"       "world"    "century"   
##  [3,] "story"    "world"      "world"      "country"  "time"      
##  [4,] "must"     "new"        "new"        "every"    "land"      
##  [5,] "every"    "make"       "freedom"    "let"      "every"     
##  [6,] "new"      "government" "strength"   "one"      "government"
##  [7,] "know"     "together"   "believe"    "liberty"  "world"     
##  [8,] "freedom"  "great"      "one"        "citizens" "one"       
##  [9,] "promise"  "home"       "time"       "new"      "promise"   
## [10,] "common"   "years"      "let"        "free"     "must"      
##       Topic 6   Topic 7    Topic 8   Topic 9      Topic 10 
##  [1,] "must"    "free"     "new"     "world"      "world"  
##  [2,] "new"     "world"    "world"   "one"        "must"   
##  [3,] "time"    "must"     "great"   "government" "may"    
##  [4,] "every"   "faith"    "free"    "freedom"    "nations"
##  [5,] "now"     "shall"    "must"    "must"       "freedom"
##  [6,] "work"    "change"   "friends" "time"       "new"    
##  [7,] "today"   "upon"     "good"    "now"        "change" 
##  [8,] "make"    "peace"    "things"  "history"    "time"   
##  [9,] "world"   "freedom"  "hand"    "new"        "today"  
## [10,] "freedom" "strength" "work"    "human"      "peace"
```


```r
knitr::kable(terms(lda, 10))
```



|Topic 1  |Topic 2    |Topic 3    |Topic 4  |Topic 5    |Topic 6 |Topic 7  |Topic 8 |Topic 9    |Topic 10 |
|:--------|:----------|:----------|:--------|:----------|:-------|:--------|:-------|:----------|:--------|
|citizens |let        |government |freedom  |new        |must    |free     |new     |world      |world    |
|country  |peace      |must       |world    |century    |new     |world    |world   |one        |must     |
|story    |world      |world      |country  |time       |time    |must     |great   |government |may      |
|must     |new        |new        |every    |land       |every   |faith    |free    |freedom    |nations  |
|every    |make       |freedom    |let      |every      |now     |shall    |must    |must       |freedom  |
|new      |government |strength   |one      |government |work    |change   |friends |time       |new      |
|know     |together   |believe    |liberty  |world      |today   |upon     |good    |now        |change   |
|freedom  |great      |one        |citizens |one        |make    |peace    |things  |history    |time     |
|promise  |home       |time       |new      |promise    |world   |freedom  |hand    |new        |today    |
|common   |years      |let        |free     |must       |freedom |strength |work    |human      |peace    |
