---
title: Workflow
weight: 5
draft: false
bibliography: ../references.bib
---

Before working through the next few chapters, it helps to see the whole map at once. **quanteda** has three basic types of objects, and almost everything you do with the package involves moving between them.

A [corpus](/basic-operations/corpus) saves character strings and document-level variables together in a data frame, combining each text with the information attached to it. A [tokens](/basic-operations/tokens) object stores those texts as a list of word vectors instead. Tokens are less compact than plain character strings, but they keep track of where each word sits in the original text. That positional information is what functions such as `textstat_collocations()`, `tokens_ngrams()`, `tokens_select()` and `fcm()` with its `window` argument use for string-of-words analysis. A [document-feature matrix](/basic-operations/dfm), or DFM, represents documents and their feature frequencies as a matrix. A DFM is the most efficient of the three structures, but no longer records where in a document each word appeared. That is why the many `textstat_*` and `textmodel_*` functions for non-positional, bag-of-words analysis take a DFM as their starting point. For a broader overview of how this workflow fits into quantitative text analysis as a field, see Benoit (2020).

Text analysis with **quanteda** goes through all three object types, either explicitly, when you create them yourself one step at a time, or implicitly, when a function creates them behind the scenes. The diagram below shows how they connect: raw text files and any accompanying variables (such as the date or author of a document) become a corpus, a corpus becomes tokens, and tokens become a DFM.

{{<mermaid align="left">}}

graph TD
D[Text files]
V[Document-level variables]
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

For example, if character vectors are given to `dfm()`, it internally constructs corpus and tokens objects before creating a DFM.

## References

<div id="refs" class="references csl-bib-body hanging-indent">

<div id="ref-benoit2020" class="csl-entry">

Benoit, Kenneth. 2020. “Text as Data: An Overview.” In *The SAGE Handbook of Research Methods in Political Science and International Relations*, edited by Luigi Curini and Robert Franzese. Sage. <https://doi.org/10.4135/9781526486387>.

</div>

</div>
