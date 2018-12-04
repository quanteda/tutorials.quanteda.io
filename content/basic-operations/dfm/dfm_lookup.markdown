---
title: Look up dictionary
weight: 30
draft: false
---


```r
require(quanteda)
```

[laver-garry.cat](https://raw.githubusercontent.com/quanteda/tutorials.quanteda.io/master/content/dictionary/laver-garry.cat) is a Wordstat dictionary that contain political left-right ideology keywords (Laver and Garry 2000). 


```r
lg_dict <- dictionary(file = "content/dictionary/laver-garry.cat")
```



`dfm_lookup()` translates dictionary values to keys in a DFM.


```r
irish_toks <- tokens(data_corpus_irishbudget2010, remove_punct = TRUE)
irish_dfm <- dfm(irish_toks)
lg_dfm <- dfm_lookup(irish_dfm, dictionary = lg_dict)
head(lg_dfm)
```

```
## Document-feature matrix of: 6 documents, 20 features (30.0% sparse).
## 6 x 20 sparse Matrix of class "dfm"
##                                   features
## docs                               CULTURE CULTURE.CULTURE-HIGH
##   2010_BUDGET_01_Brian_Lenihan_FF        8                    1
##   2010_BUDGET_02_Richard_Bruton_FG      35                    0
##   2010_BUDGET_03_Joan_Burton_LAB        31                    1
##   2010_BUDGET_04_Arthur_Morgan_SF       53                    0
##   2010_BUDGET_05_Brian_Cowen_FF         15                    1
##   2010_BUDGET_06_Enda_Kenny_FG          25                    1
##                                   features
## docs                               CULTURE.CULTURE-POPULAR CULTURE.SPORT
##   2010_BUDGET_01_Brian_Lenihan_FF                        0             0
##   2010_BUDGET_02_Richard_Bruton_FG                       0             0
##   2010_BUDGET_03_Joan_Burton_LAB                         1             0
##   2010_BUDGET_04_Arthur_Morgan_SF                        3             0
##   2010_BUDGET_05_Brian_Cowen_FF                          0             0
##   2010_BUDGET_06_Enda_Kenny_FG                           0             0
##                                   features
## docs                               ECONOMY.+STATE+ ECONOMY.=STATE=
##   2010_BUDGET_01_Brian_Lenihan_FF              115             355
##   2010_BUDGET_02_Richard_Bruton_FG              35             131
##   2010_BUDGET_03_Joan_Burton_LAB               134             206
##   2010_BUDGET_04_Arthur_Morgan_SF               92             286
##   2010_BUDGET_05_Brian_Cowen_FF                 92             244
##   2010_BUDGET_06_Enda_Kenny_FG                  46             138
##                                   features
## docs                               ECONOMY.-STATE-
##   2010_BUDGET_01_Brian_Lenihan_FF              113
##   2010_BUDGET_02_Richard_Bruton_FG              35
##   2010_BUDGET_03_Joan_Burton_LAB                61
##   2010_BUDGET_04_Arthur_Morgan_SF               49
##   2010_BUDGET_05_Brian_Cowen_FF                 81
##   2010_BUDGET_06_Enda_Kenny_FG                  27
##                                   features
## docs                               ENVIRONMENT.CON ENVIRONMENT
##   2010_BUDGET_01_Brian_Lenihan_FF                            4
##   2010_BUDGET_02_Richard_Bruton_FG                           3
##   2010_BUDGET_03_Joan_Burton_LAB                             0
##   2010_BUDGET_04_Arthur_Morgan_SF                            1
##   2010_BUDGET_05_Brian_Cowen_FF                              7
##   2010_BUDGET_06_Enda_Kenny_FG                               2
##                                   features
## docs                               ENVIRONMENT.PRO ENVIRONMENT
##   2010_BUDGET_01_Brian_Lenihan_FF                           17
##   2010_BUDGET_02_Richard_Bruton_FG                           2
##   2010_BUDGET_03_Joan_Burton_LAB                             6
##   2010_BUDGET_04_Arthur_Morgan_SF                            9
##   2010_BUDGET_05_Brian_Cowen_FF                             17
##   2010_BUDGET_06_Enda_Kenny_FG                               6
##                                   features
## docs                               GROUPS.ETHNIC GROUPS.WOMEN
##   2010_BUDGET_01_Brian_Lenihan_FF              0            0
##   2010_BUDGET_02_Richard_Bruton_FG             0            0
##   2010_BUDGET_03_Joan_Burton_LAB               0            3
##   2010_BUDGET_04_Arthur_Morgan_SF              0            0
##   2010_BUDGET_05_Brian_Cowen_FF                0            0
##   2010_BUDGET_06_Enda_Kenny_FG                 0            1
##                                   features
## docs                               INSTITUTIONS.CONSERVATIVE
##   2010_BUDGET_01_Brian_Lenihan_FF                         13
##   2010_BUDGET_02_Richard_Bruton_FG                         6
##   2010_BUDGET_03_Joan_Burton_LAB                           5
##   2010_BUDGET_04_Arthur_Morgan_SF                          6
##   2010_BUDGET_05_Brian_Cowen_FF                           19
##   2010_BUDGET_06_Enda_Kenny_FG                            10
##                                   features
## docs                               INSTITUTIONS.NEUTRAL
##   2010_BUDGET_01_Brian_Lenihan_FF                    63
##   2010_BUDGET_02_Richard_Bruton_FG                   63
##   2010_BUDGET_03_Joan_Burton_LAB                     68
##   2010_BUDGET_04_Arthur_Morgan_SF                    48
##   2010_BUDGET_05_Brian_Cowen_FF                      34
##   2010_BUDGET_06_Enda_Kenny_FG                       34
##                                   features
## docs                               INSTITUTIONS.RADICAL
##   2010_BUDGET_01_Brian_Lenihan_FF                    17
##   2010_BUDGET_02_Richard_Bruton_FG                   26
##   2010_BUDGET_03_Joan_Burton_LAB                     11
##   2010_BUDGET_04_Arthur_Morgan_SF                     9
##   2010_BUDGET_05_Brian_Cowen_FF                      10
##   2010_BUDGET_06_Enda_Kenny_FG                        9
##                                   features
## docs                               LAW_AND_ORDER.LAW-CONSERVATIVE
##   2010_BUDGET_01_Brian_Lenihan_FF                              11
##   2010_BUDGET_02_Richard_Bruton_FG                             14
##   2010_BUDGET_03_Joan_Burton_LAB                                6
##   2010_BUDGET_04_Arthur_Morgan_SF                              22
##   2010_BUDGET_05_Brian_Cowen_FF                                 4
##   2010_BUDGET_06_Enda_Kenny_FG                                 18
##                                   features
## docs                               LAW_AND_ORDER.LAW-LIBERAL RURAL URBAN
##   2010_BUDGET_01_Brian_Lenihan_FF                          0     9     0
##   2010_BUDGET_02_Richard_Bruton_FG                         0     0     0
##   2010_BUDGET_03_Joan_Burton_LAB                           0     2     3
##   2010_BUDGET_04_Arthur_Morgan_SF                          0     2     1
##   2010_BUDGET_05_Brian_Cowen_FF                            0     8     1
##   2010_BUDGET_06_Enda_Kenny_FG                             0     0     2
##                                   features
## docs                               VALUES.CONSERVATIVE VALUES.LIBERAL
##   2010_BUDGET_01_Brian_Lenihan_FF                   19              0
##   2010_BUDGET_02_Richard_Bruton_FG                  14              0
##   2010_BUDGET_03_Joan_Burton_LAB                     5              1
##   2010_BUDGET_04_Arthur_Morgan_SF                   16              2
##   2010_BUDGET_05_Brian_Cowen_FF                     13              0
##   2010_BUDGET_06_Enda_Kenny_FG                       7              1
```

You can also pass a dictionary to `dfm()` to simplify your code. Therefore, the code above and below are equivalent.


```r
lg_dfm <- dfm(data_corpus_irishbudget2010, dictionary = lg_dict, remove_punct = TRUE)
```

{{% notice note %}}
`dfm_lookup()` cannot find multi-word expressions since a document-feature matrix does not store information about positions of words. Yet, `dfm()` can handle multi-word expressions because dictionary lookup is performed internally on tokens using `tokens_lookup()`. Afterwards, you can create a document-feature matrix from this object.
{{% /notice %}}

