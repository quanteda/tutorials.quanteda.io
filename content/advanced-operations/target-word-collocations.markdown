---
title: Identify related words of keywords
weight: 40
draft: false
---

Rather than searching for fixed phrases, sometimes you want to discover which words tend to cluster around a particular keyword, without knowing in advance what those words might be. We can identify such related words based on their distance from a keyword in the documents. In this example, we build a list of words related to the European Union by comparing how often words occur near mentions of the EU against how often the same words occur everywhere else.


``` r
library(quanteda)
library(quanteda.textstats)
```

The corpus contains 6,000 Guardian news articles from 2012 to 2016.


``` r
corp_news <- download("data_corpus_guardian")
```




``` r
toks_news <- tokens(corp_news, remove_punct = TRUE)
```

We split the corpus into two halves: words that occur close to our keywords, and words that do not. We do this by building two tokens objects: one keeps only the words within a 10-word window of the keywords (`eu`), and one removes that same window. Both use `tokens_keep()` and `tokens_remove()`, exactly as in the [Basic Operations chapter](/basic-operations/tokens/tokens_select).


``` r
eu <- c("EU", "europ*", "european union")
toks_inside <- tokens_keep(toks_news, pattern = eu, window = 10)
toks_inside <- tokens_remove(toks_inside, pattern = eu) # remove the keywords
toks_outside <- tokens_remove(toks_news, pattern = eu, window = 10)
```

We can now compute each word's association with the keywords using `textstat_keyness()`, a function covered in more detail in the [Statistical Analysis chapter](/statistical-analysis/). `textstat_keyness()` compares how often a word occurs in one set of documents (here, the contexts inside our window) against another (the contexts outside it), and returns a score for how distinctively associated each word is with the target set.


``` r
dfmat_inside <- dfm(toks_inside)
dfmat_outside <- dfm(toks_outside)

tstat_key_inside <- textstat_keyness(rbind(dfmat_inside, dfmat_outside),
                                     target = seq_len(ndoc(dfmat_inside)))
head(tstat_key_inside, 50)
```

```
##          feature      chi2 p n_target n_reference
## 1          union 4279.6722 0      416         805
## 2     referendum 3819.2927 0      365         691
## 3     membership 3631.9881 0      216         197
## 4        britain 3060.9045 0      455        1435
## 5          leave 1699.8642 0      320        1274
## 6       migrants 1605.8573 0      157         306
## 7             uk 1587.1436 0      603        4272
## 8     commission 1328.7408 0      224         802
## 9      britain's 1316.4365 0      205         678
## 10       juncker 1312.2652 0       71          52
## 11       leaders 1291.7785 0      254        1053
## 12       eastern 1229.1425 0      111         195
## 13   jean-claude 1215.7701 0       50          17
## 14        summit 1095.1844 0      146         410
## 15      brussels 1056.4994 0      166         554
## 16        schulz  914.1570 0       43          22
## 17     countries  847.0449 0      276        1746
## 18       markets  748.0280 0      175         847
## 19       in-work  733.0960 0       44          39
## 20          tusk  698.1110 0       61         100
## 21           the  669.6816 0    10314      272480
## 22     migration  663.0141 0       84         223
## 23        greece  657.0010 0      130         542
## 24   @openeurope  644.4474 0       20           0
## 25        brexit  592.8974 0      137         656
## 26 renegotiation  570.0703 0       35          32
## 27       leaving  562.7425 0      119         527
## 28       central  562.4720 0      153         840
## 29       cameron  539.3895 0      236        1843
## 30      refugees  516.7987 0      141         776
## 31      schengen  490.8114 0       39          55
## 32  negotiations  488.9191 0      104         463
## 33      benefits  476.0669 0      132         736
## 34        turkey  469.8321 0       95         404
## 35          uk's  441.8822 0      105         515
## 36       staying  434.0229 0       50         116
## 37      reformed  384.8886 0       24          22
## 38          vote  380.9989 0      232        2232
## 39          exit  375.2831 0       59         197
## 40       migrant  374.7056 0       48         129
## 41        crisis  374.2713 0      165        1295
## 42  @plpermrepeu  373.1686 0       12           0
## 43     cameron's  368.2226 0       71         289
## 44        treaty  346.9279 0       46         125
## 45        member  345.7662 0      133         950
## 46    parliament  334.2194 0      152        1218
## 47    luxembourg  334.1223 0       28          42
## 48        remain  333.2430 0      133         975
## 49       barroso  330.9597 0       15           6
## 50         brake  328.6164 0       39          93
```

The words at the top of this table occur disproportionately often near mentions of the EU, compared with the rest of the corpus. That makes them good candidates for the vocabulary of EU-related news coverage.
