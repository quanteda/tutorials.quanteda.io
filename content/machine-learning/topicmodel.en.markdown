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

Further, after removal of function words and punctuations in `dfm()`, we keep only the top 5% of the most frequent features (`min_termfreq = 0.95`) that appear in less than 10% of all documents (`max_docfreq = 0.1`)
 using `dfm_trim()` to focus on common but distinguishing features.


```r
news_dfm <- dfm(news_corp, remove_punct = TRUE, remove = stopwords('en')) %>% 
            dfm_remove(c('*-time', '*-timeUpdated', 'GMT', 'BST')) %>% 
            dfm_trim(min_termfreq = 0.95, termfreq_type = "quantile", 
                     max_docfreq = 0.1, docfreq_type = "prop")
news_dfm <- news_dfm[ntoken(news_dfm) > 0,]
```

**quanteda** does not implement its own topic models, but you can easily access `LDA()` from the **topicmodel** package through `convert()`. `k = 10` specifies the number of topics to be discovered. This is an important parameter and you should try a variety of values.


```r
dtm <- convert(news_dfm, to = "topicmodels")
lda <- LDA(dtm, k = 10)
```

You can extract the most important terms for each topic from the model using `terms()`.


```r
terms(lda, 10)
```

```
##       Topic 1      Topic 2     Topic 3    Topic 4    Topic 5      
##  [1,] "australian" "oil"       "doctors"  "isis"     "climate"    
##  [2,] "australia"  "markets"   "nhs"      "syria"    "energy"     
##  [3,] "labor"      "sales"     "scotland" "military" "water"      
##  [4,] "turnbull"   "prices"    "junior"   "islamic"  "food"       
##  [5,] "budget"     "rates"     "drug"     "un"       "apple"      
##  [6,] "senate"     "shares"    "funding"  "forces"   "development"
##  [7,] "coalition"  "investors" "scottish" "muslim"   "project"    
##  [8,] "malcolm"    "banks"     "contract" "russian"  "homes"      
##  [9,] "liberal"    "trading"   "patients" "syrian"   "google"     
## [10,] "cuts"       "quarter"   "medical"  "peace"    "gas"        
##       Topic 6      Topic 7    Topic 8   Topic 9       Topic 10  
##  [1,] "corbyn"     "khan"     "clinton" "refugees"    "officers"
##  [2,] "johnson"    "game"     "sanders" "immigration" "violence"
##  [3,] "brussels"   "students" "cruz"    "turkey"      "prison"  
##  [4,] "boris"      "age"      "hillary" "china"       "victims" 
##  [5,] "talks"      "church"   "obama"   "refugee"     "abuse"   
##  [6,] "19"         "child"    "trump's" "asylum"      "sexual"  
##  [7,] "benefits"   "birth"    "bernie"  "border"      "criminal"
##  [8,] "cabinet"    "parents"  "ted"     "chinese"     "charges" 
##  [9,] "membership" "study"    "rubio"   "aid"         "officer" 
## [10,] "summit"     "felt"     "senator" "sea"         "arrested"
```

You can then obtain the most likely topics using `topics()` and save them as a document-level variable.


```r
docvars(news_dfm, 'topic') <- topics(lda)
head(topics(lda), 20)
```

```
## text136751 text136585 text139163 text169133 text153451 text163885 
##          5          9          5          7         10          9 
## text157885 text173244 text137394 text169408 text184646 text127410 
##          7          9          1          7          6         10 
## text134923 text169695 text147917 text157535 text177078 text174393 
##          6          2          7          9          5          8 
## text181782 text143323 
##          7          3
```

{{% notice info %}}
If you want to learn more about Topic Models, see:  
Blei, David M., Andrew Y. Ng, and Michael I. Jordan. 2003. "Latent Dirichlet Allocation."" _The Journal of Machine Learning Research_ 3(1): 993-1022.
{{% /notice %}}

