---
title: Select features
weight: 20
draft: false
---



```r
require(quanteda)
```



```r
ie_dfm <- dfm(data_corpus_irishbudget2010, remove_punct = TRUE)
nfeat(ie_dfm)
```

```
## [1] 5127
```

You can select features from a DFM using `dfm_select()`.


```r
nostop_ie_dfm <- dfm_select(ie_dfm, stopwords('en'), selection = 'remove')
nfeat(nostop_ie_dfm)
```

```
## [1] 5008
```

`dfm_remove()` is an alias to `dfm_select(selection = 'remove')`. Therefore, the code above and below are equivalent.


```r
nostop_ie_dfm <- dfm_remove(ie_dfm, stopwords('en'))
nfeat(nostop_ie_dfm)
```

```
## [1] 5008
```

You can also select features based on the length of features. 


```r
long_ie_dfm <- dfm_select(ie_dfm, min_nchar = 5)
nfeat(long_ie_dfm)
```

```
## [1] 4274
```

```r
topfeatures(long_ie_dfm, 10)
```

```
##     people     budget government     public   minister    economy 
##        266        260        236        179        170        160 
##      those      which      there      their 
##        160        156        147        144
```

While `dfm_select()` selects features based on patters, `dfm_trim()` does based on feature frequencies. If `min_count = 10`, features that occur less than 10 times in the corpus are removed.


```r
freq_ie_dfm <- dfm_trim(ie_dfm, min_count = 10)
nfeat(long_ie_dfm)
```

```
## [1] 4274
```

If `max_docfreq = 0.5`, features that occur in more than half of the documents are removed.


```r
docfreq_ie_dfm <- dfm_trim(ie_dfm, max_docfreq = 0.5)
nfeat(docfreq_ie_dfm)
```

```
## [1] 4745
```
