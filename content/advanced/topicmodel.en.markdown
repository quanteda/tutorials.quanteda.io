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
##       Topic 1      Topic 2   Topic 3      Topic 4   Topic 5  Topic 6 
##  [1,] "new"        "new"     "government" "freedom" "let"    "new"   
##  [2,] "world"      "great"   "world"      "liberty" "world"  "must"  
##  [3,] "must"       "world"   "one"        "must"    "man"    "every" 
##  [4,] "century"    "things"  "must"       "world"   "change" "less"  
##  [5,] "time"       "must"    "freedom"    "one"     "new"    "let"   
##  [6,] "let"        "free"    "time"       "human"   "must"   "work"  
##  [7,] "today"      "good"    "now"        "new"     "union"  "world" 
##  [8,] "government" "friends" "history"    "every"   "fellow" "time"  
##  [9,] "work"       "hand"    "let"        "country" "shall"  "today" 
## [10,] "every"      "time"    "god"        "time"    "every"  "common"
##       Topic 7      Topic 8    Topic 9   Topic 10  
##  [1,] "let"        "country"  "may"     "must"    
##  [2,] "peace"      "free"     "world"   "time"    
##  [3,] "world"      "world"    "peace"   "make"    
##  [4,] "new"        "every"    "nations" "every"   
##  [5,] "make"       "must"     "freedom" "together"
##  [6,] "government" "faith"    "must"    "citizens"
##  [7,] "together"   "citizens" "seek"    "country" 
##  [8,] "great"      "never"    "help"    "one"     
##  [9,] "home"       "one"      "upon"    "new"     
## [10,] "years"      "make"     "free"    "freedom"
```


```r
knitr::kable(terms(lda, 10))
```



|Topic 1    |Topic 2 |Topic 3    |Topic 4 |Topic 5 |Topic 6 |Topic 7    |Topic 8  |Topic 9 |Topic 10 |
|:----------|:-------|:----------|:-------|:-------|:-------|:----------|:--------|:-------|:--------|
|new        |new     |government |freedom |let     |new     |let        |country  |may     |must     |
|world      |great   |world      |liberty |world   |must    |peace      |free     |world   |time     |
|must       |world   |one        |must    |man     |every   |world      |world    |peace   |make     |
|century    |things  |must       |world   |change  |less    |new        |every    |nations |every    |
|time       |must    |freedom    |one     |new     |let     |make       |must     |freedom |together |
|let        |free    |time       |human   |must    |work    |government |faith    |must    |citizens |
|today      |good    |now        |new     |union   |world   |together   |citizens |seek    |country  |
|government |friends |history    |every   |fellow  |time    |great      |never    |help    |one      |
|work       |hand    |let        |country |shall   |today   |home       |one      |upon    |new      |
|every      |time    |god        |time    |every   |common  |years      |make     |free    |freedom  |
