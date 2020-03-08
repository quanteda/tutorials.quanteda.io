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
##       Topic 1     Topic 2    Topic 3     Topic 4    Topic 5        
##  [1,] "prices"    "mps"      "water"     "syria"    "officers"     
##  [2,] "rate"      "corbyn"   "game"      "military" "investigation"
##  [3,] "oil"       "tory"     "music"     "refugees" "justice"      
##  [4,] "markets"   "osborne"  "games"     "isis"     "officer"      
##  [5,] "rates"     "mp"       "friends"   "russian"  "prison"       
##  [6,] "shares"    "shadow"   "town"      "syrian"   "abuse"        
##  [7,] "sales"     "scotland" "park"      "forces"   "trial"        
##  [8,] "banks"     "cuts"     "space"     "attacks"  "victims"      
##  [9,] "investors" "scottish" "film"      "russia"   "sexual"       
## [10,] "quarter"   "voters"   "residents" "islamic"  "arrested"     
##       Topic 6        Topic 7     Topic 8       Topic 9      Topic 10     
##  [1,] "trump"        "customers" "australia"   "french"     "climate"    
##  [2,] "clinton"      "online"    "australian"  "brussels"   "energy"     
##  [3,] "republican"   "account"   "labor"       "2016"       "food"       
##  [4,] "sanders"      "firm"      "turnbull"    "greece"     "development"
##  [5,] "obama"        "data"      "education"   "paris"      "nhs"        
##  [6,] "donald"       "stores"    "senate"      "february"   "doctors"    
##  [7,] "cruz"         "website"   "federal"     "brexit"     "patients"   
##  [8,] "presidential" "sales"     "schools"     "talks"      "data"       
##  [9,] "2016"         "buy"       "aest"        "referendum" "gas"        
## [10,] "hillary"      "deals"     "immigration" "france"     "emissions"
```

You can then obtain the most likely topics using `topics()` and save them as a document-level variable.


```r
dfmat_news$topic <- topics(lda)
head(topics(lda), 20)
```

```
## text136751 text118588  text45146  text93623 text136585  text65682 text107174 
##         10          3          1          8          9          5          7 
##  text22792  text32425 text139163 text169133  text90312 text153451  text31104 
##          5          4          7          5          8          5          2 
## text163885  text81309 text157885  text99128 text173244  text27905 
##         10          1          3          3          7          4
```

{{% notice info %}}
If you want to learn more about Topic Models, see:  
Blei, David M., Andrew Y. Ng, and Michael I. Jordan. 2003. "Latent Dirichlet Allocation."" _The Journal of Machine Learning Research_ 3(1): 993-1022.
{{% /notice %}}

