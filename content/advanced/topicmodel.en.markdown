---
title: Topic models
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
##       Topic 1   Topic 2          Topic 3    Topic 4 Topic 5   Topic 6  
##  [1,] "must"    "let"            "let"      "world" "new"     "new"    
##  [2,] "free"    "peace"          "world"    "let"   "freedom" "world"  
##  [3,] "world"   "world"          "sides"    "must"  "century" "great"  
##  [4,] "faith"   "new"            "new"      "new"   "every"   "free"   
##  [5,] "freedom" "responsibility" "pledge"   "today" "one"     "must"   
##  [6,] "time"    "government"     "ask"      "now"   "time"    "friends"
##  [7,] "every"   "great"          "citizens" "make"  "world"   "good"   
##  [8,] "change"  "home"           "power"    "time"  "liberty" "things" 
##  [9,] "one"     "abroad"         "shall"    "peace" "must"    "hand"   
## [10,] "peace"   "make"           "free"     "know"  "work"    "work"   
##       Topic 7  Topic 8      Topic 9      Topic 10 
##  [1,] "new"    "one"        "government" "must"   
##  [2,] "must"   "world"      "must"       "world"  
##  [3,] "every"  "government" "believe"    "freedom"
##  [4,] "less"   "now"        "world"      "new"    
##  [5,] "let"    "new"        "one"        "nations"
##  [6,] "work"   "must"       "time"       "may"    
##  [7,] "world"  "freedom"    "freedom"    "peace"  
##  [8,] "time"   "time"       "work"       "know"   
##  [9,] "today"  "every"      "man"        "seek"   
## [10,] "common" "together"   "let"        "country"
```


```r
knitr::kable(terms(lda, 10))
```



|Topic 1 |Topic 2        |Topic 3  |Topic 4 |Topic 5 |Topic 6 |Topic 7 |Topic 8    |Topic 9    |Topic 10 |
|:-------|:--------------|:--------|:-------|:-------|:-------|:-------|:----------|:----------|:--------|
|must    |let            |let      |world   |new     |new     |new     |one        |government |must     |
|free    |peace          |world    |let     |freedom |world   |must    |world      |must       |world    |
|world   |world          |sides    |must    |century |great   |every   |government |believe    |freedom  |
|faith   |new            |new      |new     |every   |free    |less    |now        |world      |new      |
|freedom |responsibility |pledge   |today   |one     |must    |let     |new        |one        |nations  |
|time    |government     |ask      |now     |time    |friends |work    |must       |time       |may      |
|every   |great          |citizens |make    |world   |good    |world   |freedom    |freedom    |peace    |
|change  |home           |power    |time    |liberty |things  |time    |time       |work       |know     |
|one     |abroad         |shall    |peace   |must    |hand    |today   |every      |man        |seek     |
|peace   |make           |free     |know    |work    |work    |common  |together   |let        |country  |
