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
##       Topic 1      Topic 2  Topic 3     Topic 4          Topic 5     
##  [1,] "new"        "new"    "let"       "let"            "must"      
##  [2,] "century"    "must"   "world"     "peace"          "time"      
##  [3,] "must"       "every"  "country"   "world"          "freedom"   
##  [4,] "world"      "less"   "new"       "new"            "government"
##  [5,] "government" "let"    "citizens"  "responsibility" "citizens"  
##  [6,] "time"       "work"   "power"     "government"     "world"     
##  [7,] "let"        "world"  "one"       "great"          "one"       
##  [8,] "together"   "time"   "never"     "home"           "new"       
##  [9,] "one"        "today"  "every"     "abroad"         "every"     
## [10,] "land"       "common" "president" "make"           "now"       
##       Topic 6   Topic 7   Topic 8      Topic 9    Topic 10
##  [1,] "freedom" "world"   "government" "world"    "must"  
##  [2,] "world"   "free"    "must"       "peace"    "world" 
##  [3,] "free"    "peace"   "believe"    "let"      "change"
##  [4,] "liberty" "nations" "world"      "know"     "new"   
##  [5,] "new"     "freedom" "one"        "make"     "today" 
##  [6,] "must"    "may"     "time"       "earth"    "time"  
##  [7,] "time"    "must"    "freedom"    "now"      "let"   
##  [8,] "great"   "upon"    "work"       "new"      "now"   
##  [9,] "work"    "shall"   "man"        "together" "man"   
## [10,] "day"     "faith"   "let"        "voices"   "union"
```


```r
knitr::kable(terms(lda, 10))
```



|Topic 1    |Topic 2 |Topic 3   |Topic 4        |Topic 5    |Topic 6 |Topic 7 |Topic 8    |Topic 9  |Topic 10 |
|:----------|:-------|:---------|:--------------|:----------|:-------|:-------|:----------|:--------|:--------|
|new        |new     |let       |let            |must       |freedom |world   |government |world    |must     |
|century    |must    |world     |peace          |time       |world   |free    |must       |peace    |world    |
|must       |every   |country   |world          |freedom    |free    |peace   |believe    |let      |change   |
|world      |less    |new       |new            |government |liberty |nations |world      |know     |new      |
|government |let     |citizens  |responsibility |citizens   |new     |freedom |one        |make     |today    |
|time       |work    |power     |government     |world      |must    |may     |time       |earth    |time     |
|let        |world   |one       |great          |one        |time    |must    |freedom    |now      |let      |
|together   |time    |never     |home           |new        |great   |upon    |work       |new      |now      |
|one        |today   |every     |abroad         |every      |work    |shall   |man        |together |man      |
|land       |common  |president |make           |now        |day     |faith   |let        |voices   |union    |
