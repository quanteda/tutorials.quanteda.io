---
title: Similarity between Twitter users
weight: 10
draft: false
---


```r
require(quanteda)
require(readtext)
```

Import Tweets from JSON (.json) file. [twitter.json](https://raw.githubusercontent.com/quanteda/quanteda_tutorials/master/content/data/twitter.json) is located in data directory of this tutorial package.


```r
twitter_data <- readtext("content/data/twitter.json", source = "twitter")
```



Construct a corpus of Tweets.


```r
tweet_corp <- corpus(twitter_data)
```

Construct a DFM removing tags and links.


```r
tweet_dfm <- dfm(tweet_corp,
                 remove_punct = TRUE, remove_url = TRUE,
                 remove = c('*.tt', '*.uk', '*.com', 'rt', '#*', '@*')) %>% 
             dfm_remove(stopwords('en'))

ndoc(tweet_dfm)
```

```
## [1] 7504
```

```r
topfeatures(tweet_dfm)
```

```
##          vote conservatives        labour         today         share 
##          1817           929           676           666           647 
##       britain          find        fairer        voting      tomorrow 
##           625           613           571           559           548
```

Group documents by usernames.


```r
user_dfm <- dfm_group(tweet_dfm, groups = docvars(tweet_dfm, 'screen_name'))
ndoc(user_dfm)
```

```
## [1] 5061
```

Remove rare (less than 10 times) and short (one character) features, and convert count to proportion using `dfm_weight()`. 


```r
prop_user_dfm <- user_dfm %>% 
                 dfm_select(min_nchar = 2) %>% 
                 dfm_trim(min_termfreq = 10) %>% 
                 dfm_weight('prop')
```

Calculate user-user similarity using `textstat_dist()`.


```r
user_dist <- textstat_dist(prop_user_dfm)
user_clust <- hclust(user_dist)
plot(user_clust, labels = FALSE)
```

<img src="/advanced-operations/twitter-user-similarity_files/figure-html/unnamed-chunk-8-1.svg" width="768" />

