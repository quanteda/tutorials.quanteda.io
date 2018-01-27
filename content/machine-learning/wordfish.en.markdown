---
title: Wordfish
weight: 20
draft: false
---


```r
require(quanteda)
```

{{% notice note %}}
Wordfish is Poisson scaling model of one-dimensional document positions (Slapin and Proksch 2008). Wordfish also allows for scaling documents, but compared to Wordscores reference scores/texts are not required. Wordfish is an unsupervised one-dimensional text scaling method, meaning that it estimates the positions of documents solely based on the observed word frequencies. 

Slapin, Jonathan and Sven-Oliver Proksch. 2008. "A Scaling Model for Estimating Time-Series Party Positions from Texts." _American Journal of Political Science_ 52(3): 705-772.
{{% /notice %}}

In this example, we show how to apply Wordfish to the Irish budget speeches from 2010. First, we create `dfm`, afterwards we run Wordfish.


```r
irishbudget_dfm <- dfm(data_corpus_irishbudget2010, remove_punct = TRUE)
irishbudget_wf <- textmodel_wordfish(irishbudget_dfm, dir = c(6,5))

summary(irishbudget_wf)
```

```
## 
## Call:
## textmodel_wordfish.dfm(x = irishbudget_dfm, dir = c(6, 5))
## 
## Estimated Document Positions:
##                                          theta      se
## 2010_BUDGET_01_Brian_Lenihan_FF        1.79850 0.02033
## 2010_BUDGET_02_Richard_Bruton_FG      -0.61065 0.02887
## 2010_BUDGET_03_Joan_Burton_LAB        -1.15900 0.01570
## 2010_BUDGET_04_Arthur_Morgan_SF       -0.07961 0.02941
## 2010_BUDGET_05_Brian_Cowen_FF          1.77640 0.02355
## 2010_BUDGET_06_Enda_Kenny_FG          -0.75603 0.02673
## 2010_BUDGET_07_Kieran_ODonnell_FG     -0.47934 0.04340
## 2010_BUDGET_08_Eamon_Gilmore_LAB      -0.59202 0.03024
## 2010_BUDGET_09_Michael_Higgins_LAB    -0.99421 0.04015
## 2010_BUDGET_10_Ruairi_Quinn_LAB       -0.90757 0.04260
## 2010_BUDGET_11_John_Gormley_Green      1.17718 0.07182
## 2010_BUDGET_12_Eamon_Ryan_Green        0.17377 0.06320
## 2010_BUDGET_13_Ciaran_Cuffe_Green      0.70997 0.07230
## 2010_BUDGET_14_Caoimhghin_OCaolain_SF -0.05739 0.03903
## 
## Estimated Feature Scores:
##         when      i presented   the supplementary  budget     to   this
## beta -0.1106 0.3609    0.3973 0.236         1.111 0.07857 0.3503 0.2898
## psi   1.6165 2.7062   -1.7668 5.288        -1.112 2.69189 4.4841 3.4357
##       house   last   april    said     we   could   work    our    way
## beta 0.1871 0.2826 -0.1076 -0.7762 0.4585 -0.5531 0.5638 0.7278 0.3186
## psi  1.0381 0.9852 -0.5583 -0.4402 3.4869  1.0854 1.1128 2.5124 1.4141
##      through  period     of severe economic distress  today    can  report
## beta  0.6467  0.5356 0.3197  1.260   0.4639    1.822 0.1327 0.3458  0.6639
## psi   1.1610 -0.1649 4.4314 -1.981   1.5658   -4.396 0.8394 1.5584 -0.2379
##         that notwithstanding difficulties   past
## beta 0.06237           1.822        1.206 0.5161
## psi  3.80904          -4.396       -1.331 0.9320
```

We can plot the results of a fitted scaling model using `textplot_scale1d()`.


```r
doclab <- apply(docvars(data_corpus_irishbudget2010, c("name", "party")), 
                1, paste, collapse = " ")

textplot_scale1d(irishbudget_wf, doclabels = doclab)
```

<img src="/machine-learning/wordfish.en_files/figure-html/unnamed-chunk-3-1.svg" width="768" />

The function also allows to plot scores by a grouping variable, in this case the party affiliation of the speakers.


```r
textplot_scale1d(irishbudget_wf, doclabels = doclab,
                 groups = docvars(data_corpus_irishbudget2010, "party"))
```

<img src="/machine-learning/wordfish.en_files/figure-html/unnamed-chunk-4-1.svg" width="768" />

Finally, we can plot the estimated word positions.


```r
textplot_scale1d(irishbudget_wf, margin = "features", 
                 highlighted = c("government", "global", "children", 
                                 "bank", "economy", "the", "citizenship",
                                 "productivity", "deficit"))
```

<img src="/machine-learning/wordfish.en_files/figure-html/unnamed-chunk-5-1.svg" width="768" />


