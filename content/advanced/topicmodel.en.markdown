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
##       Topic 1    Topic 2   Topic 3   Topic 4      Topic 5     
##  [1,] "new"      "freedom" "world"   "world"      "government"
##  [2,] "must"     "must"    "free"    "one"        "must"      
##  [3,] "every"    "world"   "peace"   "government" "believe"   
##  [4,] "country"  "new"     "nations" "freedom"    "world"     
##  [5,] "common"   "liberty" "freedom" "must"       "one"       
##  [6,] "citizens" "time"    "may"     "time"       "time"      
##  [7,] "work"     "work"    "must"    "now"        "freedom"   
##  [8,] "time"     "today"   "upon"    "history"    "work"      
##  [9,] "know"     "every"   "shall"   "new"        "man"       
## [10,] "world"    "let"     "faith"   "human"      "let"       
##       Topic 6      Topic 7 Topic 8    Topic 9   Topic 10    
##  [1,] "new"        "world" "let"      "change"  "let"       
##  [2,] "century"    "new"   "world"    "must"    "peace"     
##  [3,] "time"       "great" "sides"    "man"     "new"       
##  [4,] "land"       "make"  "new"      "union"   "world"     
##  [5,] "every"      "one"   "pledge"   "world"   "must"      
##  [6,] "government" "know"  "ask"      "old"     "time"      
##  [7,] "world"      "today" "citizens" "land"    "government"
##  [8,] "one"        "now"   "power"    "every"   "make"      
##  [9,] "promise"    "let"   "shall"    "liberty" "every"     
## [10,] "must"       "peace" "free"     "justice" "together"
```


```r
knitr::kable(terms(lda, 10))
```



|Topic 1  |Topic 2 |Topic 3 |Topic 4    |Topic 5    |Topic 6    |Topic 7 |Topic 8  |Topic 9 |Topic 10   |
|:--------|:-------|:-------|:----------|:----------|:----------|:-------|:--------|:-------|:----------|
|new      |freedom |world   |world      |government |new        |world   |let      |change  |let        |
|must     |must    |free    |one        |must       |century    |new     |world    |must    |peace      |
|every    |world   |peace   |government |believe    |time       |great   |sides    |man     |new        |
|country  |new     |nations |freedom    |world      |land       |make    |new      |union   |world      |
|common   |liberty |freedom |must       |one        |every      |one     |pledge   |world   |must       |
|citizens |time    |may     |time       |time       |government |know    |ask      |old     |time       |
|work     |work    |must    |now        |freedom    |world      |today   |citizens |land    |government |
|time     |today   |upon    |history    |work       |one        |now     |power    |every   |make       |
|know     |every   |shall   |new        |man        |promise    |let     |shall    |liberty |every      |
|world    |let     |faith   |human      |let        |must       |peace   |free     |justice |together   |
