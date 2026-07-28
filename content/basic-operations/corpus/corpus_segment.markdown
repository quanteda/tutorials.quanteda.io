---
title: Extract tags from texts
weight: 40
draft: false
---

Real-world documents are often not clean, self-contained texts. A transcript might mix several speakers together, or a single file might bundle several sections that were only tagged with a marker such as `##DOC1`. `corpus_segment()` splits a document wherever a pattern occurs. Each segment becomes its own document, and the matched pattern is recorded as a document-level variable. Use it to analyse sections of documents or transcripts separately.


``` r
library(quanteda)
```

### Document sections

In this example, each section of text is preceded by a tag such as `##INTRO` or `##DOC1`. The pattern `"##*"` matches any word starting with two hash symbols, so `corpus_segment()` splits the text at each tag and uses the tag itself as the new document's `pattern` variable.


``` r
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

The two original documents have become six, one per tagged section. `cbind()` here shows the resulting document-level variable and text side by side.

### Speaker identifiers

The same idea works for transcripts, where each speaker turn begins with a name followed by a colon. Here the pattern is a regular expression, `valuetype = "regex"`, that matches a capitalised first and last name followed by a colon, so each speaker turn becomes its own document.


``` r
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

Mr Smith's two turns, `text1.1` and `text1.3`, remain two separate documents rather than being merged back together, even though Mrs Jones's turn sits between them in the transcript. What ties his two turns together is the `pattern` docvar, which records the matched speaker tag for each document. Because that identity is stored as a docvar rather than left inside the text, you can use it later to isolate or compare speakers, for example with `corpus_subset()`.


``` r
corp_smith <- corpus_subset(corp_speakers, pattern == "Mr. Smith:")
cbind(docvars(corp_smith), text = as.character(corp_smith))
```

```
##            pattern                 text
## text1.1 Mr. Smith:                Text.
## text1.3 Mr. Smith: I'm speaking, again.
```

The same docvar also works as a grouping variable. [`dfm_group()`](/basic-operations/dfm/dfm_group), covered in a later chapter, merges each speaker's turns into a single row, so you can compare their word usage directly instead of one row per turn.


``` r
dfmat_speakers <- tokens(corp_speakers, remove_punct = TRUE) |>
  dfm() |>
  dfm_group(groups = pattern)
print(dfmat_speakers)
```

```
## Document-feature matrix of: 2 documents, 5 features (40.00% sparse) and 1 docvar.
##              features
## docs          text more i'm speaking again
##   Mr. Smith:     1    0   1        1     1
##   Mrs. Jones:    1    1   0        0     0
```

Splitting into sentences is normally a job for `corpus_reshape()`, covered in the [previous chapter](/basic-operations/corpus/corpus_reshape). But you can achieve a similar result with `corpus_segment()` by segmenting on punctuation and setting `pattern_position = "after"`, which keeps the punctuation mark at the end of the preceding segment rather than at the start of the next one.


``` r
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

