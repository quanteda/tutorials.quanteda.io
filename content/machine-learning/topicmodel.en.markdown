---
title: Topic models
weight: 50
draft: false
---

Topics models are unsupervised document classification techniques. By modeling distributions of topics over words and words over documents, topic models identify the most discriminatory groups of documents automatically. 


``` r
library(quanteda)
library(quanteda.corpora)
library(seededlda)
library(lubridate)
```


``` r
corp_news <- download("data_corpus_guardian")
```



We will select only news articles published in 2016 using `corpus_subset()` and the `year` function from the **lubridate** package. 


``` r
corp_news_2016 <- corpus_subset(corp_news, year(date) == 2016)
ndoc(corp_news_2016)
```

```
## [1] 1959
```

Further, after removal of function words and punctuation in `dfm()`, we will only keep the top 20% of the most frequent features (`min_termfreq = 0.8`) that appear in less than 10% of all documents (`max_docfreq = 0.1`) using `dfm_trim()` to focus on common but distinguishing features.


``` r
toks_news <- tokens(corp_news_2016, remove_punct = TRUE, remove_numbers = TRUE, remove_symbol = TRUE)
toks_news <- tokens_remove(toks_news, pattern = c(stopwords("en"), "*-time", "updated-*", "gmt", "bst"))
dfmat_news <- dfm(toks_news) |> 
              dfm_trim(min_termfreq = 0.8, termfreq_type = "quantile",
                       max_docfreq = 0.1, docfreq_type = "prop")
```

**quanteda** does not implement topic models, but you can fit LDA and seeded-LDA with the **seededlda** package.

### LDA

`k = 10` specifies the number of topics to be discovered. This is an important parameter and you should try a variety of values and validate the outputs of your topic models thoroughly.


``` r
tmod_lda <- textmodel_lda(dfmat_news, k = 10)
```

You can extract the most important terms for each topic from the model using `terms()`.


``` r
terms(tmod_lda, 10)
```

```
##       topic1       topic2    topic3       topic4       topic5     topic6     
##  [1,] "corbyn"     "clinton" "housing"    "australia"  "refugees" "doctors"  
##  [2,] "johnson"    "sanders" "funding"    "australian" "syria"    "violence" 
##  [3,] "brussels"   "cruz"    "income"     "labor"      "isis"     "education"
##  [4,] "talks"      "hillary" "review"     "turnbull"   "military" "nhs"      
##  [5,] "cabinet"    "obama"   "cuts"       "senate"     "syrian"   "hospital" 
##  [6,] "boris"      "trump's" "scheme"     "coalition"  "un"       "medical"  
##  [7,] "benefits"   "bernie"  "businesses" "malcolm"    "islamic"  "drug"     
##  [8,] "tory"       "ted"     "fund"       "program"    "turkey"   "child"    
##  [9,] "leadership" "rubio"   "homes"      "budget"     "forces"   "drugs"    
## [10,] "membership" "senator" "budget"     "liberal"    "muslim"   "girls"    
##       topic7      topic8     topic9          topic10   
##  [1,] "oil"       "officers" "climate"       "game"    
##  [2,] "markets"   "prison"   "water"         "apple"   
##  [3,] "sales"     "victims"  "energy"        "facebook"
##  [4,] "prices"    "criminal" "food"          "users"   
##  [5,] "investors" "dead"     "gas"           "google"  
##  [6,] "shares"    "officer"  "air"           "music"   
##  [7,] "banks"     "crime"    "project"       "games"   
##  [8,] "trading"   "arrested" "environmental" "tv"      
##  [9,] "rates"     "incident" "residents"     "internet"
## [10,] "quarter"   "black"    "environment"   "video"
```

You can then obtain the most likely topics using `topics()` and save them as a document-level variable.


``` r
head(topics(tmod_lda), 20)
```

```
## text136751 text136585 text139163 text169133 text153451 text163885 text157885 
##     topic1     topic1     topic3     topic8     topic8     topic9    topic10 
## text173244 text137394 text169408 text184646 text127410 text134923 text169695 
##     topic9     topic4    topic10     topic1     topic8     topic1     topic7 
## text147917 text157535 text177078 text174393 text181782 text143323 
##    topic10     topic9     topic9     topic2     topic6     topic1 
## 10 Levels: topic1 topic2 topic3 topic4 topic5 topic6 topic7 topic8 ... topic10
```

``` r
# assign topic as a new document-level variable
dfmat_news$topic <- topics(tmod_lda)

# cross-table of the topic frequency
table(dfmat_news$topic)
```

```
## 
##  topic1  topic2  topic3  topic4  topic5  topic6  topic7  topic8  topic9 topic10 
##     210     207     199     110     217     168     184     235     200     222
```

### Seeded LDA

In the seeded LDA, you can pre-define topics in LDA using a dictionary of "seed" words.


``` r
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


``` r
tmod_slda <- textmodel_seededlda(dfmat_news, dictionary = dict_topic)
```

Some of the topic words are seed words, but the seeded LDA identifies many other related words.


``` r
terms(tmod_slda, 20)
```

```
##       economy      politics      society       diplomacy      military   
##  [1,] "markets"    "clinton"     "hospital"    "labor"        "military" 
##  [2,] "banks"      "sanders"     "schools"     "corbyn"       "syria"    
##  [3,] "oil"        "cruz"        "prison"      "turnbull"     "officers" 
##  [4,] "sales"      "obama"       "water"       "johnson"      "refugees" 
##  [5,] "energy"     "hillary"     "food"        "cabinet"      "terrorist"
##  [6,] "prices"     "trump's"     "hospitals"   "brussels"     "isis"     
##  [7,] "stock"      "bernie"      "climate"     "australian"   "army"     
##  [8,] "banking"    "ted"         "development" "budget"       "syrian"   
##  [9,] "sector"     "senator"     "violence"    "benefits"     "victims"  
## [10,] "rates"      "rubio"       "education"   "talks"        "un"       
## [11,] "investors"  "gun"         "drug"        "australia"    "forces"   
## [12,] "shares"     "politicians" "game"        "shadow"       "islamic"  
## [13,] "businesses" "primary"     "population"  "coalition"    "crime"    
## [14,] "china"      "race"        "apple"       "leadership"   "abuse"    
## [15,] "costs"      "elections"   "girls"       "boris"        "sexual"   
## [16,] "trading"    "kasich"      "project"     "negotiations" "criminal" 
## [17,] "income"     "candidates"  "users"       "immigration"  "aid"      
## [18,] "quarter"    "photograph"  "study"       "senate"       "peace"    
## [19,] "gas"        "delegates"   "drugs"       "jeremy"       "turkey"   
## [20,] "housing"    "america"     "medical"     "tory"         "officer"
```

`topics()` returns dictionary keys as the most likely topics of documents.


``` r
head(topics(tmod_slda), 20)
```

```
## text136751 text136585 text139163 text169133 text153451 text163885 text157885 
##    society  diplomacy    economy   military   military    economy   politics 
## text173244 text137394 text169408 text184646 text127410 text134923 text169695 
##  diplomacy  diplomacy    society  diplomacy   military  diplomacy    economy 
## text147917 text157535 text177078 text174393 text181782 text143323 
##    society    economy    society   politics    society  diplomacy 
## Levels: economy politics society diplomacy military
```

``` r
# assign topics from seeded LDA as a document-level variable to the dfm
dfmat_news$topic2 <- topics(tmod_slda)

# cross-table of the topic frequency
table(dfmat_news$topic2)
```

```
## 
##   economy  politics   society diplomacy  military 
##       353       227       517       371       484
```

{{% notice ref %}}

- Blei, David M., Andrew Y. Ng, and Michael I. Jordan. 2003. "[Latent Dirichlet Allocation](https://www.jmlr.org/papers/volume3/blei03a/blei03a.pdf)." _The Journal of Machine Learning Research_ 3(1): 993-1022.  
- Lu, B., Ott, M., Cardie, C., & Tsou, B. K. 2011. "[Multi-aspect Sentiment Analysis with Topic Models](https://www.cs.cornell.edu/home/cardie/papers/masa-sentire-2011.pdf)". _Proceeding of the 2011 IEEE 11th International Conference on Data Mining Workshops_, 81–88.

{{% /notice %}}

