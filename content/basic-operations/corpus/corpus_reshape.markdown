---
title: Change units of texts
weight: 20
chapter: false
draft: false
---



```r
require(quanteda)
```

Once you have created a corpus, you can change the units of texts. This can be either done on a pattern match through `corpus_segment()` or by recasting the units of a corpus to sentences or paragraphs through `corpus_reshape()`.

## Segmenting a corpus

Segment a corpus using tags. 



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

Segmenting a transcript based on speaker identifiers


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

You can also segment character values. Here we segment a text into clauses


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

## Reshaping a corpus

For a corpus, you can reshape (or recast) the documents to a different level of aggregation. Units of aggregation can be defined as documents, paragraphs, or sentences. Because the corpus object records its current "units" status, it is possible to move from recast units back to original units, for example from documents, to sentences, and then back to documents (possibly after modifying the sentences). We use the Guardian corpus as an example.


```r
news_corp <- quanteda.corpora::download('data_corpus_guardian')

news_corp_sentences <- corpus_reshape(news_corp, to = "sentences")

summary(news_corp_sentences, 5)
```

```
## Corpus consisting of 201097 documents, showing 5 documents:
## 
##          Text Types Tokens Sentences    tid          pub      edition
##  text136751.1    14     16         1 136751 The Guardian  4:44 PM GMT
##  text136751.2    20     21         1 136751 The Guardian  4:44 PM GMT
##  text136751.3    22     26         1 136751 The Guardian  4:44 PM GMT
##  text136751.4     3      5         1 136751 The Guardian  4:44 PM GMT
##  text118588.1    28     28         1 118588 The Guardian 11:47 AM GMT
##        date by len section
##  2016-02-26 NA 364      NA
##  2016-02-26 NA 364      NA
##  2016-02-26 NA 364      NA
##  2016-02-26 NA 364      NA
##  2015-11-22 NA 858      NA
##                                                                                                                                                                                                                               head
##  Green news roundup: Air pollution, coral bleaching and whaling; The week's top environment news stories and green events. If you are not already receiving this roundup, sign up here to get the briefing delivered to your inbox
##  Green news roundup: Air pollution, coral bleaching and whaling; The week's top environment news stories and green events. If you are not already receiving this roundup, sign up here to get the briefing delivered to your inbox
##  Green news roundup: Air pollution, coral bleaching and whaling; The week's top environment news stories and green events. If you are not already receiving this roundup, sign up here to get the briefing delivered to your inbox
##  Green news roundup: Air pollution, coral bleaching and whaling; The week's top environment news stories and green events. If you are not already receiving this roundup, sign up here to get the briefing delivered to your inbox
##   The innovators: the lightweight tank that turns snorkellers into divers; Dyson Award runner-up Cathal Redmond has invented the Express Dive air tank which can treble the length of time humans can hold their breath underwater
##                              hash
##  4dbbc870c56d9ac66cd5d2849b9500ff
##  4dbbc870c56d9ac66cd5d2849b9500ff
##  4dbbc870c56d9ac66cd5d2849b9500ff
##  4dbbc870c56d9ac66cd5d2849b9500ff
##  f0f8e302fc9886bb0a8472a02974b336
## 
## Source:  Combination of corpuses x[[1]] and x[[2]]
## Created: Wed Jan 24 22:38:41 2018
## Notes:   corpus_reshape.corpus(news_corp, to = "sentences")
```

The corpus now increased from 6,000 documents (i.e. newspaper articles) to 201,097 documents (i.e. sentences).

Now we could, for instance, select only sentences that contain at least 10 words and reshape the corpus back to documents. For this, we use `corpus_subset()` and apply `corpus_reshape()` to the subsetted corpus


```r
# create document-level variable with number of tokens
docvars(news_corp_sentences, "ntoken") <- ntoken(news_corp_sentences, remove_punct = TRUE)

ndoc(news_corp_sentences)
```

```
## [1] 201097
```

```r
# subset the corpus
news_corp_sentences_subset <- corpus_subset(news_corp_sentences, ntoken >= 10)

ndoc(news_corp_sentences_subset)
```

```
## [1] 172290
```

```r
# reshape back to the level of documents

news_corp_documens_subset <- corpus_reshape(news_corp_sentences_subset, to = "documents")

ndoc(news_corp_documens_subset)
```

```
## [1] 5986
```

