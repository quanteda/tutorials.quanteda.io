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
##       Topic 1   Topic 2   Topic 3   Topic 4    Topic 5    Topic 6         
##  [1,] "new"     "let"     "new"     "must"     "world"    "let"           
##  [2,] "every"   "new"     "world"   "time"     "peace"    "peace"         
##  [3,] "world"   "world"   "great"   "make"     "let"      "world"         
##  [4,] "one"     "every"   "free"    "every"    "know"     "new"           
##  [5,] "must"    "power"   "must"    "together" "make"     "responsibility"
##  [6,] "century" "today"   "friends" "citizens" "earth"    "government"    
##  [7,] "land"    "must"    "good"    "country"  "now"      "great"         
##  [8,] "time"    "shall"   "things"  "one"      "new"      "home"          
##  [9,] "great"   "time"    "hand"    "new"      "together" "abroad"        
## [10,] "now"     "nations" "work"    "freedom"  "voices"   "make"          
##       Topic 7      Topic 8   Topic 9      Topic 10  
##  [1,] "must"       "world"   "freedom"    "free"    
##  [2,] "world"      "nations" "world"      "world"   
##  [3,] "government" "freedom" "one"        "must"    
##  [4,] "time"       "may"     "must"       "strength"
##  [5,] "today"      "must"    "time"       "faith"   
##  [6,] "let"        "peace"   "history"    "freedom" 
##  [7,] "work"       "seek"    "liberty"    "peace"   
##  [8,] "new"        "country" "human"      "shall"   
##  [9,] "freedom"    "know"    "every"      "upon"    
## [10,] "now"        "story"   "government" "new"
```


```r
knitr::kable(terms(lda, 10))
```



|Topic 1 |Topic 2 |Topic 3 |Topic 4  |Topic 5  |Topic 6        |Topic 7    |Topic 8 |Topic 9    |Topic 10 |
|:-------|:-------|:-------|:--------|:--------|:--------------|:----------|:-------|:----------|:--------|
|new     |let     |new     |must     |world    |let            |must       |world   |freedom    |free     |
|every   |new     |world   |time     |peace    |peace          |world      |nations |world      |world    |
|world   |world   |great   |make     |let      |world          |government |freedom |one        |must     |
|one     |every   |free    |every    |know     |new            |time       |may     |must       |strength |
|must    |power   |must    |together |make     |responsibility |today      |must    |time       |faith    |
|century |today   |friends |citizens |earth    |government     |let        |peace   |history    |freedom  |
|land    |must    |good    |country  |now      |great          |work       |seek    |liberty    |peace    |
|time    |shall   |things  |one      |new      |home           |new        |country |human      |shall    |
|great   |time    |hand    |new      |together |abroad         |freedom    |know    |every      |upon     |
|now     |nations |work    |freedom  |voices   |make           |now        |story   |government |new      |
