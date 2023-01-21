---
title: Identify related words of keywords
weight: 40
draft: false
---

We can identify related words of keywords based on their distance in the documents. In this example, we created a list of words related to the European Union by comparing frequency of words inside and outside of their contexts.


```r
require(quanteda)
require(quanteda.textstats)
```

This corpus contains 6,000 Guardian news articles from 2012 to 2016.


```r
corp_news <- download("data_corpus_guardian")
```




```r
toks_news <- tokens(corp_news, remove_punct = TRUE)
```

We will select two tokens objects for words inside and outside of the 10-word windows of the keywords (`eu`). 


```r
eu <- c("EU", "europ*", "european union")
toks_inside <- tokens_keep(toks_news, pattern = eu, window = 10)
toks_inside <- tokens_remove(toks_inside, pattern = eu) # remove the keywords
toks_outside <- tokens_remove(toks_news, pattern = eu, window = 10)
```

We can compute words' association with the keywords using `textstat_keyness()`.


```r
dfmat_inside <- dfm(toks_inside)
dfmat_outside <- dfm(toks_outside)

tstat_key_inside <- textstat_keyness(rbind(dfmat_inside, dfmat_outside), 
                                     target = seq_len(ndoc(dfmat_inside)))
head(tstat_key_inside, 50)
```

```
##          feature      chi2 p n_target n_reference
## 1          union 4279.9478 0      416         805
## 2     referendum 3819.5381 0      365         691
## 3     membership 3632.2083 0      216         197
## 4        britain 3061.1165 0      455        1435
## 5          leave 1699.9878 0      320        1274
## 6       migrants 1605.9608 0      157         306
## 7             uk 1587.8043 0      603        4271
## 8     commission 1328.8352 0      224         802
## 9      britain's 1316.5285 0      205         678
## 10       juncker 1312.3439 0       71          52
## 11       leaders 1291.8734 0      254        1053
## 12       eastern 1229.2208 0      111         195
## 13   jean-claude 1215.8414 0       50          17
## 14        summit 1095.2587 0      146         410
## 15      brussels 1056.5733 0      166         554
## 16        schulz  914.2112 0       43          22
## 17     countries  847.1159 0      276        1746
## 18       markets  748.0853 0      175         847
## 19       in-work  733.1404 0       44          39
## 20          tusk  698.1552 0       61         100
## 21           the  671.2398 0    10316      272476
## 22     migration  663.0587 0       84         223
## 23        greece  658.3684 0      130         541
## 24   @openeurope  644.4845 0       20           0
## 25        brexit  570.2952 0      134         651
## 26 renegotiation  570.1049 0       35          32
## 27       leaving  562.7845 0      119         527
## 28       central  562.5168 0      153         840
## 29       cameron  539.4390 0      236        1843
## 30      refugees  516.8398 0      141         776
## 31      schengen  490.8421 0       39          55
## 32  negotiations  488.9557 0      104         463
## 33      benefits  476.1050 0      132         736
## 34        turkey  469.8668 0       95         404
## 35          uk's  441.9161 0      105         515
## 36       staying  434.0516 0       50         116
## 37      reformed  384.9120 0       24          22
## 38          vote  381.0381 0      232        2232
## 39          exit  375.3093 0       59         197
## 40       migrant  374.7308 0       48         129
## 41        crisis  374.3057 0      165        1295
## 42  @plpermrepeu  373.1901 0       12           0
## 43     cameron's  368.2495 0       71         289
## 44        treaty  346.9514 0       46         125
## 45        member  345.7966 0      133         950
## 46    parliament  334.2505 0      152        1218
## 47    luxembourg  334.1433 0       28          42
## 48        remain  333.2727 0      133         975
## 49       barroso  330.9792 0       15           6
## 50         brake  328.6382 0       39          93
```
