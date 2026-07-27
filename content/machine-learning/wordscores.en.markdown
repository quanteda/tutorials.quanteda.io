---
title: Wordscores
weight: 20
draft: false
---

Wordscores is a scaling model for estimating the positions (mostly of political actors) for dimensions that are specified a priori. Wordscores was introduced in Laver, Benoit and Garry (2003) and is widely used among political scientists.


``` r
library(quanteda)
library(quanteda.textmodels)
library(quanteda.textplots)
library(quanteda.corpora)
```

Training a Wordscores model requires reference scores for texts whose policy positions on well-defined a priori dimensions are "known". Afterwards, Wordscores estimates the positions for the remaining "virgin" texts.

In this example, we will use manifestos of the 2013 and 2017 German federal elections. For the 2013 elections we will assign the average expert evaluations from the 2014 [Chapel Hill Expert Survey](https://www.chesdata.eu/) for the five major parties in order to predict the party positions for the 2017 manifestos.


``` r
corp_ger <- download(url = "https://www.dropbox.com/s/uysdoep4unfz3zp/data_corpus_germanifestos.rds?dl=1")
```




``` r
summary(corp_ger)
```

```
## Corpus consisting of 12 documents, showing 12 documents:
## 
##          Text Types Tokens Sentences year   party ref_score
##      AfD 2013   450    951        45 2013     AfD        NA
##  CDU-CSU 2013  7546  46771      2578 2013 CDU-CSU      5.92
##      FDP 2013  7909  42488      2411 2013     FDP      6.53
##   Gruene 2013 13722  94065      5220 2013  Gruene      3.61
##    Linke 2013  8370  43695      1916 2013   Linke      1.23
##      SPD 2013  8298  47634      2579 2013     SPD      3.76
##      AfD 2017  5860  19461       746 2017     AfD        NA
##  CDU-CSU 2017  4827  22003      1301 2017 CDU-CSU        NA
##      FDP 2017  8563  38738      2018 2017     FDP        NA
##   Gruene 2017 13064  75390      3409 2017  Gruene        NA
##    Linke 2017 11570  67808      2846 2017   Linke        NA
##      SPD 2017  8283  43028      2486 2017     SPD        NA
```

Now we can apply the Wordscores algorithm to a document-feature matrix.


``` r
# tokenize texts
toks_ger <- tokens(corp_ger, remove_punct = TRUE)

# create a document-feature matrix
dfmat_ger <- dfm(toks_ger) |> 
             dfm_remove(pattern = stopwords("de"))

# apply Wordscores algorithm to document-feature matrix
tmod_ws <- textmodel_wordscores(dfmat_ger, y = corp_ger$ref_score, smooth = 1)
summary(tmod_ws)
```

```
## 
## Call:
## textmodel_wordscores.dfm(x = dfmat_ger, y = corp_ger$ref_score, 
##     smooth = 1)
## 
## Reference Document Statistics:
##              score total min  max    mean median
## AfD 2013        NA   455   0   23 0.01121      0
## CDU-CSU 2013  5.92 23054   0  245 0.56789      0
## FDP 2013      6.53 20593   0  187 0.50727      0
## Gruene 2013   3.61 45751   0  398 1.12698      0
## Linke 2013    1.23 21001   0  234 0.51732      0
## SPD 2013      3.76 23142   0  214 0.57006      0
## AfD 2017        NA  9850   0  108 0.24263      0
## CDU-CSU 2017    NA 10711   0  136 0.26384      0
## FDP 2017        NA 19292   0  261 0.47522      0
## Gruene 2017     NA 40689   0 1100 1.00229      0
## Linke 2017      NA 33246   0  788 0.81895      0
## SPD 2017        NA 20768   0  186 0.51158      0
## 
## Wordscores:
## (showing first 30 elements)
##           alternative           deutschland          wahlprogramm 
##                 3.290                 4.742                 3.296 
##       währungspolitik               fordern             geordnete 
##                 4.531                 3.255                 4.242 
##             auflösung euro-währungsgebietes               braucht 
##                 3.336                 4.242                 4.155 
##                  euro               ländern               schadet 
##                 3.333                 4.228                 3.912 
##      wiedereinführung            nationaler             währungen 
##                 4.466                 4.579                 4.242 
##             schaffung             kleinerer            stabilerer 
##                 4.290                 4.427                 4.242 
##      währungsverbünde                    dm                  darf 
##                 4.242                 4.242                 3.871 
##                  tabu              änderung          europäischen 
##                 4.159                 4.227                 4.360 
##              verträge                 staat           ausscheiden 
##                 3.553                 4.794                 3.698 
##           ermöglichen                  volk          demokratisch 
##                 4.356                 4.242                 2.271
```

Next, we will predict the Wordscores for the unknown virgin texts.


``` r
pred_ws <- predict(tmod_ws, se.fit = TRUE, newdata = dfmat_ger)
```

Finally, we can plot the fitted scaling model using **quanteda**'s `textplot_scale1d()` function.


``` r
textplot_scale1d(pred_ws)
```

<img src="/machine-learning/wordscores.en_files/figure-html/unnamed-chunk-7-1.png" alt="" width="672" />


{{% notice ref %}}
- Laver, Michael, Kenneth R Benoit, and John Garry. 2003. "[Extracting Policy Positions From Political Texts Using Words as Data](https://doi.org/10.1017/S0003055403000698)." _American Political Science Review_ 97(2): 311-331.  
- Lowe, Will. 2008. "[Understanding Wordscores](https://doi.org/10.1093/pan/mpn004)." _Political Analysis_ 16(4): 356-371.
{{% /notice%}}
