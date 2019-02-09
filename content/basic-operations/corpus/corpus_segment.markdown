---
title: Extract tags from texts
weight: 40
draft: false
---


```r
require(quanteda)
```

Using `corpus_segment()`, you can extract segments of texts and tags from documents. This is particular useful when you analyze sections of documents or transcripts separately.

### Document sections


```r
corp_tagged <- corpus(c("##INTRO This is the introduction.
                         ##DOC1 This is the first document.  Second sentence in Doc 1.
                         ##DOC3 Third document starts here.  End of third document.",
                        "##INTRO Document ##NUMBER Two starts before ##NUMBER Three."))
corp_sect <- corpus_segment(corp_tagged, pattern = "##*")

cbind(texts(corp_sect), docvars(corp_sect))
```

```
##                                               texts(corp_sect)  pattern
## text1.1                              This is the introduction.  ##INTRO
## text1.2 This is the first document.  Second sentence in Doc 1.   ##DOC1
## text1.3    Third document starts here.  End of third document.   ##DOC3
## text2.1                                               Document  ##INTRO
## text2.2                                      Two starts before ##NUMBER
## text2.3                                                 Three. ##NUMBER
```

### Speaker identifiers


```r
corp_speeches <- corpus("Mr. Smith: Text.
                       Mrs. Jones: More text.
                       Mr. Smith: I'm speaking, again.")
corp_speakers <- corpus_segment(corp_speeches, pattern = "\\b[A-Z].+\\s[A-Z][a-z]+:", valuetype = "regex")
cbind(texts(corp_speakers), docvars(corp_speakers))
```

```
##         texts(corp_speakers)     pattern
## text1.1                Text.  Mr. Smith:
## text1.2           More text. Mrs. Jones:
## text1.3 I'm speaking, again.  Mr. Smith:
```

You should use `corpus_reshape()` to split documents into sentences, but you can do similar operations using `corpus_segment()` by setting `pattern_position = "after"`.


```r
corp <- corpus(c(d1 = "This, is a sentence?  You: come here.", 
                 d2 = "Yes, yes okay."))
corp_sent <- corpus_segment(corp, pattern = "\\p{P}", valuetype = "regex", 
                            extract_pattern = FALSE, pattern_position = "after")
texts(corp_sent)
```

```
##             d1.1             d1.2             d1.3             d1.4 
##          "This," "is a sentence?"           "You:"     "come here." 
##             d2.1             d2.2 
##           "Yes,"      "yes okay."
```

