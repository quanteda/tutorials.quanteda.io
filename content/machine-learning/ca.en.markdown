---
title: Correspondence analysis
weight: 40
draft: false
---

Wordscores and Wordfish, in the previous two chapters, both reduce documents to a single dimension. Correspondence analysis is a technique to scale documents on multiple dimensions at once, which is useful when a single dimension is insufficient. Correspondence analysis is similar to principal component analysis, but works directly on the kind of count data (a contingency table) that a document-feature matrix provides. Like with any unsupervised scaling method, ex-post validation is required in order to interpret the output.


``` r
library(quanteda)
library(quanteda.textmodels)
library(quanteda.textplots)
library(ggplot2)
```



`textmodel_ca()` provides similar functionality to the standalone **ca** package, but the version in **quanteda.textmodels** works directly from a document-feature matrix, so you do not need to build a contingency table by hand first. We return to the Irish budget speeches used for Wordfish in the [previous chapter](/machine-learning/wordfish), so you can compare how the two methods position the same documents.

Even though correspondence analysis produces multiple dimensions, you can still plot documents on just the single most important one using `textplot_scale1d()`, in the same way as for Wordscores and Wordfish.


``` r
toks_irish <- tokens(data_corpus_irishbudget2010, remove_punct = TRUE)
dfmat_irish <- dfm(toks_irish) |> 
               dfm_remove(pattern = stopwords("en"))

tmod_ca <- textmodel_ca(dfmat_irish)
textplot_scale1d(tmod_ca)
```

<img src="/machine-learning/ca.en_files/figure-html/unnamed-chunk-3-1.png" alt="" width="672" />

The advantage of correspondence analysis is that it does not stop at one dimension. To plot documents on a multi-dimensional scale instead, use `coef()` to obtain the coordinates for whichever dimensions you choose.


``` r
dat_ca <- data.frame(dim1 = coef(tmod_ca, doc_dim = 1)$coef_document,
                     dim2 = coef(tmod_ca, doc_dim = 2)$coef_document)
dat_ca$doc <- rownames(dat_ca)
head(dat_ca)
```

```
##                            dim1        dim2                  doc
## Lenihan, Brian (FF)   1.3828052 -0.05807441  Lenihan, Brian (FF)
## Bruton, Richard (FG) -0.7146821  1.47808497 Bruton, Richard (FG)
## Burton, Joan (LAB)   -1.0058431  1.22260744   Burton, Joan (LAB)
## Morgan, Arthur (SF)  -0.2365257  0.01445752  Morgan, Arthur (SF)
## Cowen, Brian (FF)     1.4590321 -0.21215002    Cowen, Brian (FF)
## Kenny, Enda (FG)     -0.9335218 -0.04006165     Kenny, Enda (FG)
```

``` r
ggplot(dat_ca, aes(x = dim1, y = dim2, label = doc)) +
  geom_text(size = 3, alpha = 0.7) +
  coord_equal(xlim = c(-2, 2), ylim = c(-2, 2)) +
  labs(x = "Dimension 1", y = "Dimension 2")
```

<img src="/machine-learning/ca.en_files/figure-html/unnamed-chunk-4-1.png" alt="" width="480" />

Speakers positioned close together on this two-dimensional map used similar language along both dimensions, while speakers far apart differed on at least one of them. The map can separate speakers who look similar on one axis but differ sharply on another, something a one-dimensional method such as Wordfish cannot show.

