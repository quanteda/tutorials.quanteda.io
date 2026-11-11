---
title: Topic models
weight: 50
draft: false
bibliography: ../references.bib
---

Like Wordfish, topic models are unsupervised: you do not need any labelled documents to use them. Rather than positioning documents on a scale, they group documents by subject matter. By modelling how topics are distributed over words, and how words are distributed over documents, topic models identify the most distinguishing groups of documents automatically, without you needing to specify in advance what those groups should be.

``` r
library(quanteda)
library(quanteda.corpora)
library(seededlda)
library(lubridate)
```

The corpus contains 6,000 Guardian news articles from 2012 to 2016.

``` r
corp_news <- download("data_corpus_guardian")
```

Topic models can be slow to fit on large corpora, so we will select only news articles published in 2016 using `corpus_subset()` and the `year()` function from the **lubridate** package, to keep this example fast enough to run interactively.

``` r
corp_news_2016 <- corpus_subset(corp_news, year(date) == 2016)
ndoc(corp_news_2016)
```

    ## [1] 1959

Topic models also work best on a focused vocabulary. Very common words add little information about topic, since they appear everywhere, and very rare words add mostly noise, since the model has too little evidence about them. After removing function words and punctuation in `tokens()`, we will therefore only keep the top 20% of the most frequent remaining features (`min_termfreq = 0.8`) that also appear in fewer than 10% of all documents (`max_docfreq = 0.1`), using `dfm_trim()` to focus on features that are common enough to be reliable but still distinguish one document from another.

``` r
toks_news <- tokens(corp_news_2016, remove_punct = TRUE, remove_numbers = TRUE, remove_symbol = TRUE)
toks_news <- tokens_remove(toks_news, pattern = c(stopwords("en"), "*-time", "updated-*", "gmt", "bst"))
dfmat_news <- dfm(toks_news) |> 
              dfm_trim(min_termfreq = 0.8, termfreq_type = "quantile",
                       max_docfreq = 0.1, docfreq_type = "prop")
```

**quanteda** does not implement topic models itself, but you can fit Latent Dirichlet Allocation (LDA) (Blei et al. 2003) and seeded LDA using the companion **seededlda** package, on a DFM built as you have throughout this tutorial. Several other widely used topic modelling packages also work out of the box with a **quanteda** DFM, once converted to the format each one expects: **topicmodels** (Grün and Hornik 2011), **stm** (Roberts et al. 2019) for structural topic models that let you incorporate document-level covariates, and **keyATM** (Eshima et al. 2024) for keyword-assisted topic models, a more flexible alternative to the seeded LDA shown later on this page.

{{% notice tip %}}
`convert()`, which you already met when comparing documents earlier in this tutorial, reshapes a DFM into the `"topicmodels"` or `"stm"` format directly. **keyATM** instead provides its own `keyATM_read()` function, which reads a **quanteda** DFM without needing `convert()` at all.

``` r
# topicmodels
dtm_tm <- convert(dfmat_news, to = "topicmodels")

# stm
dat_stm <- convert(dfmat_news, to = "stm")

# keyATM
docs_keyatm <- keyATM_read(texts = dfmat_news)
```

{{% /notice %}}

### LDA

Standard LDA is fully unsupervised: you only tell it how many topics to look for, and it discovers the topics and their associated words entirely on its own. `k = 10` specifies the number of topics to be discovered, an important parameter: try several values and validate the outputs of your topic models, since there is no single “correct” number of topics for a given corpus.

``` r
tmod_lda <- textmodel_lda(dfmat_news, k = 10)
```

A fitted topic model does not label its topics for you; it only associates words and documents with numbered topics. You can extract the most important terms for each topic from the model using `terms()`, which is usually how you work out what each topic represents.

``` r
terms(tmod_lda, 10)
```

    ##       topic1     topic2       topic3     topic4         topic5    topic6       
    ##  [1,] "syria"    "corbyn"     "officers" "brussels"     "love"    "climate"    
    ##  [2,] "refugees" "johnson"    "prison"   "talks"        "church"  "water"      
    ##  [3,] "isis"     "shadow"     "victims"  "summit"       "game"    "energy"     
    ##  [4,] "military" "leadership" "sexual"   "benefits"     "park"    "food"       
    ##  [5,] "syrian"   "boris"      "abuse"    "ireland"      "felt"    "gas"        
    ##  [6,] "islamic"  "jeremy"     "criminal" "migrants"     "son"     "development"
    ##  [7,] "un"       "tory"       "officer"  "french"       "parents" "hospital"   
    ##  [8,] "forces"   "cabinet"    "arrested" "greece"       "mother"  "drug"       
    ##  [9,] "muslim"   "khan"       "charges"  "emergency"    "story"   "project"    
    ## [10,] "aid"      "doctors"    "cases"    "negotiations" "gay"     "medical"    
    ##       topic7      topic8    topic9        topic10     
    ##  [1,] "oil"       "clinton" "australia"   "sales"     
    ##  [2,] "markets"   "sanders" "australian"  "housing"   
    ##  [3,] "prices"    "cruz"    "labor"       "customers" 
    ##  [4,] "banks"     "hillary" "turnbull"    "apple"     
    ##  [5,] "rates"     "obama"   "senate"      "google"    
    ##  [6,] "investors" "trump's" "coalition"   "users"     
    ##  [7,] "shares"    "bernie"  "violence"    "food"      
    ##  [8,] "trading"   "ted"     "budget"      "technology"
    ##  [9,] "quarter"   "rubio"   "legislation" "sold"      
    ## [10,] "china"     "senator" "schools"     "businesses"

Glancing down each list of ten words, deciding what the topic “is”, and moving on is tempting. Chang et al. (2009) showed that this kind of eyeballing is an unreliable way to judge topic quality: in their experiments, the topics that looked most coherent to a human skimming the top words were not reliably the topics that a model’s own internal fit statistics rated highest, and vice versa. They proposed a more rigorous alternative, the word intrusion test: show human readers a topic’s top words with one extra, unrelated “intruder” word mixed in, and see whether the readers can spot the intruder. If they consistently can, the topic’s words hang together in a way people recognise; if they cannot, the topic is not as coherent as reading its top ten words might suggest. There is a parallel topic intrusion test for whether a document’s assigned topic matches how a human would describe that document. Applying a proper word or topic intrusion test is beyond the scope of this tutorial, but is worth doing before you report topic labels as a finding rather than as a convenient description.

Once you can see what each topic represents, you can obtain the most likely topic for every document using `topics()` and save the result as a document-level variable, turning an unsupervised model into a practical document classification.

``` r
head(topics(tmod_lda), 20)
```

    ## text136751 text136585 text139163 text169133 text153451 text163885 text157885 
    ##     topic9     topic4    topic10     topic3     topic3     topic6     topic5 
    ## text173244 text137394 text169408 text184646 text127410 text134923 text169695 
    ##     topic2     topic9     topic5     topic2     topic3     topic2     topic7 
    ## text147917 text157535 text177078 text174393 text181782 text143323 
    ##     topic5     topic6     topic6     topic8     topic5     topic2 
    ## 10 Levels: topic1 topic2 topic3 topic4 topic5 topic6 topic7 topic8 ... topic10

``` r
# assign topic as a new document-level variable
dfmat_news$topic <- topics(tmod_lda)

# cross-table of the topic frequency
table(dfmat_news$topic)
```

    ## 
    ##  topic1  topic2  topic3  topic4  topic5  topic6  topic7  topic8  topic9 topic10 
    ##     197     261     236      61     243     209     176     191     147     231

### Seeded LDA

Standard LDA gives you no control over what its topics turn out to be about. Seeded LDA addresses this, an approach implemented in **seededlda** by Watanabe and Baturo (2023). You can pre-define topics in LDA using a dictionary of “seed” words, which nudges the model towards topics you already have a substantive interest in, similar in spirit to how you [used dictionaries earlier in this tutorial](/basic-operations/tokens/tokens_lookup). Here, though, the dictionary guides an otherwise unsupervised model rather than directly counting matches.

``` r
# load dictionary containing seed words
dict_topic <- dictionary(file = "../dictionary/topics.yml")
print(dict_topic)
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

Unlike standard LDA, where you had to choose `k` directly, the number of topics in seeded LDA is determined automatically by the number of keys in the dictionary. Next, we fit the seeded LDA model using `textmodel_seededlda()`, passing it the dictionary of relevant keywords in place of a raw topic count.

``` r
tmod_slda <- textmodel_seededlda(dfmat_news, dictionary = dict_topic)
```

Some of the words returned for each topic are seed words we supplied ourselves, but the seeded LDA model identifies many other related words that were never in the original dictionary. A small hand-written list is expanded into a fuller topic vocabulary this way.

``` r
terms(tmod_slda, 20)
```

    ##       economy       politics      society     diplomacy    military   
    ##  [1,] "markets"     "politicians" "hospital"  "clinton"    "military" 
    ##  [2,] "banks"       "labor"       "prison"    "sanders"    "refugees" 
    ##  [3,] "oil"         "corbyn"      "schools"   "cruz"       "syria"    
    ##  [4,] "climate"     "elections"   "officers"  "obama"      "isis"     
    ##  [5,] "energy"      "turnbull"    "violence"  "hillary"    "terrorist"
    ##  [6,] "sales"       "johnson"     "hospitals" "trump's"    "army"     
    ##  [7,] "prices"      "australian"  "cases"     "bernie"     "un"       
    ##  [8,] "stock"       "budget"      "sexual"    "senator"    "syrian"   
    ##  [9,] "sector"      "cabinet"     "abuse"     "ted"        "islamic"  
    ## [10,] "food"        "talks"       "parents"   "rubio"      "turkey"   
    ## [11,] "rates"       "benefits"    "child"     "gun"        "aid"      
    ## [12,] "banking"     "brussels"    "facebook"  "primary"    "forces"   
    ## [13,] "businesses"  "australia"   "drug"      "race"       "refugee"  
    ## [14,] "investors"   "shadow"      "medical"   "kasich"     "peace"    
    ## [15,] "costs"       "leadership"  "officer"   "candidates" "russian"  
    ## [16,] "housing"     "coalition"   "mother"    "photograph" "border"   
    ## [17,] "shares"      "boris"       "victims"   "americans"  "russia"   
    ## [18,] "average"     "senate"      "mental"    "delegates"  "saudi"    
    ## [19,] "development" "chancellor"  "crime"     "america"    "french"   
    ## [20,] "trading"     "tory"        "drugs"     "supporters" "paris"

Because each topic now has a meaningful name rather than just a number, `topics()` returns the dictionary keys directly as the most likely topic for each document.

``` r
head(topics(tmod_slda), 20)
```

    ## text136751 text136585 text139163 text169133 text153451 text163885 text157885 
    ##    economy   politics    economy    society    society    economy  diplomacy 
    ## text173244 text137394 text169408 text184646 text127410 text134923 text169695 
    ##   military   politics    society   politics    society   politics    economy 
    ## text147917 text157535 text177078 text174393 text181782 text143323 
    ##  diplomacy    economy    economy  diplomacy    society   politics 
    ## Levels: economy politics society diplomacy military

``` r
# assign topics from seeded LDA as a document-level variable to the dfm
dfmat_news$topic2 <- topics(tmod_slda)

# cross-table of the topic frequency
table(dfmat_news$topic2)
```

    ## 
    ##   economy  politics   society diplomacy  military 
    ##       512       358       586       211       285

## References

<div id="refs" class="references csl-bib-body hanging-indent">

<div id="ref-blei2003" class="csl-entry">

Blei, David M., Andrew Y. Ng, and Michael I. Jordan. 2003. “Latent Dirichlet Allocation.” *The Journal of Machine Learning Research* 3 (1): 993–1022. <https://www.jmlr.org/papers/volume3/blei03a/blei03a.pdf>.

</div>

<div id="ref-chang2009" class="csl-entry">

Chang, Jonathan, Sean Gerrish, Chong Wang, Jordan L. Boyd-Graber, and David M. Blei. 2009. “Reading Tea Leaves: How Humans Interpret Topic Models.” *Advances in Neural Information Processing Systems* 22: 288–96. <https://proceedings.neurips.cc/paper/2009/hash/f92586a25bb3145facd64ab20fd554ff-Abstract.html>.

</div>

<div id="ref-eshima2024" class="csl-entry">

Eshima, Shusei, Kosuke Imai, and Tomoya Sasaki. 2024. “Keyword-Assisted Topic Models.” *American Journal of Political Science* 68 (2): 730–50. <https://doi.org/10.1111/ajps.12779>.

</div>

<div id="ref-grun2011" class="csl-entry">

Grün, Bettina, and Kurt Hornik. 2011. “Topicmodels: An r Package for Fitting Topic Models.” *Journal of Statistical Software* 40 (13): 1–30. <https://doi.org/10.18637/jss.v040.i13>.

</div>

<div id="ref-roberts2019" class="csl-entry">

Roberts, Margaret E., Brandon M. Stewart, and Dustin Tingley. 2019. “Stm: An r Package for Structural Topic Models.” *Journal of Statistical Software* 91 (2): 1–40. <https://doi.org/10.18637/jss.v091.i02>.

</div>

<div id="ref-watanabealexander2023" class="csl-entry">

Watanabe, Kohei, and Alexander Baturo. 2023. “Seeded Sequential LDA: A Semi-Supervised Algorithm for Topic-Specific Analysis of Sentences.” *Social Science Computer Review* 42 (1): 224–48. <https://doi.org/10.1177/08944393231178605>.

</div>

</div>
