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
##       Topic 1      Topic 2     Topic 3    Topic 4      Topic 5     
##  [1,] "new"        "new"       "must"     "world"      "government"
##  [2,] "must"       "world"     "let"      "must"       "must"      
##  [3,] "world"      "great"     "new"      "new"        "believe"   
##  [4,] "let"        "country"   "time"     "now"        "world"     
##  [5,] "century"    "must"      "citizens" "freedom"    "one"       
##  [6,] "time"       "make"      "world"    "time"       "time"      
##  [7,] "government" "day"       "country"  "one"        "freedom"   
##  [8,] "work"       "today"     "freedom"  "government" "work"      
##  [9,] "today"      "president" "every"    "let"        "man"       
## [10,] "together"   "right"     "pledge"   "every"      "let"       
##       Topic 6    Topic 7          Topic 8    Topic 9   Topic 10  
##  [1,] "world"    "let"            "freedom"  "must"    "free"    
##  [2,] "peace"    "peace"          "liberty"  "world"   "world"   
##  [3,] "let"      "world"          "every"    "freedom" "faith"   
##  [4,] "know"     "new"            "one"      "nations" "peace"   
##  [5,] "make"     "responsibility" "country"  "may"     "shall"   
##  [6,] "earth"    "government"     "world"    "change"  "upon"    
##  [7,] "now"      "great"          "history"  "seek"    "freedom" 
##  [8,] "new"      "home"           "free"     "justice" "must"    
##  [9,] "together" "abroad"         "time"     "new"     "strength"
## [10,] "voices"   "together"       "citizens" "time"    "country"
```


```r
knitr::kable(terms(lda, 10))
```



|Topic 1    |Topic 2   |Topic 3  |Topic 4    |Topic 5    |Topic 6  |Topic 7        |Topic 8  |Topic 9 |Topic 10 |
|:----------|:---------|:--------|:----------|:----------|:--------|:--------------|:--------|:-------|:--------|
|new        |new       |must     |world      |government |world    |let            |freedom  |must    |free     |
|must       |world     |let      |must       |must       |peace    |peace          |liberty  |world   |world    |
|world      |great     |new      |new        |believe    |let      |world          |every    |freedom |faith    |
|let        |country   |time     |now        |world      |know     |new            |one      |nations |peace    |
|century    |must      |citizens |freedom    |one        |make     |responsibility |country  |may     |shall    |
|time       |make      |world    |time       |time       |earth    |government     |world    |change  |upon     |
|government |day       |country  |one        |freedom    |now      |great          |history  |seek    |freedom  |
|work       |today     |freedom  |government |work       |new      |home           |free     |justice |must     |
|today      |president |every    |let        |man        |together |abroad         |time     |new     |strength |
|together   |right     |pledge   |every      |let        |voices   |together       |citizens |time    |country  |
