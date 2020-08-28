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
require(seededlda)
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

**quanteda** does not implement topic models, but you can fit LDA and seeded-LDA with the **seededlda** package.

### LDA

`k = 10` specifies the number of topics to be discovered. This is an important parameter and you should try a variety of values.


```r
tmod_lda <- textmodel_lda(dfmat_news, k = 10)
```

You can extract the most important terms for each topic from the model using `terms()`.


```r
terms(tmod_lda, 10)
```

```
##       topic1       topic2          topic3    topic4     topic5      
##  [1,] "mps"        "climate"       "died"    "online"   "australia" 
##  [2,] "referendum" "energy"        "town"    "game"     "australian"
##  [3,] "corbyn"     "food"          "friends" "games"    "nhs"       
##  [4,] "osborne"    "water"         "son"     "music"    "education" 
##  [5,] "mp"         "development"   "car"     "users"    "labor"     
##  [6,] "tory"       "gas"           "dead"    "internet" "funding"   
##  [7,] "cabinet"    "project"       "mother"  "google"   "doctors"   
##  [8,] "shadow"     "emissions"     "father"  "apple"    "child"     
##  [9,] "johnson"    "environmental" "road"    "twitter"  "schools"   
## [10,] "parties"    "environment"   "shot"    "tv"       "patients"  
##       topic6          topic7         topic8     topic9     topic10    
##  [1,] "officers"      "trump"        "military" "markets"  "customers"
##  [2,] "investigation" "clinton"      "syria"    "rate"     "housing"  
##  [3,] "justice"       "republican"   "refugees" "prices"   "banks"    
##  [4,] "prison"        "sanders"      "isis"     "oil"      "sales"    
##  [5,] "officer"       "obama"        "attacks"  "rates"    "buy"      
##  [6,] "inquiry"       "donald"       "french"   "euro"     "income"   
##  [7,] "trial"         "cruz"         "forces"   "quarter"  "costs"    
##  [8,] "allegations"   "presidential" "russian"  "2016"     "property" 
##  [9,] "criminal"      "2016"         "syrian"   "greece"   "homes"    
## [10,] "crime"         "hillary"      "russia"   "eurozone" "cash"
```

You can then obtain the most likely topics using `topics()` and save them as a document-level variable.


```r
head(topics(tmod_lda), 20)
```

```
##  [1] "topic4"  "topic4"  "topic9"  "topic5"  "topic1"  "topic6"  "topic10"
##  [8] "topic3"  "topic8"  "topic10" "topic3"  "topic2"  "topic6"  "topic1" 
## [15] "topic2"  "topic9"  "topic4"  "topic8"  "topic2"  "topic8"
```

```r
dfmat_news$topic <- topics(tmod_lda)
```

### Seeded LDA

In the seeded LDA, you can predefine topics in LDA using a dictionary of "seed" words.


```r
dict_topic <- dictionary(file = "../dictionary/topics.yml")
print(dict_topic)
```

```
## Dictionary object with 5 key entries.
## - [economy]:
##   - market*, money, bank*, stock*, bond*, industry, company, shop*
## - [politics]:
##   - lawmaker*, politician*, election*, voter*
## - [society]:
##   - police, prison*, school*, hospital*
## - [diplomacy]:
##   - ambassador*, diplomat*, embassy, treaty
## - [military]:
##   - military, soldier*, terrorist*, marine, navy, army
```

The number of topics is determined by the number keys in the dictionary.


```r
tmod_slda <- textmodel_seededlda(dfmat_news, dictionary = dict_topic)
```

Some of the topic words are seed words, but the seeded LDA identifies many other related words.


```r
terms(tmod_slda, 20)
```

```
##       economy     politics       society         diplomacy    military    
##  [1,] "banks"     "voters"       "hospital"      "treaty"     "military"  
##  [2,] "markets"   "politicians"  "schools"       "ambassador" "army"      
##  [3,] "stock"     "elections"    "prison"        "embassy"    "terrorist" 
##  [4,] "banking"   "politician"   "hospitals"     "diplomatic" "soldiers"  
##  [5,] "shop"      "voter"        "prisoners"     "diplomats"  "terrorists"
##  [6,] "shopping"  "trump"        "prisons"       "diplomat"   "marine"    
##  [7,] "bank's"    "clinton"      "officers"      "mps"        "navy"      
##  [8,] "shops"     "republican"   "investigation" "referendum" "soldier"   
##  [9,] "marketing" "sanders"      "justice"       "budget"     "syria"     
## [10,] "shoppers"  "obama"        "child"         "australia"  "climate"   
## [11,] "bond"      "donald"       "officer"       "corbyn"     "refugees"  
## [12,] "bankers"   "cruz"         "cases"         "australian" "paris"     
## [13,] "stocks"    "york"         "woman"         "ministers"  "water"     
## [14,] "bonds"     "presidential" "medical"       "mp"         "isis"      
## [15,] "energy"    "2016"         "violence"      "osborne"    "french"    
## [16,] "sales"     "candidate"    "parents"       "cuts"       "attacks"   
## [17,] "rate"      "american"     "abuse"         "commission" "russian"   
## [18,] "prices"    "hillary"      "doctors"       "coalition"  "un"        
## [19,] "oil"       "black"        "trial"         "tory"       "forces"    
## [20,] "data"      "game"         "victims"       "labor"      "syrian"
```

`topics()` on returns dictionary keys as the most likely topics of documents.


```r
head(topics(tmod_slda), 20)
```

```
##  [1] "society"   "economy"   "economy"   "diplomacy" "diplomacy" "society"  
##  [7] "economy"   "society"   "military"  "economy"   "politics"  "diplomacy"
## [13] "society"   "diplomacy" "economy"   "economy"   "politics"  "military" 
## [19] "military"  "military"
```

```r
dfmat_news$topic2 <- topics(tmod_slda)
```

{{% notice info %}}
If you want to learn more about Topic Models, see:  
Blei, David M., Andrew Y. Ng, and Michael I. Jordan. 2003. "Latent Dirichlet Allocation." _The Journal of Machine Learning Research_ 3(1): 993-1022.  
Lu, B., Ott, M., Cardie, C., & Tsou, B. K. (2011). _Multi-aspect sentiment analysis with topic models_. 2011 IEEE 11th International Conference on Data Mining Workshops, 81–88.
{{% /notice %}}

