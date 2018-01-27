---
title: Feature selection
weight: 30
chapter: false
draft: false
---



```r
require(quanteda)
```

## Select features from a dfm

`dfm_select()` selects or removes features from a dfm, based on feature name matches with pattern. Often, `dfm_select()` is used to eliminate features from a `dfm` already constructed, such as stopwords, or to select only terms of interest from a dictionary.



```r
# create a dfm
my_dfm <- dfm(c("My Christmas was ruined by your opposition tax plan.",
               "Does the United_States or Sweden have more progressive taxation?"),
             tolower = FALSE)

# construct a dictionary
my_dict <- dictionary(list(countries = c("United_States", "Sweden", "France"),
                          words_ending_in_y = c("by", "my"),
                          not_in_text = "blahblah"))
```

First, we select only those features from `my_dfm` that also occur in `my_dict`.


```r
dfm_select(my_dfm, my_dict)
```

```
## Document-feature matrix of: 2 documents, 4 features (50% sparse).
## 2 x 4 sparse Matrix of class "dfm"
##        features
## docs    My by United_States Sweden
##   text1  1  1             0      0
##   text2  0  0             1      1
```

```r
dfm_select(my_dfm, my_dict, case_insensitive = FALSE)
```

```
## Document-feature matrix of: 2 documents, 1 feature (50% sparse).
## 2 x 1 sparse Matrix of class "dfm"
##        features
## docs    by
##   text1  1
##   text2  0
```

The `selection` argument allows to specifiy whether the features that occur both in the `dfm` and the `dictionary` should be kept or removed. `valuetype` specifies the type of pattern matching.


```r
dfm_select(my_dfm, c("s$", ".y"), selection = "keep", valuetype = "regex")
```

```
## Document-feature matrix of: 2 documents, 6 features (50% sparse).
## 2 x 6 sparse Matrix of class "dfm"
##        features
## docs    My Christmas was by Does United_States
##   text1  1         1   1  1    0             0
##   text2  0         0   0  0    1             1
```

```r
dfm_select(my_dfm, c("s$", ".y"), selection = "remove", valuetype = "regex")
```

```
## Document-feature matrix of: 2 documents, 14 features (50% sparse).
## 2 x 14 sparse Matrix of class "dfm"
##        features
## docs    ruined your opposition tax plan . the or Sweden have more
##   text1      1    1          1   1    1 1   0  0      0    0    0
##   text2      0    0          0   0    0 0   1  1      1    1    1
##        features
## docs    progressive taxation ?
##   text1           0        0 0
##   text2           1        1 1
```

```r
dfm_select(my_dfm, stopwords("english"), selection = "keep", valuetype = "fixed")
```

```
## Document-feature matrix of: 2 documents, 9 features (50% sparse).
## 2 x 9 sparse Matrix of class "dfm"
##        features
## docs    My was by your Does the or have more
##   text1  1   1  1    1    0   0  0    0    0
##   text2  0   0  0    0    1   1  1    1    1
```

```r
dfm_select(my_dfm, stopwords("english"), selection = "remove", valuetype = "fixed")
```

```
## Document-feature matrix of: 2 documents, 11 features (50% sparse).
## 2 x 11 sparse Matrix of class "dfm"
##        features
## docs    Christmas ruined opposition tax plan . United_States Sweden
##   text1         1      1          1   1    1 1             0      0
##   text2         0      0          0   0    0 0             1      1
##        features
## docs    progressive taxation ?
##   text1           0        0 0
##   text2           1        1 1
```

You can also select based on character length.


```r
dfm_select(my_dfm, min_nchar = 5)
```

```
## Document-feature matrix of: 2 documents, 7 features (50% sparse).
## 2 x 7 sparse Matrix of class "dfm"
##        features
## docs    Christmas ruined opposition United_States Sweden progressive
##   text1         1      1          1             0      0           0
##   text2         0      0          0             1      1           1
##        features
## docs    taxation
##   text1        0
##   text2        1
```

Note that you can use the same functions for feature co-occurrence matrix using `fcm_select()`. 
