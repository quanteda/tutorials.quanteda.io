---
title: Word/document embedding
weight: 80
draft: false
---

Embedding is a technique to represent words or documents by dense and low dimensional vectors that capture word their similarities. These vectors can be used to compute their similarity or to train supervised machine learning algorithms. We can tarin word2vec and doc2vec models using **wordvector** package. 

Install the **wordvector** package from CRAN.


``` r
install.packages("wordvector")
```


``` r
library(quanteda)
library(quanteda.corpora)
library(wordvector)
```

Download a corpus with news articles using **quanteda.corpora**'s `download()` function.


``` r
corp_news <- download("data_corpus_guardian")
```



Since word2vec and doc2vec are trained on tokens objects using context windows, we do not need to segment news articles into sentences, but reshaping prevents the windows from spanning across sentences. 


``` r
# tokenize text corpus and remove various features
corp_sent <- corpus_reshape(corp_news, to =  "sentences")
toks_sent <- corp_sent |> 
    tokens(remove_punct = TRUE, remove_symbols = TRUE, 
           remove_numbers = TRUE, remove_url = TRUE) |> 
    tokens_remove(stopwords("en", source = "marimo")) |>
    tokens_remove("guardian*")

ndoc(toks_sent)
```

```
## [1] 202011
```

``` r
length(types(toks_sent))
```

```
## [1] 105324
```
## Word embedding

We can train word2vec using the `cbow` (continuous bag-of-words) or `sg` (skip-gram) the algorithm. When `dim` is larger, the models capture more subtle information but require greater computational resources. After training, we can extract document vector using `as.matrix()`.


``` r
wov <- textmodel_word2vec(toks_sent, dim = 50, type = "cbow", min_count = 5, verbose = TRUE)
```

```
## Training continuous BOW model with 50 dimensions
##  ...using 28 threads for distributed computing
##  ...initializing
##  ...negative sampling in 10 iterations
##  ......iteration 1 elapsed time: 0.22 seconds (alpha: 0.0457)
##  ......iteration 2 elapsed time: 0.41 seconds (alpha: 0.0412)
##  ......iteration 3 elapsed time: 0.65 seconds (alpha: 0.0363)
##  ......iteration 4 elapsed time: 0.85 seconds (alpha: 0.0315)
##  ......iteration 5 elapsed time: 1.07 seconds (alpha: 0.0268)
##  ......iteration 6 elapsed time: 1.29 seconds (alpha: 0.0221)
##  ......iteration 7 elapsed time: 1.53 seconds (alpha: 0.0172)
##  ......iteration 8 elapsed time: 1.75 seconds (alpha: 0.0125)
##  ......iteration 9 elapsed time: 1.97 seconds (alpha: 0.0079)
##  ......iteration 10 elapsed time: 2.14 seconds (alpha: 0.0042)
##  ...complete
```

``` r
dim(as.matrix(wov))
```

```
## [1] 29491    50
```

`similarity()` is a user-friendly function for finding similar words. If `analogy()` wrapper is used, cosine similarity is computed based on the weighted vectors.


``` r
head(similarity(wov, "london", mode = "numeric"))
```

```
##                 london
## london      1.00000000
## climate    -0.05814339
## change      0.02099635
## want       -0.05332255
## understand -0.04906032
## assistant  -0.01201705
```

``` r
head(similarity(wov, "london")) # top 6 most similar words
```

```
##      london      
## [1,] "london"    
## [2,] "manchester"
## [3,] "birmingham"
## [4,] "liverpool" 
## [5,] "glasgow"   
## [6,] "midlands"
```

``` r
head(similarity(wov, analogy(~ berlin - germany + france)))
```

```
##      [,1]       
## [1,] "berlin"   
## [2,] "amsterdam"
## [3,] "rome"     
## [4,] "munich"   
## [5,] "jordan"   
## [6,] "vienna"
```

Since word2vec is a language model, we can also compute the probability of a word conditional on context words. 


``` r
head(probability(wov, "eu", mode = "numeric")) 
```

```
##                     eu
## london     0.007255814
## climate    0.113369969
## change     0.284147372
## want       0.424482619
## understand 0.197798591
## assistant  0.006043436
```

``` r
head(probability(wov, "eu")) # top 6 most likely contexts
```

```
##      eu          
## [1,] "leave"     
## [2,] "membership"
## [3,] "eu"        
## [4,] "referendum"
## [5,] "brexit"    
## [6,] "leaders"
```

## Document embedding

We can train doc2vec with the `dm` (distributed memory) or `dbow` (distributed bag-of-words) model a similar manner as word vectors.


``` r
dov <- textmodel_doc2vec(toks_sent, dim = 50, type = "dbow", min_count = 5, verbose = TRUE)
```

```
## Training distributed BOW model with 50 dimensions
##  ...using 28 threads for distributed computing
##  ...initializing
##  ...negative sampling in 10 iterations
##  ......iteration 1 elapsed time: 0.17 seconds (alpha: 0.0454)
##  ......iteration 2 elapsed time: 0.38 seconds (alpha: 0.0404)
##  ......iteration 3 elapsed time: 0.56 seconds (alpha: 0.0356)
##  ......iteration 4 elapsed time: 0.75 seconds (alpha: 0.0306)
##  ......iteration 5 elapsed time: 0.93 seconds (alpha: 0.0258)
##  ......iteration 6 elapsed time: 1.11 seconds (alpha: 0.0209)
##  ......iteration 7 elapsed time: 1.32 seconds (alpha: 0.0157)
##  ......iteration 8 elapsed time: 1.67 seconds (alpha: 0.0098)
##  ......iteration 9 elapsed time: 1.92 seconds (alpha: 0.0060)
##  ......iteration 10 elapsed time: 2.22 seconds (alpha: 0.0021)
##  ...complete
```

``` r
dim(as.matrix(dov))
```

```
## [1] 202011     50
```

We have to specify layer = "documents" to find similar documents because the `dm` model contains both word and document vectors.


``` r
s <- similarity(dov, "text99432.1", layer = "documents")
head(s)
```

```
##      text99432.1    
## [1,] "text99432.1"  
## [2,] "text47095.29" 
## [3,] "text92895.1"  
## [4,] "text128597.4" 
## [5,] "text135808.21"
## [6,] "text179115.2"
```
We can retrieve the most similar documents (sentences) from the corpus.


``` r
print(corp_sent[s], max_nchar = -1)
```

```
## Corpus consisting of 202,011 documents and 9 docvars.
## text99432.1 :
## "At least half of all UK banknotes in circulation are being used for purposes
## such as drug dealing, prostitution and dodgy business deals, or are being held
## abroad, according to a Bank of England report."
## 
## text47095.29 :
## "Captions: 6.5% Medium-term equilibrium unemployment, according to the Bank of
## England - a definition of full employment"
## 
## text92895.1 :
## "Barclays was subjected to more regulatory attention than any other UK high
## street bank in 2014, receiving 186 visits from the Financial Conduct Authority's
## officials."
## 
## text128597.4 :
## "UK arms sales in the three-month period from July to September 2015 for
## the export category that covers missiles, rockets and bombs amounted to
## 1,066,216,510, the BIS documents show."
## 
## text135808.21 :
## "This, according to the Bank, is affecting the growth rate of earnings."
## 
## text179115.2 :
## "Of the 44m hectolitres (7.74bn pints) of beer sold in the UK during 2015, 51%
## was sold in the off-trade, which is dominated by large supermarkets, according
## to the British Beer and Pub Association (BBPA)."
## 
## [ reached max_ndoc ... 202,005 more documents ]
```

{{% notice ref %}}
- Mikolov T. et al. 2013. "[Distributed Representations of Words and Phrases and their Compositionality](https://arxiv.org/abs/1310.4546)". arxiv.
- Le V. Q. 2014. "[Distributed Representations of Sentences and Documents](https://arxiv.org/abs/1405.4053)". arxiv.
{{% /notice %}}
