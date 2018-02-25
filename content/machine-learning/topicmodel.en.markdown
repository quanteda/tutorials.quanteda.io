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
##       Topic 1      Topic 2   Topic 3    Topic 4      Topic 5     
##  [1,] "sales"      "clinton" "doctors"  "corbyn"     "australia" 
##  [2,] "housing"    "sanders" "hospital" "shadow"     "australian"
##  [3,] "customers"  "cruz"    "child"    "leadership" "labor"     
##  [4,] "apple"      "hillary" "nhs"      "khan"       "turnbull"  
##  [5,] "google"     "obama"   "mental"   "jeremy"     "budget"    
##  [6,] "income"     "trump's" "parents"  "tory"       "senate"    
##  [7,] "businesses" "bernie"  "girls"    "parties"    "funding"   
##  [8,] "users"      "ted"     "sexual"   "scottish"   "violence"  
##  [9,] "homes"      "rubio"   "mother"   "commons"    "malcolm"   
## [10,] "average"    "senator" "son"      "scotland"   "coalition" 
##       Topic 6        Topic 7     Topic 8         Topic 9       Topic 10  
##  [1,] "brussels"     "oil"       "climate"       "refugees"    "officers"
##  [2,] "johnson"      "markets"   "water"         "syrian"      "isis"    
##  [3,] "talks"        "china"     "energy"        "syria"       "military"
##  [4,] "boris"        "prices"    "food"          "turkey"      "prison"  
##  [5,] "benefits"     "chinese"   "project"       "un"          "islamic" 
##  [6,] "19"           "banks"     "development"   "aid"         "forces"  
##  [7,] "summit"       "investors" "gas"           "immigration" "criminal"
##  [8,] "french"       "rates"     "environmental" "refugee"     "arrested"
##  [9,] "negotiations" "shares"    "game"          "asylum"      "victims" 
## [10,] "cameron's"    "trading"   "environment"   "border"      "officer"
```

You can then obtain the most likely topics using `topics()` and save them as a document-level variable.


```r
docvars(news_dfm, 'topic') <- topics(lda)
head(topics(lda), 20)
```

```
## text136751 text136585 text139163 text169133 text153451 text163885 
##          8          4          1          3         10          8 
## text157885 text173244 text137394 text169408 text184646 text127410 
##          2          4          4          3          4         10 
## text134923 text169695 text147917 text157535 text177078 text174393 
##          6          7          3          8          8          2 
## text181782 text143323 
##          3          4
```

{{% notice info %}}
If you want to learn more about Topic Models, see:  
Blei, David M., Andrew Y. Ng, and Michael I. Jordan. 2003. "Latent Dirichlet Allocation."" _The Journal of Machine Learning Research_ 3(1): 993-1022.
{{% /notice %}}

