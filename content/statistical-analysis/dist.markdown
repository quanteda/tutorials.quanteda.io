---
title: Document/feature similarity
weight: 30
chapter: false
draft: false
---


```r
require(quanteda)
require(quanteda.textstats)
```

`textstat_dist()` calculates similarities of documents or features for various measures. The output is compatible with R's `dist()`, so hierarchical clustering can be performed without any transformation.


```r
toks_inaug <- tokens(data_corpus_inaugural)
dfmat_inaug <- dfm(toks_inaug, remove = stopwords("en"))
```

```
## Warning: 'remove' is deprecated; use dfm_remove() instead
```

```r
tstat_dist <- as.dist(textstat_dist(dfmat_inaug))
clust <- hclust(tstat_dist)
plot(clust, xlab = "Distance", ylab = NULL)
```

<img src="/statistical-analysis/dist_files/figure-html/unnamed-chunk-2-1.png" width="960" />


