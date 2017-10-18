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
##       Topic 1   Topic 2   Topic 3      Topic 4    Topic 5     
##  [1,] "may"     "freedom" "new"        "free"     "government"
##  [2,] "world"   "liberty" "must"       "world"    "world"     
##  [3,] "nations" "must"    "time"       "faith"    "let"       
##  [4,] "peace"   "world"   "century"    "peace"    "one"       
##  [5,] "freedom" "one"     "every"      "shall"    "peace"     
##  [6,] "seek"    "human"   "one"        "upon"     "now"       
##  [7,] "must"    "every"   "government" "freedom"  "time"      
##  [8,] "upon"    "new"     "citizens"   "must"     "man"       
##  [9,] "help"    "know"    "world"      "strength" "make"      
## [10,] "justice" "country" "together"   "country"  "earth"     
##       Topic 6      Topic 7          Topic 8  Topic 9  Topic 10  
##  [1,] "world"      "let"            "must"   "new"    "country" 
##  [2,] "let"        "peace"          "world"  "must"   "citizens"
##  [3,] "freedom"    "world"          "new"    "world"  "every"   
##  [4,] "new"        "new"            "today"  "great"  "new"     
##  [5,] "one"        "responsibility" "let"    "man"    "never"   
##  [6,] "government" "government"     "time"   "old"    "many"    
##  [7,] "must"       "home"           "work"   "change" "world"   
##  [8,] "time"       "great"          "every"  "time"   "one"     
##  [9,] "peace"      "abroad"         "now"    "work"   "great"   
## [10,] "human"      "better"         "change" "day"    "must"
```


```r
knitr::kable(terms(lda, 10))
```



|Topic 1 |Topic 2 |Topic 3    |Topic 4  |Topic 5    |Topic 6    |Topic 7        |Topic 8 |Topic 9 |Topic 10 |
|:-------|:-------|:----------|:--------|:----------|:----------|:--------------|:-------|:-------|:--------|
|may     |freedom |new        |free     |government |world      |let            |must    |new     |country  |
|world   |liberty |must       |world    |world      |let        |peace          |world   |must    |citizens |
|nations |must    |time       |faith    |let        |freedom    |world          |new     |world   |every    |
|peace   |world   |century    |peace    |one        |new        |new            |today   |great   |new      |
|freedom |one     |every      |shall    |peace      |one        |responsibility |let     |man     |never    |
|seek    |human   |one        |upon     |now        |government |government     |time    |old     |many     |
|must    |every   |government |freedom  |time       |must       |home           |work    |change  |world    |
|upon    |new     |citizens   |must     |man        |time       |great          |every   |time    |one      |
|help    |know    |world      |strength |make       |peace      |abroad         |now     |work    |great    |
|justice |country |together   |country  |earth      |human      |better         |change  |day     |must     |
