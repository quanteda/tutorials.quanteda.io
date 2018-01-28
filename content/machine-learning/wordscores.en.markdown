---
title: Wordscores
weight: 20
draft: false
---

Wordscores is a scaling model for estimating the positions (mostly of political actors) for dimensions that are specified a priori. Wordscores was introduced in Laver, Benoit and Garry (2003) and is widely used among political scientists.


```r
require(quanteda)
require(ggplot2)
```

Training a Wordscores model requires reference scores for texts whose policy positions on well-defined a priori dimensions are "known". Afterwards, Wordscores estimates the positions for the remaining "virgin" texts.

We use manifestos of the 2013 and 2017 German federal elections. For the 2013 elections we assigne the average expert evaluations from the 2014 [Chapel Hill Expert Survey](https://www.chesdata.eu/) for the five major parties, and predict the party positions for the 2017 manifestos.



```r
ger_corpus <- readRDS(file = "content/data/data_corpus_germanifestos.rds")
summary(ger_corpus)
```



Now we can apply the Wordscores algorithm to the document-feature matrix.


```r
ger_dfm <- dfm(ger_corpus, remove = stopwords("de"), remove_punct = TRUE)
ger_ws <- textmodel_wordscores(ger_dfm, y = docvars(ger_corpus, "ref_score"))
summary(ger_ws)
```

```
## 
## Call:
## textmodel_wordscores.dfm(x = ger_dfm, y = docvars(ger_corpus, 
##     "ref_score"))
## 
## Reference Document Statistics:
##              score total min  max    mean median
## AfD 2013        NA   455   0   23 0.01121      0
## CDU-CSU 2013  5.92 23054   0  245 0.56796      0
## FDP 2013      6.53 20593   0  187 0.50733      0
## Gruene 2013   3.61 45747   0  398 1.12702      0
## Linke 2013    1.23 21001   0  234 0.51738      0
## SPD 2013      3.76 23142   0  214 0.57013      0
## AfD 2017        NA  9850   0  108 0.24266      0
## CDU-CSU 2017    NA 10710   0  136 0.26385      0
## FDP 2017        NA 19291   0  261 0.47525      0
## Gruene 2017     NA 40689   0 1100 1.00241      0
## Linke 2017      NA 33243   0  788 0.81897      0
## SPD 2017        NA 20765   0  186 0.51157      0
## 
## Wordscores:
## (showing first 30 elements)
##      alternative      deutschland     wahlprogramm  währungspolitik 
##            3.078            4.828            2.615            5.920 
##          fordern        auflösung          braucht             euro 
##            3.142            1.230            4.261            3.266 
##          ländern          schadet wiedereinführung       nationaler 
##            4.273            3.577            4.959            4.746 
##        schaffung        kleinerer             darf             tabu 
##            4.387            5.146            3.879            3.760 
##         änderung     europäischen         verträge            staat 
##            4.288            4.436            3.242            4.928 
##      ausscheiden      ermöglichen     demokratisch          währung 
##            1.979            4.438            1.833            5.897 
##      entscheiden           dürfen             dass         erzwingt 
##            4.343            3.594            4.243            3.610 
##          weitere              esm 
##            4.972            5.856
```

Next, we predict the Wordscores for the unknown virgin texts.


```r
ger_ws_predict <- predict(ger_ws, newdata = ger_dfm)
```

Finally, we can plot the fitted scaling model using **quanteda**'s `textplot_scale1d()` function.


```r
textplot_scale1d(ger_ws_predict)
```

<img src="/machine-learning/wordscores.en_files/figure-html/unnamed-chunk-6-1.svg" width="768" />


{{% notice info %}}
If you want to learn more about Wordscores, see:  
Laver, Michael, Kenneth R Benoit, and John Garry. 2003. "Extracting Policy Positions From Political Texts Using Words as Data." _American Political Science Review_ 97(2): 311-331.  
Lowe, Will. 2008. "Understanding Wordscores". _Political Analysis_ 16(4): 356-371.
{{% /notice%}}
