---
title: Simple frequency analysis
weight: 10
chapter: false
draft: false
---


```r
require(quanteda)
```

Unlike `topfeatures()`,`textstat_frequency() shows term and document frequencies. It can also be used to find the most frequent features within groups.


```r
tweet_corp <- quanteda.corpora::download(url = 'https://www.dropbox.com/s/846skn1i5elbnd2/data_corpus_sampletweets.rds?dl=1')
tweet_toks <- tokens(tweet_corp, remove_punct = TRUE) 
tweet_dfm <- dfm(tweet_toks, select = "#*")
freq <- textstat_frequency(tweet_dfm, n = 10, groups = docvars(tweet_dfm, 'lang'))
head(freq, 100)
```

```
##                                               feature frequency rank
## 1                                            #twitter         1    1
## 2                                      #canviemeuropa         1    2
## 3                                              #prest         1    3
## 4                                            #psifizo         1    4
## 5                                      #ekloges2014gr         1    5
## 6                                          #ekloges14         1    6
## 7                                        #ekloges2014         1    7
## 8                                        #mmkisat2014         1    8
## 9                                               #iihf         1    9
## 10                                            #ep2014         1    1
## 11                                         #yourvoice         1    2
## 12                                      #eudebate2014         1    3
## 13  #<U+0432><U+0435><U+043B><U+0438><U+043A><U+043E>         1    4
## 14                                  #savedonbaspeople         1    1
## 15                                    #vitoriagasteiz         1    2
## 16                                            #ep14dk        31    1
## 17                                             #dkpol        18    2
## 18                                             #eupol         7    3
## 19                                         #vindtilep         6    4
## 20                                     #patentdomstol         4    5
## 21                                          #cymru1af         2    6
## 22                                  #huskdetnu-tagget         1    7
## 23                                              #ep14         1    8
## 24                                             #swpat         1    9
## 25                                           #karenep         1   10
## 26                                            #ep2014        34    1
## 27                                               #vvd        10    2
## 28                                                #eu         8    3
## 29                                               #pvv         8    4
## 30                                               #d66         7    5
## 31                                              #pvda         7    6
## 32                                              #vk14         5    7
## 33                                              #ttip         5    8
## 34                                               #cda         4    9
## 35                                    #sterknlsterkeu         4   10
## 36                                              #ukip       108    1
## 37                                            #ep2014        87    2
## 38                                        #telleurope        31    3
## 39                                     #votegreen2014        23    4
## 40                                              #ep14        19    5
## 41                                             #bbcqt        18    6
## 42                                                #eu        14    7
## 43                                              #vinb        13    8
## 44                                      #knockthevote        11    9
## 45                                        #votepirate        10   10
## 46                                          #wales1st         2    1
## 47                                            #ep2014         1    2
## 48                                          #cymru1af         1    3
## 49                                      #häkkänen2014         1    4
## 50                                        #eurovaalit        22    1
## 51                                          #kokoomus         5    2
## 52                                           #vihreät         3    3
## 53                                          #euvaalit         3    4
## 54                                               #209         2    5
## 55                                           #sdp2014         2    6
## 56                                        #vasemmisto         2    7
## 57                                               #sdp         2    8
## 58                                                #eu         1    9
## 59                                  #debateuropeestv3         1   10
## 60                                            #ep2014       128    1
## 61                                            #ee2014        66    2
## 62                                   #europeennes2014        43    3
## 63                                                #fn        43    4
## 64                                       #notreeurope        37    5
## 65                                        #telleurope        24    6
## 66                                               #fdg        23    7
## 67                                               #ump        21    8
## 68                                   #européennes2014        20    9
## 69                                            #europe        17   10
## 70                                            #ep2014        26    1
## 71                                           #piraten        12    2
## 72                                        #telleurope         8    3
## 73                                              #ttip         7    4
## 74                                           #tvduell         6    5
## 75                                         #votegreen         5    6
## 76                                               #afd         5    7
## 77                                        #europawahl         4    8
## 78                                      #knockthevote         4    9
## 79                                               #spd         4   10
## 80                                            #ep2014         1    1
## 81                                        #telleurope         1    2
## 82                                       #withjuncker         1    3
## 83                                          #conchita         1    4
## 84                                      #naftemporiki         1    5
## 85                                        #vouliwatch         1    6
## 86                                            #cyprus         1    7
## 87                                     #live_newsroom         1    8
## 88                                        #votepirate         1    1
## 89             #toiaussiaidenadinearetrouversamalette         1    2
## 90                                               #ftw         1    1
## 91                                           #salvini       235    1
## 92                                             #fdian       202    2
## 93                                              #lega        95    3
## 94                                       #alzalatesta        88    4
## 95                                        #votameloni        83    5
## 96                                     #quota96scuola        74    6
## 97                                    #iovotoitaliano        61    7
## 98                                         #bastaeuro        51    8
## 99                                     #scelgogiorgia        46    9
## 100                                      #europee2014        43   10
##     docfreq     group
## 1         1    Basque
## 2         1    Basque
## 3         1    Basque
## 4         1    Basque
## 5         1    Basque
## 6         1    Basque
## 7         1    Basque
## 8         1    Basque
## 9         1    Basque
## 10        1 Bulgarian
## 11        1 Bulgarian
## 12        1 Bulgarian
## 13        1 Bulgarian
## 14        1  Croatian
## 15        1  Croatian
## 16       31    Danish
## 17       18    Danish
## 18        7    Danish
## 19        6    Danish
## 20        4    Danish
## 21        2    Danish
## 22        1    Danish
## 23        1    Danish
## 24        1    Danish
## 25        1    Danish
## 26       34     Dutch
## 27       10     Dutch
## 28        6     Dutch
## 29        8     Dutch
## 30        7     Dutch
## 31        7     Dutch
## 32        5     Dutch
## 33        4     Dutch
## 34        4     Dutch
## 35        4     Dutch
## 36      105   English
## 37       87   English
## 38       31   English
## 39       22   English
## 40       19   English
## 41       18   English
## 42       14   English
## 43       13   English
## 44       11   English
## 45       10   English
## 46        2  Estonian
## 47        1  Estonian
## 48        1  Estonian
## 49        1  Estonian
## 50       22   Finnish
## 51        5   Finnish
## 52        3   Finnish
## 53        3   Finnish
## 54        2   Finnish
## 55        2   Finnish
## 56        2   Finnish
## 57        2   Finnish
## 58        1   Finnish
## 59        1   Finnish
## 60      128    French
## 61       66    French
## 62       43    French
## 63       43    French
## 64       37    French
## 65       24    French
## 66       23    French
## 67       20    French
## 68       20    French
## 69       17    French
## 70       26    German
## 71       12    German
## 72        8    German
## 73        7    German
## 74        6    German
## 75        5    German
## 76        5    German
## 77        4    German
## 78        4    German
## 79        4    German
## 80        1     Greek
## 81        1     Greek
## 82        1     Greek
## 83        1     Greek
## 84        1     Greek
## 85        1     Greek
## 86        1     Greek
## 87        1     Greek
## 88        1   Haitian
## 89        1   Haitian
## 90        1 Hungarian
## 91      235   Italian
## 92      202   Italian
## 93       95   Italian
## 94       88   Italian
## 95       83   Italian
## 96       73   Italian
## 97       61   Italian
## 98       51   Italian
## 99       46   Italian
## 100      43   Italian
```


