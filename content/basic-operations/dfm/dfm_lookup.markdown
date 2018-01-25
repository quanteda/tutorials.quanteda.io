---
title: Dictionary lookup
weight: 20
chapter: false
draft: false
---


```r
require(quanteda)
require(ggplot2)
```


## Applying equivalency classes: dictionaries, thesaruses

Dictionary creation is done through the `dictionary()` function, which classes a named list of characters as a dictionary.

We apply dictionaries to a dfm using the `dfm_lookup()` function.  Through the `valuetype`, argument, we can match patterns of one of three types: `"glob"`, `"regex"`, or `"fixed"`.

```r
my_dict <- dictionary(list(christmas = c("Christmas", "Santa", "holiday"),
                          opposition = c("Opposition", "reject", "notincorpus"),
                          taxglob = "tax*",
                          taxregex = "tax.+$",
                          country = c("United_States", "Sweden")))
my_dfm <- dfm(c("My Christmas was ruined by your opposition tax plan.",
               "Does the United_States or Sweden have more progressive taxation?"),
             remove = stopwords("english"), verbose = FALSE)
my_dfm
```

```
## Document-feature matrix of: 2 documents, 11 features (50% sparse).
## 2 x 11 sparse Matrix of class "dfm"
##        features
## docs    christmas ruined opposition tax plan . united_states sweden
##   text1         1      1          1   1    1 1             0      0
##   text2         0      0          0   0    0 0             1      1
##        features
## docs    progressive taxation ?
##   text1           0        0 0
##   text2           1        1 1
```

```r
# glob format
dfm_lookup(my_dfm, my_dict, valuetype = "glob")
```

```
## Document-feature matrix of: 2 documents, 5 features (50% sparse).
## 2 x 5 sparse Matrix of class "dfm"
##        features
## docs    christmas opposition taxglob taxregex country
##   text1         1          1       1        0       0
##   text2         0          0       1        0       2
```

```r
dfm_lookup(my_dfm, my_dict, valuetype = "glob", case_insensitive = FALSE)
```

```
## Document-feature matrix of: 2 documents, 5 features (50% sparse).
## 2 x 5 sparse Matrix of class "dfm"
##        features
## docs    christmas opposition taxglob taxregex country
##   text1         1          1       1        0       0
##   text2         0          0       1        0       2
```

```r
# regex v. glob format: note that "united_states" is a regex match for "tax*"
dfm_lookup(my_dfm, my_dict, valuetype = "glob")
```

```
## Document-feature matrix of: 2 documents, 5 features (50% sparse).
## 2 x 5 sparse Matrix of class "dfm"
##        features
## docs    christmas opposition taxglob taxregex country
##   text1         1          1       1        0       0
##   text2         0          0       1        0       2
```

```r
dfm_lookup(my_dfm, my_dict, valuetype = "regex", case_insensitive = TRUE)
```

```
## Document-feature matrix of: 2 documents, 5 features (40% sparse).
## 2 x 5 sparse Matrix of class "dfm"
##        features
## docs    christmas opposition taxglob taxregex country
##   text1         1          1       1        0       0
##   text2         0          0       2        1       2
```

```r
# fixed format: no pattern matching
dfm_lookup(my_dfm, my_dict, valuetype = "fixed")
```

```
## Document-feature matrix of: 2 documents, 5 features (70% sparse).
## 2 x 5 sparse Matrix of class "dfm"
##        features
## docs    christmas opposition taxglob taxregex country
##   text1         1          1       0        0       0
##   text2         0          0       0        0       2
```

```r
dfm_lookup(my_dfm, my_dict, valuetype = "fixed", case_insensitive = FALSE)
```

```
## Document-feature matrix of: 2 documents, 5 features (70% sparse).
## 2 x 5 sparse Matrix of class "dfm"
##        features
## docs    christmas opposition taxglob taxregex country
##   text1         1          1       0        0       0
##   text2         0          0       0        0       2
```

It is also possible to pass through a dictionary at the time of `dfm()` creation.

```r
# dfm with dictionaries
my_corpus <- corpus_subset(data_corpus_inaugural, Year > 1900)
my_dict <- dictionary(list(christmas = c("Christmas", "Santa", "holiday"),
                          opposition = c("Opposition", "reject", "notincorpus"),
                          taxing = "taxing",
                          taxation = "taxation",
                          taxregex = "tax*",
                          country = "united states"))
dict_dfm <- dfm(my_corpus, dictionary = my_dict)
head(dict_dfm)
```

```
## Document-feature matrix of: 6 documents, 6 features (61.1% sparse).
## 6 x 6 sparse Matrix of class "dfm"
##                 features
## docs             christmas opposition taxing taxation taxregex country
##   1901-McKinley          0          2      0        1        1       9
##   1905-Roosevelt         0          0      0        0        0       0
##   1909-Taft              0          1      0        4        6       6
##   1913-Wilson            0          0      0        1        1       0
##   1917-Wilson            0          0      0        0        0       1
##   1921-Harding           0          0      0        1        2       1
```

There is also a related "thesaurus" feature, which collapses words in a dictionary but is not exclusive.

```r
my_texts <- c("British English tokenises differently, with more colour.",
             "American English tokenizes color as one word.")

my_dict <- dictionary(list(color = "colo*r", tokenize = "tokeni?e*"))

dfm(my_texts, thesaurus = my_dict)
```

```
## Document-feature matrix of: 2 documents, 13 features (34.6% sparse).
## 2 x 13 sparse Matrix of class "dfm"
##        features
## docs    COLOR TOKENIZE british english differently , with more . american
##   text1     1        1       1       1           1 1    1    1 1        0
##   text2     1        1       0       1           0 0    0    0 1        1
##        features
## docs    as one word
##   text1  0   0    0
##   text2  1   1    1
```


## Using the Lexicoder Sentiment Dictionary

**quanteda** includes the 2015 Lexicoder Sentiment Dictionary (Young annd Soroka, 2012) which can be used for sentiment analysis of English text. 


```r
dfm_inaug <- dfm(data_corpus_inaugural, remove_punct = TRUE)

inaugural_sentiment <- dfm_lookup(dfm_inaug, data_dictionary_LSD2015)
```

To plot the sentiment of each text, we can use the `convert()` function and export the `dfm` to a data.frame.


```r
data_sentiment <- convert(inaugural_sentiment, to = "data.frame")

data_sentiment$speech <- rownames(data_sentiment)

# get number of tokens for each speech
data_sentiment$tokens_speech <- ntoken(data_corpus_inaugural)

# get net positive sentiment: 100 * (positive - negative) / tokens_speech)
data_sentiment$net_positive_sentiment <- with(data_sentiment, 100 * (positive - negative) / tokens_speech)
```

Now we can plot the net positive sentiment for each text in ascendingn order.


```r
ggplot(data_sentiment, aes(x = reorder(speech, net_positive_sentiment), y = net_positive_sentiment)) +
  geom_point() + 
  coord_flip() +
  labs(x = NULL, y = "Positive sentiment") +
  theme_minimal()
```

<img src="/basic-operations/dfm/dfm_lookup_files/figure-html/unnamed-chunk-7-1.svg" width="768" />
