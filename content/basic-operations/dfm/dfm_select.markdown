---
title: Select features
weight: 20
draft: false
---


```r
require(quanteda)
```


```r
irish_dfm <- dfm(data_corpus_irishbudget2010, remove_punct = TRUE)
nfeat(irish_dfm)
```

```
## [1] 5127
```

You can select features from a DFM using `dfm_select()`.


```r
nostop_irish_dfm <- dfm_select(irish_dfm, stopwords('en'), selection = 'remove')
nfeat(nostop_irish_dfm)
```

```
## [1] 5008
```

`dfm_remove()` is an alias to `dfm_select(selection = 'remove')`. Therefore, the code above and below are equivalent.


```r
nostop_irish_dfm <- dfm_remove(irish_dfm, stopwords('en'))
nfeat(nostop_irish_dfm)
```

```
## [1] 5008
```

You can also select features based on the length of features. 


```r
long_irish_dfm <- dfm_select(irish_dfm, min_nchar = 5)
nfeat(long_irish_dfm)
```

```
## [1] 4274
```

```r
topfeatures(long_irish_dfm, 10)
```

```
##     people     budget government     public   minister    economy 
##        266        260        236        179        170        160 
##      those      which      there      their 
##        160        156        147        144
```

While `dfm_select()` selects features based on patterns, `dfm_trim()` does this based on feature frequencies. If `min_termfreq = 10`, features that occur less than 10 times in the corpus are removed.


```r
freq_irish_dfm <- dfm_trim(irish_dfm, min_termfreq = 10)
nfeat(freq_irish_dfm)
```

```
## [1] 660
```

If `max_docfreq = 0.1`, features that occur in more than 10% of the documents are removed.


```r
docfreq_irish_dfm <- dfm_trim(irish_dfm, max_docfreq = 0.1, docfreq_type = "prop")
nfeat(docfreq_irish_dfm)
```

```
## [1] 2713
```
