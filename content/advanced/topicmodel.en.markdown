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
##       Topic 1      Topic 2      Topic 3      Topic 4   Topic 5     
##  [1,] "new"        "new"        "new"        "freedom" "government"
##  [2,] "world"      "century"    "must"       "every"   "must"      
##  [3,] "let"        "time"       "country"    "liberty" "believe"   
##  [4,] "peace"      "land"       "citizens"   "world"   "world"     
##  [5,] "great"      "every"      "freedom"    "must"    "one"       
##  [6,] "government" "government" "spirit"     "time"    "time"      
##  [7,] "make"       "world"      "story"      "work"    "freedom"   
##  [8,] "time"       "one"        "government" "new"     "work"      
##  [9,] "today"      "promise"    "world"      "day"     "man"       
## [10,] "better"     "must"       "know"       "one"     "let"       
##       Topic 6   Topic 7  Topic 8 Topic 9      Topic 10  
##  [1,] "world"   "let"    "world" "world"      "must"    
##  [2,] "free"    "world"  "let"   "one"        "country" 
##  [3,] "peace"   "man"    "must"  "government" "one"     
##  [4,] "nations" "change" "new"   "freedom"    "every"   
##  [5,] "freedom" "new"    "today" "must"       "make"    
##  [6,] "may"     "must"   "now"   "time"       "time"    
##  [7,] "must"    "union"  "make"  "now"        "new"     
##  [8,] "upon"    "fellow" "time"  "history"    "now"     
##  [9,] "shall"   "shall"  "peace" "new"        "together"
## [10,] "faith"   "every"  "know"  "human"      "citizens"
```


```r
knitr::kable(terms(lda, 10))
```



|Topic 1    |Topic 2    |Topic 3    |Topic 4 |Topic 5    |Topic 6 |Topic 7 |Topic 8 |Topic 9    |Topic 10 |
|:----------|:----------|:----------|:-------|:----------|:-------|:-------|:-------|:----------|:--------|
|new        |new        |new        |freedom |government |world   |let     |world   |world      |must     |
|world      |century    |must       |every   |must       |free    |world   |let     |one        |country  |
|let        |time       |country    |liberty |believe    |peace   |man     |must    |government |one      |
|peace      |land       |citizens   |world   |world      |nations |change  |new     |freedom    |every    |
|great      |every      |freedom    |must    |one        |freedom |new     |today   |must       |make     |
|government |government |spirit     |time    |time       |may     |must    |now     |time       |time     |
|make       |world      |story      |work    |freedom    |must    |union   |make    |now        |new      |
|time       |one        |government |new     |work       |upon    |fellow  |time    |history    |now      |
|today      |promise    |world      |day     |man        |shall   |shall   |peace   |new        |together |
|better     |must       |know       |one     |let        |faith   |every   |know    |human      |citizens |
