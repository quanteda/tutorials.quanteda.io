---
title: Document/feature similarity
weight: 30
chapter: false
draft: false
---

You met the idea of comparing documents by their word usage in the [Advanced Operations chapter](/advanced-operations/), when clustering Twitter users. Here we explain that comparison in a bit more detail. `textstat_dist()` calculates the distance between documents or features, based on how different their word frequencies are, for several standard distance measures. Wrapping the output with R's own `as.dist()` function makes it compatible with `hclust()`, so hierarchical clustering can be performed on it directly.


``` r
library(quanteda)
library(quanteda.textstats)
```

We tokenise `data_corpus_inaugural` and remove stopwords, since function words such as "the" and "of" would otherwise dominate the comparison without telling us anything about a speech's actual content.


``` r
toks_inaug <- tokens(data_corpus_inaugural)
dfmat_inaug <- dfm(toks_inaug) |>
  dfm_remove(stopwords("en"))
tstat_dist <- as.dist(textstat_dist(dfmat_inaug))
clust <- hclust(tstat_dist)
plot(clust, xlab = "Distance", ylab = NULL)
```

<img src="/statistical-analysis/dist_files/figure-html/unnamed-chunk-2-1.png" alt="" width="960" />

As with the Twitter user clustering example earlier in this tutorial, speeches that join low down in this dendrogram used more similar vocabulary than speeches that only join higher up. Speeches from presidents of the same era, or the same party, often cluster together, since the language of political speech shifts gradually over time.


