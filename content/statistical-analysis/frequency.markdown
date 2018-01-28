---
title: Simple frequency analysis
weight: 10
chapter: false
draft: false
---


```r
require(quanteda)
require(quanteda.corpora)
require(ggplot2)
```

Unlike `topfeatures()`,`textstat_frequency()` shows term and document frequencies. It can also be used to find the most frequent features within groups.


```r
tweet_corp <- download(url = 'https://www.dropbox.com/s/846skn1i5elbnd2/data_corpus_sampletweets.rds?dl=1')
tweet_toks <- tokens(tweet_corp, remove_punct = TRUE) 
tweet_dfm <- dfm(tweet_toks, select = "#*")
freq <- textstat_frequency(tweet_dfm, n = 5, groups = docvars(tweet_dfm, 'lang'))
head(freq, 100)
```

```
##                                               feature frequency rank
## 1                                            #twitter         1    1
## 2                                      #canviemeuropa         1    2
## 3                                              #prest         1    3
## 4                                            #psifizo         1    4
## 5                                      #ekloges2014gr         1    5
## 6                                             #ep2014         1    1
## 7                                          #yourvoice         1    2
## 8                                       #eudebate2014         1    3
## 9   #<U+0432><U+0435><U+043B><U+0438><U+043A><U+043E>         1    4
## 10                                  #savedonbaspeople         1    1
## 11                                    #vitoriagasteiz         1    2
## 12                                            #ep14dk        31    1
## 13                                             #dkpol        18    2
## 14                                             #eupol         7    3
## 15                                         #vindtilep         6    4
## 16                                     #patentdomstol         4    5
## 17                                            #ep2014        34    1
## 18                                               #vvd        10    2
## 19                                                #eu         8    3
## 20                                               #pvv         8    4
## 21                                               #d66         7    5
## 22                                              #ukip       108    1
## 23                                            #ep2014        87    2
## 24                                        #telleurope        31    3
## 25                                     #votegreen2014        23    4
## 26                                              #ep14        19    5
## 27                                          #wales1st         2    1
## 28                                            #ep2014         1    2
## 29                                          #cymru1af         1    3
## 30                                      #häkkänen2014         1    4
## 31                                        #eurovaalit        22    1
## 32                                          #kokoomus         5    2
## 33                                           #vihreät         3    3
## 34                                          #euvaalit         3    4
## 35                                               #209         2    5
## 36                                            #ep2014       128    1
## 37                                            #ee2014        66    2
## 38                                   #europeennes2014        43    3
## 39                                                #fn        43    4
## 40                                       #notreeurope        37    5
## 41                                            #ep2014        26    1
## 42                                           #piraten        12    2
## 43                                        #telleurope         8    3
## 44                                              #ttip         7    4
## 45                                           #tvduell         6    5
## 46                                            #ep2014         1    1
## 47                                        #telleurope         1    2
## 48                                       #withjuncker         1    3
## 49                                          #conchita         1    4
## 50                                      #naftemporiki         1    5
## 51                                        #votepirate         1    1
## 52             #toiaussiaidenadinearetrouversamalette         1    2
## 53                                               #ftw         1    1
## 54                                           #salvini       235    1
## 55                                             #fdian       202    2
## 56                                              #lega        95    3
## 57                                       #alzalatesta        88    4
## 58                                        #votameloni        83    5
## 59                                            #lv10es         1    1
## 60                                            #lbrd14         1    1
## 61                                            #ep2014         2    1
## 62                                                #ff         2    2
## 63                                        #ukpolitics         2    3
## 64                                                #eu         1    4
## 65                             #tydzienwygranychszans         1    5
## 66                                   #ovotoquellesdoe         4    1
## 67                                            #ep2014         2    2
## 68                                  #elpoderdelagente         2    3
## 69                                      #éahoradopobo         2    4
## 70                                      #eahoradopobo         2    5
## 71                                              #ukip         1    1
## 72                                       #loriuesvida         1    2
## 73                                           #pertots         1    3
## 74                                               #bcn         1    4
## 75                                          #valència         1    5
## 76                                             #fdian         5    1
## 77                                       #alzalatesta         4    2
## 78                                    #iovotoitaliano         2    3
## 79                                       #europee2014         2    4
## 80                                      #votaitaliano         2    5
## 81                                            #ep2014         1    1
## 82                                      #100happydays         1    2
## 83                                       #withjuncker         1    3
## 84                                              #le14         1    4
## 85                                           #poplave         1    5
## 86                                      #caraacaratve        97    1
## 87                                    #tumueveseuropa        61    2
## 88                                            #votapp        58    3
## 89                                        #podemos25m        42    4
## 90                                  #primaveraeuropea        38    5
## 91                                             #svpol        31    1
## 92                                           #dinröst        17    2
## 93                                          #pldebatt        10    3
## 94                                             #eupol        10    4
## 95                                      #piratpartiet        10    5
## 96                                 #votaloquelesduele         1    1
## 97                                                #ff         1    1
## 98                                               #csi         1    2
## 99                                            #ep2014         2    1
## 100                                     #leseuropeens         2    2
##     docfreq      group
## 1         1     Basque
## 2         1     Basque
## 3         1     Basque
## 4         1     Basque
## 5         1     Basque
## 6         1  Bulgarian
## 7         1  Bulgarian
## 8         1  Bulgarian
## 9         1  Bulgarian
## 10        1   Croatian
## 11        1   Croatian
## 12       31     Danish
## 13       18     Danish
## 14        7     Danish
## 15        6     Danish
## 16        4     Danish
## 17       34      Dutch
## 18       10      Dutch
## 19        6      Dutch
## 20        8      Dutch
## 21        7      Dutch
## 22      105    English
## 23       87    English
## 24       31    English
## 25       22    English
## 26       19    English
## 27        2   Estonian
## 28        1   Estonian
## 29        1   Estonian
## 30        1   Estonian
## 31       22    Finnish
## 32        5    Finnish
## 33        3    Finnish
## 34        3    Finnish
## 35        2    Finnish
## 36      128     French
## 37       66     French
## 38       43     French
## 39       43     French
## 40       37     French
## 41       26     German
## 42       12     German
## 43        8     German
## 44        7     German
## 45        6     German
## 46        1      Greek
## 47        1      Greek
## 48        1      Greek
## 49        1      Greek
## 50        1      Greek
## 51        1    Haitian
## 52        1    Haitian
## 53        1  Hungarian
## 54      235    Italian
## 55      202    Italian
## 56       95    Italian
## 57       88    Italian
## 58       83    Italian
## 59        1    Latvian
## 60        1  Norwegian
## 61        2     Polish
## 62        2     Polish
## 63        2     Polish
## 64        1     Polish
## 65        1     Polish
## 66        4 Portuguese
## 67        2 Portuguese
## 68        2 Portuguese
## 69        2 Portuguese
## 70        2 Portuguese
## 71        1   Romanian
## 72        1   Romanian
## 73        1   Romanian
## 74        1   Romanian
## 75        1   Romanian
## 76        5     Slovak
## 77        4     Slovak
## 78        2     Slovak
## 79        2     Slovak
## 80        2     Slovak
## 81        1    Slovene
## 82        1    Slovene
## 83        1    Slovene
## 84        1    Slovene
## 85        1    Slovene
## 86       97    Spanish
## 87       61    Spanish
## 88       58    Spanish
## 89       42    Spanish
## 90       38    Spanish
## 91       31    Swedish
## 92       16    Swedish
## 93       10    Swedish
## 94       10    Swedish
## 95       10    Swedish
## 96        1    Tagalog
## 97        1 Undetected
## 98        1 Undetected
## 99        2 Vietnamese
## 100       2 Vietnamese
```

You can also plot the frequencies easily using `ggplot()`.


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


