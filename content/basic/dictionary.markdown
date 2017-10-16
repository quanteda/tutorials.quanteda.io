---
title: Dictionary analysis
weight: 50
chapter: true
draft: true
---



# Dictionary Analysis of Texts

## Applying equivalency classes: dictionaries, thesaruses

Dictionary creation is done through the `dictionary()` function, which classes a named list of characters as a dictionary.

```r
# import the Laver-Garry dictionary from http://bit.ly/1FH2nvf
lgdict <- dictionary(file = "LaverGarry.cat")
budgdfm <- dfm(data_corpus_irishbudget2010, dictionary = lgdict, verbose = TRUE)
## Creating a dfm from a corpus input...
##    ... lowercasing
##    ... found 14 documents, 5,140 features
##    ... applying a dictionary consisting of 20 keys
##    ... created a 14 x 20 sparse dfm
##    ... complete. 
## Elapsed time: 0.37 seconds.
head(budgdfm)
## Document-feature matrix of: 6 documents, 6 features (33.3% sparse).
## 6 x 6 sparse Matrix of class "dfm"
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

# import a LIWC formatted dictionary
# liwcdict <- dictionary(file = "~/Dropbox/QUANTESS/dictionaries/LIWC/LIWC2015_English_Flat.dic")
# dictdfm <- dfm(data_corpus_inaugural, dictionary = liwcdict, verbose = TRUE)
# dictdfm[50:58, c("money", "power")]
```

We apply dictionaries to a dfm using the `dfm_lookup()` function.  Through the `valuetype`, argument, we can match patterns of one of three types: `"glob"`, `"regex"`, or `"fixed"`.

```r
myDict <- dictionary(list(christmas = c("Christmas", "Santa", "holiday"),
                          opposition = c("Opposition", "reject", "notincorpus"),
                          taxglob = "tax*",
                          taxregex = "tax.+$",
                          country = c("United_States", "Sweden")))
myDfm <- dfm(c("My Christmas was ruined by your opposition tax plan.",
               "Does the United_States or Sweden have more progressive taxation?"),
             remove = stopwords("english"), verbose = FALSE)
myDfm
## Document-feature matrix of: 2 documents, 11 features (50% sparse).
## 2 x 11 sparse Matrix of class "dfm"
##        features
## docs    christmas ruined opposition tax plan . united_states sweden
##   text1         1      1          1   1    1 1             0      0
##   text2         0      0          0   0    0 0             1      1
##        features
## docs    progressive taxation ?
##   text1           0        0 0
##   text2           1        1 1

# glob format
dfm_lookup(myDfm, myDict, valuetype = "glob")
## Document-feature matrix of: 2 documents, 5 features (50% sparse).
## 2 x 5 sparse Matrix of class "dfm"
##        features
## docs    christmas opposition taxglob taxregex country
##   text1         1          1       1        0       0
##   text2         0          0       1        0       2
dfm_lookup(myDfm, myDict, valuetype = "glob", case_insensitive = FALSE)
## Document-feature matrix of: 2 documents, 5 features (50% sparse).
## 2 x 5 sparse Matrix of class "dfm"
##        features
## docs    christmas opposition taxglob taxregex country
##   text1         1          1       1        0       0
##   text2         0          0       1        0       2

# regex v. glob format: note that "united_states" is a regex match for "tax*"
dfm_lookup(myDfm, myDict, valuetype = "glob")
## Document-feature matrix of: 2 documents, 5 features (50% sparse).
## 2 x 5 sparse Matrix of class "dfm"
##        features
## docs    christmas opposition taxglob taxregex country
##   text1         1          1       1        0       0
##   text2         0          0       1        0       2
dfm_lookup(myDfm, myDict, valuetype = "regex", case_insensitive = TRUE)
## Document-feature matrix of: 2 documents, 5 features (40% sparse).
## 2 x 5 sparse Matrix of class "dfm"
##        features
## docs    christmas opposition taxglob taxregex country
##   text1         1          1       1        0       0
##   text2         0          0       2        1       2

# fixed format: no pattern matching
dfm_lookup(myDfm, myDict, valuetype = "fixed")
## Document-feature matrix of: 2 documents, 5 features (70% sparse).
## 2 x 5 sparse Matrix of class "dfm"
##        features
## docs    christmas opposition taxglob taxregex country
##   text1         1          1       0        0       0
##   text2         0          0       0        0       2
dfm_lookup(myDfm, myDict, valuetype = "fixed", case_insensitive = FALSE)
## Document-feature matrix of: 2 documents, 5 features (70% sparse).
## 2 x 5 sparse Matrix of class "dfm"
##        features
## docs    christmas opposition taxglob taxregex country
##   text1         1          1       0        0       0
##   text2         0          0       0        0       2
```

It is also possible to pass through a dictionary at the time of `dfm()` creation.

```r
# dfm with dictionaries
mycorpus <- corpus_subset(data_corpus_inaugural, Year > 1900)
mydict <- dictionary(list(christmas = c("Christmas", "Santa", "holiday"),
                          opposition = c("Opposition", "reject", "notincorpus"),
                          taxing = "taxing",
                          taxation = "taxation",
                          taxregex = "tax*",
                          country = "united states"))
dictDfm <- dfm(mycorpus, dictionary = mydict)
head(dictDfm)
## Document-feature matrix of: 6 documents, 6 features (61.1% sparse).
## 6 x 6 sparse Matrix of class "dfm"
##                 features
## docs             christmas opposition taxing taxation taxregex country
##   1901-McKinley          0          2      0        1        1       9
##   1905-Roosevelt         0          0      0        0        0       0
##   1909-Taft              0          1      0        4        6       6
##   1913-Wilson            0          0      0        1        1       0
##   1917-Wilson            0          0      0        0        0       1
##   1921-Harding           0          0      0        1        2       1
```

Finally, there is a related "thesaurus" feature, which collapses words in a dictionary but is not exclusive.

```r
mytexts <- c("British English tokenises differently, with more colour.",
             "American English tokenizes color as one word.")
mydict <- dictionary(list(color = "colo*r", tokenize = "tokeni?e*"))
dfm(mytexts, thesaurus = mydict)
## Document-feature matrix of: 2 documents, 13 features (34.6% sparse).
## 2 x 13 sparse Matrix of class "dfm"
##        features
## docs    COLOR TOKENIZE british english differently , with more . american
##   text1     1        1       1       1           1 1    1    1 1        0
##   text2     1        1       0       1           0 0    0    0 1        1
##        features
## docs    as one word
##   text1  0   0    0
##   text2  1   1    1
```
