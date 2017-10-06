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
##       Topic 1      Topic 2      Topic 3      Topic 4 Topic 5     
##  [1,] "world"      "world"      "government" "new"   "new"       
##  [2,] "one"        "let"        "must"       "must"  "must"      
##  [3,] "government" "new"        "believe"    "every" "time"      
##  [4,] "freedom"    "peace"      "world"      "less"  "century"   
##  [5,] "must"       "must"       "one"        "let"   "every"     
##  [6,] "time"       "today"      "time"       "work"  "one"       
##  [7,] "now"        "make"       "freedom"    "today" "government"
##  [8,] "history"    "government" "work"       "time"  "citizens"  
##  [9,] "new"        "time"       "man"        "world" "together"  
## [10,] "human"      "together"   "let"        "now"   "world"     
##       Topic 6   Topic 7   Topic 8    Topic 9    Topic 10 
##  [1,] "world"   "freedom" "free"     "country"  "new"    
##  [2,] "nations" "liberty" "world"    "citizens" "great"  
##  [3,] "may"     "must"    "must"     "every"    "world"  
##  [4,] "let"     "world"   "faith"    "new"      "must"   
##  [5,] "peace"   "one"     "shall"    "never"    "free"   
##  [6,] "freedom" "human"   "change"   "many"     "friends"
##  [7,] "seek"    "new"     "upon"     "world"    "things" 
##  [8,] "new"     "every"   "peace"    "one"      "good"   
##  [9,] "power"   "country" "freedom"  "great"    "hand"   
## [10,] "free"    "time"    "strength" "must"     "need"
```


```r
knitr::kable(terms(lda, 10))
```



|Topic 1    |Topic 2    |Topic 3    |Topic 4 |Topic 5    |Topic 6 |Topic 7 |Topic 8  |Topic 9  |Topic 10 |
|:----------|:----------|:----------|:-------|:----------|:-------|:-------|:--------|:--------|:--------|
|world      |world      |government |new     |new        |world   |freedom |free     |country  |new      |
|one        |let        |must       |must    |must       |nations |liberty |world    |citizens |great    |
|government |new        |believe    |every   |time       |may     |must    |must     |every    |world    |
|freedom    |peace      |world      |less    |century    |let     |world   |faith    |new      |must     |
|must       |must       |one        |let     |every      |peace   |one     |shall    |never    |free     |
|time       |today      |time       |work    |one        |freedom |human   |change   |many     |friends  |
|now        |make       |freedom    |today   |government |seek    |new     |upon     |world    |things   |
|history    |government |work       |time    |citizens   |new     |every   |peace    |one      |good     |
|new        |time       |man        |world   |together   |power   |country |freedom  |great    |hand     |
|human      |together   |let        |now     |world      |free    |time    |strength |must     |need     |
