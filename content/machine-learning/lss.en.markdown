---
title: Latent Semantic Scaling
weight: 70
draft: false
bibliography: ../references.bib
---

Latent Semantic Scaling (LSS) (Watanabe 2021) brings together two ideas from earlier in this tutorial: scaling documents along a dimension, as with Wordscores and Wordfish, and learning from a small set of seed words, as with Newsmap. LSS is a cost-efficient semi-supervised document scaling technique. Rather than reference texts with known scores, as in Wordscores, it relies on word embeddings, statistical representations of words based on the contexts they typically appear in. You only need to provide a small set of “seed words” to locate documents along a dimension of your choosing, such as positive versus negative sentiment.

Install the **LSX** package (Watanabe 2023) from CRAN.

``` r
install.packages("LSX")
```

``` r
library(quanteda)
library(quanteda.corpora)
library(LSX)
library(ggplot2)
```

Download a corpus with news articles using **quanteda.corpora**’s `download()` function, the same Guardian corpus used throughout the [Statistical Analysis](/statistical-analysis/), [Advanced Operations](/advanced-operations/) and [Topic Models](/machine-learning/topicmodel) chapters.

``` r
corp_news <- download("data_corpus_guardian")
```

Word embeddings work best when estimated from short spans of text: two words that appear in the same sentence are more likely to be semantically related than two words that merely appear somewhere in the same lengthy article. We therefore segment news articles into sentences using `corpus_reshape()`, as in the [Basic Operations chapter](/basic-operations/corpus/corpus_reshape). We also use the [Marimo](https://github.com/koheiw/marimo) stopwords list (`source = "marimo"`), introduced in the [Different Languages chapter](/multilingual/), to remove words common in news reports that would otherwise add noise.

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

    ##     people        new       also         us        can government       last 
    ##      11169       8024       7901       7091       6972       6821       6335 
    ##        now      years       time      first       just         uk     police 
    ##       5883       5839       5694       5382       5369       4875       4621 
    ##       like      party        get       make       made   minister 
    ##       4584       3890       3852       3844       3752       3680

We will use a small set of positive and negative sentiment seed words, bundled with **LSX**, to perform sentiment analysis. `as.seedwords()` converts a dictionary of positive and negative terms into the numeric format LSS expects, with positive words coded `1` and negative words coded `-1`.

``` r
seed <- as.seedwords(data_dictionary_sentiment)
print(seed)
```

    ##        good        nice   excellent    positive   fortunate     correct 
    ##           1           1           1           1           1           1 
    ##    superior         bad       nasty        poor    negative unfortunate 
    ##           1          -1          -1          -1          -1          -1 
    ##       wrong    inferior 
    ##          -1          -1

With the seed words defined, LSS computes the polarity of words frequent in the context of a topic you specify, here the economy, by measuring how semantically close each word is to the positive and negative seed words within that context. We first identify which words belong to that context using `char_context(pattern = "econom*")`, then fit the model on those context words.

``` r
# identify context words 
eco <- char_context(toks_sent, pattern = "econom*", p = 0.05)

# run LSS model
tmod_lss <- textmodel_lss(dfmat_sent, seeds = seed,
                          terms = eco, k = 300, cache = TRUE)
```

    ## Reading cache file: lss_cache/svds_f98d674157c68a93.RDS

``` r
head(coef(tmod_lss), 20) # most positive words
```

    ##        good    positive      status opportunity     success      energy 
    ##  0.12819821  0.09788287  0.09714820  0.09390205  0.08007415  0.07838368 
    ##     quarter       third    strategy       model     rolling      slowed 
    ##  0.07251407  0.06723918  0.06498128  0.06464805  0.06351532  0.06298351 
    ##     welcome      fourth         key    maintain      points        halt 
    ##  0.05979544  0.05959782  0.05942578  0.05897810  0.05887537  0.05850875 
    ##     overall      polled 
    ##  0.05807422  0.05801402

``` r
tail(coef(tmod_lss), 20) # most negative words
```

    ## policymakers        worse  uncertainty          low      raising        taxes 
    ##  -0.08386265  -0.08413907  -0.08631062  -0.08973375  -0.09040336  -0.09078335 
    ##      reserve        raise          cut       easing       blamed      cutting 
    ##  -0.09165052  -0.09259311  -0.09310034  -0.09341340  -0.09550058  -0.09650599 
    ##       caused     interest       warned       bubble        rates     negative 
    ##  -0.09698655  -0.10771788  -0.11458312  -0.11666338  -0.11836263  -0.12625902 
    ##         poor          bad 
    ##  -0.13563914  -0.14518455

These coefficients are the output of LSS: a polarity score for every context word, learned from a dozen or so seed words. As an initial assessment of the model output, we cross-reference its results against the Lexicoder Sentiment Dictionary (`data_dictionary_LSD2015`), a larger, manually compiled sentiment dictionary already used in [Advanced Operations](/advanced-operations/). Highlighting the words LSD2015 independently classifies as negative shows that many of the words LSS also scored as negative, though not all, overlap with this outside source.

``` r
textplot_terms(tmod_lss, data_dictionary_LSD2015["negative"])
```

<img src="/machine-learning/lss.en_files/figure-html/unnamed-chunk-11-1.png" alt="" width="576" />

The model was trained on individual sentences, but we want a sentiment score per article, not per sentence. We reconstruct the original articles from their sentences using `dfm_group()`, as before, then predict the polarity of whole documents.

``` r
dfmat_doc <- dfm_group(dfmat_sent)
dat <- docvars(dfmat_doc)
dat$fit <- predict(tmod_lss, newdata = dfmat_doc)
```

A single sentiment score per article is still noisy from one article to the next. We can smooth polarity scores over time to visualise the underlying trend using `smooth_lss()`, similar to the kernel smoothing used for daily sentiment in [Advanced Operations](/advanced-operations/). With `engine = "locfit"`, smoothing is fast even with many documents.

``` r
dat_smooth <- smooth_lss(dat, engine = "locfit")
head(dat_smooth)
```

    ##         date time        fit    se.fit
    ## 1 2012-01-02    0 -0.2945235 0.1258062
    ## 2 2012-01-03    1 -0.2936453 0.1238585
    ## 3 2012-01-04    2 -0.2927721 0.1219488
    ## 4 2012-01-05    3 -0.2919040 0.1200769
    ## 5 2012-01-06    4 -0.2910407 0.1182423
    ## 6 2012-01-07    5 -0.2901821 0.1164449

In the plot below, the circles are polarity scores of individual documents, plotted with heavy transparency so that dense clusters of points show up as darker regions. The curve is their local mean over time, shown with a 95% confidence interval, and together they give a clear read of how economic sentiment in the news rose and fell across the period covered by the corpus. The shape of this curve also depends on the smoother’s own settings, not only on the data: a different span or bandwidth in `smooth_lss()` can make a trend look sharper or flatter than it really is. Try a few alternative specifications before drawing conclusions from the curve, and back up any pattern you find with a formal test rather than relying on the plot alone.

``` r
# create data frame with date and confidence intervals
dat_ci <- data.frame(date = dat_smooth$date,
                      upper = dat_smooth$fit + dat_smooth$se.fit * 1.96,
                      lower = dat_smooth$fit - dat_smooth$se.fit * 1.96)

ggplot() +
  geom_point(data = dat, aes(x = date, y = fit), alpha = 0.05) +
  geom_line(data = dat_smooth, aes(x = date, y = fit)) +
  geom_line(data = dat_ci, aes(x = date, y = upper), linetype = "dotted") +
  geom_line(data = dat_ci, aes(x = date, y = lower), linetype = "dotted") +
  geom_hline(yintercept = 0, linetype = "dashed") +
  coord_cartesian(ylim = c(-0.5, 0.5)) +
  labs(x = "Time", y = "Economic sentiment")
```

<img src="/machine-learning/lss.en_files/figure-html/unnamed-chunk-14-1.png" alt="" width="672" />

## References

<div id="refs" class="references csl-bib-body hanging-indent">

<div id="ref-watanabe2021" class="csl-entry">

Watanabe, Kohei. 2021. “Latent Semantic Scaling: A Semisupervised Text Analysis Technique for New Domains and Languages.” *Communication Methods and Measures* 15 (2): 81–102. <https://doi.org/10.1080/19312458.2020.1832976>.

</div>

<div id="ref-watanabe2023" class="csl-entry">

Watanabe, Kohei. 2023. *Introduction to LSX: The Package for Latent Semantic Scaling*. <http://koheiw.github.io/LSX/articles/pkgdown/introduction.html>.

</div>

</div>
