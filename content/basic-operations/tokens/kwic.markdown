---
title: Keyword-in-contexts
weight: 20
draft: false
---


```r
require(quanteda)
```


```r
toks <- tokens(data_char_ukimmig2010)
```

You can see how keywords are in the actual contexts using `kwic()`. 


```r
immig_kw <- kwic(toks, 'immig*')
head(immig_kw, 10)
```

```
##                                                                  
##    [BNP, 1]                                       | IMMIGRATION |
##   [BNP, 16]                    SOLVE.- At current | immigration |
##   [BNP, 78]                 a halt to all further | immigration |
##   [BNP, 85]        the deportation of all illegal | immigrants  |
##  [BNP, 169]          Britain, regardless of their | immigration |
##  [BNP, 197] admission that they orchestrated mass | immigration |
##  [BNP, 272]            grave peril, threatened by | immigration |
##  [BNP, 374]                  ), legal Third World | immigrants  |
##  [BNP, 531]        to second and third generation |  immigrant  |
##  [BNP, 661]                     are added in, the |  immigrant  |
##                                           
##  : AN UNPARALLELED CRISIS WHICH           
##  and birth rates, indigenous              
##  , the deportation of all                 
##  , a halt to the                          
##  status.- The BNP                         
##  to change forcibly Britain's demographics
##  and multiculturalism. In the             
##  made up 14.7 percent(                    
##  mothers. Figures released by             
##  birth rate is estimated to
```

`kwic()` also takes multiple keywords in a character vector.


```r
immig2_kw <- kwic(toks, c('immig*', 'migra*'))
head(immig2_kw, 10)
```

```
##                                                                  
##    [BNP, 1]                                       | IMMIGRATION |
##   [BNP, 16]                    SOLVE.- At current | immigration |
##   [BNP, 78]                 a halt to all further | immigration |
##   [BNP, 85]        the deportation of all illegal | immigrants  |
##  [BNP, 169]          Britain, regardless of their | immigration |
##  [BNP, 197] admission that they orchestrated mass | immigration |
##  [BNP, 272]            grave peril, threatened by | immigration |
##  [BNP, 374]                  ), legal Third World | immigrants  |
##  [BNP, 531]        to second and third generation |  immigrant  |
##  [BNP, 661]                     are added in, the |  immigrant  |
##                                           
##  : AN UNPARALLELED CRISIS WHICH           
##  and birth rates, indigenous              
##  , the deportation of all                 
##  , a halt to the                          
##  status.- The BNP                         
##  to change forcibly Britain's demographics
##  and multiculturalism. In the             
##  made up 14.7 percent(                    
##  mothers. Figures released by             
##  birth rate is estimated to
```

If you want to find multi-word expressions, separate words by whitespace and wrap the character vector by `phrase()`.


```r
asylum_kw <- kwic(toks, phrase('asylum seeker*'))
head(asylum_kw)
```

```
##                                                                      
##  [BNP, 1958:1959] all illegal immigrants and bogus | asylum seekers |
##  [BNP, 2159:2160]            region concerned. An' | asylum seeker  |
##  [BNP, 2192:2193]          country. Because every' | asylum seeker  |
##  [BNP, 2218:2219]     there are currently no legal | asylum seekers |
##  [BNP, 2265:2266]  of illegal immigrants and bogus | asylum seekers |
##  [BNP, 2296:2297]  benefits system for these bogus | asylum seekers |
##                               
##  , including their dependents.
##  ' who has crossed dozens     
##  ' in Britain has crossed     
##  in Britain today. It         
##  , that there are no          
##  is removed, the flood
```

Texts does do not alwasy do not appear nicely in R console, so you can use `View()` to see in an interactive HTML table.


```r
View(immig2_kw)
```

