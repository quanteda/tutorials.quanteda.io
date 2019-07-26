---
title: Change units of texts
weight: 30
draft: false
---


```r
require(quanteda)
```

`corpus_reshape()` allows to change the unit of texts between documents, paragraphs and sentences. Since it records document identifiers, texts can be restored to the original unit even if the corpus is modified by other functions.


```r
corp <- corpus(data_char_ukimmig2010)
ndoc(corp)
```

```
## [1] 9
```

Change the unit of texts to sentences.


```r
corp_sent <- corpus_reshape(corp, to = 'sentences')
ndoc(corp_sent)
```

```
## [1] 207
```

Restore the original documents.


```r
corp_documents <- corpus_reshape(corp_sent, to = 'documents')
ndoc(corp_documents)
```

```
## [1] 9
```

If you apply `corpus_subset()` to `corp_sent`, you can only keep long senteces (more than 10 words).


```r
corp_longsent <- corpus_subset(corp_sent, ntoken(corp_sent) >= 10)
ndoc(corp_longsent)
```

```
## [1] 183
```

```r
corp_documents_long <- corpus_reshape(corp_longsent, to = 'documents')
ndoc(corp_documents_long)
```

```
## [1] 9
```

