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
##       Topic 1   Topic 2  Topic 3  Topic 4          Topic 5     
##  [1,] "must"    "world"  "world"  "let"            "government"
##  [2,] "new"     "man"    "must"   "world"          "one"       
##  [3,] "time"    "now"    "today"  "peace"          "world"     
##  [4,] "make"    "new"    "new"    "new"            "must"      
##  [5,] "world"   "change" "change" "responsibility" "country"   
##  [6,] "great"   "let"    "let"    "shall"          "now"       
##  [7,] "free"    "peace"  "time"   "government"     "time"      
##  [8,] "freedom" "know"   "work"   "home"           "great"     
##  [9,] "work"    "one"    "fellow" "help"           "president" 
## [10,] "today"   "must"   "idea"   "great"          "believe"   
##       Topic 6      Topic 7      Topic 8    Topic 9      Topic 10 
##  [1,] "world"      "new"        "freedom"  "new"        "world"  
##  [2,] "one"        "must"       "country"  "century"    "free"   
##  [3,] "government" "strength"   "every"    "time"       "peace"  
##  [4,] "freedom"    "together"   "liberty"  "land"       "must"   
##  [5,] "must"       "spirit"     "citizens" "every"      "nations"
##  [6,] "time"       "world"      "must"     "must"       "freedom"
##  [7,] "now"        "human"      "world"    "world"      "may"    
##  [8,] "history"    "dream"      "time"     "one"        "upon"   
##  [9,] "new"        "government" "know"     "government" "know"   
## [10,] "human"      "freedom"    "work"     "promise"    "shall"
```


```r
knitr::kable(terms(lda, 10))
```



|Topic 1 |Topic 2 |Topic 3 |Topic 4        |Topic 5    |Topic 6    |Topic 7    |Topic 8  |Topic 9    |Topic 10 |
|:-------|:-------|:-------|:--------------|:----------|:----------|:----------|:--------|:----------|:--------|
|must    |world   |world   |let            |government |world      |new        |freedom  |new        |world    |
|new     |man     |must    |world          |one        |one        |must       |country  |century    |free     |
|time    |now     |today   |peace          |world      |government |strength   |every    |time       |peace    |
|make    |new     |new     |new            |must       |freedom    |together   |liberty  |land       |must     |
|world   |change  |change  |responsibility |country    |must       |spirit     |citizens |every      |nations  |
|great   |let     |let     |shall          |now        |time       |world      |must     |must       |freedom  |
|free    |peace   |time    |government     |time       |now        |human      |world    |world      |may      |
|freedom |know    |work    |home           |great      |history    |dream      |time     |one        |upon     |
|work    |one     |fellow  |help           |president  |new        |government |know     |government |know     |
|today   |must    |idea    |great          |believe    |human      |freedom    |work     |promise    |shall    |
