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

Further, after removal of function words and punctuation in `dfm()`, we keep only the top 20% of the most frequent features (`min_termfreq = 0.8`) that appear in less than 10% of all documents (`max_docfreq = 0.1`) using `dfm_trim()` to focus on common but distinguishing features.


```r
toks_news <- tokens(corp_news_2016, remove_punct = TRUE, remove_numbers = TRUE, remove_symbol = TRUE)
toks_news <- tokens_remove(toks_news, pattern = c(stopwords("en"), "*-time", "updated-*", "gmt", "bst"))
dfmat_news <- dfm(toks_news) %>% 
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
##       topic1      topic2    topic3      topic4     topic5         topic6     
##  [1,] "oil"       "climate" "doctors"   "apple"    "johnson"      "isis"     
##  [2,] "markets"   "water"   "violence"  "game"     "brussels"     "military" 
##  [3,] "sales"     "energy"  "nhs"       "facebook" "talks"        "islamic"  
##  [4,] "prices"    "food"    "education" "users"    "benefits"     "china"    
##  [5,] "banks"     "gas"     "medical"   "music"    "boris"        "muslim"   
##  [6,] "investors" "homes"   "hospital"  "games"    "membership"   "forces"   
##  [7,] "rates"     "khan"    "drug"      "play"     "negotiations" "syria"    
##  [8,] "shares"    "housing" "girls"     "internet" "summit"       "saudi"    
##  [9,] "sector"    "project" "study"     "google"   "cabinet"      "terrorist"
## [10,] "trading"   "air"     "schools"   "phone"    "agreement"    "chinese"  
##       topic7       topic8    topic9        topic10   
##  [1,] "labor"      "clinton" "refugees"    "officers"
##  [2,] "australian" "sanders" "syrian"      "prison"  
##  [3,] "australia"  "cruz"    "un"          "criminal"
##  [4,] "corbyn"     "hillary" "turkey"      "victims" 
##  [5,] "turnbull"   "obama"   "syria"       "officer" 
##  [6,] "budget"     "trump's" "aid"         "dead"    
##  [7,] "shadow"     "bernie"  "border"      "incident"
##  [8,] "leadership" "ted"     "refugee"     "black"   
##  [9,] "senate"     "rubio"   "asylum"      "shooting"
## [10,] "coalition"  "senator" "immigration" "arrested"
```

You can then obtain the most likely topics using `topics()` and save them as a document-level variable.


```r
head(topics(tmod_lda), 20)
```

```
## text136751 text136585 text139163 text169133 text153451 text163885 text157885 
##     topic2     topic9     topic1    topic10    topic10     topic2     topic4 
## text173244 text137394 text169408 text184646 text127410 text134923 text169695 
##     topic2     topic7     topic4     topic5    topic10     topic5     topic1 
## text147917 text157535 text177078 text174393 text181782 text143323 
##     topic4     topic2     topic2     topic8     topic3     topic7 
## 10 Levels: topic1 topic2 topic3 topic4 topic5 topic6 topic7 topic8 ... topic10
```

```r
# assign topic as a new document-level variable
dfmat_news$topic <- topics(tmod_lda)

# cross-table of the topic frequency
table(dfmat_news$topic)
```

```
## 
##  topic1  topic2  topic3  topic4  topic5  topic6  topic7  topic8  topic9 topic10 
##     281     205     219     209      92     163     213     207     123     240
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
##       economy      politics      society       diplomacy      military   
##  [1,] "markets"    "clinton"     "hospital"    "labor"        "military" 
##  [2,] "banks"      "sanders"     "schools"     "corbyn"       "syria"    
##  [3,] "oil"        "cruz"        "prison"      "johnson"      "officers" 
##  [4,] "sales"      "obama"       "water"       "turnbull"     "isis"     
##  [5,] "prices"     "hillary"     "food"        "cabinet"      "refugees" 
##  [6,] "stock"      "trump's"     "climate"     "talks"        "terrorist"
##  [7,] "banking"    "bernie"      "hospitals"   "australian"   "army"     
##  [8,] "sector"     "senator"     "development" "brussels"     "syrian"   
##  [9,] "rates"      "ted"         "education"   "budget"       "victims"  
## [10,] "investors"  "rubio"       "violence"    "benefits"     "forces"   
## [11,] "china"      "gun"         "project"     "coalition"    "islamic"  
## [12,] "energy"     "politicians" "game"        "australia"    "un"       
## [13,] "shares"     "primary"     "population"  "shadow"       "crime"    
## [14,] "housing"    "race"        "drug"        "leadership"   "sexual"   
## [15,] "trading"    "elections"   "funding"     "parties"      "officer"  
## [16,] "costs"      "kasich"      "drugs"       "boris"        "criminal" 
## [17,] "income"     "candidates"  "users"       "negotiations" "abuse"    
## [18,] "businesses" "photograph"  "apple"       "tory"         "incident" 
## [19,] "quarter"    "delegates"   "girls"       "summit"       "dead"     
## [20,] "annual"     "americans"   "study"       "senate"       "child"
```

`topics()` on returns dictionary keys as the most likely topics of documents.


```r
head(topics(tmod_slda), 20)
```

```
## text136751 text136585 text139163 text169133 text153451 text163885 text157885 
##    society  diplomacy    economy   military   military    society   politics 
## text173244 text137394 text169408 text184646 text127410 text134923 text169695 
##  diplomacy  diplomacy    society  diplomacy   military  diplomacy    economy 
## text147917 text157535 text177078 text174393 text181782 text143323 
##    society    economy    society   politics    society  diplomacy 
## Levels: economy politics society diplomacy military
```

```r
# assign topics from seeded LDA as a document-level variable to the dfm
dfmat_news$topic2 <- topics(tmod_slda)

# cross-table of the topic frequency
table(dfmat_news$topic2)
```

```
## 
##   economy  politics   society diplomacy  military 
##       331       237       530       378       476
```

{{% notice ref %}}

- Blei, David M., Andrew Y. Ng, and Michael I. Jordan. 2003. "[Latent Dirichlet Allocation](https://www.jmlr.org/papers/volume3/blei03a/blei03a.pdf)." _The Journal of Machine Learning Research_ 3(1): 993-1022.  
- Lu, B., Ott, M., Cardie, C., & Tsou, B. K. 2011. "[Multi-aspect Sentiment Analysis with Topic Models](https://www.cs.cornell.edu/home/cardie/papers/masa-sentire-2011.pdf)". _Proceeding of the 2011 IEEE 11th International Conference on Data Mining Workshops_, 81–88.

{{% /notice %}}

