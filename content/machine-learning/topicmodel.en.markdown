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
##       Topic 1    Topic 2      Topic 3   Topic 4      Topic 5   
##  [1,] "world"    "government" "freedom" "new"        "free"    
##  [2,] "peace"    "must"       "liberty" "must"       "world"   
##  [3,] "let"      "world"      "must"    "world"      "faith"   
##  [4,] "know"     "time"       "every"   "great"      "peace"   
##  [5,] "make"     "new"        "world"   "good"       "shall"   
##  [6,] "earth"    "work"       "one"     "free"       "upon"    
##  [7,] "now"      "let"        "change"  "freedom"    "freedom" 
##  [8,] "new"      "freedom"    "justice" "strength"   "must"    
##  [9,] "together" "today"      "time"    "government" "strength"
## [10,] "voices"   "now"        "man"     "work"       "country" 
##       Topic 6      Topic 7    Topic 8    Topic 9      Topic 10    
##  [1,] "world"      "must"     "citizens" "new"        "let"       
##  [2,] "freedom"    "time"     "country"  "world"      "world"     
##  [3,] "must"       "make"     "story"    "must"       "new"       
##  [4,] "peace"      "every"    "must"     "century"    "peace"     
##  [5,] "may"        "together" "every"    "time"       "country"   
##  [6,] "one"        "citizens" "new"      "let"        "great"     
##  [7,] "time"       "country"  "know"     "today"      "every"     
##  [8,] "nations"    "one"      "freedom"  "government" "one"       
##  [9,] "government" "new"      "promise"  "work"       "government"
## [10,] "new"        "freedom"  "common"   "every"      "make"
```


```r
knitr::kable(terms(lda, 10))
```



|Topic 1  |Topic 2    |Topic 3 |Topic 4    |Topic 5  |Topic 6    |Topic 7  |Topic 8  |Topic 9    |Topic 10   |
|:--------|:----------|:-------|:----------|:--------|:----------|:--------|:--------|:----------|:----------|
|world    |government |freedom |new        |free     |world      |must     |citizens |new        |let        |
|peace    |must       |liberty |must       |world    |freedom    |time     |country  |world      |world      |
|let      |world      |must    |world      |faith    |must       |make     |story    |must       |new        |
|know     |time       |every   |great      |peace    |peace      |every    |must     |century    |peace      |
|make     |new        |world   |good       |shall    |may        |together |every    |time       |country    |
|earth    |work       |one     |free       |upon     |one        |citizens |new      |let        |great      |
|now      |let        |change  |freedom    |freedom  |time       |country  |know     |today      |every      |
|new      |freedom    |justice |strength   |must     |nations    |one      |freedom  |government |one        |
|together |today      |time    |government |strength |government |new      |promise  |work       |government |
|voices   |now        |man     |work       |country  |new        |freedom  |common   |every      |make       |
