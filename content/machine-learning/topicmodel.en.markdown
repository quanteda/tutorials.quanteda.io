---
title: Topic models
draft: false
---


```r
require(quanteda)
require(topicmodels)
```


```r
mycorpus <- corpus_subset(data_corpus_inaugural, Year > 1950)
quantdfm <- dfm(mycorpus, verbose = FALSE, remove_punct = TRUE,
                remove = c(stopwords('en'), 'will', 'us', 'nation', 'can', 'peopl*', 'americ*'))
ldadfm <- convert(quantdfm, to = "topicmodels")
lda <- LDA(ldadfm, control = list(alpha = 0.1), k = 10)
terms(lda, 10)
```

```
##       Topic 1   Topic 2      Topic 3    Topic 4    Topic 5    Topic 6   
##  [1,] "world"   "world"      "free"     "must"     "let"      "freedom" 
##  [2,] "peace"   "one"        "world"    "citizens" "new"      "country" 
##  [3,] "nations" "government" "faith"    "country"  "world"    "one"     
##  [4,] "may"     "freedom"    "peace"    "time"     "must"     "every"   
##  [5,] "know"    "must"       "shall"    "every"    "man"      "liberty" 
##  [6,] "earth"   "time"       "upon"     "new"      "change"   "world"   
##  [7,] "freedom" "now"        "freedom"  "freedom"  "together" "citizens"
##  [8,] "seek"    "history"    "must"     "make"     "strength" "day"     
##  [9,] "new"     "new"        "strength" "story"    "now"      "great"   
## [10,] "now"     "human"      "country"  "god"      "freedom"  "now"     
##       Topic 7      Topic 8   Topic 9  Topic 10    
##  [1,] "new"        "new"     "new"    "government"
##  [2,] "world"      "world"   "must"   "must"      
##  [3,] "let"        "great"   "every"  "believe"   
##  [4,] "must"       "free"    "less"   "world"     
##  [5,] "time"       "must"    "let"    "one"       
##  [6,] "government" "friends" "work"   "time"      
##  [7,] "century"    "good"    "world"  "freedom"   
##  [8,] "every"      "things"  "time"   "work"      
##  [9,] "peace"      "hand"    "today"  "man"       
## [10,] "today"      "work"    "common" "let"
```


```r
knitr::kable(terms(lda, 10))
```



|Topic 1 |Topic 2    |Topic 3  |Topic 4  |Topic 5  |Topic 6  |Topic 7    |Topic 8 |Topic 9 |Topic 10   |
|:-------|:----------|:--------|:--------|:--------|:--------|:----------|:-------|:-------|:----------|
|world   |world      |free     |must     |let      |freedom  |new        |new     |new     |government |
|peace   |one        |world    |citizens |new      |country  |world      |world   |must    |must       |
|nations |government |faith    |country  |world    |one      |let        |great   |every   |believe    |
|may     |freedom    |peace    |time     |must     |every    |must       |free    |less    |world      |
|know    |must       |shall    |every    |man      |liberty  |time       |must    |let     |one        |
|earth   |time       |upon     |new      |change   |world    |government |friends |work    |time       |
|freedom |now        |freedom  |freedom  |together |citizens |century    |good    |world   |freedom    |
|seek    |history    |must     |make     |strength |day      |every      |things  |time    |work       |
|new     |new        |strength |story    |now      |great    |peace      |hand    |today   |man        |
|now     |human      |country  |god      |freedom  |now      |today      |work    |common  |let        |
