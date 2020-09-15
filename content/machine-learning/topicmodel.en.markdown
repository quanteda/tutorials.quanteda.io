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



We only select news stories published in 2016 using `corpus_subset()` and the `year` function from the **lubridate** package. 


```r
corp_news_2016 <- corpus_subset(corp_news, year(date) >= 2016)
ndoc(corp_news_2016)
```

```
## [1] 1959
```

Further, after removal of function words and punctuation in `dfm()`, we keep only the top 5% of the most frequent features (`min_termfreq = 0.95`) that appear in less than 10% of all documents (`max_docfreq = 0.1`) using `dfm_trim()` to focus on common but distinguishing features.


```r
dfmat_news <- corp_news_2016 %>% 
    dfm(remove_punct = TRUE, remove = stopwords("en")) %>% 
    dfm_remove(c("*-time", "*-timeUpdated", "GMT", "BST")) %>% 
    dfm_trim(min_termfreq = 0.95, termfreq_type = "quantile", 
             max_docfreq = 0.1, docfreq_type = "prop")

# remove "empty" documents without any tokens
dfmat_news <- dfmat_news[ntoken(dfmat_news) > 0,]
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
##       topic1     topic2     topic3       topic4   topic5     topic6     
##  [1,] "refugees" "officers" "australia"  "game"   "corbyn"   "oil"      
##  [2,] "syria"    "victims"  "australian" "video"  "johnson"  "markets"  
##  [3,] "isis"     "prison"   "labor"      "son"    "benefits" "prices"   
##  [4,] "syrian"   "criminal" "turnbull"   "story"  "brussels" "shares"   
##  [5,] "military" "charges"  "senate"     "mother" "talks"    "rates"    
##  [6,] "islamic"  "crime"    "coalition"  "love"   "cabinet"  "banks"    
##  [7,] "un"       "sexual"   "budget"     "church" "boris"    "investors"
##  [8,] "turkey"   "officer"  "liberal"    "felt"   "19"       "trading"  
##  [9,] "muslim"   "incident" "malcolm"    "read"   "tory"     "sector"   
## [10,] "forces"   "arrested" "royal"      "heart"  "jeremy"   "quarter"  
##       topic7    topic8       topic9        topic10    
##  [1,] "clinton" "sales"      "climate"     "doctors"  
##  [2,] "sanders" "customers"  "water"       "nhs"      
##  [3,] "cruz"    "apple"      "energy"      "housing"  
##  [4,] "obama"   "google"     "food"        "education"
##  [5,] "hillary" "businesses" "china"       "violence" 
##  [6,] "trump's" "users"      "gas"         "hospital" 
##  [7,] "bernie"  "technology" "project"     "funding"  
##  [8,] "ted"     "sold"       "development" "medical"  
##  [9,] "rubio"   "sale"       "chinese"     "drug"     
## [10,] "senator" "iphone"     "air"         "child"
```

You can then obtain the most likely topics using `topics()` and save them as a document-level variable.


```r
head(topics(tmod_lda), 20)
```

```
##  [1] "topic5" "topic5" "topic8" "topic4" "topic2" "topic9" "topic4" "topic9"
##  [9] "topic3" "topic4" "topic5" "topic2" "topic5" "topic6" "topic4" "topic9"
## [17] "topic9" "topic7" "topic4" "topic5"
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
##     183     190     267     118     261     223     160     179     186     184
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
##       economy      politics      society      diplomacy      military    
##  [1,] "markets"    "politicians" "hospital"   "treaty"       "military"  
##  [2,] "banks"      "elections"   "schools"    "refugees"     "terrorist" 
##  [3,] "stock"      "politician"  "prison"     "corbyn"       "army"      
##  [4,] "banking"    "voter"       "hospitals"  "johnson"      "soldiers"  
##  [5,] "shop"       "clinton"     "prisons"    "brussels"     "terrorists"
##  [6,] "shopping"   "sanders"     "australia"  "talks"        "officers"  
##  [7,] "bank's"     "cruz"        "australian" "19"           "isis"      
##  [8,] "marketing"  "obama"       "labor"      "cabinet"      "syria"     
##  [9,] "oil"        "hillary"     "turnbull"   "benefits"     "islamic"   
## [10,] "energy"     "trump's"     "funding"    "boris"        "forces"    
## [11,] "sales"      "senator"     "doctors"    "chancellor"   "victims"   
## [12,] "climate"    "bernie"      "education"  "shadow"       "video"     
## [13,] "prices"     "ted"         "nhs"        "negotiations" "died"      
## [14,] "food"       "rubio"       "violence"   "leadership"   "un"        
## [15,] "businesses" "gun"         "drug"       "french"       "officer"   
## [16,] "sector"     "primary"     "medical"    "tory"         "syrian"    
## [17,] "investors"  "race"        "laws"       "jeremy"       "son"       
## [18,] "china"      "candidates"  "budget"     "summit"       "facebook"  
## [19,] "rates"      "photograph"  "senate"     "membership"   "criminal"  
## [20,] "shares"     "kasich"      "royal"      "migrants"     "mother"
```

`topics()` on returns dictionary keys as the most likely topics of documents.


```r
head(topics(tmod_slda), 20)
```

```
##  [1] "society"   "diplomacy" "economy"   "military"  "military"  "economy"  
##  [7] "politics"  "diplomacy" "society"   "military"  "diplomacy" "military" 
## [13] "diplomacy" "economy"   "military"  "economy"   "economy"   "politics" 
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
##       266       498       610       210       367
```

{{% notice ref %}}

- Blei, David M., Andrew Y. Ng, and Michael I. Jordan. 2003. "Latent Dirichlet Allocation." _The Journal of Machine Learning Research_ 3(1): 993-1022.  
- Lu, B., Ott, M., Cardie, C., & Tsou, B. K. (2011). _Multi-aspect sentiment analysis with topic models_. 2011 IEEE 11th International Conference on Data Mining Workshops, 81–88.

{{% /notice %}}

