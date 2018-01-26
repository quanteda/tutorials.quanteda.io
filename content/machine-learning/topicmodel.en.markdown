---
title: Topic models
draft: true
---


```r
require(quanteda)
require(topicmodels)
```


```r
mycorpus <- corpus_subset(data_corpus_inaugural, Year > 1950)
quantdfm <- dfm(mycorpus, verbose = FALSE, remove_punct = TRUE,
                remove = c(stopwords('english'), 'will', 'us', 'nation', 'can', 'peopl*', 'americ*'))
ldadfm <- convert(quantdfm, to = "topicmodels")
lda <- LDA(ldadfm, control = list(alpha = 0.1), k = 10)
terms(lda, 10)
```

```
##       Topic 1      Topic 2      Topic 3          Topic 4   Topic 5     
##  [1,] "world"      "new"        "let"            "world"   "government"
##  [2,] "freedom"    "century"    "peace"          "must"    "must"      
##  [3,] "must"       "world"      "world"          "new"     "believe"   
##  [4,] "government" "time"       "new"            "today"   "world"     
##  [5,] "citizens"   "must"       "responsibility" "let"     "one"       
##  [6,] "one"        "great"      "government"     "now"     "time"      
##  [7,] "time"       "government" "great"          "country" "freedom"   
##  [8,] "new"        "work"       "home"           "every"   "work"      
##  [9,] "history"    "land"       "abroad"         "one"     "man"       
## [10,] "now"        "one"        "make"           "time"    "let"       
##       Topic 6  Topic 7    Topic 8    Topic 9   Topic 10 
##  [1,] "world"  "let"      "must"     "freedom" "world"  
##  [2,] "man"    "new"      "time"     "every"   "free"   
##  [3,] "now"    "world"    "together" "liberty" "peace"  
##  [4,] "new"    "together" "make"     "world"   "nations"
##  [5,] "change" "must"     "every"    "must"    "freedom"
##  [6,] "let"    "sides"    "equal"    "time"    "may"    
##  [7,] "peace"  "pledge"   "still"    "work"    "must"   
##  [8,] "know"   "human"    "one"      "new"     "upon"   
##  [9,] "one"    "strength" "new"      "day"     "shall"  
## [10,] "must"   "freedom"  "freedom"  "one"     "faith"
```


```r
knitr::kable(terms(lda, 10))
```



|Topic 1    |Topic 2    |Topic 3        |Topic 4 |Topic 5    |Topic 6 |Topic 7  |Topic 8  |Topic 9 |Topic 10 |
|:----------|:----------|:--------------|:-------|:----------|:-------|:--------|:--------|:-------|:--------|
|world      |new        |let            |world   |government |world   |let      |must     |freedom |world    |
|freedom    |century    |peace          |must    |must       |man     |new      |time     |every   |free     |
|must       |world      |world          |new     |believe    |now     |world    |together |liberty |peace    |
|government |time       |new            |today   |world      |new     |together |make     |world   |nations  |
|citizens   |must       |responsibility |let     |one        |change  |must     |every    |must    |freedom  |
|one        |great      |government     |now     |time       |let     |sides    |equal    |time    |may      |
|time       |government |great          |country |freedom    |peace   |pledge   |still    |work    |must     |
|new        |work       |home           |every   |work       |know    |human    |one      |new     |upon     |
|history    |land       |abroad         |one     |man        |one     |strength |new      |day     |shall    |
|now        |one        |make           |time    |let        |must    |freedom  |freedom  |one     |faith    |
