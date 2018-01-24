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
##       Topic 1          Topic 2  Topic 3   Topic 4    Topic 5     
##  [1,] "peace"          "must"   "must"    "freedom"  "let"       
##  [2,] "world"          "world"  "may"     "liberty"  "government"
##  [3,] "let"            "change" "world"   "every"    "world"     
##  [4,] "free"           "new"    "freedom" "one"      "freedom"   
##  [5,] "shall"          "today"  "nations" "country"  "must"      
##  [6,] "faith"          "time"   "time"    "world"    "believe"   
##  [7,] "new"            "let"    "peace"   "history"  "time"      
##  [8,] "responsibility" "now"    "new"     "free"     "new"       
##  [9,] "freedom"        "fellow" "seek"    "time"     "man"       
## [10,] "history"        "work"   "one"     "citizens" "one"       
##       Topic 6   Topic 7      Topic 8      Topic 9  Topic 10    
##  [1,] "country" "world"      "new"        "new"    "new"       
##  [2,] "one"     "know"       "must"       "must"   "world"     
##  [3,] "every"   "new"        "world"      "every"  "one"       
##  [4,] "world"   "peace"      "great"      "less"   "government"
##  [5,] "great"   "make"       "good"       "let"    "time"      
##  [6,] "new"     "now"        "free"       "work"   "must"      
##  [7,] "never"   "let"        "freedom"    "world"  "century"   
##  [8,] "now"     "citizens"   "strength"   "time"   "every"     
##  [9,] "back"    "country"    "government" "today"  "now"       
## [10,] "make"    "government" "work"       "common" "let"
```


```r
knitr::kable(terms(lda, 10))
```



|Topic 1        |Topic 2 |Topic 3 |Topic 4  |Topic 5    |Topic 6 |Topic 7    |Topic 8    |Topic 9 |Topic 10   |
|:--------------|:-------|:-------|:--------|:----------|:-------|:----------|:----------|:-------|:----------|
|peace          |must    |must    |freedom  |let        |country |world      |new        |new     |new        |
|world          |world   |may     |liberty  |government |one     |know       |must       |must    |world      |
|let            |change  |world   |every    |world      |every   |new        |world      |every   |one        |
|free           |new     |freedom |one      |freedom    |world   |peace      |great      |less    |government |
|shall          |today   |nations |country  |must       |great   |make       |good       |let     |time       |
|faith          |time    |time    |world    |believe    |new     |now        |free       |work    |must       |
|new            |let     |peace   |history  |time       |never   |let        |freedom    |world   |century    |
|responsibility |now     |new     |free     |new        |now     |citizens   |strength   |time    |every      |
|freedom        |fellow  |seek    |time     |man        |back    |country    |government |today   |now        |
|history        |work    |one     |citizens |one        |make    |government |work       |common  |let        |
