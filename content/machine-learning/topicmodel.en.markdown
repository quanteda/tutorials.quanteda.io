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

Further, after removal of function words and punctuations in `dfm()`, we keep only top 5% of the most frequent features (`min_termfreq = 0.95`) that appear less than 10% of all document (`max_docfreq = 0.1`)
 using `dfm_trim()` to focus to common but distinguishing features.


```r
news_dfm <- dfm(news_corp, remove_punct = TRUE, remove = stopwords('en')) %>% 
            dfm_remove(c('*-time', '*-timeUpdated', 'GMT', 'BST')) %>% 
            dfm_trim(min_termfreq = 0.95, termfreq_type = "quantile", 
                     max_docfreq = 0.1, docfreq_type = "prop")
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
##       Topic 1   Topic 2      Topic 3    Topic 4      Topic 5      
##  [1,] "clinton" "corbyn"     "doctors"  "funding"    "climate"    
##  [2,] "sanders" "leadership" "hospital" "budget"     "water"      
##  [3,] "cruz"    "johnson"    "nhs"      "cuts"       "food"       
##  [4,] "obama"   "shadow"     "housing"  "income"     "drug"       
##  [5,] "hillary" "cabinet"    "homes"    "businesses" "energy"     
##  [6,] "trump's" "parties"    "patients" "energy"     "population" 
##  [7,] "bernie"  "tory"       "junior"   "scheme"     "drugs"      
##  [8,] "ted"     "khan"       "medical"  "green"      "development"
##  [9,] "rubio"   "jeremy"     "church"   "benefits"   "study"      
## [10,] "senator" "liberal"    "contract" "review"     "air"        
##       Topic 6      Topic 7    Topic 8        Topic 9    Topic 10   
##  [1,] "violence"   "refugees" "brussels"     "apple"    "oil"      
##  [2,] "officers"   "syria"    "talks"        "sales"    "labor"    
##  [3,] "prison"     "isis"     "19"           "google"   "markets"  
##  [4,] "victims"    "syrian"   "benefits"     "game"     "turnbull" 
##  [5,] "sexual"     "military" "ireland"      "china"    "prices"   
##  [6,] "australia"  "turkey"   "summit"       "users"    "banks"    
##  [7,] "abuse"      "un"       "french"       "facebook" "investors"
##  [8,] "australian" "islamic"  "negotiations" "games"    "shares"   
##  [9,] "criminal"   "aid"      "johnson"      "chinese"  "rates"    
## [10,] "arrested"   "refugee"  "photograph"   "iphone"   "senate"
```

You can then obtain the most likely topics using `topics()` and save them as a document-level variable.


```r
docvars(news_dfm, 'topic') <- topics(lda)
head(topics(lda), 20)
```

```
## text136751 text136585 text139163 text169133 text153451 text163885 
##          7          6          7          4          4          7 
## text157885 text173244 text137394 text169408 text184646 text127410 
##          9          1         10          3          6          4 
## text134923 text169695 text147917 text157535 text177078 text174393 
##          6          2          3          7          7          9 
## text181782 text143323 
##          1          6
```

{{% notice info %}}
If you want to learn more about Topic Models, see:  
Blei, David M., Andrew Y. Ng, and Michael I. Jordan. 2003. "Latent Dirichlet Allocation."" _The Journal of Machine Learning Research_ 3(1): 993-1022.
{{% /notice %}}

