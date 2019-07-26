---
title: Similarity between Twitter users
weight: 10
draft: false
---


```r
require(quanteda)
require(readtext)
```

Import Tweets from JSON (.json) file. [twitter.json](https://raw.githubusercontent.com/quanteda/tutorials.quanteda.io/master/content/data/twitter.json) is located in data directory of this tutorial package.


```r
data_twitter <- readtext("content/data/twitter.json", source = "twitter")
```



Construct a corpus of Tweets.


```r
corp_tweets <- corpus(data_twitter)
```

Construct a document-feature matrix removing tags and links.


```r
dfmat_tweets <- dfm(corp_tweets,
                 remove_punct = TRUE, remove_url = TRUE,
                 remove = c('*.tt', '*.uk', '*.com', 'rt', '#*', '@*')) %>% 
             dfm_remove(stopwords('en'))

ndoc(dfmat_tweets)
```

```
## [1] 7504
```

```r
topfeatures(dfmat_tweets)
```

```
##          vote conservatives        labour         today         share 
##          1873           953           758           674           648 
##       britain          find        fairer        voting      tomorrow 
##           639           615           571           570           565
```

Group documents by usernames.


```r
dfmat_users <- dfm_group(dfmat_tweets, groups = 'screen_name')
ndoc(dfmat_users)
```

```
## [1] 5061
```

Remove rare (less than 10 times) and short (one character) features, and keep only users with more than 50 tokens in total.


```r
dfmat_users <- dfmat_users %>% 
    dfm_select(min_nchar = 2) %>% 
    dfm_trim(min_termfreq = 10) 
dfmat_users <- dfmat_users[ntoken(dfmat_users) > 50,]
```

Calculate user-user similarity using `textstat_dist()`.


```r
tstat_dist <- as.dist(textstat_dist(dfmat_users))
user_clust <- hclust(tstat_dist)
plot(user_clust)
```

<img src="/advanced-operations/twitter-user-similarity_files/figure-html/unnamed-chunk-8-1.png" width="672" />

