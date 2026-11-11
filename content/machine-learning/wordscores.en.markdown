---
title: Wordscores
weight: 20
draft: false
bibliography: ../references.bib
---

The previous two chapters classified documents into discrete categories. Wordscores tackles a different kind of task: placing documents on a continuous scale, such as a left-right ideological spectrum, rather than sorting them into separate boxes. Wordscores is a scaling model for estimating the positions, most often of political actors, on dimensions that are specified in advance, or *a priori*. Wordscores was introduced in Laver et al. (2003) and is widely used among political scientists.

``` r
library(quanteda)
library(quanteda.textmodels)
library(quanteda.textplots)
library(quanteda.corpora)
```

Training a Wordscores model requires reference scores: texts whose policy positions on well-defined, predetermined dimensions are already “known” from some independent source, such as an expert survey. Wordscores learns the association between words and positions from these reference texts, then estimates the positions for the remaining “virgin” texts, whose scores are what we want to find out.

In this example, we will use manifestos from the 2013 and 2017 German federal elections. For the 2013 manifestos, which act as our reference texts, we assign the average expert evaluations from the 2014 [Chapel Hill Expert Survey](https://www.chesdata.eu/) for the five major parties, and use these known positions to predict the party positions for the 2017 manifestos, our virgin texts.

``` r
corp_ger <- download(url = "https://www.dropbox.com/s/uysdoep4unfz3zp/data_corpus_germanifestos.rds?dl=1")
```

``` r
summary(corp_ger)
```

    ## Corpus consisting of 12 documents, showing 12 documents:
    ## 
    ##          Text Types Tokens Sentences year   party ref_score
    ##      AfD 2013   450    951        43 2013     AfD        NA
    ##  CDU-CSU 2013  7546  46771      2527 2013 CDU-CSU      5.92
    ##      FDP 2013  7909  42488      2375 2013     FDP      6.53
    ##   Gruene 2013 13722  94065      5126 2013  Gruene      3.61
    ##    Linke 2013  8370  43695      1850 2013   Linke      1.23
    ##      SPD 2013  8298  47634      2532 2013     SPD      3.76
    ##      AfD 2017  5860  19461       715 2017     AfD        NA
    ##  CDU-CSU 2017  4827  22003      1256 2017 CDU-CSU        NA
    ##      FDP 2017  8563  38738      1925 2017     FDP        NA
    ##   Gruene 2017 13064  75390      3220 2017  Gruene        NA
    ##    Linke 2017 11570  67808      2755 2017   Linke        NA
    ##      SPD 2017  8283  43028      2401 2017     SPD        NA

`ref_score` is filled in for five of the six 2013 manifestos (all but the AfD, for which no expert evaluation was available) and missing (`NA`) for all 2017 manifestos; this missingness pattern is what tells `textmodel_wordscores()` which documents are reference texts and which are virgin texts. We first remove German stopwords (`stopwords("de)`), then apply the Wordscores algorithm to the resulting document-feature matrix.

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

The `y` argument is the vector of reference scores you just inspected, including the `NA` values for the 2017 manifestos, and `smooth = 1` adds a small constant to every word count to avoid problems with words that happen not to appear in a particular reference text. Next, we predict the Wordscores for the unknown virgin texts, using `se.fit = TRUE` to also obtain a standard error for each estimate, which reflects how confident the model is in that particular score.

``` r
pred_ws <- predict(tmod_ws, se.fit = TRUE, newdata = dfmat_ger)
```

Finally, we can plot the fitted scaling model using **quanteda**’s `textplot_scale1d()` function, which shows each party’s estimated position along with its confidence interval.

``` r
textplot_scale1d(pred_ws)
```

<img src="/machine-learning/wordscores.en_files/figure-html/unnamed-chunk-7-1.png" alt="" width="672" />

The confidence intervals matter here: Lowe (2008) shows that Wordscores estimates can be sensitive to seemingly minor choices, such as the smoothing constant or how positions are rescaled, so a position without its uncertainty is only half the picture.

## References

<div id="refs" class="references csl-bib-body hanging-indent">

<div id="ref-laver2003" class="csl-entry">

Laver, Michael, Kenneth R. Benoit, and John Garry. 2003. “Extracting Policy Positions from Political Texts Using Words as Data.” *American Political Science Review* 97 (2): 311–31. <https://doi.org/10.1017/S0003055403000698>.

</div>

<div id="ref-lowe2008" class="csl-entry">

Lowe, Will. 2008. “Understanding Wordscores.” *Political Analysis* 16 (4): 356–71. <https://doi.org/10.1093/pan/mpn004>.

</div>

</div>
