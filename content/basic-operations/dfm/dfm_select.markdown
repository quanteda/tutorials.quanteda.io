---
title: Select features
weight: 20
draft: false
---


```r
require(quanteda)
```


```r
dfmat_irish <- dfm(data_corpus_irishbudget2010, remove_punct = TRUE)
nfeat(dfmat_irish)
```

```
## [1] 5127
```

You can select features from a DFM using `dfm_select()`.


```r
dfmat_irish_nostop <- dfm_select(dfmat_irish, pattern = stopwords('en'), selection = 'remove')
nfeat(dfmat_irish_nostop)
```

```
## [1] 5008
```

`dfm_remove()` is an alias to `dfm_select(selection = 'remove')`. Therefore, the code above and below are equivalent.


```r
dfmat_irish_nostop <- dfm_remove(dfmat_irish, pattern = stopwords('en'))
nfeat(dfmat_irish_nostop)
```

```
## [1] 5008
```

You can also select features based on the length of features. 


```r
dfmat_irish_long <- dfm_select(dfmat_irish, min_nchar = 5)
nfeat(dfmat_irish_long)
```

```
## [1] 4274
```

```r
topfeatures(dfmat_irish_long, 10)
```

```
##     people     budget government     public   minister    economy 
##        266        260        236        179        170        160 
##      those      which      there      their 
##        160        156        147        144
```

While `dfm_select()` selects features based on patterns, `dfm_trim()` does this based on feature frequencies. If `min_termfreq = 10`, features that occur less than 10 times in the corpus are removed.


```r
dfmat_irish_freq <- dfm_trim(dfmat_irish, min_termfreq = 10)
nfeat(dfmat_irish_freq)
```

```
## [1] 660
```

If `max_docfreq = 0.1`, features that occur in more than 10% of the documents are removed.


```r
dfmat_irish_docfreq <- dfm_trim(dfmat_irish, max_docfreq = 0.1, docfreq_type = "prop")
nfeat(dfmat_irish_docfreq)
```

```
## [1] 2713
```
