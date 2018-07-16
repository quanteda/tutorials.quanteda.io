---
title: Wordfish
weight: 30
draft: false
---

Wordfish is a Poisson scaling model of one-dimensional document positions (Slapin and Proksch 2008). Wordfish also allows for scaling documents, but compared to Wordscores reference scores/texts are not required. Wordfish is an unsupervised one-dimensional text scaling method, meaning that it estimates the positions of documents solely based on the observed word frequencies. 


```r
require(quanteda)
```

In this example, we show how to apply Wordfish to the Irish budget speeches from 2010. First, we create `dfm`, afterwards we run Wordfish.


```r
irish_dfm <- dfm(data_corpus_irishbudget2010, remove_punct = TRUE)
wf <- textmodel_wordfish(irish_dfm, dir = c(6,5))
summary(wf)
```

```
## 
## Call:
## textmodel_wordfish.dfm(x = irish_dfm, dir = c(6, 5))
## 
## Estimated Document Positions:
##                                          theta      se
## 2010_BUDGET_01_Brian_Lenihan_FF        1.79444 0.02010
## 2010_BUDGET_02_Richard_Bruton_FG      -0.61774 0.02844
## 2010_BUDGET_03_Joan_Burton_LAB        -1.14741 0.01561
## 2010_BUDGET_04_Arthur_Morgan_SF       -0.08380 0.02900
## 2010_BUDGET_05_Brian_Cowen_FF          1.77416 0.02333
## 2010_BUDGET_06_Enda_Kenny_FG          -0.75762 0.02642
## 2010_BUDGET_07_Kieran_ODonnell_FG     -0.48645 0.04310
## 2010_BUDGET_08_Eamon_Gilmore_LAB      -0.59455 0.02991
## 2010_BUDGET_09_Michael_Higgins_LAB    -0.99302 0.04021
## 2010_BUDGET_10_Ruairi_Quinn_LAB       -0.90657 0.04267
## 2010_BUDGET_11_John_Gormley_Green      1.18326 0.07235
## 2010_BUDGET_12_Eamon_Ryan_Green        0.17248 0.06336
## 2010_BUDGET_13_Ciaran_Cuffe_Green      0.72229 0.07269
## 2010_BUDGET_14_Caoimhghin_OCaolain_SF -0.05949 0.03873
## 
## Estimated Feature Scores:
##         when      i presented    the supplementary  budget     to  this
## beta -0.1558 0.3217    0.3582 0.1945         1.077 0.03563 0.3097 0.249
## psi   1.6246 2.7253   -1.7925 5.3324        -1.128 2.71082 4.5208 3.462
##       house   last   april    said     we   could   work    our    way
## beta 0.1461 0.2416 -0.1554 -0.8301 0.4193 -0.6067 0.5262 0.6918 0.2772
## psi  1.0407 0.9874 -0.5725 -0.4533 3.5140  1.0865 1.1164 2.5301 1.4208
##      through  period     of severe economic distress  today    can  report
## beta  0.6125  0.4989 0.2792  1.229   0.4245    1.804 0.0922 0.3057  0.6257
## psi   1.1636 -0.1747 4.4675 -2.007   1.5741   -4.457 0.8399 1.5663 -0.2466
##        that notwithstanding difficulties   past
## beta 0.0192           1.804        1.176 0.4777
## psi  3.8389          -4.457       -1.352 0.9339
```

We can plot the results of a fitted scaling model using `textplot_scale1d()`.


```r
# create nicer labels for speakers
doclab <- paste(docvars(irish_dfm, "name"), docvars(irish_dfm, "party"))
textplot_scale1d(wf, doclabels = doclab)
```

<img src="/machine-learning/wordfish.en_files/figure-html/unnamed-chunk-3-1.png" width="672" />

The function also allows to plot scores by a grouping variable, in this case the party affiliation of the speakers.


```r
textplot_scale1d(wf, doclabels = doclab, groups = docvars(irish_dfm, "party"))
```

<img src="/machine-learning/wordfish.en_files/figure-html/unnamed-chunk-4-1.png" width="672" />

Finally, we can plot the estimated word positions and highlight certain features.


```r
textplot_scale1d(wf, margin = "features", 
                 highlighted = c("government", "global", "children", 
                                 "bank", "economy", "the", "citizenship",
                                 "productivity", "deficit"))
```

<img src="/machine-learning/wordfish.en_files/figure-html/unnamed-chunk-5-1.png" width="672" />

{{% notice info %}}
If you want to learn more about Wordfish, see:  
Slapin, Jonathan and Sven-Oliver Proksch. 2008. "A Scaling Model for Estimating Time-Series Party Positions from Texts." _American Journal of Political Science_ 52(3): 705-772.
{{% /notice %}}
