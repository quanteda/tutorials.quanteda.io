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
##       Topic 1      Topic 2   Topic 3      Topic 4      Topic 5  
##  [1,] "new"        "world"   "let"        "government" "new"    
##  [2,] "world"      "nations" "world"      "must"       "world"  
##  [3,] "must"       "must"    "new"        "believe"    "great"  
##  [4,] "century"    "may"     "peace"      "world"      "free"   
##  [5,] "time"       "let"     "great"      "one"        "must"   
##  [6,] "let"        "freedom" "every"      "time"       "friends"
##  [7,] "today"      "change"  "government" "freedom"    "good"   
##  [8,] "government" "new"     "one"        "work"       "things" 
##  [9,] "work"       "seek"    "make"       "man"        "hand"   
## [10,] "every"      "peace"   "country"    "let"        "work"   
##       Topic 6    Topic 7    Topic 8    Topic 9      Topic 10    
##  [1,] "new"      "freedom"  "free"     "new"        "world"     
##  [2,] "must"     "must"     "world"    "must"       "one"       
##  [3,] "every"    "liberty"  "faith"    "strength"   "peace"     
##  [4,] "country"  "time"     "peace"    "together"   "now"       
##  [5,] "common"   "every"    "shall"    "spirit"     "government"
##  [6,] "citizens" "one"      "upon"     "world"      "let"       
##  [7,] "work"     "country"  "freedom"  "human"      "new"       
##  [8,] "time"     "citizens" "must"     "dream"      "time"      
##  [9,] "know"     "world"    "strength" "government" "earth"     
## [10,] "world"    "make"     "country"  "freedom"    "freedom"
```


```r
knitr::kable(terms(lda, 10))
```



|Topic 1    |Topic 2 |Topic 3    |Topic 4    |Topic 5 |Topic 6  |Topic 7  |Topic 8  |Topic 9    |Topic 10   |
|:----------|:-------|:----------|:----------|:-------|:--------|:--------|:--------|:----------|:----------|
|new        |world   |let        |government |new     |new      |freedom  |free     |new        |world      |
|world      |nations |world      |must       |world   |must     |must     |world    |must       |one        |
|must       |must    |new        |believe    |great   |every    |liberty  |faith    |strength   |peace      |
|century    |may     |peace      |world      |free    |country  |time     |peace    |together   |now        |
|time       |let     |great      |one        |must    |common   |every    |shall    |spirit     |government |
|let        |freedom |every      |time       |friends |citizens |one      |upon     |world      |let        |
|today      |change  |government |freedom    |good    |work     |country  |freedom  |human      |new        |
|government |new     |one        |work       |things  |time     |citizens |must     |dream      |time       |
|work       |seek    |make       |man        |hand    |know     |world    |strength |government |earth      |
|every      |peace   |country    |let        |work    |world    |make     |country  |freedom    |freedom    |
