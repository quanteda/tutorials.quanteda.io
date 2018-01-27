---
title: Simple frequency analysis
weight: 10
chapter: false
draft: false
---


```r
require(quanteda)
require(ggplot2)
```

Unlike `topfeatures()`,`textstat_frequency()` shows term and document frequencies. It can also be used to find the most frequent features within groups.


```r
tweet_corp <- quanteda.corpora::download(url = 'https://www.dropbox.com/s/846skn1i5elbnd2/data_corpus_sampletweets.rds?dl=1')
tweet_toks <- tokens(tweet_corp, remove_punct = TRUE) 
tweet_dfm <- dfm(tweet_toks, select = "#*")
freq <- textstat_frequency(tweet_dfm, n = 5, groups = docvars(tweet_dfm, 'lang'))
head(freq, 100)
```

```
##                                    feature frequency rank docfreq
## 1                                 #twitter         1    1       1
## 2                           #canviemeuropa         1    2       1
## 3                                   #prest         1    3       1
## 4                                 #psifizo         1    4       1
## 5                           #ekloges2014gr         1    5       1
## 6                                  #ep2014         1    1       1
## 7                               #yourvoice         1    2       1
## 8                            #eudebate2014         1    3       1
## 9                                  #велико         1    4       1
## 10                       #savedonbaspeople         1    1       1
## 11                         #vitoriagasteiz         1    2       1
## 12                                 #ep14dk        31    1      31
## 13                                  #dkpol        18    2      18
## 14                                  #eupol         7    3       7
## 15                              #vindtilep         6    4       6
## 16                          #patentdomstol         4    5       4
## 17                                 #ep2014        34    1      34
## 18                                    #vvd        10    2      10
## 19                                     #eu         8    3       6
## 20                                    #pvv         8    4       8
## 21                                    #d66         7    5       7
## 22                                   #ukip       108    1     105
## 23                                 #ep2014        87    2      87
## 24                             #telleurope        31    3      31
## 25                          #votegreen2014        23    4      22
## 26                                   #ep14        19    5      19
## 27                               #wales1st         2    1       2
## 28                                 #ep2014         1    2       1
## 29                               #cymru1af         1    3       1
## 30                           #häkkänen2014         1    4       1
## 31                             #eurovaalit        22    1      22
## 32                               #kokoomus         5    2       5
## 33                                #vihreät         3    3       3
## 34                               #euvaalit         3    4       3
## 35                                    #209         2    5       2
## 36                                 #ep2014       128    1     128
## 37                                 #ee2014        66    2      66
## 38                        #europeennes2014        43    3      43
## 39                                     #fn        43    4      43
## 40                            #notreeurope        37    5      37
## 41                                 #ep2014        26    1      26
## 42                                #piraten        12    2      12
## 43                             #telleurope         8    3       8
## 44                                   #ttip         7    4       7
## 45                                #tvduell         6    5       6
## 46                                 #ep2014         1    1       1
## 47                             #telleurope         1    2       1
## 48                            #withjuncker         1    3       1
## 49                               #conchita         1    4       1
## 50                           #naftemporiki         1    5       1
## 51                             #votepirate         1    1       1
## 52  #toiaussiaidenadinearetrouversamalette         1    2       1
## 53                                    #ftw         1    1       1
## 54                                #salvini       235    1     235
## 55                                  #fdian       202    2     202
## 56                                   #lega        95    3      95
## 57                            #alzalatesta        88    4      88
## 58                             #votameloni        83    5      83
## 59                                 #lv10es         1    1       1
## 60                                 #lbrd14         1    1       1
## 61                                 #ep2014         2    1       2
## 62                                     #ff         2    2       2
## 63                             #ukpolitics         2    3       2
## 64                                     #eu         1    4       1
## 65                  #tydzieńwygranychszans         1    5       1
## 66                        #ovotoquellesdoe         4    1       4
## 67                                 #ep2014         2    2       2
## 68                       #elpoderdelagente         2    3       2
## 69                           #éahoradopobo         2    4       2
## 70                           #eahoradopobo         2    5       2
## 71                                   #ukip         1    1       1
## 72                            #loriuesvida         1    2       1
## 73                                #pertots         1    3       1
## 74                                    #bcn         1    4       1
## 75                               #valència         1    5       1
## 76                                  #fdian         5    1       5
## 77                            #alzalatesta         4    2       4
## 78                         #iovotoitaliano         2    3       2
## 79                            #europee2014         2    4       2
## 80                           #votaitaliano         2    5       2
## 81                                 #ep2014         1    1       1
## 82                           #100happydays         1    2       1
## 83                            #withjuncker         1    3       1
## 84                                   #le14         1    4       1
## 85                                #poplave         1    5       1
## 86                           #caraacaratve        97    1      97
## 87                         #tumueveseuropa        61    2      61
## 88                                 #votapp        58    3      58
## 89                             #podemos25m        42    4      42
## 90                       #primaveraeuropea        38    5      38
## 91                                  #svpol        31    1      31
## 92                                #dinröst        17    2      16
## 93                               #pldebatt        10    3      10
## 94                                  #eupol        10    4      10
## 95                           #piratpartiet        10    5      10
## 96                      #votaloquelesduele         1    1       1
## 97                                     #ff         1    1       1
## 98                                    #csi         1    2       1
## 99                                 #ep2014         2    1       2
## 100                          #leseuropeens         2    2       2
##          group
## 1       Basque
## 2       Basque
## 3       Basque
## 4       Basque
## 5       Basque
## 6    Bulgarian
## 7    Bulgarian
## 8    Bulgarian
## 9    Bulgarian
## 10    Croatian
## 11    Croatian
## 12      Danish
## 13      Danish
## 14      Danish
## 15      Danish
## 16      Danish
## 17       Dutch
## 18       Dutch
## 19       Dutch
## 20       Dutch
## 21       Dutch
## 22     English
## 23     English
## 24     English
## 25     English
## 26     English
## 27    Estonian
## 28    Estonian
## 29    Estonian
## 30    Estonian
## 31     Finnish
## 32     Finnish
## 33     Finnish
## 34     Finnish
## 35     Finnish
## 36      French
## 37      French
## 38      French
## 39      French
## 40      French
## 41      German
## 42      German
## 43      German
## 44      German
## 45      German
## 46       Greek
## 47       Greek
## 48       Greek
## 49       Greek
## 50       Greek
## 51     Haitian
## 52     Haitian
## 53   Hungarian
## 54     Italian
## 55     Italian
## 56     Italian
## 57     Italian
## 58     Italian
## 59     Latvian
## 60   Norwegian
## 61      Polish
## 62      Polish
## 63      Polish
## 64      Polish
## 65      Polish
## 66  Portuguese
## 67  Portuguese
## 68  Portuguese
## 69  Portuguese
## 70  Portuguese
## 71    Romanian
## 72    Romanian
## 73    Romanian
## 74    Romanian
## 75    Romanian
## 76      Slovak
## 77      Slovak
## 78      Slovak
## 79      Slovak
## 80      Slovak
## 81     Slovene
## 82     Slovene
## 83     Slovene
## 84     Slovene
## 85     Slovene
## 86     Spanish
## 87     Spanish
## 88     Spanish
## 89     Spanish
## 90     Spanish
## 91     Swedish
## 92     Swedish
## 93     Swedish
## 94     Swedish
## 95     Swedish
## 96     Tagalog
## 97  Undetected
## 98  Undetected
## 99  Vietnamese
## 100 Vietnamese
```

You can also plot the frequencies easily.


```r
tweet_dfm %>% 
  textstat_frequency(n = 15) %>% 
  ggplot(aes(x = reorder(feature, frequency), y = frequency)) +
  geom_point() +
  coord_flip() +
  labs(x = NULL, y = "Frequency") +
  theme_minimal()
```

<img src="/statistical-analysis/frequency_files/figure-html/unnamed-chunk-3-1.svg" width="768" />


{{% notice note %}}
Often, it might make sense to weight the features of `dfm` before analysing frequencies. `dfm_weight()` contains several options for weighting a document-feature matrix. For instance, to control for the length of each document, `scheme  = "prop"` estimates the proportion of the total feature counts.

The option `dfm_tfidf()` allows you to weight a dfm by term frequency-inverse documentn frequency, a commonly used measure in quantitative text analysis.
{{% /notice %}}

