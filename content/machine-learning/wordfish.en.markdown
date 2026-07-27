---
title: Wordfish
weight: 30
draft: false
---

Wordfish is a Poisson scaling model of one-dimensional document positions (Slapin and Proksch 2008). Wordfish also allows for scaling documents, but in comparison to Wordscores, reference scores/texts are not required. Wordfish is an unsupervised one-dimensional text scaling method, meaning that it estimates the positions of documents solely based on the observed word frequencies. 


``` r
library(quanteda)
library(quanteda.textmodels)
library(quanteda.textplots)
```

In this example, we will show how to apply Wordfish to the Irish budget speeches from 2010. First, we will create a document-feature matrix. Afterwards, we will run Wordfish.


``` r
toks_irish <- tokens(data_corpus_irishbudget2010, remove_punct = TRUE)
dfmat_irish <- dfm(toks_irish)
tmod_wf <- textmodel_wordfish(dfmat_irish, dir = c(6, 5))
summary(tmod_wf)
```

```
## 
## Call:
## textmodel_wordfish.dfm(x = dfmat_irish, dir = c(6, 5))
## 
## Estimated Document Positions:
##                              theta      se
## Lenihan, Brian (FF)        1.79403 0.02007
## Bruton, Richard (FG)      -0.62160 0.02823
## Burton, Joan (LAB)        -1.13503 0.01568
## Morgan, Arthur (SF)       -0.07841 0.02896
## Cowen, Brian (FF)          1.77846 0.02330
## Kenny, Enda (FG)          -0.75350 0.02635
## ODonnell, Kieran (FG)     -0.47615 0.04309
## Gilmore, Eamon (LAB)      -0.58406 0.02994
## Higgins, Michael (LAB)    -1.00383 0.03964
## Quinn, Ruairi (LAB)       -0.92648 0.04183
## Gormley, John (Green)      1.18361 0.07224
## Ryan, Eamon (Green)        0.14738 0.06321
## Cuffe, Ciaran (Green)      0.71541 0.07291
## OCaolain, Caoimhghin (SF) -0.03982 0.03878
## 
## Estimated Feature Scores:
##         when      i presented    the supplementary  budget     to   this  house
## beta -0.1594 0.3177    0.3603 0.1933         1.077 0.03537 0.3077 0.2473 0.1399
## psi   1.6241 2.7239   -1.7958 5.3308        -1.134 2.70993 4.5190 3.4603 1.0396
##        last   april    said     we   could   work    our   way through  period
## beta 0.2419 -0.1565 -0.8339 0.4156 -0.6138 0.5221 0.6892 0.275  0.6115  0.4985
## psi  0.9853 -0.5725 -0.4514 3.5125  1.0858 1.1151 2.5278 1.419  1.1603 -0.1779
##          of severe economic distress   today   can  report    that
## beta 0.2777  1.229   0.4237    1.799 0.09141 0.304  0.6196 0.01506
## psi  4.4656 -2.013   1.5714   -4.456 0.83875 1.564 -0.2466 3.83785
##      notwithstanding difficulties   past
## beta           1.799        1.175 0.4746
## psi           -4.456       -1.357 0.9321
```

We can plot the results of a fitted scaling model using `textplot_scale1d()`.


``` r
textplot_scale1d(tmod_wf)
```

<img src="/machine-learning/wordfish.en_files/figure-html/unnamed-chunk-3-1.png" alt="" width="672" />

The function also allows you to plot scores by a grouping variable, in this case the party affiliation of the speakers.


``` r
textplot_scale1d(tmod_wf, groups = dfmat_irish$party)
```

<img src="/machine-learning/wordfish.en_files/figure-html/unnamed-chunk-4-1.png" alt="" width="672" />

Finally, we can plot the estimated word positions and highlight certain features.


``` r
textplot_scale1d(tmod_wf, margin = "features", 
                 highlighted = c("government", "global", "children", 
                                 "bank", "economy", "the", "citizenship",
                                 "productivity", "deficit"))
```

<img src="/machine-learning/wordfish.en_files/figure-html/unnamed-chunk-5-1.png" alt="" width="672" />

{{% notice ref %}}
- Slapin, Jonathan and Sven-Oliver Proksch. 2008. "[A Scaling Model for Estimating Time-Series Party Positions from Texts](https://doi.org/10.1111/j.1540-5907.2008.00338.x)." _American Journal of Political Science_ 52(3): 705-772.
{{% /notice %}}
