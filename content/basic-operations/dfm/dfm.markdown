---
title: Constructor
weight: 10
chapter: true
draft: false
---


```
## Warning: package 'stopwords' was built under R version 3.4.3
```

Tokenizing texts, as dicussed in the previous section, is an intermediate option, and most users will want to skip straight to constructing a document-feature matrix. For this, we have a Swiss-army knife function, called `dfm()`, which performs tokenization and tabulates the extracted features into a matrix of documents by features. 


## Construct a document-feature matrix


```r
# load Guardian corpus
news_corp <- quanteda.corpora::download('data_corpus_guardian')

# construct a dfm

news_dfm <- dfm(news_corp, verbose = TRUE)
```

```
## Creating a dfm from a corpus input...
```

```
##    ... lowercasing
```

```
##    ... found 6,000 documents, 99,360 features
```

```
##    ... created a 6,000 x 99,360 sparse dfm
##    ... complete. 
## Elapsed time: 11.7 seconds.
```

From `verbose = TRUE` we can see that all words were transformed to lowercase which is the default. All of the options to tokens() can be passed to dfm(), however. For instance, this `dfm` contains punctuation and so called stopwords as we can see when we plot the 10 most frequent words in the corpus using `textstat_frequency()`. 


```r
textstat_frequency(news_dfm, n = 10)
```

```
##    feature frequency rank docfreq group
## 1      the    282794    1    5966   all
## 2        ,    227480    2    5965   all
## 3        .    203215    3    5959   all
## 4       to    135020    4    5949   all
## 5        |    120731    5    5920   all
## 6       of    120626    6    5943   all
## 7        a    106730    7    5937   all
## 8        "    105513    8    5463   all
## 9      and    103822    9    5933   all
## 10      in     96986   10    5931   all
```

## Remove features

We repeat the step above, but specify in the options to remove punctuation, English stopwords and numbers.


```r
news_dfm_small <- dfm(news_corp, remove = stopwords("en"),
                remove_punct = TRUE,
                remove_numbers = TRUE,
                verbose = TRUE)
```

```
## Creating a dfm from a corpus input...
```

```
##    ... lowercasing
```

```
##    ... found 6,000 documents, 94,362 features
```

```
##    ... removed 174 features
##    ... created a 6,000 x 94,188 sparse dfm
##    ... complete. 
## Elapsed time: 10.9 seconds.
```

```r
# check most frequent words again
textstat_frequency(news_dfm_small, n = 10)
```

```
##       feature frequency rank docfreq group
## 1        said     28413    1    4809   all
## 2      people     11169    2    3363   all
## 3         one      9884    3    3859   all
## 4         new      8024    4    3072   all
## 5        also      7901    5    3684   all
## 6          us      7091    6    2549   all
## 7         can      6972    7    2985   all
## 8  government      6821    8    2354   all
## 9        year      6570    9    2927   all
## 10       last      6335   10    3129   all
```

The **stopwords** package includes stopwords for several languages. For instance:


```r
head(stopwords("en"))
```

```
## [1] "i"      "me"     "my"     "myself" "we"     "our"
```

```r
head(stopwords("de"))
```

```
## [1] "aber"  "alle"  "allem" "allen" "aller" "alles"
```

```r
head(stopwords("ru"))
```

```
## [1] "и"   "в"   "во"  "не"  "что" "он"
```

## Stemming

Stemming relies on the **SnowballC** package's implementation of the Porter stemmer, and is available for the following languages (it does not work perfectly, but it is very fast). When constructing a corpus, you can use `stem = TRUE` to stem the words in the corpus.


```r
news_dfm_stem <- dfm(news_dfm_small, stem = TRUE)
```

Let's compare the differences:


```r
head(featnames(news_dfm_small))
```

```
## [1] "london"      "masterclass" "climate"     "change"      "want"       
## [6] "understand"
```

```r
head(featnames(news_dfm_stem))
```

```
## [1] "london"      "masterclass" "climat"      "chang"       "want"       
## [6] "understand"
```





