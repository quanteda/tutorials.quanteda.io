---
title: Segment texts
weight: 30
chapter: false
draft: false
---



```r
require(quanteda)
require(quanteda.corpora)
```


The function `corpus_segment()` allows you to segment a `corpus` object or character vector. With `corpus_segment()` you can break texts into smaller documents based on a regular pattern (for example a speaker identifier in a transcript) or a user-supplied annotation (a "tag").


## Segment a corpus using tags. 


```r
corpus_tags <- corpus(c("##INTRO This is the introduction.
                  ##DOC1 This is the first document.  Second sentence in Doc 1.
                  ##DOC3 Third document starts here.  End of third document.",
                 "##INTRO Document ##NUMBER Two starts before ##NUMBER Three."))

corp_seg_tags <- corpus_segment(corpus_tags, "##*")

cbind(texts(corp_seg_tags), docvars(corp_seg_tags), metadoc(corp_seg_tags))
```

```
##                                           texts(corp_seg_tags)  pattern
## text1.1                              This is the introduction.  ##INTRO
## text1.2 This is the first document.  Second sentence in Doc 1.   ##DOC1
## text1.3    Third document starts here.  End of third document.   ##DOC3
## text2.1                                               Document  ##INTRO
## text2.2                                      Two starts before ##NUMBER
## text2.3                                                 Three. ##NUMBER
##         _document _docid _segid
## text1.1     text1      1      1
## text1.2     text1      1      2
## text1.3     text1      1      3
## text2.1     text2      2      1
## text2.2     text2      2      2
## text2.3     text2      2      3
```


## Segmenting a transcript based on speaker identifiers


```r
corpus_speakers <- corpus("Mr. Smith: Text.\nMrs. Jones: More text.\nMr. Smith: I'm speaking, again.")

corpus_speakers_tags <- corpus_segment(corpus_speakers, pattern = "\\b[A-Z].+\\s[A-Z][a-z]+:",
                            valuetype = "regex")

cbind(texts(corpus_speakers_tags), docvars(corpus_speakers_tags), metadoc(corpus_speakers_tags))
```

```
##         texts(corpus_speakers_tags)     pattern _document _docid _segid
## text1.1                       Text.  Mr. Smith:     text1      1      1
## text1.2                  More text. Mrs. Jones:     text1      1      2
## text1.3        I'm speaking, again.  Mr. Smith:     text1      1      3
```

## Segment a character vector


```r
txt <- c(d1 = "This, is a sentence?  You: come here.", 
         d2 = "Yes, yes okay.")

char_segment(txt, pattern = "\\p{P}", valuetype = "regex", 
             pattern_position = "after", remove_pattern = FALSE)
```

```
##             d1.1             d1.2             d1.3             d1.4 
##          "This," "is a sentence?"           "You:"     "come here." 
##             d2.1             d2.2 
##           "Yes,"      "yes okay."
```
