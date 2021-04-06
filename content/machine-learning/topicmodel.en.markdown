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
##       topic1       topic2      topic3   topic4    topic5       topic6      
##  [1,] "australia"  "oil"       "love"   "clinton" "climate"    "housing"   
##  [2,] "australian" "markets"   "park"   "sanders" "water"      "customers" 
##  [3,] "labor"      "prices"    "series" "cruz"    "education"  "income"    
##  [4,] "turnbull"   "banks"     "game"   "hillary" "violence"   "sales"     
##  [5,] "senate"     "shares"    "music"  "obama"   "drug"       "homes"     
##  [6,] "apple"      "rates"     "wife"   "trump's" "medical"    "businesses"
##  [7,] "google"     "investors" "knew"   "bernie"  "hospital"   "food"      
##  [8,] "malcolm"    "trading"   "room"   "ted"     "food"       "funding"   
##  [9,] "coalition"  "quarter"   "video"  "rubio"   "population" "costs"     
## [10,] "budget"     "sales"     "couple" "senator" "girls"      "sold"      
##       topic7        topic8     topic9       topic10   
##  [1,] "refugees"    "officers" "corbyn"     "syria"   
##  [2,] "brussels"    "prison"   "johnson"    "isis"    
##  [3,] "talks"       "victims"  "leadership" "military"
##  [4,] "french"      "criminal" "shadow"     "islamic" 
##  [5,] "summit"      "sexual"   "jeremy"     "un"      
##  [6,] "migrants"    "officer"  "tory"       "syrian"  
##  [7,] "refugee"     "crime"    "cabinet"    "forces"  
##  [8,] "benefits"    "arrested" "boris"      "muslim"  
##  [9,] "immigration" "alleged"  "doctors"    "peace"   
## [10,] "asylum"      "abuse"    "khan"       "russian"
```

You can then obtain the most likely topics using `topics()` and save them as a document-level variable.


```r
head(topics(tmod_lda), 20)
```

```
##  [1] topic5 topic9 topic1 topic8 topic8 topic2 topic3 topic9 topic9 topic3
## [11] topic9 topic8 topic9 topic2 topic3 topic2 topic5 topic4 topic3 topic9
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
##     153     175     242     197     207     250      69     244     227     188
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
##  [6,] "shopping"    "refugees"    "prisoners"   "embassy"    "navy"      
##  [7,] "bank's"      "syria"       "hospitality" "labor"      "marine"    
##  [8,] "marketing"   "isis"        "prisoner"    "corbyn"     "soldier"   
##  [9,] "shops"       "un"          "officers"    "johnson"    "clinton"   
## [10,] "bankers"     "syrian"      "violence"    "turnbull"   "sanders"   
## [11,] "stocks"      "turkey"      "cases"       "budget"     "cruz"      
## [12,] "shoppers"    "islamic"     "sexual"      "australian" "obama"     
## [13,] "bond"        "aid"         "child"       "cabinet"    "hillary"   
## [14,] "bonds"       "forces"      "abuse"       "benefits"   "trump's"   
## [15,] "bankruptcy"  "refugee"     "drug"        "leadership" "bernie"    
## [16,] "bankrupt"    "peace"       "facebook"    "shadow"     "gun"       
## [17,] "banker"      "asylum"      "officer"     "australia"  "senator"   
## [18,] "stockport"   "border"      "medical"     "talks"      "ted"       
## [19,] "marketplace" "french"      "parents"     "coalition"  "rubio"     
## [20,] "oil"         "russian"     "died"        "brussels"   "primary"
```

`topics()` on returns dictionary keys as the most likely topics of documents.


```r
head(topics(tmod_slda), 20)
```

```
##  [1] economy   diplomacy economy   society   society   economy   military 
##  [8] politics  diplomacy society   diplomacy society   diplomacy economy  
## [15] society   economy   economy   military  society   diplomacy
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
##       508       279       598       356       211
```

{{% notice ref %}}

- Blei, David M., Andrew Y. Ng, and Michael I. Jordan. 2003. "Latent Dirichlet Allocation." _The Journal of Machine Learning Research_ 3(1): 993-1022.  
- Lu, B., Ott, M., Cardie, C., & Tsou, B. K. 2011. "[Multi-aspect sentiment analysis with topic models](https://www.cs.cornell.edu/home/cardie/papers/masa-sentire-2011.pdf)". _Proceeding of the 2011 IEEE 11th International Conference on Data Mining Workshops_, 81–88.

{{% /notice %}}

