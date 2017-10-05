---
title: Twitter
---



# Accessing Twitter Text

### Kenneth Benoit
### 24 April 2017

### Overview

In this section we will cover the following methods for mining and analyzing text from the twitter API:

* Creating a new application and getting access to the REST and streaming APIs
* Using the REST and streaming APIs from within R

### Accessing the twitter APIs

To access the REST and streaming APIs, you will need to create a twitter application, and generate authentication credentials associated with this application. To do this you will first need to have a twitter account. You will also need to install at least the following R packages: twitteR, 



```r
install.packages(c('twitteR', 'streamR', 'RCurl', 'ROAuth', 'httr'))
```

To register a twitter application and get your consumer keys:

1. Go to <https://apps.twitter.com> in a web browser.
2. Click on 'create new app'.
3. Give your app a unique name, a description, any relevant web address, and agree to the terms and conditions. Set the callback URL to http://127.0.0.1:1410. You might have to add a cellphone number your twitter account.
4. Go to the keys and access section of the app page, and copy your consumer key and consumer secret to the code below.
5. (optional): For actions requiring write permissions, generate an access token and access secret .


```r
require(twitteR)
require(streamR)
require(ROAuth)

consumerKey <- 'your key here'
consumerSecret <- 'your secret here'

# Try this first, to use twitteR
setup_twitter_oauth(consumerKey, consumerSecret)
results <- searchTwitter('#DemDebate')
df <- as.data.frame(t(sapply(results, as.data.frame)))
```

Then try these instructions, to use streamR:
<https://github.com/pablobarbera/streamR#installation-and-authentication>


In this section we will explore some text analysis and analysis of metadata from a corpus of tweets retrieved from the Twitter API. The tweets are a small sample from a collection of tweets relating to the European Parliament elections of 2015.

Load the data frame containing the sample tweets:


```r
require(quanteda)
## Loading required package: quanteda
## quanteda version 0.99.9
## Using 3 of 4 threads for parallel computing
## 
## Attaching package: 'quanteda'
## The following object is masked from 'package:utils':
## 
##     View
load("tweetSample.RData")
str(tweetSample)
## 'data.frame':	10000 obs. of  35 variables:
##  $ created_at          : chr  "2014-05-28 15:53:33+00:00" "2014-05-30 08:32:13+00:00" "2014-05-29 19:22:15+00:00" "2014-05-03 20:23:43+00:00" ...
##  $ geo_latitude        : num  NA NA NA NA NA NA NA NA NA NA ...
##  $ geo_longitude       : num  NA NA NA NA NA NA NA NA NA NA ...
##  $ hashtags            : chr  "['Pomeriggio5', 'Canale5']" NA NA NA ...
##  $ id                  : num  4.72e+17 4.72e+17 4.72e+17 4.63e+17 4.71e+17 ...
##  $ lang                : Factor w/ 43 levels "Arabic","Basque",..: 20 35 35 15 30 12 33 9 35 35 ...
##  $ text                : chr  "Oggi pomeriggio, a partire dalle 18.00, interverrò a #Pomeriggio5 su #Canale5 http://t.co/aqB64fH4et ST" ".@pacomarhuenda llamando El Coletas a @Pablo_Iglesias_... precisamente, si hay alguien que tiene que callarse s"| __truncated__ "Las declaraciones de Felipe Gonzalez hoy hablan por sí solas http://t.co/0LJo6zAXdc" "@KOPRITHS @GAPATZHS @MariaSpyraki και εκεί που λες εχουν πιάσει πάτο, θα καταφέρουν να σε διαψεύσουν." ...
##  $ type                : Factor w/ 3 levels "reply","retweet",..: 2 3 2 2 3 2 2 2 2 2 ...
##  $ user_followers_count: int  769 303 470 470 3662 470 67 124 1359 181 ...
##  $ user_friends_count  : int  557 789 419 647 793 910 36 90 793 258 ...
##  $ user_geo_enabled    : Factor w/ 2 levels "False","True": 1 1 2 1 2 1 2 1 1 2 ...
##  $ user_id             : num  8.40e+07 2.75e+08 4.61e+08 2.43e+09 1.62e+08 ...
##  $ user_id_str         : num  8.40e+07 2.75e+08 4.61e+08 2.43e+09 1.62e+08 ...
##  $ user_lang           : Factor w/ 40 levels "Arabic","Basque",..: 10 34 34 16 4 13 21 10 4 34 ...
##  $ user_listed_count   : int  6 13 1 1 133 4 0 3 31 7 ...
##  $ user_location       : chr  NA "Sanfer of Henares" "La Puebla ciry" NA ...
##  $ user_name           : chr  "Francesco Filini" "Carlos Marina" "Gabi Armario Cívico" "ΤΗΛΕΠΛΑΣΙΕ" ...
##  $ user_screen_name    : chr  "FrancescoFilini" "marina_carlos" "erpartecama" "THLEPLASHIE" ...
##  $ user_statuses_count : int  1880 7051 6776 666 19006 30239 1563 601 37237 2313 ...
##  $ user_time_zone      : chr  "Amsterdam" "Madrid" "Athens" NA ...
##  $ user_url            : chr  "http://rapportoaureo.wordpress.com" "http://carlosmarina.com" "http://www.cazuelaalamorisca.com" NA ...
##  $ user_created_at     : chr  "Wed, 21 Oct 2009 08:59:58 +0000" "2011-03-30 13:07:21+00:00" "Tue, 10 Jan 2012 23:23:18 +0000" "Mon, 07 Apr 2014 10:59:39 +0000" ...
##  $ user_geo_enabled.1  : Factor w/ 2 levels "False","True": 1 1 2 1 2 1 2 1 1 2 ...
##  $ user_screen_nameL   : chr  "francescofilini" "marina_carlos" "erpartecama" "thleplashie" ...
##  $ Party               : chr  NA NA NA NA ...
##  $ Party.Code          : num  NA NA NA NA NA NA NA NA NA NA ...
##  $ Sitting_2009        : Factor w/ 2 levels "Non-incumbent",..: NA NA NA NA NA NA NA NA NA NA ...
##  $ Sitting_2014        : Factor w/ 2 levels "Non-incumbent",..: NA NA NA NA NA NA NA NA NA NA ...
##  $ Name                : chr  NA NA NA NA ...
##  $ Twitter             : chr  NA NA NA NA ...
##  $ Facebook            : chr  NA NA NA NA ...
##  $ gender              : Factor w/ 2 levels "Female","Male": NA NA NA NA NA NA NA NA NA NA ...
##  $ Country             : Factor w/ 27 levels "Austria","Belgium",..: NA NA NA NA NA NA NA NA NA NA ...
##  $ hasTwitter          : Factor w/ 2 levels "No","Yes": NA NA NA NA NA NA NA NA NA NA ...
##  $ candidate           : Factor w/ 2 levels "candidate","non-candidate": NA NA NA NA NA NA NA NA NA NA ...
```



```r
require(lubridate)
## Loading required package: lubridate
## 
## Attaching package: 'lubridate'
## The following object is masked from 'package:base':
## 
##     date
require(dplyr)
## Loading required package: dplyr
## 
## Attaching package: 'dplyr'
## The following objects are masked from 'package:lubridate':
## 
##     intersect, setdiff, union
## The following objects are masked from 'package:stats':
## 
##     filter, lag
## The following objects are masked from 'package:base':
## 
##     intersect, setdiff, setequal, union
tweetSample <- mutate(tweetSample, day = yday(created_at))
tweetSample <- mutate(tweetSample, dayDate = as.Date(day-1, origin = "2014-01-01"))
juncker <- filter(tweetSample, grepl('juncker', text, ignore.case = TRUE)) %>% 
    mutate(kand = 'Juncker')
schulz <-  filter(tweetSample, grepl('schulz', text, ignore.case = TRUE)) %>% 
    mutate(kand = 'Schulz')
verhof <-  filter(tweetSample, grepl('verhofstadt', text, ignore.case = TRUE)) %>% 
    mutate(kand = 'Verhofstadt')
spitzAll <- bind_rows(juncker, schulz, verhof)
```

Once the data is in the correct format, we can use ggplot to display the candidate mentions on the a single plot:



```r
require(ggplot2)
## Loading required package: ggplot2
require(scales)
## Loading required package: scales
# mentioning kandidates names over time
plotDf <- count(spitzAll, kand, day=day) %>% 
    mutate(day = as.Date(day-1, origin = "2014-01-01"))

ggplot(data=plotDf, aes(x=day, y=n, colour=kand)) + 
    geom_line(size=1) +
    scale_y_continuous(labels = comma) + geom_vline(xintercept=as.numeric(as.Date("2014-05-15")), linetype=4) +
    geom_vline(xintercept=as.numeric(as.Date("2014-05-25")), linetype=4) +
    theme(axis.text=element_text(size=12),
          axis.title=element_text(size=14,face="bold"))
```

<img src="/_examples/twitter_files/figure-html/unnamed-chunk-6-1.svg" width="768" />


We can use the `keptFeatures` argument to `dfm()` to analyse only hashtags for each candidate's text.

```r
# Top hashtags for tweets that mention Juncker
dv <- data.frame(user = juncker$user_screen_name)
jCorp <- corpus(juncker$text, docvars = dv)
jd <- dfm(jCorp)
jd <- dfm_select(jd, "^#.+", "keep", valuetype = "regex") 
# equivalent: jd <- dfm_select(jd, "#*", "keep", valuetype = "glob") 
topfeatures(jd, nfeature(jd))
##   #withjuncker        #ep2014    #telleurope           #afd       #tvduell 
##              5              4              3              2              2 
##     #nowschulz  #eudebate2014            #rt        #ee2014          #riga 
##              1              1              1              1              1 
##   #teammartens        #eu2014  #caraacaratve           #ppe        #votapp 
##              1              1              1              1              1 
##    #votacanete #publicviewing     #tvduell's        #euhaus          #eu14 
##              1              1              1              1              1 
##    #europawahl        #berlin         #linke        #merkel       #gabriel 
##              1              1              1              1              1 
##     #wahlarena 
##              1
```

