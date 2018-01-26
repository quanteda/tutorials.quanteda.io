---
title: Wordscores
draft: true
---



Baysian


```r
data(data_corpus_amicus, package = "quanteda.corpora")
refs <- docvars(data_corpus_amicus, "trainclass")
refs <- (as.numeric(refs) - 1.5) * 2
amicusDfm <- dfm(data_corpus_amicus)
wm <- textmodel_wordscores(amicusDfm, y = refs)
```


```r
preds <- predict(wm, newdata = amicusDfm)
summary(preds)
##     Min.  1st Qu.   Median     Mean  3rd Qu.     Max. 
## -0.20673  0.02659  0.07694  0.05667  0.09922  0.23308
plot(preds ~ docvars(amicusDfm, "testclass"),
     horizontal = TRUE, xlab = "Predicted document score",
     ylab = "Test class", las = 1)
```

<img src="/machine-learning/wordscore.en_files/figure-html/unnamed-chunk-3-1.svg" width="768" />


```r
summary(wm)
## 
## Call:
## textmodel_wordscores.dfm(x = amicusDfm, y = refs)
## 
## Reference Document Statistics:
##           score total min  max    mean median
## sP1.txt      -1 13878   0  973 0.61285      0
## sP2.txt      -1 15715   0  983 0.69397      0
## sR1.txt       1 16144   0 1040 0.71292      0
## sR2.txt       1 14359   0  838 0.63409      0
## sAP01.txt    NA  7795   0  409 0.34423      0
## sAP02.txt    NA  8545   0  560 0.37735      0
## sAP03.txt    NA 10007   0  620 0.44191      0
## sAP04.txt    NA  6119   0  395 0.27021      0
## sAP05.txt    NA  9131   0  572 0.40322      0
## sAP06.txt    NA  6310   0  339 0.27865      0
## sAP07.txt    NA  5979   0  360 0.26403      0
## sAP08.txt    NA  2112   0  121 0.09327      0
## sAP09.txt    NA  5663   0  305 0.25008      0
## sAP10.txt    NA  5262   0  317 0.23237      0
## sAP11.txt    NA  7604   0  439 0.33579      0
## sAP12.txt    NA  9189   0  554 0.40578      0
## sAP13.txt    NA  6941   0  468 0.30651      0
## sAP14.txt    NA  7759   0  496 0.34264      0
## sAP15.txt    NA  7869   0  451 0.34749      0
## sAP16.txt    NA  4754   0  280 0.20994      0
## sAP17.txt    NA  6478   0  380 0.28607      0
## sAP18.txt    NA  5124   0  257 0.22628      0
## sAP19.txt    NA  4286   0  285 0.18927      0
## sAR01.txt    NA  2665   0  168 0.11769      0
## sAR02.txt    NA  9914   0  718 0.43780      0
## sAR03.txt    NA  9491   0  635 0.41912      0
## sAR04.txt    NA  9670   0  518 0.42703      0
## sAR05.txt    NA  9422   0  667 0.41607      0
## sAR06.txt    NA 11363   0  813 0.50179      0
## sAR07.txt    NA 10491   0  580 0.46328      0
## sAR08.txt    NA  8601   0  570 0.37982      0
## sAR09.txt    NA  6803   0  410 0.30042      0
## sAR10.txt    NA 10364   0  697 0.45767      0
## sAR11.txt    NA 10754   0  611 0.47490      0
## sAR12.txt    NA  4960   0  275 0.21903      0
## sAR13.txt    NA 10433   0  717 0.46072      0
## sAR14.txt    NA  7490   0  466 0.33076      0
## sAR15.txt    NA 10191   0  568 0.45003      0
## sAR16.txt    NA  8413   0  548 0.37152      0
## sAR17.txt    NA  9154   0  567 0.40424      0
## sAR18.txt    NA  9759   0  607 0.43096      0
## sAR19.txt    NA  6294   0  362 0.27794      0
## sAR20.txt    NA 11129   0  775 0.49146      0
## sAR21.txt    NA  6271   0  424 0.27693      0
## sAR22.txt    NA  9375   0  565 0.41400      0
## sAR23.txt    NA  8728   0  527 0.38543      0
## sAR24.txt    NA  9737   0  567 0.42998      0
## sAR25.txt    NA  8516   0  451 0.37607      0
## sAR26.txt    NA  9966   0  583 0.44010      0
## sAR27.txt    NA  9881   0  559 0.43634      0
## sAR28.txt    NA  7061   0  392 0.31181      0
## sAR29.txt    NA 10091   0  656 0.44562      0
## sAR30.txt    NA  7963   0  598 0.35164      0
## sAR31.txt    NA  8998   0  634 0.39735      0
## sAR32.txt    NA  9076   0  632 0.40079      0
## sAR33.txt    NA 10213   0  537 0.45100      0
## sAR34.txt    NA  8060   0  456 0.35593      0
## sAR35.txt    NA  8973   0  530 0.39625      0
## sAR36.txt    NA  2677   0  154 0.11822      0
## sAR37.txt    NA  8097   0  497 0.35756      0
## sAR38.txt    NA  8239   0  435 0.36383      0
## sAR39.txt    NA  8443   0  453 0.37284      0
## sAR40.txt    NA  9927   0  538 0.43837      0
## sAR41.txt    NA  8700   0  518 0.38419      0
## sAR42.txt    NA  3067   0  219 0.13544      0
## sAR43.txt    NA  8528   0  420 0.37660      0
## sAR44.txt    NA 11769   0  680 0.51972      0
## sAR45.txt    NA  9297   0  592 0.41055      0
## sAR46.txt    NA 10248   0  606 0.45255      0
## sAR47.txt    NA  2266   0  116 0.10007      0
## sAR48.txt    NA  5251   0  264 0.23188      0
## sAR49.txt    NA  8401   0  451 0.37099      0
## sAR50.txt    NA  8200   0  452 0.36211      0
## sAR51.txt    NA  4096   0  233 0.18088      0
## sAR52.txt    NA  9528   0  562 0.42076      0
## sAR53.txt    NA  5343   0  344 0.23595      0
## sAR54.txt    NA 10514   0  692 0.46430      0
## sAR55.txt    NA   240   0   17 0.01060      0
## sAR56.txt    NA  9396   0  563 0.41493      0
## sAR58.txt    NA  4923   0  379 0.21740      0
## sAR59.txt    NA  6729   0  411 0.29715      0
## sAR60.txt    NA  7972   0  446 0.35204      0
## sAR61.txt    NA  7266   0  526 0.32087      0
## sAR62.txt    NA  4790   0  290 0.21153      0
## sAR63.txt    NA  5390   0  284 0.23802      0
## sAR64.txt    NA 10472   0  623 0.46244      0
## sAR65.txt    NA 10716   0  617 0.47322      0
## sAR66.txt    NA  8143   0  474 0.35959      0
## sAR67.txt    NA  5960   0  308 0.26319      0
## sAR68.txt    NA  7683   0  452 0.33928      0
## sAR71.txt    NA  9540   0  595 0.42129      0
## sAR72.txt    NA  7498   0  516 0.33111      0
## sAR73.txt    NA  8582   0  453 0.37898      0
## sAR74.txt    NA  3626   0  213 0.16012      0
## sAR75.txt    NA  7560   0  502 0.33385      0
## sAR76.txt    NA  5924   0  404 0.26160      0
## sAR77.txt    NA  5312   0  307 0.23458      0
## sAR78.txt    NA  3091   0  193 0.13650      0
## sAR79.txt    NA  5303   0  281 0.23418      0
## sAR80.txt    NA  7397   0  572 0.32665      0
## sAR81.txt    NA  2742   0  128 0.12109      0
## sAR83.txt    NA  6115   0  348 0.27004      0
## 
## Wordscores:
## (showing first 30 elements)
##         in   granting          a     strong preference admissions 
##   0.047853  -0.540637  -0.042990   0.522223  -0.820373  -0.130065 
##         to applicants       from     select      group         of 
##   0.023612   0.103039  -0.105820  -0.045440  -0.407826   0.050707 
##     racial        and     ethnic minorities          ,        the 
##  -0.628995   0.180073  -0.700242  -0.208520   0.028571   0.005564 
##        law     school    invokes         an   interest       that 
##  -0.268321  -0.046736  -1.000000  -0.134417  -0.653693   0.009562 
##      court        has      never   accepted         as compelling 
##   0.013899  -0.095673  -0.084363  -0.468231  -0.085846  -0.554968
```
