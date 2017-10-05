---
title: Topic classification
---

## Topic models:

```r
require(quanteda)
```

```
## Loading required package: quanteda
```

```
## quanteda version 0.99.9
```

```
## Using 3 of 4 threads for parallel computing
```

```
## 
## Attaching package: 'quanteda'
```

```
## The following object is masked from 'package:utils':
## 
##     View
```

```r
require(topicmodels)
```

```
## Loading required package: topicmodels
```

```r
mycorpus <- corpus_subset(data_corpus_inaugural, Year > 1950)
quantdfm <- dfm(mycorpus, verbose = FALSE, remove_punct = TRUE,
                remove = c(stopwords('english'), 'will', 'us', 'nation', 'can', 'peopl*', 'americ*'))
ldadfm <- convert(quantdfm, to = "topicmodels")
lda <- LDA(ldadfm, control = list(alpha = 0.1), k = 20)
terms(lda, 10)
```

```
##       Topic 1      Topic 2          Topic 3  Topic 4    Topic 5 
##  [1,] "new"        "let"            "new"    "freedom"  "world" 
##  [2,] "century"    "world"          "must"   "liberty"  "things"
##  [3,] "time"       "peace"          "every"  "every"    "free"  
##  [4,] "land"       "responsibility" "less"   "one"      "day"   
##  [5,] "every"      "home"           "let"    "country"  "must"  
##  [6,] "government" "policies"       "work"   "world"    "time"  
##  [7,] "world"      "shall"          "world"  "history"  "hand"  
##  [8,] "one"        "new"            "time"   "free"     "good"  
##  [9,] "promise"    "make"           "today"  "time"     "work"  
## [10,] "must"       "years"          "common" "citizens" "know"  
##       Topic 6      Topic 7    Topic 8   Topic 9   Topic 10  Topic 11  
##  [1,] "new"        "free"     "may"     "change"  "new"     "must"    
##  [2,] "must"       "world"    "world"   "must"    "great"   "time"    
##  [3,] "strength"   "faith"    "nations" "man"     "today"   "make"    
##  [4,] "together"   "peace"    "peace"   "union"   "ask"     "every"   
##  [5,] "spirit"     "shall"    "freedom" "world"   "old"     "together"
##  [6,] "world"      "upon"     "seek"    "old"     "like"    "citizens"
##  [7,] "human"      "freedom"  "must"    "land"    "first"   "country" 
##  [8,] "dream"      "must"     "upon"    "every"   "friends" "one"     
##  [9,] "government" "strength" "help"    "liberty" "strong"  "new"     
## [10,] "freedom"    "country"  "justice" "justice" "speaker" "freedom" 
##       Topic 12   Topic 13  Topic 14     Topic 15     Topic 16    
##  [1,] "world"    "must"    "government" "new"        "world"     
##  [2,] "peace"    "time"    "must"       "peace"      "one"       
##  [3,] "let"      "one"     "believe"    "let"        "government"
##  [4,] "know"     "great"   "world"      "role"       "freedom"   
##  [5,] "make"     "old"     "one"        "abroad"     "must"      
##  [6,] "earth"    "long"    "time"       "government" "time"      
##  [7,] "now"      "nations" "freedom"    "great"      "now"       
##  [8,] "new"      "work"    "work"       "world"      "history"   
##  [9,] "together" "now"     "man"        "right"      "new"       
## [10,] "voices"   "power"   "let"        "together"   "human"     
##       Topic 17   Topic 18   Topic 19 Topic 20 
##  [1,] "let"      "citizens" "world"  "country"
##  [2,] "world"    "country"  "must"   "one"    
##  [3,] "sides"    "story"    "today"  "every"  
##  [4,] "new"      "must"     "new"    "world"  
##  [5,] "pledge"   "every"    "change" "great"  
##  [6,] "ask"      "new"      "let"    "new"    
##  [7,] "citizens" "know"     "time"   "never"  
##  [8,] "power"    "freedom"  "work"   "now"    
##  [9,] "shall"    "promise"  "fellow" "back"   
## [10,] "free"     "common"   "idea"   "make"
```
