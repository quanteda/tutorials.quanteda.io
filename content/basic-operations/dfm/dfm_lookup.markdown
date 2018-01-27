---
title: Look up dictionary
weight: 30
draft: false
---


```r
require(quanteda)
```

`laver-garry.cat` is a Wordstat dictionary that contain left-right political ideology keywords (Laver and Garry 2000). 


```r
lg_dict <- dictionary(file = "content/dictionary/laver-garry.cat")
```



`dfm_lookup()` translates dictionary values to keys in a DFM.


```r
ie_toks <- tokens(data_corpus_irishbudget2010, remove_punct = TRUE)
ie_dfm <- dfm(ie_toks)
lg_dfm <- dfm_lookup(ie_dfm, lg_dict)
```

You can also pass a dictionary to `dfm()` to simplify your code. Therefore, the code above and below are equivalent.


```r
lsd_dfm <- dfm(data_corpus_irishbudget2010, dictionary = lg_dict, remove_punct = TRUE)
```

{{% notice note %}}
`dfm_lookup()` cannot find multi-word expressions since a document-feature matrix does not store information about positions of words. Yet, `dfm()` can handle multi-word expressions because dictionary lookup is performed internally on tokens using `tokens_lookup()`.
{{% /notice %}}


```r
topfeatures(lg_dfm)
```

```
##                ECONOMY.=STATE=                ECONOMY.+STATE+ 
##                           1963                            803 
##                ECONOMY.-STATE-           INSTITUTIONS.NEUTRAL 
##                            462                            405 
##                        CULTURE            VALUES.CONSERVATIVE 
##                            266                            123 
##           INSTITUTIONS.RADICAL LAW_AND_ORDER.LAW-CONSERVATIVE 
##                            117                            111 
##    ENVIRONMENT.PRO ENVIRONMENT      INSTITUTIONS.CONSERVATIVE 
##                             89                             84
```

