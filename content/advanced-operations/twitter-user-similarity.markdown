---
title: Compute similarity between authors
weight: 10
draft: false
bibliography: ../references.bib
---

We bring together several of the tools from earlier chapters in a single worked example. We can compute how similar authors are to one another by grouping their documents and comparing each author’s word usage with everyone else’s. In this example, we group Twitter posts by handle name and compute similarities between the users.

``` r
library(quanteda)
library(quanteda.textstats)
library(readtext)
```

We import tweets from a JSON (`.json`) file, as covered in the [Data Import chapter](/import-data/multiple-files). [twitter.json](https://raw.githubusercontent.com/quanteda/tutorials.quanteda.io/master/content/data/twitter.json) is located in the data directory of this tutorial package.

{{% notice warning %}}
Working with social media data raises ethical and legal questions that go beyond the technical steps shown here: platform terms of service, users’ reasonable expectations of privacy even for public posts, and data protection law all bear on what you may collect and how you may use it.
{{% /notice %}}

``` r
dat_twitter <- readtext("../data/twitter.json", source = "twitter")
```

We construct a corpus of tweets in the usual way, from the data frame that `readtext()` returned.

``` r
corp_tweets <- corpus(dat_twitter)
```

Social media posts contain hashtags, links and short abbreviations. We construct a document-feature matrix after tokenising and removing punctuation, URLs and symbols in one step. We then remove hashtags, mentions, common web domains and English stopwords.

``` r
dfmat_tweets <- corp_tweets |> 
    tokens(remove_punct = TRUE, remove_url = TRUE, remove_symbols = TRUE) |> 
    dfm() |> 
    dfm_remove(pattern = c("*.tt", "*.uk", "*.com", "rt", "#*", "@*")) |> 
    dfm_remove(pattern = stopwords("en"))
ndoc(dfmat_tweets)
```

    ## [1] 7504

``` r
topfeatures(dfmat_tweets)
```

    ##          vote conservatives        labour         today         share 
    ##          1886           959           774           676           649 
    ##       britain          find      tomorrow        fairer        voting 
    ##           639           615           571           571           570

Right now, every row of `dfmat_tweets` is one tweet, but we want to compare users, not individual tweets. We group documents by Twitter handle name (`screen_name`), using `dfm_group()` exactly as you did with speeches in the [Basic Operations chapter](/basic-operations/dfm/dfm_group), so that all of one user’s tweets are combined into a single row.

``` r
dfmat_users <- dfm_group(dfmat_tweets, groups = screen_name)
ndoc(dfmat_users)
```

    ## [1] 5061

``` r
dfmat_users <- dfmat_users |> 
    dfm_select(min_nchar = 2) |> 
    dfm_trim(min_termfreq = 10) 
dfmat_users <- dfmat_users[ntoken(dfmat_users) > 50,]
```

Finally, we calculate user-user similarity using `textstat_dist()`, which measures how different two documents’ word usage is from one another. Feeding the result into `hclust()` performs hierarchical clustering, grouping the most similar users together in a dendrogram that lets us read off which users wrote in the most similar way.

``` r
tstat_dist <- as.dist(textstat_dist(dfmat_users))
user_clust <- hclust(tstat_dist)
plot(user_clust)
```

<img src="/advanced-operations/twitter-user-similarity_files/figure-html/unnamed-chunk-7-1.png" alt="" width="672" />

Users that are joined together low in the tree are the most similar to one another in their word usage, while users that only join much higher up have comparatively little in common. Note that this reflects similarity in vocabulary alone.

## References
