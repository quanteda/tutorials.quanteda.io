---
title: Identify related words of keywords
weight: 40
draft: false
---

We can identify related words of keywords based on their distance in the documents. In this example, we created a list of words related to the European Union by comparing frequency of words inside and outside of their contexts.


```r
require(quanteda)
```

This corpus contains 6,000 Guardian news articles from 2012 to 2016.


```r
corp_news <- download("data_corpus_guardian")
```




```r
toks_news <- tokens(corp_news, remove_punct = TRUE)
```

We select two tokens objects for words inside and outside of the 10-word windows of the keywords (`eu`). 


```r
eu <- c("EU", "europ*", "european union")
toks_inside <- tokens_keep(toks_news, pattern = eu, window = 10)
toks_inside <- tokens_remove(toks_news, pattern = eu) # remove the patterns
toks_outside <- tokens_remove(toks_news, pattern = eu, window = 10)
```

We compute words' association with the keywords using `textstat_keyness()`.


```r
dfmat_inside <- dfm(toks_inside)
dfmat_outside <- dfm(toks_outside)

tstat_key_inside <- textstat_keyness(rbind(dfmat_inside, dfmat_outside), 
                                     target = seq_len(ndoc(dfmat_inside)))
tstat_key_inside_subset <- tstat_key_inside[tstat_key_inside$n_target > 10, ]
head(tstat_key_inside_subset, 50)
```

```
##          feature      chi2            p n_target n_reference
## 1          union 74.044350 0.000000e+00     1221         805
## 2     membership 70.493500 0.000000e+00      413         197
## 3     referendum 64.981058 7.771561e-16     1056         694
## 4        britain 49.777715 1.721845e-12     1890        1436
## 5       migrants 26.922514 2.117775e-07      463         308
## 6    jean-claude 26.878444 2.166620e-07       67          18
## 7        juncker 26.830776 2.220724e-07      123          52
## 8          leave 26.388087 2.792576e-07     1594        1278
## 9             uk 23.656193 1.151746e-06     4874        4279
## 10       eastern 21.547340 3.452011e-06      306         195
## 11     britain's 21.172666 4.197080e-06      883         679
## 12    commission 21.004489 4.582086e-06     1026         804
## 13       leaders 20.215239 6.919949e-06     1307        1055
## 14        schulz 20.053605 7.530146e-06       65          22
## 15   @openeurope 19.440068 1.038063e-05       20           0
## 16        summit 16.956103 3.825414e-05      556         414
## 17      brussels 16.198158 5.704956e-05      720         558
## 18       in-work 14.647158 1.296300e-04       83          39
## 19     countries 13.023767 3.075625e-04     2022        1747
## 20          tusk 12.580040 3.898884e-04      161         100
## 21  @plpermrepeu 11.664031 6.372014e-04       12           0
## 22       markets 11.631098 6.485824e-04     1022         848
## 23 renegotiation 11.402247 7.335534e-04       67          32
## 24     migration 10.726569 1.056081e-03      307         224
## 25        greece 10.109257 1.475264e-03      671         543
## 26           the  9.558500 1.990256e-03   282792      272672
## 27       leaving  8.932709 2.801086e-03      646         527
## 28       central  8.654548 3.262461e-03      993         841
## 29      schengen  8.579716 3.399283e-03       94          56
## 30        brexit  8.178528 4.238915e-03      785         656
## 31       cameron  8.011521 4.648069e-03     2079        1846
## 32       barroso  7.914451 4.904146e-03       21           6
## 33      reformed  7.804417 5.211868e-03       46          22
## 34  negotiations  7.757826 5.348030e-03      567         463
## 35   enltrpolish  7.664434 5.631994e-03       15           3
## 36      refugees  7.514811 6.119371e-03      917         780
## 37       staying  7.504025 6.156124e-03      166         116
## 38        turkey  7.481093 6.235022e-03      499         404
## 39      benefits  7.298592 6.900867e-03      868         737
## 40          uk's  6.963112 8.320718e-03      620         515
## 41       migrant  6.229540 1.256364e-02      177         129
## 42    luxembourg  6.228814 1.256879e-02       70          42
## 43   continental  6.010918 1.421763e-02       25          10
## 44        treaty  5.903470 1.511108e-02      171         125
## 45          exit  5.855698 1.552670e-02      256         198
## 46         brake  5.699191 1.697273e-02      132          93
## 47          vote  5.608539 1.787316e-02     2464        2235
## 48        crisis  5.566897 1.830316e-02     1460        1297
## 49        non-eu  5.465400 1.939662e-02       21           8
## 50        member  5.336312 2.088562e-02     1083         950
```
