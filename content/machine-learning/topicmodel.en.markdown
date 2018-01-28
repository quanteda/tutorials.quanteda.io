---
title: Topic models
weight: 50
draft: false
---

Topics models are unsupervised document classification techniques. By modeling distributions of topics over words and words over documents, topic models identify the most discriminatory groups of documents automatically. 


```r
require(quanteda)
require(quanteda.corpora)
require(lubridate)
require(topicmodels)
```


```r
news_corp <- download('data_corpus_guardian')
```



We only select news stories published in 2016 using `corpus_subset()`. 


```r
news_corp <- corpus_subset(news_corp, year(docvars(news_corp, 'date')) >= 2016)
ndoc(news_corp)
```

```
## [1] 1959
```

Further, after removal of function words and punctuations in `dfm()`, we remove rare and common features and using `dfm_trim()` to reduce time to fit a model.


```r
news_dfm <- dfm(news_corp, remove_punct = TRUE, remove = stopwords('en')) %>% 
            dfm_remove(c('*-time', 'updated-*')) %>% 
            dfm_trim(min_count = 0.95, max_docfreq = 0.1)
news_dfm <- news_dfm[ntoken(news_dfm) > 0,]
```

**quanteda** does not implement own topic models, but you can easily access to `LDA()` from the **topicmodel** package through `convert()`. `k = 10` specifies the number of topics to be discovered.  


```r
dtm <- convert(news_dfm, to = "topicmodels")
lda <- LDA(dtm, k = 10)
```

You can extract most important terms from the model using `terms()`.


```r
terms(lda, 10)
```

```
##       Topic 1      Topic 2       Topic 3   Topic 4    Topic 5    
##  [1,] "food"       "climate"     "clinton" "refugees" "sales"    
##  [2,] "water"      "energy"      "sanders" "syria"    "customers"
##  [3,] "de"         "housing"     "cruz"    "isis"     "apple"    
##  [4,] "el"         "gas"         "gmt"     "syrian"   "china"    
##  [5,] "sea"        "development" "hillary" "military" "google"   
##  [6,] "island"     "funding"     "obama"   "islamic"  "users"    
##  [7,] "litvinenko" "project"     "trump's" "un"       "games"    
##  [8,] "la"         "homes"       "bst"     "turkey"   "sale"     
##  [9,] "revolution" "khan"        "bernie"  "muslim"   "sold"     
## [10,] "scientists" "education"   "ted"     "aid"      "iphone"   
##       Topic 6      Topic 7    Topic 8      Topic 9      Topic 10   
##  [1,] "gmt"        "officers" "corbyn"     "doctors"    "oil"      
##  [2,] "bst"        "prison"   "johnson"    "violence"   "markets"  
##  [3,] "labor"      "arrested" "gmt"        "australia"  "prices"   
##  [4,] "turnbull"   "shooting" "shadow"     "nhs"        "banks"    
##  [5,] "brussels"   "black"    "boris"      "hospital"   "rates"    
##  [6,] "19"         "incident" "leadership" "drug"       "investors"
##  [7,] "talks"      "officer"  "tory"       "medical"    "bst"      
##  [8,] "senate"     "victims"  "jeremy"     "child"      "shares"   
##  [9,] "benefits"   "criminal" "cabinet"    "australian" "trading"  
## [10,] "australian" "dead"     "scottish"   "cases"      "quarter"
```

You can then obtain the most likely topics using `topics()` and save as a document-level variable.


```r
docvars(news_dfm, 'topic') <- topics(lda)
head(topics(lda), 20)
```

```
## text136751 text136585 text139163 text169133 text153451 text163885 
##          2          8          5          7          7          2 
## text157885 text173244 text137394 text169408 text184646 text127410 
##          3          2          8          1          8          1 
## text134923 text169695 text147917 text157535 text177078 text174393 
##          8         10          1          2          2          3 
## text181782 text143323 
##          1          8
```

