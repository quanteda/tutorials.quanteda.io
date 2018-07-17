---
title: Correspondence analysis
weight: 40
draft: false
---

Correspondence analysis is a technique to scale documents on multiple dimensions. Correspondence analysis is similar to principal component analysis but works for categorical variables (contingency table).


```r
require(quanteda)
```

`textmodel_ca()` provides similar functionality to the **ca** package, but **quanteda**'s version is more efficient for textual data.

For visualization, we change the document names. 


```r
corp <- data_corpus_irishbudget2010
docnames(corp) <- paste(docvars(corp, "name"), docvars(corp, "party"), sep = ", ")
```

You can plot positions of documents on a one-dimensional scale using `textplot_scale1d()`.


```r
ca <- dfm(corp, remove_punct = TRUE, remove = stopwords('en')) %>% textmodel_ca()
textplot_scale1d(ca)
```

<img src="/machine-learning/ca.en_files/figure-html/unnamed-chunk-3-1.svg" width="768" />

If you want to plot documents on multi-dimensional scale, you use `coef()` to obtain coordinates of lower dimensions.  


```r
ca_data <- data.frame(dim1 = coef(ca, doc_dim = 1)$coef_document, 
                      dim2 = coef(ca, doc_dim = 2)$coef_document)
head(ca_data)
```

```
##                   dim1        dim2
## Lenihan, FF  1.3947058  0.07857887
## Bruton, FG  -0.7102673  0.75538166
## Burton, LAB -1.0420867  1.82837918
## Morgan, SF  -0.2428268 -0.09447121
## Cowen, FF    1.4579375 -0.12655387
## Kenny, FG   -0.9269172 -0.24479883
```

```r
plot(1, xlim = c(-2, 2), ylim = c(-2, 2), type = 'n', xlab = 'Dimension 1', ylab = 'Dimension 2')
grid()
text(ca_data$dim1, ca_data$dim2, labels = rownames(ca_data), cex = 0.8, col = rgb(0, 0, 0, 0.7))
```

<img src="/machine-learning/ca.en_files/figure-html/unnamed-chunk-4-1.svg" width="672" />

