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
corp_news <- download('data_corpus_guardian')
```



We only select news stories published in 2016 using `corpus_subset()`. 


```r
corp_news_subset <- corpus_subset(corp_news, 'date' >= 2016)
ndoc(corp_news_subset)
```

```
## [1] 6000
```

Further, after removal of function words and punctuation in `dfm()`, we keep only the top 5% of the most frequent features (`min_termfreq = 0.95`) that appear in less than 10% of all documents (`max_docfreq = 0.1`)
 using `dfm_trim()` to focus on common but distinguishing features.


```r
dfmat_news <- dfm(corp_news, remove_punct = TRUE, remove = stopwords('en')) %>% 
    dfm_remove(c('*-time', '*-timeUpdated', 'GMT', 'BST')) %>% 
    dfm_trim(min_termfreq = 0.95, termfreq_type = "quantile", 
             max_docfreq = 0.1, docfreq_type = "prop")

dfmat_news <- dfmat_news[ntoken(dfmat_news) > 0,]
```

**quanteda** does not implement topic models, but you can easily access `LDA()` from the **topicmodel** package through `convert()`. `k = 10` specifies the number of topics to be discovered. This is an important parameter and you should try a variety of values.


```r
dtm <- convert(dfmat_news, to = "topicmodels")
lda <- LDA(dtm, k = 10)
```

You can extract the most important terms for each topic from the model using `terms()`.


```r
terms(lda, 10)
```

```
##       Topic 1         Topic 2      Topic 3         Topic 4   Topic 5     
##  [1,] "climate"       "australia"  "officers"      "game"    "mps"       
##  [2,] "energy"        "australian" "investigation" "music"   "referendum"
##  [3,] "water"         "budget"     "justice"       "friends" "corbyn"    
##  [4,] "food"          "cuts"       "officer"       "town"    "tory"      
##  [5,] "development"   "labor"      "prison"        "games"   "mp"        
##  [6,] "gas"           "funding"    "arrested"      "film"    "johnson"   
##  [7,] "emissions"     "osborne"    "trial"         "road"    "scottish"  
##  [8,] "project"       "spending"   "allegations"   "park"    "voters"    
##  [9,] "environmental" "turnbull"   "criminal"      "airport" "brexit"    
## [10,] "environment"   "changes"    "inquiry"       "space"   "2016"      
##       Topic 6     Topic 7     Topic 8    Topic 9        Topic 10   
##  [1,] "nhs"       "child"     "syria"    "trump"        "markets"  
##  [2,] "customers" "education" "military" "clinton"      "prices"   
##  [3,] "online"    "parents"   "refugees" "republican"   "rate"     
##  [4,] "data"      "violence"  "isis"     "sanders"      "shares"   
##  [5,] "sales"     "students"  "attacks"  "obama"        "oil"      
##  [6,] "patients"  "schools"   "russian"  "donald"       "rates"    
##  [7,] "users"     "mental"    "syrian"   "cruz"         "banks"    
##  [8,] "buy"       "church"    "forces"   "presidential" "investors"
##  [9,] "google"    "girls"     "russia"   "2016"         "quarter"  
## [10,] "apple"     "society"   "islamic"  "hillary"      "euro"
```

You can then obtain the most likely topics using `topics()` and save them as a document-level variable.


```r
docvars(dfmat_news, 'topic') <- topics(lda)
head(topics(lda), 20)
```

```
## text136751 text118588  text45146  text93623 text136585  text65682 text107174 
##          1          4         10          2          5          3          6 
##  text22792  text32425 text139163 text169133  text90312 text153451  text31104 
##          3          8          6          4          1          3          5 
## text163885  text81309 text157885  text99128 text173244  text27905 
##          1         10          4          4          1          8
```

{{% notice info %}}
If you want to learn more about Topic Models, see:  
Blei, David M., Andrew Y. Ng, and Michael I. Jordan. 2003. "Latent Dirichlet Allocation."" _The Journal of Machine Learning Research_ 3(1): 993-1022.
{{% /notice %}}

