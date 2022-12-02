---
title: Compound tokens
weight: 40
draft: false
---


```r
require(quanteda)
require(quanteda.textstats)
options(width = 110)
```


```r
toks <- tokens(data_char_ukimmig2010)
```

Various multi-word expressions are important in social scientific research.


```r
kw_multiword <- kwic(toks, pattern = phrase(c("asylum seeker*", "british citizen*")))
head(kw_multiword, 10)
```

```
## Keyword-in-context with 10 matches.                                                                                                             
##       [BNP, 1724:1725]        the honour and benefit of | British citizenship | has gone to people who       
##       [BNP, 1958:1959] all illegal immigrants and bogus |   asylum seekers    | , including their dependents.
##       [BNP, 2159:2160]            region concerned. An' |    asylum seeker    | ' who has crossed dozens     
##       [BNP, 2192:2193]          country. Because every' |    asylum seeker    | ' in Britain has crossed     
##       [BNP, 2218:2219]     there are currently no legal |   asylum seekers    | in Britain today. It         
##       [BNP, 2265:2266]  of illegal immigrants and bogus |   asylum seekers    | , that there are no          
##       [BNP, 2296:2297]  benefits system for these bogus |   asylum seekers    | is removed, the flood        
##  [Conservative, 68:69]          could be carried out by |  British citizens   | , given the right training   
##        [Greens, 77:78]      immigration: over 5 million |  British Citizens   | benefit from other countries'
##      [Labour, 337:338]        economy and the values of | British citizenship | , and step up our
```

To preserve these expressions in a bag-of-word analysis, you have to compound them using `tokens_compound()`.


```r
toks_comp <- tokens_compound(toks, pattern = phrase(c("asylum seeker*", "british citizen*")))
kw_comp <- kwic(toks_comp, pattern = c("asylum_seeker*", "british_citizen*"))
head(kw_comp, 10)
```

```
## Keyword-in-context with 10 matches.                                                                                                          
##         [BNP, 1724]        the honour and benefit of | British_citizenship | has gone to people who       
##         [BNP, 1957] all illegal immigrants and bogus |   asylum_seekers    | , including their dependents.
##         [BNP, 2157]            region concerned. An' |    asylum_seeker    | ' who has crossed dozens     
##         [BNP, 2189]          country. Because every' |    asylum_seeker    | ' in Britain has crossed     
##         [BNP, 2214]     there are currently no legal |   asylum_seekers    | in Britain today. It         
##         [BNP, 2260]  of illegal immigrants and bogus |   asylum_seekers    | , that there are no          
##         [BNP, 2290]  benefits system for these bogus |   asylum_seekers    | is removed, the flood        
##  [Conservative, 68]          could be carried out by |  British_citizens   | , given the right training   
##        [Greens, 77]      immigration: over 5 million |  British_Citizens   | benefit from other countries'
##       [Labour, 337]        economy and the values of | British_citizenship | , and step up our
```

{{% notice tip %}}
You can discover muti-words expressions in your tokens using `textstat_collocations()`. See [Compunding multi-word expressions](../../../advanced-operations/compound-mutiword-expressions/) to learn how to do it.
{{% /notice %}}



