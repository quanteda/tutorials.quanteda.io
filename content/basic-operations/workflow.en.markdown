---
title: Workflow
weight: 5
draft: false
---

**quanteda** has three basic types of objects:

1.  [Corpus](basic-operations/corpus)
    
    * Saves character strings and variables in a data frame
    * Combines texts with document-level variables

2.  [Tokens](basic-operations/tokens)
    
    * Stores tokens in a list of vectors
    * More efficient than character strings, but preserves positions of words 
    * Positional (string-of-words) analysis is performed using `textstat_collocations()`, `tokens_ngrams()` and `tokens_select()` or `fcm()` with `window` option

3.  [Document-feature matrix (DFM)](basic-operations/dfm)

    * Represents frequencies of features in documents in a matrix
    * The most efficient structure, but it does not have information on positions of words 
    * Non-positional (bag-of-words) analysis are performed using many of the `textstat_*` and `textmodel_*` functions 

Text analysis with **quanteda** goes through all those three types of objects either explicitly or implicitly.

{{<mermaid align="left">}}
    graph TD
    D[Text files]
    V[Document variables]
    C(Corpus)
    T(Tokens)
    AP["Positional analysis (string-of-words)"]
    AN["Non-positional analysis (bag-of-words)"]
    M(DFM)
    style C stroke-width:4px
    style T stroke-width:4px
    style M stroke-width:4px
    D --> C
    V --> C 
    C --> T 
    T --> M
    T -.-> AP
    M -.-> AN
{{< /mermaid >}}

For example, if character vectors are given to `dfm()`, it internally constructs corpus and tokens, before a DFM. 
