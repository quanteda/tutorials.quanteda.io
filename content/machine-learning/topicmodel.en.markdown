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
##       Topic 1      Topic 2    Topic 3      Topic 4      Topic 5 
##  [1,] "world"      "freedom"  "let"        "new"        "must"  
##  [2,] "freedom"    "world"    "peace"      "must"       "new"   
##  [3,] "must"       "liberty"  "world"      "world"      "every" 
##  [4,] "government" "let"      "new"        "let"        "world" 
##  [5,] "citizens"   "country"  "make"       "century"    "change"
##  [6,] "one"        "free"     "government" "time"       "man"   
##  [7,] "time"       "every"    "together"   "government" "now"   
##  [8,] "new"        "citizens" "great"      "work"       "time"  
##  [9,] "history"    "one"      "home"       "today"      "old"   
## [10,] "now"        "hope"     "years"      "together"   "work"  
##       Topic 6      Topic 7     Topic 8   Topic 9    Topic 10  
##  [1,] "government" "new"       "may"     "must"     "free"    
##  [2,] "must"       "world"     "world"   "time"     "world"   
##  [3,] "believe"    "great"     "nations" "make"     "faith"   
##  [4,] "world"      "country"   "peace"   "every"    "peace"   
##  [5,] "one"        "must"      "freedom" "together" "shall"   
##  [6,] "time"       "make"      "seek"    "citizens" "upon"    
##  [7,] "freedom"    "day"       "must"    "country"  "freedom" 
##  [8,] "work"       "today"     "upon"    "one"      "must"    
##  [9,] "man"        "president" "help"    "new"      "strength"
## [10,] "let"        "right"     "justice" "freedom"  "country"
```


```r
knitr::kable(terms(lda, 10))
```



|Topic 1    |Topic 2  |Topic 3    |Topic 4    |Topic 5 |Topic 6    |Topic 7   |Topic 8 |Topic 9  |Topic 10 |
|:----------|:--------|:----------|:----------|:-------|:----------|:---------|:-------|:--------|:--------|
|world      |freedom  |let        |new        |must    |government |new       |may     |must     |free     |
|freedom    |world    |peace      |must       |new     |must       |world     |world   |time     |world    |
|must       |liberty  |world      |world      |every   |believe    |great     |nations |make     |faith    |
|government |let      |new        |let        |world   |world      |country   |peace   |every    |peace    |
|citizens   |country  |make       |century    |change  |one        |must      |freedom |together |shall    |
|one        |free     |government |time       |man     |time       |make      |seek    |citizens |upon     |
|time       |every    |together   |government |now     |freedom    |day       |must    |country  |freedom  |
|new        |citizens |great      |work       |time    |work       |today     |upon    |one      |must     |
|history    |one      |home       |today      |old     |man        |president |help    |new      |strength |
|now        |hope     |years      |together   |work    |let        |right     |justice |freedom  |country  |
