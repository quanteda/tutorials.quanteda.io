---
title: Topic models
weight: 50
draft: false
---

Topics models are unsupervised document classification techniques. By modeling distributions of topics over words and words over documents, topic models identify the most discriminatory groups of documents automatically. 


```r
require(quanteda)
require(quanteda.corpora)
require(seededlda)
require(lubridate)
```


```r
corp_news <- download("data_corpus_guardian")
```



We only select news articles published in 2016 using `corpus_subset()` and the `year` function from the **lubridate** package. 


```r
corp_news_2016 <- corpus_subset(corp_news, year(date) == 2016)
ndoc(corp_news_2016)
```

```
## [1] 1959
```

Further, after removal of function words and punctuation in `dfm()`, we keep only the top 5% of the most frequent features (`min_termfreq = 0.8`) that appear in less than 10% of all documents (`max_docfreq = 0.1`) using `dfm_trim()` to focus on common but distinguishing features.


```r
dfmat_news <- dfm(corp_news_2016, 
                  remove_punct = TRUE, remove_numbers = TRUE, remove_symbol = TRUE,
                  remove = c(stopwords("en"), "*-time", "*-timeUpdated", "GMT", "BST")) %>% 
              dfm_trim(min_termfreq = 0.8, termfreq_type = "quantile",
                       max_docfreq = 0.1, docfreq_type = "prop")
```

**quanteda** does not implement topic models, but you can fit LDA and seeded-LDA with the **seededlda** package.

### LDA

`k = 10` specifies the number of topics to be discovered. This is an important parameter and you should try a variety of values and validate the outputs of your topic models thoroughly.


```r
tmod_lda <- textmodel_lda(dfmat_news, k = 10)
```

You can extract the most important terms for each topic from the model using `terms()`.


```r
terms(tmod_lda, 10)
```

```
##       topic1      topic2   topic3       topic4        topic5      
##  [1,] "oil"       "church" "sales"      "refugees"    "labor"     
##  [2,] "markets"   "love"   "housing"    "brussels"    "corbyn"    
##  [3,] "prices"    "son"    "apple"      "talks"       "turnbull"  
##  [4,] "banks"     "story"  "customers"  "french"      "budget"    
##  [5,] "investors" "wife"   "google"     "immigration" "johnson"   
##  [6,] "shares"    "game"   "technology" "migrants"    "shadow"    
##  [7,] "trading"   "mother" "users"      "summit"      "australian"
##  [8,] "rates"     "room"   "sold"       "refugee"     "leadership"
##  [9,] "sector"    "music"  "iphone"     "benefits"    "coalition" 
## [10,] "quarter"   "black"  "buy"        "asylum"      "senate"    
##       topic6          topic7    topic8      topic9     topic10   
##  [1,] "climate"       "clinton" "doctors"   "syria"    "officers"
##  [2,] "water"         "sanders" "nhs"       "isis"     "prison"  
##  [3,] "energy"        "cruz"    "education" "military" "victims" 
##  [4,] "gas"           "hillary" "violence"  "islamic"  "criminal"
##  [5,] "food"          "obama"   "funding"   "syrian"   "officer" 
##  [6,] "project"       "trump's" "medical"   "un"       "crime"   
##  [7,] "air"           "bernie"  "australia" "forces"   "incident"
##  [8,] "environmental" "ted"     "hospital"  "muslim"   "arrested"
##  [9,] "residents"     "rubio"   "drug"      "china"    "shooting"
## [10,] "sea"           "senator" "drugs"     "peace"    "charges"
```

You can then obtain the most likely topics using `topics()` and save them as a document-level variable.


```r
head(topics(tmod_lda), 20)
```

```
##  [1] "topic6"  "topic5"  "topic3"  "topic2"  "topic10" "topic6"  "topic2" 
##  [8] "topic6"  "topic5"  "topic2"  "topic5"  "topic10" "topic4"  "topic1" 
## [15] "topic2"  "topic6"  "topic6"  "topic7"  "topic2"  "topic5"
```

```r
# assign topic as a new document-level variable
dfmat_news$topic <- topics(tmod_lda)

# cross-table of the topic frequency
table(dfmat_news$topic)
```

```
## 
##  topic1 topic10  topic2  topic3  topic4  topic5  topic6  topic7  topic8  topic9 
##     197     220     218     213      82     270     179     195     215     170
```

### Seeded LDA

In the seeded LDA, you can predefine topics in LDA using a dictionary of "seed" words.


```r
# load dictionary containing seed words
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

The number of topics is determined by the number of keys in the dictionary. Next, we fit the seeded LDA model using `textmodel_seededlda()` and specify the dictionary with our relevant keywords.


```r
tmod_slda <- textmodel_seededlda(dfmat_news, dictionary = dict_topic)
```

Some of the topic words are seed words, but the seeded LDA identifies many other related words.


```r
terms(tmod_slda, 20)
```

```
##       economy       politics      society       diplomacy      military    
##  [1,] "markets"     "politicians" "hospital"    "treaty"       "military"  
##  [2,] "banks"       "elections"   "schools"     "ambassador"   "terrorist" 
##  [3,] "stock"       "politician"  "prison"      "diplomatic"   "army"      
##  [4,] "banking"     "voter"       "hospitals"   "diplomats"    "soldiers"  
##  [5,] "shop"        "lawmakers"   "prisons"     "diplomat"     "terrorists"
##  [6,] "shopping"    "clinton"     "prisoners"   "embassy"      "navy"      
##  [7,] "bank's"      "sanders"     "hospitality" "corbyn"       "marine"    
##  [8,] "marketing"   "cruz"        "prisoner"    "johnson"      "soldier"   
##  [9,] "shops"       "obama"       "officers"    "brussels"     "australia" 
## [10,] "bankers"     "hillary"     "violence"    "cabinet"      "australian"
## [11,] "stocks"      "trump's"     "sexual"      "talks"        "refugees"  
## [12,] "shoppers"    "senator"     "cases"       "benefits"     "syria"     
## [13,] "bond"        "bernie"      "drug"        "shadow"       "labor"     
## [14,] "bonds"       "ted"         "parents"     "chancellor"   "turnbull"  
## [15,] "bankruptcy"  "rubio"       "abuse"       "boris"        "isis"      
## [16,] "bankrupt"    "gun"         "child"       "leadership"   "syrian"    
## [17,] "banker"      "primary"     "facebook"    "negotiations" "un"        
## [18,] "stockport"   "race"        "medical"     "doctors"      "aid"       
## [19,] "marketplace" "candidates"  "water"       "tory"         "islamic"   
## [20,] "oil"         "kasich"      "officer"     "jeremy"       "forces"
```

`topics()` on returns dictionary keys as the most likely topics of documents.


```r
head(topics(tmod_slda), 20)
```

```
##  [1] "economy"   "diplomacy" "economy"   "society"   "society"   "economy"  
##  [7] "politics"  "diplomacy" "diplomacy" "society"   "diplomacy" "society"  
## [13] "diplomacy" "economy"   "society"   "economy"   "economy"   "society"  
## [19] "society"   "diplomacy"
```

```r
# assign topics from seeded LDA as a document-level variable to the dfm
dfmat_news$topic2 <- topics(tmod_slda)

# cross-table of the topic frequency
table(dfmat_news$topic2)
```

```
## 
## diplomacy   economy  military  politics   society 
##       287       508       319       220       625
```

{{% notice ref %}}

- Blei, David M., Andrew Y. Ng, and Michael I. Jordan. 2003. "Latent Dirichlet Allocation." _The Journal of Machine Learning Research_ 3(1): 993-1022.  
- Lu, B., Ott, M., Cardie, C., & Tsou, B. K. 2011. "[Multi-aspect sentiment analysis with topic models](https://www.cs.cornell.edu/home/cardie/papers/masa-sentire-2011.pdf)". _Proceeding of the 2011 IEEE 11th International Conference on Data Mining Workshops_, 81–88.

{{% /notice %}}

