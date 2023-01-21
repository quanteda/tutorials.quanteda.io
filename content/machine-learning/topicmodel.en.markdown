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
##       topic1       topic2          topic3     topic4     topic5   
##  [1,] "sales"      "climate"       "black"    "syria"    "clinton"
##  [2,] "apple"      "water"         "father"   "isis"     "sanders"
##  [3,] "game"       "energy"        "son"      "military" "cruz"   
##  [4,] "customers"  "project"       "shooting" "islamic"  "hillary"
##  [5,] "google"     "gas"           "wife"     "syrian"   "obama"  
##  [6,] "users"      "food"          "dead"     "un"       "trump's"
##  [7,] "food"       "development"   "mother"   "forces"   "bernie" 
##  [8,] "technology" "air"           "died"     "muslim"   "ted"    
##  [9,] "games"      "environmental" "shot"     "russian"  "rubio"  
## [10,] "play"       "population"    "tried"    "peace"    "senator"
##       topic6         topic7      topic8     topic9       topic10     
##  [1,] "refugees"     "oil"       "doctors"  "corbyn"     "australia" 
##  [2,] "brussels"     "markets"   "violence" "johnson"    "australian"
##  [3,] "talks"        "prices"    "prison"   "leadership" "labor"     
##  [4,] "summit"       "rates"     "cases"    "shadow"     "turnbull"  
##  [5,] "benefits"     "investors" "officers" "jeremy"     "senate"    
##  [6,] "migrants"     "banks"     "sexual"   "tory"       "funding"   
##  [7,] "french"       "shares"    "abuse"    "boris"      "budget"    
##  [8,] "refugee"      "sector"    "medical"  "cabinet"    "education" 
##  [9,] "turkey"       "trading"   "hospital" "khan"       "coalition" 
## [10,] "negotiations" "quarter"   "drug"     "commons"    "schools"
```

You can then obtain the most likely topics using `topics()` and save them as a document-level variable.


```r
head(topics(tmod_lda), 20)
```

```
## text136751 text136585 text139163 text169133 text153451 text163885 text157885 
##     topic2     topic9     topic1     topic3     topic8     topic2     topic3 
## text173244 text137394 text169408 text184646 text127410 text134923 text169695 
##     topic2     topic9     topic3     topic9     topic2     topic9     topic7 
## text147917 text157535 text177078 text174393 text181782 text143323 
##     topic3     topic2     topic2     topic3     topic3     topic9 
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
##     210     178     258     197     191      69     256     242     220     131
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
##       economy      politics      society     diplomacy     military      
##  [1,] "markets"    "clinton"     "prison"    "water"       "military"    
##  [2,] "banks"      "sanders"     "hospital"  "food"        "labor"       
##  [3,] "oil"        "cruz"        "syria"     "climate"     "corbyn"      
##  [4,] "sales"      "obama"       "schools"   "development" "johnson"     
##  [5,] "prices"     "hillary"     "officers"  "project"     "turnbull"    
##  [6,] "stock"      "trump's"     "refugees"  "population"  "brussels"    
##  [7,] "energy"     "bernie"      "isis"      "game"        "cabinet"     
##  [8,] "banking"    "senator"     "syrian"    "drug"        "talks"       
##  [9,] "sector"     "ted"         "victims"   "users"       "australian"  
## [10,] "rates"      "rubio"       "un"        "apple"       "budget"      
## [11,] "investors"  "gun"         "forces"    "education"   "coalition"   
## [12,] "shares"     "politicians" "violence"  "drugs"       "benefits"    
## [13,] "housing"    "primary"     "islamic"   "communities" "leadership"  
## [14,] "trading"    "race"        "hospitals" "study"       "australia"   
## [15,] "costs"      "elections"   "crime"     "funding"     "shadow"      
## [16,] "china"      "kasich"      "military"  "air"         "boris"       
## [17,] "businesses" "candidates"  "aid"       "girls"       "terrorist"   
## [18,] "income"     "photograph"  "criminal"  "games"       "negotiations"
## [19,] "quarter"    "delegates"   "abuse"     "patients"    "summit"      
## [20,] "lower"      "supporters"  "sexual"    "students"    "parties"
```

`topics()` returns dictionary keys as the most likely topics of documents.


```r
head(topics(tmod_slda), 20)
```

```
## text136751 text136585 text139163 text169133 text153451 text163885 text157885 
##  diplomacy   military    economy    society    society    economy   politics 
## text173244 text137394 text169408 text184646 text127410 text134923 text169695 
##   military   military  diplomacy   military    society   military    economy 
## text147917 text157535 text177078 text174393 text181782 text143323 
##  diplomacy    economy  diplomacy   politics  diplomacy   military 
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
##       345       231       482       512       382
```

{{% notice ref %}}

- Blei, David M., Andrew Y. Ng, and Michael I. Jordan. 2003. "[Latent Dirichlet Allocation](https://www.jmlr.org/papers/volume3/blei03a/blei03a.pdf)." _The Journal of Machine Learning Research_ 3(1): 993-1022.  
- Lu, B., Ott, M., Cardie, C., & Tsou, B. K. 2011. "[Multi-aspect Sentiment Analysis with Topic Models](https://www.cs.cornell.edu/home/cardie/papers/masa-sentire-2011.pdf)". _Proceeding of the 2011 IEEE 11th International Conference on Data Mining Workshops_, 81–88.

{{% /notice %}}

