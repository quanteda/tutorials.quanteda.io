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
<<<<<<< HEAD
##       Topic 1      Topic 2      Topic 3   Topic 4          Topic 5   
##  [1,] "must"       "new"        "must"    "let"            "free"    
##  [2,] "world"      "must"       "may"     "new"            "world"   
##  [3,] "government" "strength"   "world"   "world"          "must"    
##  [4,] "time"       "together"   "freedom" "peace"          "freedom" 
##  [5,] "today"      "spirit"     "nations" "every"          "peace"   
##  [6,] "let"        "world"      "time"    "responsibility" "faith"   
##  [7,] "work"       "human"      "peace"   "government"     "new"     
##  [8,] "freedom"    "dream"      "new"     "time"           "strength"
##  [9,] "new"        "government" "seek"    "better"         "know"    
## [10,] "believe"    "freedom"    "one"     "today"          "work"    
##       Topic 6      Topic 7    Topic 8    Topic 9      Topic 10  
##  [1,] "world"      "freedom"  "country"  "new"        "let"     
##  [2,] "must"       "liberty"  "citizens" "century"    "world"   
##  [3,] "one"        "every"    "every"    "time"       "peace"   
##  [4,] "freedom"    "one"      "new"      "land"       "new"     
##  [5,] "now"        "country"  "never"    "every"      "know"    
##  [6,] "time"       "world"    "many"     "government" "earth"   
##  [7,] "new"        "history"  "story"    "world"      "man"     
##  [8,] "union"      "free"     "great"    "one"        "now"     
##  [9,] "government" "time"     "must"     "promise"    "make"    
## [10,] "man"        "citizens" "world"    "must"       "together"
=======
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
>>>>>>> b59f8d5e7707795f3ee8a9243174ca7b91d14b59
```


```r
knitr::kable(terms(lda, 10))
```



<<<<<<< HEAD
|Topic 1    |Topic 2    |Topic 3 |Topic 4        |Topic 5  |Topic 6    |Topic 7  |Topic 8  |Topic 9    |Topic 10 |
|:----------|:----------|:-------|:--------------|:--------|:----------|:--------|:--------|:----------|:--------|
|must       |new        |must    |let            |free     |world      |freedom  |country  |new        |let      |
|world      |must       |may     |new            |world    |must       |liberty  |citizens |century    |world    |
|government |strength   |world   |world          |must     |one        |every    |every    |time       |peace    |
|time       |together   |freedom |peace          |freedom  |freedom    |one      |new      |land       |new      |
|today      |spirit     |nations |every          |peace    |now        |country  |never    |every      |know     |
|let        |world      |time    |responsibility |faith    |time       |world    |many     |government |earth    |
|work       |human      |peace   |government     |new      |new        |history  |story    |world      |man      |
|freedom    |dream      |new     |time           |strength |union      |free     |great    |one        |now      |
|new        |government |seek    |better         |know     |government |time     |must     |promise    |make     |
|believe    |freedom    |one     |today          |work     |man        |citizens |world    |must       |together |
=======
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
>>>>>>> b59f8d5e7707795f3ee8a9243174ca7b91d14b59
