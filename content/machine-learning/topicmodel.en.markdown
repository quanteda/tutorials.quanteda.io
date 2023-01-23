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



We will select only news articles published in 2016 using `corpus_subset()` and the `year` function from the **lubridate** package. 


```r
corp_news_2016 <- corpus_subset(corp_news, year(date) == 2016)
ndoc(corp_news_2016)
```

```
## [1] 1959
```

Further, after removal of function words and punctuation in `dfm()`, we will only keep the top 20% of the most frequent features (`min_termfreq = 0.8`) that appear in less than 10% of all documents (`max_docfreq = 0.1`) using `dfm_trim()` to focus on common but distinguishing features.


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
##       topic1      topic2       topic3    topic4     topic5    topic6    
##  [1,] "oil"       "corbyn"     "son"     "officers" "clinton" "refugees"
##  [2,] "markets"   "johnson"    "parents" "violence" "sanders" "brussels"
##  [3,] "prices"    "leadership" "felt"    "prison"   "cruz"    "talks"   
##  [4,] "investors" "boris"      "park"    "victims"  "hillary" "french"  
##  [5,] "shares"    "shadow"     "room"    "sexual"   "obama"   "summit"  
##  [6,] "rates"     "jeremy"     "love"    "abuse"    "trump's" "refugee" 
##  [7,] "banks"     "tory"       "mother"  "criminal" "bernie"  "migrants"
##  [8,] "trading"   "doctors"    "story"   "officer"  "ted"     "turkey"  
##  [9,] "quarter"   "junior"     "father"  "crime"    "rubio"   "benefits"
## [10,] "sector"    "khan"       "knew"    "incident" "senator" "asylum"  
##       topic7       topic8     topic9       topic10      
##  [1,] "sales"      "syria"    "australia"  "climate"    
##  [2,] "apple"      "isis"     "australian" "water"      
##  [3,] "customers"  "military" "labor"      "energy"     
##  [4,] "google"     "islamic"  "turnbull"   "food"       
##  [5,] "users"      "un"       "budget"     "development"
##  [6,] "technology" "syrian"   "funding"    "gas"        
##  [7,] "games"      "forces"   "housing"    "drug"       
##  [8,] "game"       "muslim"   "senate"     "medical"    
##  [9,] "iphone"     "peace"    "education"  "hospital"   
## [10,] "app"        "china"    "coalition"  "patients"
```

You can then obtain the most likely topics using `topics()` and save them as a document-level variable.


```r
head(topics(tmod_lda), 20)
```

```
## text136751 text136585 text139163 text169133 text153451 text163885 text157885 
##    topic10     topic2     topic7     topic3     topic4    topic10     topic3 
## text173244 text137394 text169408 text184646 text127410 text134923 text169695 
##     topic2     topic9     topic3     topic2     topic4     topic2     topic1 
## text147917 text157535 text177078 text174393 text181782 text143323 
##     topic3     topic7    topic10     topic3     topic3     topic2 
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
##     200     218     233     236     192      83     204     185     191     210
```

### Seeded LDA

In the seeded LDA, you can pre-define topics in LDA using a dictionary of "seed" words.


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

The number of topics is determined by the number of keys in the dictionary. Next, we can fit the seeded LDA model using `textmodel_seededlda()` and specify the dictionary with our relevant keywords.


```r
tmod_slda <- textmodel_seededlda(dfmat_news, dictionary = dict_topic)
```

Some of the topic words are seed words, but the seeded LDA identifies many other related words.


```r
terms(tmod_slda, 20)
```

```
##       economy       politics      society     diplomacy   military    
##  [1,] "markets"     "clinton"     "hospital"  "refugees"  "military"  
##  [2,] "banks"       "sanders"     "prison"    "syria"     "labor"     
##  [3,] "oil"         "cruz"        "schools"   "brussels"  "australian"
##  [4,] "energy"      "obama"       "violence"  "isis"      "corbyn"    
##  [5,] "climate"     "hillary"     "officers"  "talks"     "australia" 
##  [6,] "sales"       "trump's"     "hospitals" "un"        "turnbull"  
##  [7,] "prices"      "bernie"      "victims"   "syrian"    "johnson"   
##  [8,] "stock"       "ted"         "cases"     "french"    "budget"    
##  [9,] "food"        "rubio"       "sexual"    "turkey"    "leadership"
## [10,] "rates"       "senator"     "drug"      "islamic"   "cabinet"   
## [11,] "sector"      "gun"         "officer"   "border"    "shadow"    
## [12,] "banking"     "politicians" "abuse"     "aid"       "funding"   
## [13,] "businesses"  "primary"     "crime"     "summit"    "coalition" 
## [14,] "investors"   "race"        "facebook"  "refugee"   "senate"    
## [15,] "housing"     "elections"   "medical"   "migrants"  "terrorist" 
## [16,] "shares"      "kasich"      "child"     "agreement" "boris"     
## [17,] "costs"       "candidates"  "mental"    "forces"    "doctors"   
## [18,] "average"     "delegates"   "died"      "military"  "tory"      
## [19,] "development" "photograph"  "parents"   "russian"   "parties"   
## [20,] "trading"     "supporters"  "mother"    "asylum"    "jeremy"
```

`topics()` returns dictionary keys as the most likely topics of documents.


```r
head(topics(tmod_slda), 20)
```

```
## text136751 text136585 text139163 text169133 text153451 text163885 text157885 
##    economy   military    economy    society    society    economy   politics 
## text173244 text137394 text169408 text184646 text127410 text134923 text169695 
##   military   military    society   military    society   military    economy 
## text147917 text157535 text177078 text174393 text181782 text143323 
##    society    economy    economy   politics    society   military 
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
##       479       219       633       233       388
```

{{% notice ref %}}

- Blei, David M., Andrew Y. Ng, and Michael I. Jordan. 2003. "[Latent Dirichlet Allocation](https://www.jmlr.org/papers/volume3/blei03a/blei03a.pdf)." _The Journal of Machine Learning Research_ 3(1): 993-1022.  
- Lu, B., Ott, M., Cardie, C., & Tsou, B. K. 2011. "[Multi-aspect Sentiment Analysis with Topic Models](https://www.cs.cornell.edu/home/cardie/papers/masa-sentire-2011.pdf)". _Proceeding of the 2011 IEEE 11th International Conference on Data Mining Workshops_, 81–88.

{{% /notice %}}

