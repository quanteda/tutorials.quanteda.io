---
title: Keyword-in-contexts
weight: 20
draft: false
---

Before counting words or fitting a model, it often helps to read how a word is actually used in context. We call this close reading a "keyword-in-context", or KWIC, analysis, one of the most useful sanity checks in text analysis: it confirms that a word means what you assume it means in your texts.


``` r
library(quanteda)
options(width = 110)
```


``` r
toks <- tokens(data_char_ukimmig2010)
```

`kwic()` searches a tokens object for a pattern and returns every occurrence, together with the words immediately before and after it, in a table you can scan by eye.


``` r
kw_immig <- kwic(toks, pattern =  "immig*")
head(kw_immig, 10)
```

```
## Keyword-in-context with 10 matches.                                                                                                           
##    [BNP, 1]                                       | IMMIGRATION | : AN UNPARALLELED CRISIS WHICH           
##   [BNP, 16]                   SOLVE. - At current | immigration | and birth rates, indigenous              
##   [BNP, 78]                 a halt to all further | immigration | , the deportation of all                 
##   [BNP, 85]        the deportation of all illegal | immigrants  | , a halt to the                          
##  [BNP, 169]          Britain, regardless of their | immigration | status. - The BNP                        
##  [BNP, 197] admission that they orchestrated mass | immigration | to change forcibly Britain's demographics
##  [BNP, 272]            grave peril, threatened by | immigration | and multiculturalism. In the             
##  [BNP, 374]                  ), legal Third World | immigrants  | made up 14.7 percent (                   
##  [BNP, 531]        to second and third generation |  immigrant  | mothers. Figures released by             
##  [BNP, 661]                     are added in, the |  immigrant  | birth rate is estimated to
```

The asterisk in `"immig*"` is a wildcard, so this single pattern matches "immigration", "immigrant" and any other word beginning with "immig". Each row shows the matched keyword in the `keyword` column, flanked by its surrounding words in the `pre` and `post` columns, along with which document it came from.

`kwic()` also takes multiple keywords in a character vector, so you can search for related terms in a single call.


``` r
kw_immig2 <- kwic(toks, pattern = c("immig*", "migra*"))
head(kw_immig2, 10)
```

```
## Keyword-in-context with 10 matches.                                                                                                           
##    [BNP, 1]                                       | IMMIGRATION | : AN UNPARALLELED CRISIS WHICH           
##   [BNP, 16]                   SOLVE. - At current | immigration | and birth rates, indigenous              
##   [BNP, 78]                 a halt to all further | immigration | , the deportation of all                 
##   [BNP, 85]        the deportation of all illegal | immigrants  | , a halt to the                          
##  [BNP, 169]          Britain, regardless of their | immigration | status. - The BNP                        
##  [BNP, 197] admission that they orchestrated mass | immigration | to change forcibly Britain's demographics
##  [BNP, 272]            grave peril, threatened by | immigration | and multiculturalism. In the             
##  [BNP, 374]                  ), legal Third World | immigrants  | made up 14.7 percent (                   
##  [BNP, 531]        to second and third generation |  immigrant  | mothers. Figures released by             
##  [BNP, 661]                     are added in, the |  immigrant  | birth rate is estimated to
```

With the `window` argument, you can specify how many words of context to display on either side of the keyword. The default is five; here we widen it to seven, which is useful when five words is not enough to judge how a term is being used.


``` r
kw_immig3 <- kwic(toks, pattern = c("immig*", "migra*"), window = 7)
head(kw_immig3, 10)
```

```
## Keyword-in-context with 10 matches.                                                                              
##    [BNP, 1]                                                    | IMMIGRATION |
##   [BNP, 16]                        BNP CAN SOLVE. - At current | immigration |
##   [BNP, 78]                 will include a halt to all further | immigration |
##   [BNP, 85]        immigration, the deportation of all illegal | immigrants  |
##  [BNP, 169]             crimes in Britain, regardless of their | immigration |
##  [BNP, 197] that party's admission that they orchestrated mass | immigration |
##  [BNP, 272]                   is in grave peril, threatened by | immigration |
##  [BNP, 374]                         ( ONS ), legal Third World | immigrants  |
##  [BNP, 531]      include births to second and third generation |  immigrant  |
##  [BNP, 661]                    these figures are added in, the |  immigrant  |
##                                                  
##  : AN UNPARALLELED CRISIS WHICH ONLY THE         
##  and birth rates, indigenous British people      
##  , the deportation of all illegal immigrants     
##  , a halt to the" asylum                         
##  status. - The BNP will review                   
##  to change forcibly Britain's demographics and to
##  and multiculturalism. In the absence of         
##  made up 14.7 percent ( 7.5 million              
##  mothers. Figures released by the ONS            
##  birth rate is estimated to be around
```

If you want to search for a multi-word expression rather than a single word, separate the words with a space and wrap the character vector in `phrase()`. Without `phrase()`, `kwic()` treats "asylum seeker*" as one long, unmatchable pattern rather than two consecutive words, and reports no matches, with no warning that anything went wrong.


``` r
# without phrase(), "asylum seeker*" is one unmatchable pattern
kwic(toks, pattern = "asylum seeker*")
```

```
## Keyword-in-context with 0 matches.
```

Wrapping the same pattern in `phrase()` tells `kwic()` to treat it as two consecutive words instead of one, so it can find the genuine matches.


``` r
kw_asylum <- kwic(toks, pattern = phrase("asylum seeker*"))
head(kw_asylum)
```

```
## Keyword-in-context with 6 matches.                                                                                                   
##  [BNP, 1958:1959] all illegal immigrants and bogus | asylum seekers | , including their dependents.
##  [BNP, 2159:2160]            region concerned. An' | asylum seeker  | ' who has crossed dozens     
##  [BNP, 2192:2193]          country. Because every' | asylum seeker  | ' in Britain has crossed     
##  [BNP, 2218:2219]     there are currently no legal | asylum seekers | in Britain today. It         
##  [BNP, 2265:2266]  of illegal immigrants and bogus | asylum seekers | , that there are no          
##  [BNP, 2296:2297]  benefits system for these bogus | asylum seekers | is removed, the flood
```

{{% notice warning %}}
Forgetting `phrase()` around a multi-word pattern does not raise an error, in `kwic()` or in any of the other pattern-matching functions you will meet in this tutorial. Instead it quietly matches nothing, or matches something other than what you intended. Whenever a pattern you are searching for contains a space, wrap it in `phrase()`.
{{% /notice %}}

Texts do not always appear nicely in your R console, especially once a table has many rows or wide columns. Use `View()` to open the keywords-in-context in an interactive, scrollable table in your IDE.


``` r
View(kw_asylum)
```

