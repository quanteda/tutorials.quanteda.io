---
title: Hebrew
weight: 20
draft: false
---

{{% author %}}By Elad Segev{{% /author %}} 


```r
require(quanteda)
require(quanteda.corpora)
options(width = 110)
```


```r
corp <- corpus_reshape(data_corpus_udhr["heb"], to = "paragraphs")
toks <- tokens(corp, remove_punct = TRUE, remove_numbers = TRUE) %>% 
  tokens_remove(stopwords("he", source = "marimo"))
print(toks[2], max_ndoc = 1, max_ntoken = -1)
```

```
## Tokens consisting of 1 document and 4 docvars.
## heb :
##  [1] "הואיל"       "והכרה"       "בכבוד"       "הטבעי"       "אשר"         "בני"         "משפהת"      
##  [8] "האדם"        "ובזכויותיהם" "השוות"       "והבלתי"      "נפקעות"      "יסוד"        "החופש"      
## [15] "הצדק"        "והשלום"      "בעולם"
```


```r
dfmat <- dfm(toks)
print(dfmat)
```

```
## Document-feature matrix of: 89 documents, 704 features (98.5% sparse) and 4 docvars.
##        features
## docs    הכרזה באי עולם בדבר זכויות האדם הואיל והכרה בכבוד הטבעי
##   heb.1     1   1    1    1      1    1     0     0     0     0
##   heb.2     0   0    0    0      0    1     1     1     1     1
##   heb.3     0   0    1    0      0    1     1     0     0     0
##   heb.4     0   0    0    0      0    2     1     0     0     0
##   heb.5     0   0    0    0      0    0     1     0     0     0
##   heb.6     0   0    0    0      0    1     1     0     0     0
## [ reached max_ndoc ... 83 more documents, reached max_nfeat ... 694 more features ]
```

