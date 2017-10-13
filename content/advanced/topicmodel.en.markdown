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
##       Topic 1      Topic 2    Topic 3    Topic 4  Topic 5   Topic 6  
##  [1,] "world"      "world"    "freedom"  "must"   "country" "new"    
##  [2,] "must"       "peace"    "liberty"  "world"  "one"     "must"   
##  [3,] "one"        "let"      "every"    "time"   "every"   "world"  
##  [4,] "freedom"    "know"     "one"      "new"    "world"   "work"   
##  [5,] "now"        "make"     "country"  "today"  "great"   "time"   
##  [6,] "time"       "earth"    "world"    "change" "new"     "know"   
##  [7,] "new"        "now"      "history"  "let"    "never"   "today"  
##  [8,] "union"      "new"      "free"     "make"   "now"     "every"  
##  [9,] "government" "together" "time"     "work"   "back"    "country"
## [10,] "man"        "voices"   "citizens" "every"  "make"    "day"    
##       Topic 7   Topic 8      Topic 9    Topic 10    
##  [1,] "world"   "government" "free"     "new"       
##  [2,] "nations" "must"       "world"    "let"       
##  [3,] "may"     "world"      "faith"    "world"     
##  [4,] "let"     "new"        "peace"    "century"   
##  [5,] "peace"   "freedom"    "shall"    "government"
##  [6,] "freedom" "strength"   "upon"     "peace"     
##  [7,] "seek"    "believe"    "freedom"  "time"      
##  [8,] "new"     "one"        "must"     "every"     
##  [9,] "power"   "time"       "strength" "one"       
## [10,] "free"    "let"        "country"  "great"
```


```r
knitr::kable(terms(lda, 10))
```



|Topic 1    |Topic 2  |Topic 3  |Topic 4 |Topic 5 |Topic 6 |Topic 7 |Topic 8    |Topic 9  |Topic 10   |
|:----------|:--------|:--------|:-------|:-------|:-------|:-------|:----------|:--------|:----------|
|world      |world    |freedom  |must    |country |new     |world   |government |free     |new        |
|must       |peace    |liberty  |world   |one     |must    |nations |must       |world    |let        |
|one        |let      |every    |time    |every   |world   |may     |world      |faith    |world      |
|freedom    |know     |one      |new     |world   |work    |let     |new        |peace    |century    |
|now        |make     |country  |today   |great   |time    |peace   |freedom    |shall    |government |
|time       |earth    |world    |change  |new     |know    |freedom |strength   |upon     |peace      |
|new        |now      |history  |let     |never   |today   |seek    |believe    |freedom  |time       |
|union      |new      |free     |make    |now     |every   |new     |one        |must     |every      |
|government |together |time     |work    |back    |country |power   |time       |strength |one        |
|man        |voices   |citizens |every   |make    |day     |free    |let        |country  |great      |
