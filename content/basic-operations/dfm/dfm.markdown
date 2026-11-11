---
title: Construct a DFM
weight: 10
draft: false
---

A tokens object still keeps track of word order, but many statistical text analysis does not actually need order, only how often each word occurs. A document-feature matrix, ignores the word order and instead counts how many times each word (or "feature") appears in each document.



``` r
library(quanteda)
library(quanteda.textstats)
```




``` r
toks_inaug <- tokens(data_corpus_inaugural, remove_punct = TRUE)
dfmat_inaug <- dfm(toks_inaug)
print(dfmat_inaug)
```

```
## Document-feature matrix of: 60 documents, 9,573 features (91.99% sparse) and 4 docvars.
##                  features
## docs              fellow-citizens  of the senate and house representatives among vicissitudes incident
##   1789-Washington               1  71 116      1  48     2               2     1            1        1
##   1793-Washington               0  11  13      0   2     0               0     0            0        0
##   1797-Adams                    3 140 163      1 130     0               2     4            0        0
##   1801-Jefferson                2 104 130      0  81     0               0     1            0        0
##   1805-Jefferson                0 101 143      0  93     0               0     7            0        0
##   1809-Madison                  1  69 104      0  43     0               0     0            0        0
## [ reached max_ndoc ... 54 more documents, reached max_nfeat ... 9,563 more features ]
```

The printed summary tells you the shape of the matrix (how many documents and how many distinct features) and how sparse it is: the proportion of cells that are zero. Text data typically produces very sparse matrices, since any one document only ever uses a small fraction of all the words that appear somewhere in the whole corpus. You can get the number of documents and features directly with `ndoc()` and `nfeat()`.


``` r
ndoc(dfmat_inaug)
```

```
## [1] 60
```

``` r
nfeat(dfmat_inaug)
```

```
## [1] 9573
```

You can also obtain the names of documents and features with `docnames()` and `featnames()`.


``` r
head(docnames(dfmat_inaug), 20)
```

```
##  [1] "1789-Washington" "1793-Washington" "1797-Adams"      "1801-Jefferson"  "1805-Jefferson" 
##  [6] "1809-Madison"    "1813-Madison"    "1817-Monroe"     "1821-Monroe"     "1825-Adams"     
## [11] "1829-Jackson"    "1833-Jackson"    "1837-VanBuren"   "1841-Harrison"   "1845-Polk"      
## [16] "1849-Taylor"     "1853-Pierce"     "1857-Buchanan"   "1861-Lincoln"    "1865-Lincoln"
```

``` r
head(featnames(dfmat_inaug), 20)
```

```
##  [1] "fellow-citizens" "of"              "the"             "senate"          "and"            
##  [6] "house"           "representatives" "among"           "vicissitudes"    "incident"       
## [11] "to"              "life"            "no"              "event"           "could"          
## [16] "have"            "filled"          "me"              "with"            "greater"
```


The most frequent features can be found more directly using `topfeatures()`.

``` r
topfeatures(dfmat_inaug, 10)
```

```
##   the    of   and    to    in     a   our    we  that    be 
## 10309  7271  5550  4678  2856  2342  2295  1912  1852  1542
```

Just like normal matrices, you can use `rowSums()` and `colSums()` to calculate marginals, as you saw in the [Introduction chapter](/introduction/r-commands). `rowSums()` gives the total number of words in each document; `colSums()` gives the total number of times each word occurs across the corpus.


``` r
head(rowSums(dfmat_inaug), 10)
```

```
## 1789-Washington 1793-Washington      1797-Adams  1801-Jefferson  1805-Jefferson    1809-Madison 
##            1430             135            2318            1726            2166            1175 
##    1813-Madison     1817-Monroe     1821-Monroe      1825-Adams 
##            1210            3370            4472            2915
```

``` r
head(colSums(dfmat_inaug), 10)
```

```
## fellow-citizens              of             the          senate             and           house 
##              39            7271           10309              15            5550              11 
## representatives           among    vicissitudes        incident 
##              19             108               5               8
```

So far, every count in the matrix has been a raw frequency. Raw counts are not always the fairest basis for comparison: a long document naturally contains more of every word than a short one, because it has more words overall. To convert counts to proportions within each document, so that documents of different lengths become comparable, use `dfm_weight(scheme = "prop")`.


``` r
dfmat_inaug_prop <- dfm_weight(dfmat_inaug, scheme  = "prop")
print(dfmat_inaug_prop)
```

```
## Document-feature matrix of: 60 documents, 9,573 features (91.99% sparse) and 4 docvars.
##                  features
## docs              fellow-citizens         of        the       senate        and       house representatives
##   1789-Washington    0.0006993007 0.04965035 0.08111888 0.0006993007 0.03356643 0.001398601    0.0013986014
##   1793-Washington    0            0.08148148 0.09629630 0            0.01481481 0              0           
##   1797-Adams         0.0012942192 0.06039689 0.07031924 0.0004314064 0.05608283 0              0.0008628128
##   1801-Jefferson     0.0011587486 0.06025492 0.07531866 0            0.04692932 0              0           
##   1805-Jefferson     0            0.04662973 0.06602031 0            0.04293629 0              0           
##   1809-Madison       0.0008510638 0.05872340 0.08851064 0            0.03659574 0              0           
##                  features
## docs                     among vicissitudes     incident
##   1789-Washington 0.0006993007 0.0006993007 0.0006993007
##   1793-Washington 0            0            0           
##   1797-Adams      0.0017256255 0            0           
##   1801-Jefferson  0.0005793743 0            0           
##   1805-Jefferson  0.0032317636 0            0           
##   1809-Madison    0            0            0           
## [ reached max_ndoc ... 54 more documents, reached max_nfeat ... 9,563 more features ]
```

{{% notice tip %}}
`textstat_frequency()`, described in Chapter 4, offers more advanced functionalities than `topfeatures()` and returns a `data.frame` object, which is easier to use as the input for further analyses.
{{% /notice %}}

`"prop"` is only one of several weighting schemes available through the `scheme` argument of `dfm_weight()`. Two other schemes are worth knowing early on. `"logcount"` compresses large counts by taking their logarithm, which stops a handful of very frequent words from dominating a comparison. `"boolean"` reduces every count to a simple 0/1 flag for whether a word appears at all, regardless of how many times. Applying `topfeatures()` after `"boolean"` weighting therefore does not tell you the most frequent words, but the words that appear in the most documents.


``` r
topfeatures(dfm_weight(dfmat_inaug, scheme = "logcount"), 10)
```

```
##      the       of      and       to       in        a      our     that       is       be 
## 188.2903 179.2293 173.3189 168.9676 155.4927 148.9649 146.2271 143.9651 137.1995 136.8473
```

``` r
topfeatures(dfm_weight(dfmat_inaug, scheme = "boolean"), 10)
```

```
##   of  the  and   to have that   by   in   it   be 
##   60   60   60   60   60   60   60   60   60   60
```

Every one of these top ten words scores 60 under `"boolean"` weighting: each appears in all 60 inaugural speeches. That's a different, and in this case less interesting, ranking than the raw-frequency ranking from `topfeatures(dfmat_inaug, 10)` above.

{{% notice tip %}}
Further schemes, `"propmax"`, `"augmented"` and `"logave"`, weight counts relative to a document's own most frequent feature rather than its total length. See `?dfm_weight` for the exact formula behind each scheme.
{{% /notice %}}

Proportions treat every word as equally informative. In practice, though, a word that appears in almost every document, such as "government" in a set of political speeches, tells you little about what makes any one document distinctive. You can also weight the frequency count by how unevenly a feature is distributed across documents: use `dfm_tfidf()` (term frequency-inverse document frequency) for this. Features that are common everywhere are downweighted, while features that are concentrated in a few documents are upweighted.


``` r
dfmat_inaug_tfidf <- dfm_tfidf(dfmat_inaug)
print(dfmat_inaug_tfidf)
```

```
## Document-feature matrix of: 60 documents, 9,573 features (91.99% sparse) and 4 docvars.
##                  features
## docs              fellow-citizens of the    senate and    house representatives     among vicissitudes
##   1789-Washington       0.4993976  0   0 0.8239087   0 1.750123        1.264046 0.1446828     1.079181
##   1793-Washington       0          0   0 0           0 0               0        0             0       
##   1797-Adams            1.4981929  0   0 0.8239087   0 0               1.264046 0.5787312     0       
##   1801-Jefferson        0.9987953  0   0 0           0 0               0        0.1446828     0       
##   1805-Jefferson        0          0   0 0           0 0               0        1.0127796     0       
##   1809-Madison          0.4993976  0   0 0           0 0               0        0             0       
##                  features
## docs              incident
##   1789-Washington        1
##   1793-Washington        0
##   1797-Adams             0
##   1801-Jefferson         0
##   1805-Jefferson         0
##   1809-Madison           0
## [ reached max_ndoc ... 54 more documents, reached max_nfeat ... 9,563 more features ]
```

{{% notice warning %}}
Even after applying `dfm_weight()` or `dfm_tfidf()`, `topfeatures()` works on a document-feature matrix, but it can be misleading if applied to more than one document.
{{% /notice %}}

## Converting a DFM to other formats

A DFM is stored internally as a sparse matrix: the many zero counts are not actually stored in memory. That's what makes it possible to work with document-feature matrices containing millions of cells. Sometimes, though, you need an ordinary, fully "dense" R matrix instead, for example to pass a small example into a function that does not know about sparse matrices. `as.matrix()` converts a DFM into a standard base R matrix.


``` r
dfmat_small <- dfm(tokens(c(doc1 = "a a a b b c", doc2 = "a b b b b b")))
as.matrix(dfmat_small)
```

```
##       features
## docs   a b c
##   doc1 3 2 1
##   doc2 1 5 0
```

{{% notice warning %}}
Only convert a DFM with `as.matrix()` for small examples like this one, or after trimming it down with `dfm_trim()`. Converting a large, realistically sparse DFM into a dense matrix can exhaust your computer's memory, since every zero suddenly has to be stored explicitly.
{{% /notice %}}
