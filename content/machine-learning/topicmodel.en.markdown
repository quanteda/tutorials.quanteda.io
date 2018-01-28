---
title: Topic models
weight: 50
draft: false
---

Topics models are unsupervised document classification techniques. By modeling distributions of topics over words and words over documents, topic models identify the most discrimnatory groups of documents automatically. 


```r
require(quanteda)
require(quanteda.corpora)
require(lubridate)
```

```
## Warning: package 'lubridate' was built under R version 3.4.3
```

```r
require(topicmodels)
```

```
## Warning: package 'topicmodels' was built under R version 3.4.3
```


```r
news_corp <- download('data_corpus_guardian')
```



We only select news stories publihsed in 2016 using `corpus_subset()`. Futher, after removal of function words and punctuations in `dfm()`, we remove 95% of low frequency features uisng `dfm_trim()` to reduce time to fit a model.


```r
news_corp <- corpus_subset(news_corp, year(docvars(news_corp, 'date')) >= 2016)
news_dfm <- dfm(news_corp, remove_punct = TRUE, remove = stopwords('en')) %>% 
            dfm_trim(min_count = 0.95)
```

**quanteda** does not impliment topic models, but you can easily access to `LDA()` from the **topicmodel** package through `convert()`. `k = 10` specifies the number of topics to be discovered.  


```r
dtm <- convert(news_dfm, to = "topicmodels")
lda <- LDA(dtm, k = 10)
```

You can extract most important terms from the model using `terms()`.


```r
terms(lda, 10)
```

```
##       Topic 1      Topic 2    Topic 3               Topic 4  Topic 5   
##  [1,] "said"       "eu"       "block-time"          "one"    "said"    
##  [2,] "government" "said"     "published-time"      "people" "oil"     
##  [3,] "people"     "party"    "gmt"                 "says"   "economy" 
##  [4,] "health"     "labour"   "2016"                "can"    "growth"  
##  [5,] "australia"  "uk"       "bst"                 "like"   "year"    
##  [6,] "australian" "cameron"  "says"                "just"   "market"  
##  [7,] "work"       "minister" "updated-timeupdated" "get"    "uk"      
##  [8,] "women"      "vote"     "february"            "time"   "us"      
##  [9,] "related"    "britain"  "photograph"          "first"  "markets" 
## [10,] "new"        "european" "today"               "think"  "economic"
##       Topic 6   Topic 7      Topic 8      Topic 9     Topic 10  
##  [1,] "said"    "said"       "trump"      "said"      "said"    
##  [2,] "new"     "people"     "clinton"    "tax"       "police"  
##  [3,] "us"      "us"         "said"       "company"   "told"    
##  [4,] "years"   "refugees"   "sanders"    "business"  "court"   
##  [5,] "climate" "government" "cruz"       "year"      "one"     
##  [6,] "one"     "syria"      "republican" "uk"        "case"    
##  [7,] "can"     "security"   "donald"     "companies" "two"     
##  [8,] "also"    "isis"       "campaign"   "new"       "law"     
##  [9,] "water"   "country"    "new"        "pay"       "officers"
## [10,] "people"  "attacks"    "state"      "also"      "also"
```

You can then obtaine the most likely topics using `topics()` and save as a document-level variable.


```r
docvars(news_dfm, 'topic') <- topics(lda)
head(topics(lda), 20)
```

```
## text136751 text136585 text139163 text169133 text153451 text163885 
##          6          3          9          4         10          6 
## text157885 text173244 text137394 text169408 text184646 text127410 
##          4          2          1          4          2         10 
## text134923 text169695 text147917 text157535 text177078 text174393 
##          2          5          4          6          6          8 
## text181782 text143323 
##          4          2
```

