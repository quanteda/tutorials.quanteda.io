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
##       Topic 1      Topic 2      Topic 3   Topic 4    Topic 5    Topic 6  
##  [1,] "one"        "new"        "freedom" "let"      "must"     "world"  
##  [2,] "world"      "world"      "liberty" "new"      "citizens" "free"   
##  [3,] "government" "century"    "must"    "world"    "country"  "peace"  
##  [4,] "now"        "let"        "every"   "must"     "time"     "nations"
##  [5,] "new"        "time"       "world"   "together" "every"    "freedom"
##  [6,] "must"       "one"        "one"     "freedom"  "new"      "may"    
##  [7,] "freedom"    "government" "change"  "human"    "freedom"  "must"   
##  [8,] "time"       "now"        "justice" "strength" "make"     "upon"   
##  [9,] "every"      "together"   "time"    "pledge"   "story"    "shall"  
## [10,] "god"        "make"       "man"     "sides"    "god"      "faith"  
##       Topic 7   Topic 8          Topic 9  Topic 10    
##  [1,] "new"     "let"            "must"   "government"
##  [2,] "world"   "peace"          "world"  "must"      
##  [3,] "great"   "world"          "new"    "believe"   
##  [4,] "free"    "new"            "today"  "world"     
##  [5,] "must"    "responsibility" "let"    "one"       
##  [6,] "friends" "government"     "time"   "time"      
##  [7,] "good"    "great"          "work"   "freedom"   
##  [8,] "things"  "home"           "every"  "work"      
##  [9,] "hand"    "abroad"         "now"    "man"       
## [10,] "work"    "make"           "change" "let"
```


```r
knitr::kable(terms(lda, 10))
```



|Topic 1    |Topic 2    |Topic 3 |Topic 4  |Topic 5  |Topic 6 |Topic 7 |Topic 8        |Topic 9 |Topic 10   |
|:----------|:----------|:-------|:--------|:--------|:-------|:-------|:--------------|:-------|:----------|
|one        |new        |freedom |let      |must     |world   |new     |let            |must    |government |
|world      |world      |liberty |new      |citizens |free    |world   |peace          |world   |must       |
|government |century    |must    |world    |country  |peace   |great   |world          |new     |believe    |
|now        |let        |every   |must     |time     |nations |free    |new            |today   |world      |
|new        |time       |world   |together |every    |freedom |must    |responsibility |let     |one        |
|must       |one        |one     |freedom  |new      |may     |friends |government     |time    |time       |
|freedom    |government |change  |human    |freedom  |must    |good    |great          |work    |freedom    |
|time       |now        |justice |strength |make     |upon    |things  |home           |every   |work       |
|every      |together   |time    |pledge   |story    |shall   |hand    |abroad         |now     |man        |
|god        |make       |man     |sides    |god      |faith   |work    |make           |change  |let        |
