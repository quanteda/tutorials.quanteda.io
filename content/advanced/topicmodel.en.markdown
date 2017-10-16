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
##       Topic 1      Topic 2      Topic 3   Topic 4          Topic 5   
##  [1,] "must"       "new"        "must"    "let"            "free"    
##  [2,] "world"      "must"       "may"     "new"            "world"   
##  [3,] "government" "strength"   "world"   "world"          "must"    
##  [4,] "time"       "together"   "freedom" "peace"          "freedom" 
##  [5,] "today"      "spirit"     "nations" "every"          "peace"   
##  [6,] "let"        "world"      "time"    "responsibility" "faith"   
##  [7,] "work"       "human"      "peace"   "government"     "new"     
##  [8,] "freedom"    "dream"      "new"     "time"           "strength"
##  [9,] "new"        "government" "seek"    "better"         "know"    
## [10,] "believe"    "freedom"    "one"     "today"          "work"    
##       Topic 6      Topic 7    Topic 8    Topic 9      Topic 10  
##  [1,] "world"      "freedom"  "country"  "new"        "let"     
##  [2,] "must"       "liberty"  "citizens" "century"    "world"   
##  [3,] "one"        "every"    "every"    "time"       "peace"   
##  [4,] "freedom"    "one"      "new"      "land"       "new"     
##  [5,] "now"        "country"  "never"    "every"      "know"    
##  [6,] "time"       "world"    "many"     "government" "earth"   
##  [7,] "new"        "history"  "story"    "world"      "man"     
##  [8,] "union"      "free"     "great"    "one"        "now"     
##  [9,] "government" "time"     "must"     "promise"    "make"    
## [10,] "man"        "citizens" "world"    "must"       "together"
```


```r
knitr::kable(terms(lda, 10))
```



|Topic 1    |Topic 2    |Topic 3 |Topic 4        |Topic 5  |Topic 6    |Topic 7  |Topic 8  |Topic 9    |Topic 10 |
|:----------|:----------|:-------|:--------------|:--------|:----------|:--------|:--------|:----------|:--------|
|must       |new        |must    |let            |free     |world      |freedom  |country  |new        |let      |
|world      |must       |may     |new            |world    |must       |liberty  |citizens |century    |world    |
|government |strength   |world   |world          |must     |one        |every    |every    |time       |peace    |
|time       |together   |freedom |peace          |freedom  |freedom    |one      |new      |land       |new      |
|today      |spirit     |nations |every          |peace    |now        |country  |never    |every      |know     |
|let        |world      |time    |responsibility |faith    |time       |world    |many     |government |earth    |
|work       |human      |peace   |government     |new      |new        |history  |story    |world      |man      |
|freedom    |dream      |new     |time           |strength |union      |free     |great    |one        |now      |
|new        |government |seek    |better         |know     |government |time     |must     |promise    |make     |
|believe    |freedom    |one     |today          |work     |man        |citizens |world    |must       |together |
