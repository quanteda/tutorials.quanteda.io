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

Further, after removal of function words and punctuation in `dfm()`, we keep only the top 5% of the most frequent features (`min_termfreq = 0.95`) that appear in less than 10% of all documents (`max_docfreq = 0.1`) using `dfm_trim()` to focus on common but distinguishing features.


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
##       topic1       topic2      topic3          topic4     topic5       topic6  
##  [1,] "housing"    "oil"       "climate"       "syria"    "labor"      "church"
##  [2,] "sales"      "markets"   "water"         "isis"     "corbyn"     "love"  
##  [3,] "apple"      "prices"    "energy"        "military" "turnbull"   "game"  
##  [4,] "customers"  "banks"     "food"          "un"       "budget"     "story" 
##  [5,] "google"     "investors" "gas"           "islamic"  "johnson"    "video" 
##  [6,] "users"      "rates"     "project"       "syrian"   "australian" "wife"  
##  [7,] "technology" "shares"    "air"           "forces"   "shadow"     "son"   
##  [8,] "property"   "trading"   "environmental" "china"    "leadership" "events"
##  [9,] "iphone"     "quarter"   "land"          "muslim"   "coalition"  "black" 
## [10,] "app"        "sector"    "residents"     "peace"    "senate"     "couple"
##       topic7     topic8    topic9      topic10      
##  [1,] "officers" "clinton" "doctors"   "refugees"   
##  [2,] "prison"   "sanders" "violence"  "brussels"   
##  [3,] "victims"  "cruz"    "nhs"       "talks"      
##  [4,] "criminal" "hillary" "education" "french"     
##  [5,] "officer"  "obama"   "funding"   "immigration"
##  [6,] "arrested" "trump's" "hospital"  "summit"     
##  [7,] "incident" "bernie"  "australia" "migrants"   
##  [8,] "alleged"  "ted"     "medical"   "benefits"   
##  [9,] "shooting" "rubio"   "drug"      "refugee"    
## [10,] "dead"     "senator" "schools"   "asylum"
```

You can then obtain the most likely topics using `topics()` and save them as a document-level variable.


```r
head(topics(tmod_lda), 20)
```

```
##  [1] "topic3"  "topic5"  "topic1"  "topic6"  "topic7"  "topic3"  "topic6" 
##  [8] "topic3"  "topic5"  "topic6"  "topic5"  "topic7"  "topic10" "topic2" 
## [15] "topic6"  "topic3"  "topic3"  "topic8"  "topic6"  "topic5"
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
##     218      86     181     186     167     275     216     227     191     212
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
##       economy       politics      society       diplomacy    military    
##  [1,] "markets"     "politicians" "hospital"    "treaty"     "military"  
##  [2,] "banks"       "elections"   "schools"     "ambassador" "terrorist" 
##  [3,] "stock"       "politician"  "prison"      "diplomatic" "army"      
##  [4,] "banking"     "voter"       "hospitals"   "diplomats"  "soldiers"  
##  [5,] "shop"        "lawmakers"   "prisons"     "diplomat"   "terrorists"
##  [6,] "shopping"    "clinton"     "prisoners"   "embassy"    "navy"      
##  [7,] "bank's"      "sanders"     "hospitality" "labor"      "marine"    
##  [8,] "marketing"   "cruz"        "prisoner"    "corbyn"     "soldier"   
##  [9,] "shops"       "obama"       "violence"    "turnbull"   "refugees"  
## [10,] "bankers"     "hillary"     "officers"    "johnson"    "syria"     
## [11,] "stocks"      "trump's"     "victims"     "budget"     "water"     
## [12,] "shoppers"    "bernie"      "cases"       "cabinet"    "isis"      
## [13,] "bond"        "senator"     "parents"     "australian" "syrian"    
## [14,] "bonds"       "ted"         "sexual"      "talks"      "un"        
## [15,] "bankruptcy"  "rubio"       "abuse"       "brussels"   "turkey"    
## [16,] "bankrupt"    "gun"         "child"       "benefits"   "islamic"   
## [17,] "banker"      "primary"     "drug"        "shadow"     "aid"       
## [18,] "stockport"   "race"        "facebook"    "coalition"  "climate"   
## [19,] "marketplace" "candidates"  "mental"      "leadership" "forces"    
## [20,] "oil"         "kasich"      "officer"     "australia"  "air"
```

`topics()` on returns dictionary keys as the most likely topics of documents.


```r
head(topics(tmod_slda), 20)
```

```
##  [1] "economy"   "diplomacy" "economy"   "society"   "society"   "military" 
##  [7] "politics"  "military"  "diplomacy" "society"   "diplomacy" "military" 
## [13] "diplomacy" "economy"   "politics"  "economy"   "economy"   "society"  
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
##       363       473       354       217       552
```

{{% notice ref %}}

- Blei, David M., Andrew Y. Ng, and Michael I. Jordan. 2003. "Latent Dirichlet Allocation." _The Journal of Machine Learning Research_ 3(1): 993-1022.  
- Lu, B., Ott, M., Cardie, C., & Tsou, B. K. 2011. "[Multi-aspect sentiment analysis with topic models_](https://www.cs.cornell.edu/home/cardie/papers/masa-sentire-2011.pdf)". _Proceeding of the 2011 IEEE 11th International Conference on Data Mining Workshops_, 81–88.

{{% /notice %}}

