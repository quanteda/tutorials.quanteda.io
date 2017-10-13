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
##       Topic 1          Topic 2   Topic 3    Topic 4      Topic 5  
##  [1,] "let"            "must"    "world"    "one"        "world"  
##  [2,] "world"          "may"     "peace"    "world"      "must"   
##  [3,] "peace"          "world"   "let"      "government" "free"   
##  [4,] "new"            "freedom" "know"     "now"        "faith"  
##  [5,] "shall"          "nations" "make"     "new"        "freedom"
##  [6,] "responsibility" "time"    "earth"    "must"       "shall"  
##  [7,] "government"     "peace"   "now"      "freedom"    "work"   
##  [8,] "great"          "new"     "new"      "time"       "peace"  
##  [9,] "home"           "seek"    "together" "every"      "time"   
## [10,] "help"           "one"     "voices"   "god"        "today"  
##       Topic 6      Topic 7      Topic 8   Topic 9    Topic 10 
##  [1,] "government" "new"        "new"     "freedom"  "new"    
##  [2,] "must"       "must"       "century" "country"  "world"  
##  [3,] "world"      "strength"   "must"    "every"    "great"  
##  [4,] "time"       "together"   "land"    "liberty"  "free"   
##  [5,] "new"        "spirit"     "world"   "citizens" "must"   
##  [6,] "work"       "world"      "every"   "must"     "friends"
##  [7,] "let"        "human"      "time"    "world"    "good"   
##  [8,] "freedom"    "dream"      "one"     "time"     "things" 
##  [9,] "today"      "government" "change"  "know"     "hand"   
## [10,] "now"        "freedom"    "union"   "work"     "work"
```


```r
knitr::kable(terms(lda, 10))
```



|Topic 1        |Topic 2 |Topic 3  |Topic 4    |Topic 5 |Topic 6    |Topic 7    |Topic 8 |Topic 9  |Topic 10 |
|:--------------|:-------|:--------|:----------|:-------|:----------|:----------|:-------|:--------|:--------|
|let            |must    |world    |one        |world   |government |new        |new     |freedom  |new      |
|world          |may     |peace    |world      |must    |must       |must       |century |country  |world    |
|peace          |world   |let      |government |free    |world      |strength   |must    |every    |great    |
|new            |freedom |know     |now        |faith   |time       |together   |land    |liberty  |free     |
|shall          |nations |make     |new        |freedom |new        |spirit     |world   |citizens |must     |
|responsibility |time    |earth    |must       |shall   |work       |world      |every   |must     |friends  |
|government     |peace   |now      |freedom    |work    |let        |human      |time    |world    |good     |
|great          |new     |new      |time       |peace   |freedom    |dream      |one     |time     |things   |
|home           |seek    |together |every      |time    |today      |government |change  |know     |hand     |
|help           |one     |voices   |god        |today   |now        |freedom    |union   |work     |work     |
