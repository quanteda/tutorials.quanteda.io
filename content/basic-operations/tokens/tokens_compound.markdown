---
title: Compound tokens
weight: 40
draft: false
---


```r
require(quanteda)
```


```r
toks <- tokens(data_char_ukimmig2010)
```

There are several multi-word expressions that are important in social scientific reserach.


```r
kw_multiword <- kwic(toks, pattern = phrase(c('asylum seeker*', 'british citizen*')))
head(kw_multiword, 10)
```

```
##                                                                                
##       [BNP, 1724:1725]        the honour and benefit of | British citizenship |
##       [BNP, 1958:1959] all illegal immigrants and bogus |   asylum seekers    |
##       [BNP, 2159:2160]            region concerned. An' |    asylum seeker    |
##       [BNP, 2192:2193]          country. Because every' |    asylum seeker    |
##       [BNP, 2218:2219]     there are currently no legal |   asylum seekers    |
##       [BNP, 2265:2266]  of illegal immigrants and bogus |   asylum seekers    |
##       [BNP, 2296:2297]  benefits system for these bogus |   asylum seekers    |
##  [Conservative, 68:69]          could be carried out by |  British citizens   |
##        [Greens, 77:78]      immigration: over 5 million |  British Citizens   |
##      [Labour, 337:338]        economy and the values of | British citizenship |
##                               
##  has gone to people who       
##  , including their dependents.
##  ' who has crossed dozens     
##  ' in Britain has crossed     
##  in Britain today. It         
##  , that there are no          
##  is removed, the flood        
##  , given the right training   
##  benefit from other countries'
##  , and step up our
```

To preserve these expressions in bag-of-word analysis, you have to compound them using `tokens_compound()`.


```r
toks_comp <- tokens_compound(toks, pattern = phrase(c('asylum seeker*', 'british citizen*')))
kw_comp <- kwic(toks_comp, pattern = c('asylum_seeker*', 'british_citizen*'))
head(kw_comp, 10)
```

```
##                                                                             
##         [BNP, 1724]        the honour and benefit of | British_citizenship |
##         [BNP, 1957] all illegal immigrants and bogus |   asylum_seekers    |
##         [BNP, 2157]            region concerned. An' |    asylum_seeker    |
##         [BNP, 2189]          country. Because every' |    asylum_seeker    |
##         [BNP, 2214]     there are currently no legal |   asylum_seekers    |
##         [BNP, 2260]  of illegal immigrants and bogus |   asylum_seekers    |
##         [BNP, 2290]  benefits system for these bogus |   asylum_seekers    |
##  [Conservative, 68]          could be carried out by |  British_citizens   |
##        [Greens, 77]      immigration: over 5 million |  British_Citizens   |
##       [Labour, 337]        economy and the values of | British_citizenship |
##                               
##  has gone to people who       
##  , including their dependents.
##  ' who has crossed dozens     
##  ' in Britain has crossed     
##  in Britain today. It         
##  , that there are no          
##  is removed, the flood        
##  , given the right training   
##  benefit from other countries'
##  , and step up our
```

{{% notice tip %}}
You can discover muti-words expressions in your tokens using `textstat_collocations()`. See [Compunding multi-word expressions](../../../advanced-operations/compound-mutiword-expressions/) to learn how to do it.
{{% /notice %}}



