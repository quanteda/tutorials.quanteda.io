---
title: Change units of texts
weight: 30
draft: false
---


```r
require(quanteda)
```

`corpus_reshape()` allows to change the unit of texts between documents paragraphs and sentences. Since it records document identifiers, texts can be restored to the original units even after mofdification of corpus.


```r
corp <- corpus(data_char_ukimmig2010)
ndoc(corp)
```

```
## [1] 9
```

Change the unit of texts to sentences.


```r
sent_corp <- corpus_reshape(corp, 'sentences')
ndoc(sent_corp)
```

```
## [1] 207
```

Restore the original documents.


```r
corp2 <- corpus_reshape(sent_corp, 'documents')
ndoc(corp2)
```

```
## [1] 9
```

If you apply `corpus_subset()` to `sent_corp`, you can only keep long senteces (more than 10 words).


```r
longsent_corp <- corpus_subset(sent_corp, ntoken(sent_corp) >= 10)
ndoc(longsent_corp)
```

```
## [1] 183
```

```r
corp3 <- corpus_reshape(longsent_corp, 'documents')
ndoc(corp3)
```

```
## [1] 9
```

