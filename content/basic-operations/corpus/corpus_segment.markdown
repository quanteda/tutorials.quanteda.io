---
title: Extract tags from texts
weight: 40
draft: false
---


```r
require(quanteda)
```

Using `corpus_segment()`, you can extract segments of texts and tags from documents. This is particularly useful when you analyze sections of documents or transcripts separately.

### Document sections


```r
corp_tagged <- corpus(c("##INTRO This is the introduction.
                         ##DOC1 This is the first document.  Second sentence in Doc 1.
                         ##DOC3 Third document starts here.  End of third document.",
                        "##INTRO Document ##NUMBER Two starts before ##NUMBER Three."))
corp_sect <- corpus_segment(corp_tagged, pattern = "##*")

cbind(docvars(corp_sect), text = as.character(corp_sect))
```

```
##          pattern                                                   text
## text1.1  ##INTRO                              This is the introduction.
## text1.2   ##DOC1 This is the first document.  Second sentence in Doc 1.
## text1.3   ##DOC3    Third document starts here.  End of third document.
## text2.1  ##INTRO                                               Document
## text2.2 ##NUMBER                                      Two starts before
## text2.3 ##NUMBER                                                 Three.
```

### Speaker identifiers


```r
corp_speeches <- corpus("Mr. Smith: Text.
                        Mrs. Jones: More text.
                        Mr. Smith: I'm speaking, again.")
corp_speakers <- corpus_segment(corp_speeches, pattern = "\\b[A-Z].+\\s[A-Z][a-z]+:", valuetype = "regex")
cbind(docvars(corp_speakers), text = as.character(corp_speakers))
```

```
##             pattern                 text
## text1.1  Mr. Smith:                Text.
## text1.2 Mrs. Jones:           More text.
## text1.3  Mr. Smith: I'm speaking, again.
```

You should use `corpus_reshape()` to split documents into sentences, but you can do similar operations using `corpus_segment()` by setting `pattern_position = "after"`.


```r
corp <- corpus(c(d1 = "This, is a sentence?  You: come here.", 
                 d2 = "Yes, yes okay."))
corp_sent <- corpus_segment(corp, pattern = "\\p{P}", valuetype = "regex", 
                            extract_pattern = FALSE, pattern_position = "after")
print(corp_sent)
```

```
## Corpus consisting of 6 documents.
## d1.1 :
## "This,"
## 
## d1.2 :
## "is a sentence?"
## 
## d1.3 :
## "You:"
## 
## d1.4 :
## "come here."
## 
## d2.1 :
## "Yes,"
## 
## d2.2 :
## "yes okay."
```

