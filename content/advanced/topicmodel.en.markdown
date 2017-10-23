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
##       Topic 1      Topic 2          Topic 3      Topic 4      Topic 5    
##  [1,] "must"       "world"          "must"       "new"        "new"      
##  [2,] "world"      "let"            "government" "century"    "world"    
##  [3,] "new"        "new"            "world"      "time"       "great"    
##  [4,] "freedom"    "must"           "man"        "land"       "country"  
##  [5,] "government" "peace"          "believe"    "every"      "must"     
##  [6,] "one"        "today"          "one"        "government" "make"     
##  [7,] "time"       "government"     "time"       "world"      "day"      
##  [8,] "citizens"   "time"           "change"     "one"        "today"    
##  [9,] "now"        "responsibility" "work"       "promise"    "president"
## [10,] "together"   "great"          "freedom"    "must"       "right"    
##       Topic 6   Topic 7    Topic 8    Topic 9   Topic 10  
##  [1,] "freedom" "world"    "free"     "may"     "must"    
##  [2,] "every"   "peace"    "world"    "world"   "let"     
##  [3,] "liberty" "let"      "faith"    "nations" "new"     
##  [4,] "world"   "know"     "peace"    "peace"   "time"    
##  [5,] "must"    "make"     "shall"    "freedom" "citizens"
##  [6,] "time"    "earth"    "upon"     "seek"    "world"   
##  [7,] "work"    "now"      "freedom"  "must"    "country" 
##  [8,] "new"     "new"      "must"     "upon"    "freedom" 
##  [9,] "day"     "together" "strength" "help"    "every"   
## [10,] "one"     "voices"   "country"  "justice" "pledge"
```


```r
knitr::kable(terms(lda, 10))
```



|Topic 1    |Topic 2        |Topic 3    |Topic 4    |Topic 5   |Topic 6 |Topic 7  |Topic 8  |Topic 9 |Topic 10 |
|:----------|:--------------|:----------|:----------|:---------|:-------|:--------|:--------|:-------|:--------|
|must       |world          |must       |new        |new       |freedom |world    |free     |may     |must     |
|world      |let            |government |century    |world     |every   |peace    |world    |world   |let      |
|new        |new            |world      |time       |great     |liberty |let      |faith    |nations |new      |
|freedom    |must           |man        |land       |country   |world   |know     |peace    |peace   |time     |
|government |peace          |believe    |every      |must      |must    |make     |shall    |freedom |citizens |
|one        |today          |one        |government |make      |time    |earth    |upon     |seek    |world    |
|time       |government     |time       |world      |day       |work    |now      |freedom  |must    |country  |
|citizens   |time           |change     |one        |today     |new     |new      |must     |upon    |freedom  |
|now        |responsibility |work       |promise    |president |day     |together |strength |help    |every    |
|together   |great          |freedom    |must       |right     |one     |voices   |country  |justice |pledge   |
