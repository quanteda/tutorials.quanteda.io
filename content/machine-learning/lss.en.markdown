---
title: Latent Semantic Scaling
weight: 70
draft: false
---

Latent Semantic Scaling (LSS) is a flexible and cost-efficient semi-supervised document scaling technique. The technique relies on word embeddings and users only need to provide a small set of "seed words" to locate documents on a specific dimension.

Install the **LSX** package from CRAN.


``` r
install.packages("LSX")
```


``` r
require(quanteda)
require(quanteda.corpora)
require(LSX)
```

Download a corpus with news articles using **quanteda.corpora**'s `download()` function.


``` r
corp_news <- download("data_corpus_guardian")
```



We must segment news articles into sentences in the corpus to accurately estimate semantic proximity between words. We can also use the [Marimo](https://github.com/koheiw/marimo) stopwords list (`source = "marimo"`) to remove words commonly used in news reports.


``` r
# tokenize text corpus and remove various features
corp_sent <- corpus_reshape(corp_news, to =  "sentences")
toks_sent <- corp_sent |> 
    tokens(remove_punct = TRUE, remove_symbols = TRUE, 
           remove_numbers = TRUE, remove_url = TRUE) |> 
    tokens_remove(stopwords("en", source = "marimo")) |>
    tokens_remove(c("*-time", "*-timeUpdated", "GMT", "BST", "*.com"))  

# create a document feature matrix from the tokens object
dfmat_sent <- toks_sent |> 
    dfm() |> 
    dfm_remove(pattern = "") |> 
    dfm_trim(min_termfreq = 5)
```


``` r
topfeatures(dfmat_sent, 20)
```

```
##     people        new       also         us        can government       last 
##      11169       8024       7901       7091       6972       6821       6335 
##        now      years       time      first       just         uk     police 
##       5883       5839       5694       5382       5369       4875       4621 
##       like      party        get       make       made   minister 
##       4584       3890       3852       3844       3752       3680
```

We will use generic sentiment seed words to perform sentiment analysis.


``` r
seed <- as.seedwords(data_dictionary_sentiment)
print(seed)
```

```
##        good        nice   excellent    positive   fortunate     correct 
##           1           1           1           1           1           1 
##    superior         bad       nasty        poor    negative unfortunate 
##           1          -1          -1          -1          -1          -1 
##       wrong    inferior 
##          -1          -1
```

With the seed words, LSS computes polarity of words frequent in the context of economy. We can identify context words by `char_context(pattern = "econom*")` before fitting the model.


``` r
# identify context words 
eco <- char_context(toks_sent, pattern = "econom*", p = 0.05)

# run LSS model
tmod_lss <- textmodel_lss(dfmat_sent, seeds = seed,
                          terms = eco, k = 300, cache = TRUE)
```

```
## Reading cache file: lss_cache/svds_94144713228712f5.RDS
```


``` r
head(coef(tmod_lss), 20) # most positive words
```

```
##        good opportunity     success      status    positive    strategy 
##  0.12881433  0.09499006  0.09411731  0.08307930  0.08055107  0.07587337 
##      polled    continue      energy     quarter       hopes      slowed 
##  0.06921400  0.06874115  0.06698483  0.06488367  0.06336084  0.06245736 
##       links      future      strong   direction    believes      easily 
##  0.05797630  0.05661830  0.05642830  0.05531016  0.05500706  0.05419789 
##    regional    maintain 
##  0.05417196  0.05339294
```

``` r
tail(coef(tmod_lss), 20) # most negative words
```

```
##     effects   austerity       worse  struggling       fears     cutting 
## -0.08782788 -0.08846623 -0.08936719 -0.09036340 -0.09214304 -0.09402963 
##         low uncertainty        debt       raise      warned       taxes 
## -0.09420248 -0.09457206 -0.09840311 -0.10167368 -0.10256378 -0.10476171 
##      caused    interest     raising        poor      bubble       rates 
## -0.10605851 -0.10742273 -0.10811768 -0.11915753 -0.12241866 -0.12570268 
##         bad    negative 
## -0.12606444 -0.14584539
```

By highlighting negative words in a manually compiled sentiment dictionary (`data_dictionary_LSD2015`), we can confirm that many of the words (but not all of them) have negative meanings in the corpus.


``` r
textplot_terms(tmod_lss, data_dictionary_LSD2015["negative"])
```

<img src="/machine-learning/lss.en_files/figure-html/unnamed-chunk-10-1.png" alt="" width="768" />

We must reconstruct original articles from their sentences using `dfm_group()` before predicting polarity of documents.


``` r
dfmat_doc <- dfm_group(dfmat_sent)
dat <- docvars(dfmat_doc)
dat$fit <- predict(tmod_lss, newdata = dfmat_doc)
```

We can smooth polarity scores of documents to visualize the trend using `smooth_lss()`. If `engine = "locfit"`, smoothing is very fast even when there are many documents.


``` r
dat_smooth <- smooth_lss(dat, engine = "locfit")
head(dat_smooth)
```

```
##         date time        fit    se.fit
## 1 2012-01-02    0 -0.2347145 0.1260015
## 2 2012-01-03    1 -0.2340680 0.1240438
## 3 2012-01-04    2 -0.2334314 0.1221246
## 4 2012-01-05    3 -0.2328045 0.1202435
## 5 2012-01-06    4 -0.2321871 0.1184001
## 6 2012-01-07    5 -0.2315790 0.1165940
```

In the plot below, the circles are polarity scores of documents and the curve is their local means with 95% confidence intervals.


``` r
plot(dat$date, dat$fit, col = rgb(0, 0, 0, 0.05), pch = 16, ylim = c(-0.5, 0.5),
     xlab = "Time", ylab = "Economic sentiment")
lines(dat_smooth$date, dat_smooth$fit, type = "l")
lines(dat_smooth$date, dat_smooth$fit + dat_smooth$se.fit * 1.96, type = "l", lty = 3)
lines(dat_smooth$date, dat_smooth$fit - dat_smooth$se.fit * 1.96, type = "l", lty = 3)
abline(h = 0, lty = c(1, 2))
```

<img src="/machine-learning/lss.en_files/figure-html/unnamed-chunk-13-1.png" alt="" width="960" />

{{% notice ref %}}
- Watanabe, K. 2021. "[Latent Semantic Scaling: A Semisupervised Text Analysis Technique for New Domains and Languages](https://www.tandfonline.com/doi/full/10.1080/19312458.2020.1832976)". _Communication Methods and Measures_ 15(2): 81-102. 
- Watanabe. K. 2023. "[Introduction to LSX: The package for Latent Semantic Scaling](http://koheiw.github.io/LSX/articles/pkgdown/introduction.html)".
{{% /notice %}}
