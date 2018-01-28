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



We only select news stories publihsed in 2016 using `corpus_subset()`. Futher, after removal of function words and punctuations in `dfm()`, we remove rare and common features and uisng `dfm_trim()` to reduce time to fit a model.


```r
news_corp <- corpus_subset(news_corp, year(docvars(news_corp, 'date')) >= 2016)
news_dfm <- dfm(news_corp, remove_punct = TRUE, remove = stopwords('en')) %>% 
            dfm_trim(min_count = 0.95, max_docfreq = 0.1)
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
##       Topic 1     Topic 2    Topic 3      Topic 4          Topic 5   
##  [1,] "oil"       "officers" "corbyn"     "clinton"        "syria"   
##  [2,] "energy"    "prison"   "johnson"    "sanders"        "isis"    
##  [3,] "climate"   "victims"  "shadow"     "block-time"     "military"
##  [4,] "doctors"   "mother"   "tory"       "cruz"           "islamic" 
##  [5,] "gas"       "son"      "budget"     "published-time" "syrian"  
##  [6,] "nhs"       "died"     "leadership" "hillary"        "muslim"  
##  [7,] "junior"    "facebook" "khan"       "gmt"            "un"      
##  [8,] "patients"  "officer"  "boris"      "trump's"        "forces"  
##  [9,] "contract"  "dead"     "cabinet"    "bst"            "turkey"  
## [10,] "emissions" "father"   "jeremy"     "obama"          "russian" 
##       Topic 6       Topic 7          Topic 8    Topic 9              
##  [1,] "refugees"    "australia"      "violence" "block-time"         
##  [2,] "water"       "australian"     "china"    "gmt"                
##  [3,] "food"        "labor"          "chinese"  "published-time"     
##  [4,] "drug"        "bst"            "parties"  "updated-timeupdated"
##  [5,] "asylum"      "block-time"     "votes"    "brussels"           
##  [6,] "drugs"       "turnbull"       "marriage" "19"                 
##  [7,] "immigration" "published-time" "students" "talks"              
##  [8,] "refugee"     "senate"         "voting"   "bst"                
##  [9,] "child"       "malcolm"        "obama"    "photograph"         
## [10,] "aid"         "budget"         "church"   "markets"            
##       Topic 10    
##  [1,] "sales"     
##  [2,] "prices"    
##  [3,] "banks"     
##  [4,] "sector"    
##  [5,] "rates"     
##  [6,] "businesses"
##  [7,] "customers" 
##  [8,] "apple"     
##  [9,] "shares"    
## [10,] "income"
```

You can then obtaine the most likely topics using `topics()` and save as a document-level variable.



