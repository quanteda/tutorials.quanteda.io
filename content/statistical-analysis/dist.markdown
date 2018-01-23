---
title: Document/feature similarity
weight: 30
chapter: false
draft: false
---


```r
require(quanteda)
```


```r
inaug_toks <- tokens(data_corpus_inaugural)
inaug_dfm <- dfm(inaug_toks, remove = stopwords('en'))
dist <- textstat_dist(inaug_dfm)
clust <- hclust(dist)
plot(clust)
```

<img src="/statistical-analysis/dist_files/figure-html/unnamed-chunk-2-1.svg" width="768" />


