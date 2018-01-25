---
title: Workflow
weight: 5
chapter: false
draft: true
---

**quanteda** has three basic types of objects:

1.  [Corpus](corpus)
    
    * Saves character strings and variables in a data frame
    * Combine textual data with document-level variables

2.  [Tokens](tokens)
    
    * Stores tokens in a list of vectors
    * More efficient than on character strings, but preserves positions of words (string-of-words)

3.  [Document-feature matrix (DFM)](dfm)

    * Represents frequencies of features in documents in a sparse matrix
    * The most efficient structure, but it does not have information about positions of words (bag-of-words) 

Text analysis with **quanteda** goes through all those three types of objects either explicitly or implicitly.

{{<mermaid align="left">}}
    graph LR
    D[Text files]
    V[Document variables]
    C(Corpus)
    T(Tokens)
    AP[Positional analysis]
    AN[Non-positional analysis]
    M(Document-feature matrix)
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

For example, if character vectors are given to `dfm()`, it internally constructs corpus and tokens, before a document-feature matrix. 
