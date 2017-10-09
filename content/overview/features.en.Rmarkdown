---
title: "Features"
weight: 5
---

## Generalized, flexible corpus management
**quanteda** provides a comprehensive workflow and ecosystem for the management,
processing, and analysis of texts.  Documents and associated document- and
collection-level metadata are easily loaded and stored as a _corpus_ object,
although most of **quanteda**'s operations work on simple character objects as
well.  A corpus is designed to efficiently store all of the texts in a
collection, as well as meta-data for documents and for the collection as a
whole.  This makes it easy to perform natural language processing on the texts
in a corpus simply and quickly, such as tokenizing, stemming, or forming ngrams.
**quanteda**'s functions for tokenizing texts and forming multiple tokenized
documents into a _document-feature matrix_ are both extremely fast and extremely
simple to use.  **quanteda** can segment texts easily by words, paragraphs,
sentences, or even user-supplied delimiters and tags.

## Works nicely with UTF-8
Built on the text processing functions in the **stringi** package, which is in
turn built on C++ implementation of the [ICU](http://www.icu-project.org/)
libraries for Unicode text handling, **quanteda** pays special attention to fast
and correct implementation of Unicode and the handling of text in any character
set, following conversion internally to UTF-8.

## Built for efficiency and speed
All of the functions in **quanteda** are
built for maximum performance and scale while still being as R-based as
possible.  The package makes use of three efficient architectural elements:  
the **stringi** package for text processing, the **Matrix** package for sparse
matrix objects, and the **data.table** package for indexing large documents
efficiently.  If you can fit it into memory, **quanteda** will handle it
quickly.  (And eventually, we will make it possible to process objects even 
larger than available memory.)

## Super-fast conversion of texts into a document-feature matrix
**quanteda** is principally designed to allow users a fast and convenient method
to go from a corpus of texts to a selected matrix of documents by features, 
after defining and selecting the documents and features.  The package makes it 
easy to redefine documents, for instance by splitting them into sentences or 
paragraphs, or by tags, as well as to group them into larger documents by 
document variables, or to subset them based on logical conditions or 
combinations of document variables.  A special variation of the "dfm", a 
_feature co-occurrence matrix_, is also implemented, for direct use with 
embedding and representational models such as 
[**text2vec**](https://github.com/dselivanov/text2vec).

## Extensive feature selection capabilities
The package also implements common NLP 
feature selection functions, such as removing stopwords and stemming in numerous
languages, selecting words found in dictionaries, treating words as equivalent 
based on a user-defined "thesaurus", and trimming and weighting features based 
on document frequency, feature frequency, and related measures such as *tf-idf*.

## Qualitative exploratory tools
Easily search and save _keywords in context_, for instance, or identify keywords.  Like all of **quanteda**'s pattern matching functions, users have the option of simple "[glob](https://en.wikipedia.org/wiki/Glob_(programming) )" expressions, regular expressions, or fixed pattern matches.

## Dictionary-based analysis
**quanteda** allows fast and flexible implementation of dictionary methods, including the import and conversion of foreign dictionary formats such as those from Provalis's WordStat, the Linguistic Inquiry and Word Count (LIWC), Lexicoder, Yoshioder, and YAML.   

## Text analytic methods
Once constructed, a _dfm_ can be easily analyzed using either 
**quanteda**'s built-in tools for scaling document positions (for the "wordfish" and 
"Wordscores" models, direct use with the [**ca**](https://CRAN.R-project.org/package=ca) package for correspondence 
analysis), predictive models using Naive Bayes multinomial and Bernoulli classifiers, computing distance or similarity matrixes of features or documents, or computing readability or lexical diversity indexes.  

In addition, **quanteda** a document-feature matrix is easily used with or converted for a number 
of other text analytic tools, such as: 

*  _topic models_ (including converters for direct use with the [**topicmodels**](https://CRAN.R-project.org/package=topicmodels), 
[**LDA**](https://CRAN.R-project.org/package=lda), and [**stm**](http://www.structuraltopicmodel.com) packages);

*  machine learning through a variety of other packages that take matrix or 
matrix-like inputs.

## Planned features
Coming soon to **quanteda** are:

*  _Bootstrapping methods_ for texts that makes it easy to resample texts from 
pre-defined units, to facilitate computation of confidence intervals on textual 
statistics using techniques of non-parametric bootstrapping, but applied to the 
original texts as data.
*  _Additional predictive and analytic methods_ by expanding the `textstat_` and
`textmodel_` functions.  Current textmodel types include correspondence
analysis, "Wordscores", "Wordfish", and Naive Bayes; current textstat statistics
are readability, lexical diversity, similarity, and distance.
*  _Expanded settings_ for all objects, that will propogate through downstream
objects.
*  _Object histories_, that will propogate through downstream objects, to
enhance analytic reproducibility and transparency.

