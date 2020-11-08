  ---
title: Arabic
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
print(toks[3], max_ndoc = 1, max_ntoken = -1)
```

```
## Tokens consisting of 1 document and 4 docvars.
## heb :
##  [1] "סעיף"      "ב"         "אדם"       "זכאי"      "לזכויות"   "ולחרויות"  "שנקבעו"    "בהכרזש"   
##  [9] "הפליה"     "כלשהיא"    "מטעמי"     "גזע"       "צבע"       "מין"       "לשון"      "דח"       
## [17] "דעה"       "פוליטית"   "דעה"       "בבעיות"    "אחרות"     "מוצא"      "לאומי"     "חברתי"    
## [25] "קנין"      "לידה"      "מעמד"      "גדולה"     "יופלה"     "אדם"       "פי"        "מעמדה"    
## [33] "המדיני"    "פי"        "סמכותה"    "פי"        "מעמדה"     "הבינלאומי" "המדינה"    "הארץ"     
## [41] "שאליה"     "שייך"      "דין"       "שהארץ"     "עצמאית"    "נתונה"     "לנאמנות"   "נטולת"    
## [49] "שלטון"     "שריבונותה" "מוגבלת"    "הגבלה"     "אחרת"
```


```r
dfmat <- dfm(toks)
print(dfmat)
```

```
## Document-feature matrix of: 63 documents, 704 features (97.9% sparse) and 4 docvars.
##        features
## docs    הכרזה באי עולם בדבר זכויות האדם הואיל והכרה בכבוד הטבעי
##   heb.1     1   2    2    2      4    8     7     1     1     1
##   heb.2     0   0    0    0      0    0     0     0     0     0
##   heb.3     0   0    0    0      0    0     0     0     0     0
##   heb.4     0   0    0    0      0    0     0     0     0     0
##   heb.5     0   0    0    0      0    0     0     0     0     0
##   heb.6     0   0    0    0      0    0     0     0     0     0
## [ reached max_ndoc ... 57 more documents, reached max_nfeat ... 694 more features ]
```

