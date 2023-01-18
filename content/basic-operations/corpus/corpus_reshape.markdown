---
title: Change units of texts
weight: 30
draft: false
---


```r
require(quanteda)
```

`corpus_reshape()` allows you to change the unit of texts between documents, paragraphs and sentences. Since it records document identifiers, texts can be restored to the original unit even if the corpus is modified by other functions.


```r
corp <- corpus(data_char_ukimmig2010)
print(corp)
```

```
## Corpus consisting of 9 documents.
## BNP :
## "IMMIGRATION: AN UNPARALLELED CRISIS WHICH ONLY THE BNP CAN S..."
## 
## Coalition :
## "IMMIGRATION.  The Government believes that immigration has e..."
## 
## Conservative :
## "Attract the brightest and best to our country. Immigration h..."
## 
## Greens :
## "Immigration. Migration is a fact of life.  People have alway..."
## 
## Labour :
## "Crime and immigration The challenge for Britain We will cont..."
## 
## LibDem :
## "firm but fair immigration system Britain has always been an ..."
## 
## [ reached max_ndoc ... 3 more documents ]
```

```r
ndoc(corp)
```

```
## [1] 9
```

Change the unit of texts to sentences.


```r
corp_sent <- corpus_reshape(corp, to = "sentences")
print(corp_sent)
```

```
## Corpus consisting of 206 documents.
## BNP.1 :
## "IMMIGRATION: AN UNPARALLELED CRISIS WHICH ONLY THE BNP CAN S..."
## 
## BNP.2 :
## "The Scale of the Crisis  Britain's existence is in grave per..."
## 
## BNP.3 :
## "In the absence of urgent action, we, the indigenous British ..."
## 
## BNP.4 :
## "We, alone of all the political parties, have a decades-long ..."
## 
## BNP.5 :
## "British People Set to be a Minority within 30 - 50 Years: Th..."
## 
## BNP.6 :
## "Figures released by the ONS in January 2009 revealed that th..."
## 
## [ reached max_ndoc ... 200 more documents ]
```

```r
ndoc(corp_sent)
```

```
## [1] 206
```

Restore the original documents.


```r
corp_doc <- corpus_reshape(corp_sent, to = "documents")
print(corp_doc)
```

```
## Corpus consisting of 9 documents.
## BNP :
## "IMMIGRATION: AN UNPARALLELED CRISIS WHICH ONLY THE BNP CAN S..."
## 
## Coalition :
## "IMMIGRATION.  The Government believes that immigration has e..."
## 
## Conservative :
## "Attract the brightest and best to our country.  Immigration ..."
## 
## Greens :
## "Immigration.  Migration is a fact of life.  People have alwa..."
## 
## Labour :
## "Crime and immigration  The challenge for Britain  We will co..."
## 
## LibDem :
## "firm but fair immigration system  Britain has always been an..."
## 
## [ reached max_ndoc ... 3 more documents ]
```

```r
ndoc(corp_doc)
```

```
## [1] 9
```

If you apply `corpus_subset()` to `corp_sent`, you can only keep long sentences (more than 10 words).


```r
corp_sent_long <- corpus_subset(corp_sent, ntoken(corp_sent) >= 10)
ndoc(corp_sent_long)
```

```
## [1] 182
```

```r
corp_doc_long <- corpus_reshape(corp_sent_long, to = "documents")
ndoc(corp_doc_long)
```

```
## [1] 9
```

