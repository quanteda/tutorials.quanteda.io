---
title: Word and document embeddings
weight: 70
draft: false
---

Latent Semantic Scaling (LSS) is a flexible and cost-efficient semi-supervised document scaling technique. The technique relies on word embeddings and users only need to provide a small set of "seed words" to locate documents on a specific dimension.

Install the **LSX** package from CRAN.


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



We must segment news articles into sentences in the corpus to accurately estimate semantic proximity between words. We can also use the [Marimo](https://github.com/koheiw/marimo) stopwords list (`source = "marimo"`) to remove words commonly used in news reports.


``` r
# tokenize text corpus and remove various features
corp_sent <- corpus_reshape(corp_news, to =  "sentences")
toks_sent <- corp_sent |> 
    tokens(remove_punct = TRUE, remove_symbols = TRUE, 
           remove_numbers = TRUE, remove_url = TRUE) |> 
    tokens_remove(stopwords("en", source = "marimo")) |>
    tokens_remove(c("*-time", "*-timeUpdated", "GMT", "BST", "*.com"))  
```



``` r
wov <- textmodel_word2vec(toks_sent)

head(similarity(wov, "london"))
```

```
##      london      
## [1,] "london"    
## [2,] "glasgow"   
## [3,] "london's"  
## [4,] "newham"    
## [5,] "manchester"
## [6,] "midlands"
```

``` r
head(similarity(wov, analogy(~ berlin - germany + france)))
```

```
##      [,1]      
## [1,] "berlin"  
## [2,] "paris"   
## [3,] "brussels"
## [4,] "france"  
## [5,] "vienna"  
## [6,] "cairo"
```



``` r
dov <- textmodel_doc2vec(toks_sent)
```

{{% notice ref %}}
- Mikolov T. et al. 2013. "[Distributed Representations of Words and Phrases and their Compositionality](https://arxiv.org/abs/1310.4546)". arxiv.
- Le V. Q. 2014. "[Distributed Representations of Sentences and Documents](https://arxiv.org/abs/1405.4053)". arxiv.
{{% /notice %}}
