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
##       Topic 1   Topic 2      Topic 3          Topic 4   Topic 5   
##  [1,] "world"   "new"        "world"          "let"     "free"    
##  [2,] "nations" "must"       "let"            "new"     "world"   
##  [3,] "freedom" "strength"   "new"            "world"   "faith"   
##  [4,] "may"     "together"   "must"           "every"   "peace"   
##  [5,] "must"    "spirit"     "peace"          "country" "shall"   
##  [6,] "peace"   "world"      "today"          "now"     "upon"    
##  [7,] "seek"    "human"      "government"     "power"   "freedom" 
##  [8,] "country" "dream"      "time"           "today"   "must"    
##  [9,] "know"    "government" "responsibility" "must"    "strength"
## [10,] "story"   "freedom"    "great"          "one"     "country" 
##       Topic 6      Topic 7      Topic 8      Topic 9 Topic 10  
##  [1,] "must"       "world"      "new"        "world" "freedom" 
##  [2,] "government" "one"        "century"    "new"   "must"    
##  [3,] "world"      "government" "time"       "know"  "liberty" 
##  [4,] "man"        "freedom"    "land"       "make"  "time"    
##  [5,] "believe"    "must"       "every"      "great" "every"   
##  [6,] "one"        "time"       "government" "peace" "one"     
##  [7,] "time"       "now"        "world"      "need"  "country" 
##  [8,] "change"     "history"    "one"        "let"   "citizens"
##  [9,] "work"       "new"        "promise"    "time"  "world"   
## [10,] "freedom"    "human"      "must"       "today" "make"
```


```r
knitr::kable(terms(lda, 10))
```



|Topic 1 |Topic 2    |Topic 3        |Topic 4 |Topic 5  |Topic 6    |Topic 7    |Topic 8    |Topic 9 |Topic 10 |
|:-------|:----------|:--------------|:-------|:--------|:----------|:----------|:----------|:-------|:--------|
|world   |new        |world          |let     |free     |must       |world      |new        |world   |freedom  |
|nations |must       |let            |new     |world    |government |one        |century    |new     |must     |
|freedom |strength   |new            |world   |faith    |world      |government |time       |know    |liberty  |
|may     |together   |must           |every   |peace    |man        |freedom    |land       |make    |time     |
|must    |spirit     |peace          |country |shall    |believe    |must       |every      |great   |every    |
|peace   |world      |today          |now     |upon     |one        |time       |government |peace   |one      |
|seek    |human      |government     |power   |freedom  |time       |now        |world      |need    |country  |
|country |dream      |time           |today   |must     |change     |history    |one        |let     |citizens |
|know    |government |responsibility |must    |strength |work       |new        |promise    |time    |world    |
|story   |freedom    |great          |one     |country  |freedom    |human      |must       |today   |make     |
