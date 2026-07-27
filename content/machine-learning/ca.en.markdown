---
title: Correspondence analysis
weight: 40
draft: false
---

Correspondence analysis is a technique to scale documents on multiple dimensions. Correspondence analysis is similar to principal component analysis but works for categorical variables (contingency table).


``` r
library(quanteda)
library(quanteda.textmodels)
library(quanteda.textplots)
```

`textmodel_ca()` provides similar functionality to the **ca** package, but **quanteda**'s version is more efficient for textual data.

You can plot positions of documents on a one-dimensional scale using `textplot_scale1d()`.


``` r
toks_irish <- tokens(data_corpus_irishbudget2010, remove_punct = TRUE)
dfmat_irish <- dfm(toks_irish) |> 
               dfm_remove(pattern = stopwords("en"))

tmod_ca <- textmodel_ca(dfmat_irish)
textplot_scale1d(tmod_ca)
```

<img src="/machine-learning/ca.en_files/figure-html/unnamed-chunk-2-1.png" alt="" width="672" />

If you want to plot documents on multi-dimensional scale, you can use `coef()` to obtain coordinates of lower dimensions.  


``` r
dat_ca <- data.frame(dim1 = coef(tmod_ca, doc_dim = 1)$coef_document, 
                     dim2 = coef(tmod_ca, doc_dim = 2)$coef_document)
head(dat_ca)
```

```
##                            dim1        dim2
## Lenihan, Brian (FF)   1.3828052 -0.05807441
## Bruton, Richard (FG) -0.7146821  1.47808497
## Burton, Joan (LAB)   -1.0058431  1.22260744
## Morgan, Arthur (SF)  -0.2365257  0.01445752
## Cowen, Brian (FF)     1.4590321 -0.21215002
## Kenny, Enda (FG)     -0.9335218 -0.04006165
```

``` r
plot(1, xlim = c(-2, 2), ylim = c(-2, 2), type = "n", xlab = "Dimension 1", ylab = "Dimension 2")
grid()
text(dat_ca$dim1, dat_ca$dim2, labels = rownames(dat_ca), cex = 0.8, col = rgb(0, 0, 0, 0.7))
```

<img src="/machine-learning/ca.en_files/figure-html/unnamed-chunk-3-1.png" alt="" width="672" />

