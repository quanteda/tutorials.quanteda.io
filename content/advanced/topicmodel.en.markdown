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
##       Topic 1   Topic 2      Topic 3   Topic 4          Topic 5   
##  [1,] "let"     "must"       "world"   "let"            "world"   
##  [2,] "new"     "government" "must"    "peace"          "must"    
##  [3,] "world"   "time"       "new"     "world"          "may"     
##  [4,] "every"   "believe"    "today"   "new"            "nations" 
##  [5,] "power"   "one"        "let"     "responsibility" "freedom" 
##  [6,] "today"   "freedom"    "now"     "government"     "new"     
##  [7,] "must"    "work"       "country" "great"          "peace"   
##  [8,] "shall"   "world"      "every"   "home"           "seek"    
##  [9,] "time"    "make"       "one"     "abroad"         "strength"
## [10,] "nations" "act"        "time"    "make"           "know"    
##       Topic 6    Topic 7   Topic 8      Topic 9      Topic 10 
##  [1,] "world"    "new"     "new"        "world"      "freedom"
##  [2,] "man"      "world"   "century"    "one"        "free"   
##  [3,] "now"      "great"   "every"      "government" "world"  
##  [4,] "new"      "free"    "citizens"   "freedom"    "must"   
##  [5,] "change"   "must"    "time"       "must"       "liberty"
##  [6,] "let"      "friends" "must"       "time"       "country"
##  [7,] "know"     "good"    "promise"    "now"        "faith"  
##  [8,] "peace"    "things"  "government" "history"    "every"  
##  [9,] "together" "hand"    "world"      "new"        "one"    
## [10,] "one"      "work"    "work"       "human"      "peace"
```


```r
knitr::kable(terms(lda, 10))
```



|Topic 1 |Topic 2    |Topic 3 |Topic 4        |Topic 5  |Topic 6  |Topic 7 |Topic 8    |Topic 9    |Topic 10 |
|:-------|:----------|:-------|:--------------|:--------|:--------|:-------|:----------|:----------|:--------|
|let     |must       |world   |let            |world    |world    |new     |new        |world      |freedom  |
|new     |government |must    |peace          |must     |man      |world   |century    |one        |free     |
|world   |time       |new     |world          |may      |now      |great   |every      |government |world    |
|every   |believe    |today   |new            |nations  |new      |free    |citizens   |freedom    |must     |
|power   |one        |let     |responsibility |freedom  |change   |must    |time       |must       |liberty  |
|today   |freedom    |now     |government     |new      |let      |friends |must       |time       |country  |
|must    |work       |country |great          |peace    |know     |good    |promise    |now        |faith    |
|shall   |world      |every   |home           |seek     |peace    |things  |government |history    |every    |
|time    |make       |one     |abroad         |strength |together |hand    |world      |new        |one      |
|nations |act        |time    |make           |know     |one      |work    |work       |human      |peace    |
