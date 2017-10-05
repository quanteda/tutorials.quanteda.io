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
##       Topic 1   Topic 2  Topic 3      Topic 4   Topic 5      Topic 6  
##  [1,] "country" "new"    "government" "may"     "new"        "new"    
##  [2,] "one"     "must"   "must"       "world"   "time"       "century"
##  [3,] "every"   "every"  "believe"    "nations" "every"      "let"    
##  [4,] "world"   "less"   "world"      "peace"   "children"   "one"    
##  [5,] "great"   "let"    "one"        "freedom" "fellow"     "promise"
##  [6,] "new"     "work"   "time"       "seek"    "must"       "land"   
##  [7,] "never"   "world"  "freedom"    "must"    "government" "world"  
##  [8,] "now"     "time"   "work"       "upon"    "now"        "great"  
##  [9,] "back"    "today"  "man"        "help"    "citizens"   "human"  
## [10,] "make"    "common" "let"        "justice" "today"      "come"   
##       Topic 7    Topic 8   Topic 9     Topic 10  Topic 11     Topic 12
##  [1,] "world"    "let"     "citizens"  "change"  "new"        "world" 
##  [2,] "peace"    "new"     "let"       "must"    "must"       "must"  
##  [3,] "let"      "world"   "president" "man"     "strength"   "today" 
##  [4,] "know"     "sides"   "sides"     "union"   "together"   "let"   
##  [5,] "make"     "pledge"  "go"        "world"   "spirit"     "change"
##  [6,] "earth"    "ask"     "well"      "old"     "world"      "new"   
##  [7,] "now"      "shall"   "world"     "land"    "human"      "time"  
##  [8,] "new"      "power"   "bear"      "every"   "dream"      "work"  
##  [9,] "together" "arms"    "poverty"   "liberty" "government" "idea"  
## [10,] "voices"   "nations" "first"     "justice" "freedom"    "done"  
##       Topic 13   Topic 14     Topic 15 Topic 16         Topic 17  
##  [1,] "citizens" "world"      "new"    "let"            "freedom" 
##  [2,] "country"  "one"        "need"   "peace"          "liberty" 
##  [3,] "story"    "government" "today"  "world"          "every"   
##  [4,] "must"     "freedom"    "things" "new"            "one"     
##  [5,] "every"    "must"       "love"   "responsibility" "country" 
##  [6,] "new"      "time"       "make"   "government"     "world"   
##  [7,] "know"     "now"        "part"   "great"          "history" 
##  [8,] "freedom"  "history"    "man"    "home"           "free"    
##  [9,] "promise"  "new"        "good"   "abroad"         "time"    
## [10,] "common"   "human"      "young"  "make"           "citizens"
##       Topic 18   Topic 19  Topic 20  
##  [1,] "free"     "great"   "must"    
##  [2,] "world"    "world"   "time"    
##  [3,] "faith"    "work"    "make"    
##  [4,] "peace"    "time"    "every"   
##  [5,] "shall"    "friends" "together"
##  [6,] "upon"     "free"    "citizens"
##  [7,] "freedom"  "hand"    "country" 
##  [8,] "must"     "must"    "one"     
##  [9,] "strength" "new"     "new"     
## [10,] "country"  "strong"  "freedom"
```
