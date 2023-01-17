---
title: Keyword-in-contexts
weight: 20
draft: false
---


```r
require(quanteda)
options(width = 110)
```


```r
toks <- tokens(data_char_ukimmig2010)
```

You can see how keywords are used in the actual contexts in a concordance view produced by `kwic()`. 


```r
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

`kwic()` also takes multiple keywords in a character vector.


```r
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

With the `window` argument, you can specify the number of words to be displayed around the keyword.


```r
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

If you want to find multi-word expressions, separate words by white space and wrap the character vector by `phrase()`.


```r
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

Texts do not always appear nicely in your R console, so you can use `View()` to see the keywords-in-context in an interactive HTML table.


```r
View(kw_asylum)
```

