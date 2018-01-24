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
##       Topic 1   Topic 2      Topic 3  Topic 4   Topic 5   Topic 6   
##  [1,] "new"     "government" "world"  "new"     "world"   "must"    
##  [2,] "world"   "must"       "must"   "freedom" "peace"   "change"  
##  [3,] "great"   "believe"    "today"  "century" "let"     "time"    
##  [4,] "free"    "world"      "new"    "every"   "new"     "every"   
##  [5,] "must"    "one"        "change" "one"     "may"     "one"     
##  [6,] "friends" "time"       "let"    "time"    "nations" "new"     
##  [7,] "good"    "freedom"    "time"   "world"   "freedom" "liberty" 
##  [8,] "things"  "work"       "work"   "liberty" "great"   "world"   
##  [9,] "hand"    "man"        "fellow" "must"    "help"    "now"     
## [10,] "work"    "let"        "idea"   "work"    "time"    "together"
##       Topic 7      Topic 8    Topic 9    Topic 10  
##  [1,] "one"        "new"      "let"      "free"    
##  [2,] "world"      "must"     "world"    "world"   
##  [3,] "government" "every"    "peace"    "must"    
##  [4,] "now"        "country"  "new"      "strength"
##  [5,] "new"        "common"   "know"     "faith"   
##  [6,] "must"       "citizens" "earth"    "freedom" 
##  [7,] "freedom"    "work"     "man"      "peace"   
##  [8,] "time"       "time"     "now"      "shall"   
##  [9,] "every"      "know"     "make"     "upon"    
## [10,] "god"        "world"    "together" "new"
```


```r
knitr::kable(terms(lda, 10))
```



|Topic 1 |Topic 2    |Topic 3 |Topic 4 |Topic 5 |Topic 6  |Topic 7    |Topic 8  |Topic 9  |Topic 10 |
|:-------|:----------|:-------|:-------|:-------|:--------|:----------|:--------|:--------|:--------|
|new     |government |world   |new     |world   |must     |one        |new      |let      |free     |
|world   |must       |must    |freedom |peace   |change   |world      |must     |world    |world    |
|great   |believe    |today   |century |let     |time     |government |every    |peace    |must     |
|free    |world      |new     |every   |new     |every    |now        |country  |new      |strength |
|must    |one        |change  |one     |may     |one      |new        |common   |know     |faith    |
|friends |time       |let     |time    |nations |new      |must       |citizens |earth    |freedom  |
|good    |freedom    |time    |world   |freedom |liberty  |freedom    |work     |man      |peace    |
|things  |work       |work    |liberty |great   |world    |time       |time     |now      |shall    |
|hand    |man        |fellow  |must    |help    |now      |every      |know     |make     |upon     |
|work    |let        |idea    |work    |time    |together |god        |world    |together |new      |
