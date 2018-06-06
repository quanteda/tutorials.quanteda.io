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
            dfm_remove(c('*-time', '*-timeUpdated', 'GMT', 'BST')) %>% 
            dfm_trim(min_termfreq = 0.95, termfreq_type = "quantile", max_docfreq = 0.1)
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
##       Topic 1      Topic 2    Topic 3     Topic 4    Topic 5    Topic 6   
##  [1,] "gmt"        "officers" "oil"       "apple"    "doctors"  "refugees"
##  [2,] "cruz"       "violence" "markets"   "facebook" "nhs"      "syria"   
##  [3,] "rubio"      "prison"   "sales"     "game"     "hospital" "isis"    
##  [4,] "candidates" "child"    "prices"    "phone"    "food"     "syrian"  
##  [5,] "sanders"    "victims"  "investors" "google"   "drug"     "military"
##  [6,] "senator"    "abuse"    "shares"    "video"    "medical"  "islamic" 
##  [7,] "iowa"       "sexual"   "gmt"       "users"    "china"    "un"      
##  [8,] "ted"        "criminal" "bst"       "music"    "drugs"    "turkey"  
##  [9,] "poll"       "officer"  "banks"     "games"    "patients" "muslim"  
## [10,] "voting"     "crime"    "rates"     "iphone"   "junior"   "forces"  
##       Topic 7     Topic 8       Topic 9    Topic 10    
##  [1,] "clinton"   "climate"     "gmt"      "australia" 
##  [2,] "sanders"   "energy"      "corbyn"   "australian"
##  [3,] "hillary"   "water"       "brussels" "bst"       
##  [4,] "bst"       "housing"     "johnson"  "labor"     
##  [5,] "obama"     "gas"         "boris"    "turnbull"  
##  [6,] "gun"       "funding"     "talks"    "senate"    
##  [7,] "trump's"   "income"      "cabinet"  "malcolm"   
##  [8,] "cruz"      "homes"       "benefits" "budget"    
##  [9,] "bernie"    "development" "jeremy"   "coalition" 
## [10,] "delegates" "green"       "tory"     "greens"
```

You can then obtain the most likely topics using `topics()` and save them as a document-level variable.


```r
docvars(news_dfm, 'topic') <- topics(lda)
head(topics(lda), 20)
```

```
## text136751 text136585 text139163 text169133 text153451 text163885 
##          8          1          8          4          2          8 
## text157885 text173244 text137394 text169408 text184646 text127410 
##          7          8          1          4          9          2 
## text134923 text169695 text147917 text157535 text177078 text174393 
##          9          3          4          8          8          7 
## text181782 text143323 
##          5          9
```

{{% notice info %}}
If you want to learn more about Topic Models, see:  
Blei, David M., Andrew Y. Ng, and Michael I. Jordan. 2003. "Latent Dirichlet Allocation."" _The Journal of Machine Learning Research_ 3(1): 993-1022.
{{% /notice %}}

