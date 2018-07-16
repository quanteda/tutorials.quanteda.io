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
##       Topic 1     Topic 2       Topic 3     Topic 4     Topic 5   
##  [1,] "violence"  "australia"   "climate"   "oil"       "apple"   
##  [2,] "senate"    "australian"  "energy"    "markets"   "game"    
##  [3,] "education" "funding"     "water"     "sales"     "users"   
##  [4,] "girls"     "development" "housing"   "prices"    "google"  
##  [5,] "schools"   "food"        "gas"       "shares"    "facebook"
##  [6,] "labor"     "businesses"  "homes"     "investors" "hospital"
##  [7,] "gun"       "aid"         "khan"      "trading"   "games"   
##  [8,] "marriage"  "offshore"    "average"   "banks"     "phone"   
##  [9,] "laws"      "fund"        "emissions" "rates"     "music"   
## [10,] "australia" "sector"      "property"  "quarter"   "chinese" 
##       Topic 6      Topic 7    Topic 8      Topic 9   Topic 10  
##  [1,] "turnbull"   "officers" "corbyn"     "clinton" "syria"   
##  [2,] "budget"     "prison"   "refugees"   "sanders" "isis"    
##  [3,] "brussels"   "drug"     "johnson"    "cruz"    "military"
##  [4,] "19"         "sexual"   "jeremy"     "hillary" "syrian"  
##  [5,] "labor"      "abuse"    "doctors"    "obama"   "islamic" 
##  [6,] "benefits"   "child"    "boris"      "trump's" "forces"  
##  [7,] "talks"      "drugs"    "leadership" "bernie"  "un"      
##  [8,] "summit"     "cases"    "tory"       "ted"     "russian" 
##  [9,] "photograph" "criminal" "shadow"     "rubio"   "muslim"  
## [10,] "english"    "trial"    "junior"     "senator" "peace"
```

You can then obtain the most likely topics using `topics()` and save them as a document-level variable.


```r
docvars(news_dfm, 'topic') <- topics(lda)
head(topics(lda), 20)
```

```
## text136751 text136585 text139163 text169133 text153451 text163885 
##          3          8          2         10          7          3 
## text157885 text173244 text137394 text169408 text184646 text127410 
##          9          3          1          5          8          7 
## text134923 text169695 text147917 text157535 text177078 text174393 
##          8          4          5          3          3          9 
## text181782 text143323 
##          5          8
```

{{% notice info %}}
If you want to learn more about Topic Models, see:  
Blei, David M., Andrew Y. Ng, and Michael I. Jordan. 2003. "Latent Dirichlet Allocation."" _The Journal of Machine Learning Research_ 3(1): 993-1022.
{{% /notice %}}

