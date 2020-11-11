---
title: Latent Semantic Scaling
weight: 70
draft: false
---

Latent Semantic Scaling (LSS) is a flexible and cost-efficient semisupervised document scaling technique. The technique relies on word embeddings and users only need to provide a small set of "seed words" to locate documents on a specific dimension.

Install the **LSX** package from CRAN.


```r
install.packages("LSX")
```


```r
require(quanteda)
require(quanteda.corpora)
require(LSX)
```

Download a corpus with news articles using **quanteda.corpora**'s `download()` function.


```r
corp_news <- download("data_corpus_guardian")
```



We segment news articles into sentences in the corpus to accurately estimate semantic proximity between words. We also use the [Marimo](https://github.com/koheiw/marimo) stopwords list (`source = "marimo"`) to remove words commonly used in news reports.


```r
# tokenize text corpus and remove various features
corp_sent <- corpus_reshape(corp_news, to =  "sentences")
toks_sent <- corp_sent %>% 
    tokens(remove_punct = TRUE, remove_symbols = TRUE, 
           remove_numbers = TRUE, remove_url = TRUE) %>% 
    tokens_remove(stopwords("en", source = "marimo")) %>%
    tokens_remove(c("*-time", "*-timeUpdated", "GMT", "BST", "*.com"))  

# create a document feature matrix from the tokens object
dfmat_sent <- toks_sent %>% 
    dfm(remove = "") %>% 
    dfm_trim(min_termfreq = 5)
```


```r
topfeatures(dfmat_sent, 20)
```

```
##     people        new       also         us        can government       last 
##      11168       8024       7901       7090       6972       6821       6335 
##        now      years       time      first       just         uk     police 
##       5883       5839       5694       5380       5369       4874       4621 
##       like      party        get       make       made   minister 
##       4582       3890       3852       3844       3752       3680
```

We use generic sentiment seed words to perform sentiment analysis.


```r
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

With the seed words, LSS computes polarity of words frequent in the context of economy. We identify context words by `char_context(pattern = "econom*")` before fitting the model.


```r
# identify context words 
eco <- char_context(toks_sent, pattern = "econom*", p = 0.05)

# run LSS model
tmod_lss <- textmodel_lss(dfmat_sent, seeds = seed,
                          terms = eco, k = 300, cache = TRUE)
```

```
## Reading cache file: lss_cache/svds_d3662ce2f8b0820f.RDS
```


```r
head(coef(tmod_lss), 20) # most positive words
```

```
##        far   positive     status        job        use      share    quarter 
## 0.12629148 0.10050133 0.09527710 0.07910809 0.07489797 0.07164349 0.07082497 
##      every      force     behind      third    rolling      hopes   maintain 
## 0.06734511 0.06681834 0.06583539 0.06531428 0.06505486 0.06442772 0.06070326 
##   strategy    welcome       halt  currently    example     slowed 
## 0.06055693 0.06036880 0.05873444 0.05825310 0.05790878 0.05690293
```

```r
tail(coef(tmod_lss), 20) # most negative words
```

```
## uncertainty       worse         cut       taxes      blamed     reserve 
## -0.08457099 -0.08689701 -0.09193834 -0.09234093 -0.09276866 -0.09339824 
##     raising      easing         low     cutting       raise      warned 
## -0.09962980 -0.10091116 -0.10322200 -0.10499354 -0.10629547 -0.11080288 
##    interest      bubble    negative       rates        poor      things 
## -0.12025183 -0.12144500 -0.13226094 -0.13293624 -0.13458223 -0.13849767 
##         bad       wrong 
## -0.14773609 -0.20868570
```

By highlighting negative words in a manually compiled sentiment dictionary (`data_dictionary_LSD2015`), we can confirm that many of the words (but not all of them) have negative meanings in the corpus.


```r
textplot_terms(tmod_lss, data_dictionary_LSD2015["negative"])
```

<img src="/machine-learning/lss.en_files/figure-html/unnamed-chunk-10-1.png" width="768" />

We reconstruct original articles from their sentences using `dfm_group()` before predicting polarity of documents.


```r
dfmat_doc <- dfm_group(dfmat_sent)
dat <- docvars(dfmat_doc)
dat$fit <- predict(tmod_lss, newdata = dfmat_doc)
```

We can smooth polarity scores of documents to visualize the trend using `smooth_lss()`. If `engine = "locfit"`, smoothing is very fast even when there are many documents.


```r
dat_smooth <- smooth_lss(dat, engine = "locfit")
head(dat_smooth)
```

```
##         date time        fit    se.fit
## 1 2012-01-02    0 -0.1969236 0.1260521
## 2 2012-01-03    1 -0.1972945 0.1240897
## 3 2012-01-04    2 -0.1976635 0.1221659
## 4 2012-01-05    3 -0.1980304 0.1202802
## 5 2012-01-06    4 -0.1983952 0.1184324
## 6 2012-01-07    5 -0.1987576 0.1166221
```

In the plot below, the circles are polarity scores of documents and the curve is their local means with 95% confidence intervals.


```r
plot(dat$date, dat$fit, col = rgb(0, 0, 0, 0.05), pch = 16, ylim = c(-0.5, 0.5),
     xlab = "Time", ylab = "Economic sentiment")
lines(dat_smooth$date, dat_smooth$fit, type = "l")
lines(dat_smooth$date, dat_smooth$fit + dat_smooth$se.fit * 1.96, type = "l", lty = 3)
lines(dat_smooth$date, dat_smooth$fit - dat_smooth$se.fit * 1.96, type = "l", lty = 3)
abline(h = 0, lty = c(1, 2))
```

<img src="/machine-learning/lss.en_files/figure-html/unnamed-chunk-13-1.png" width="960" />

{{% notice ref %}}
- Watanabe, K. 2020. "[Latent Semantic Scaling: A Semisupervised Text Analysis Technique for New Domains and Languages](https://www.tandfonline.com/doi/full/10.1080/19312458.2020.1832976)". _Communication Methods and Measures_. 
{{% /notice %}}
