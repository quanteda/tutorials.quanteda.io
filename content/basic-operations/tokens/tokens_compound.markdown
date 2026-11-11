---
title: Compound tokens
weight: 40
draft: false
---

Many terms that matter in social scientific research are not single words but fixed phrases, such as "asylum seeker" or "climate change". If you tokenise text, by default `tokens()` splits these phrases into their separate parts, so "asylum" and "seeker" become two unrelated tokens rather than one meaningful unit.


``` r
library(quanteda)
library(quanteda.textstats)
```




``` r
toks <- tokens(data_char_ukimmig2010)
```

We can first check how often such phrases occur using `kwic()`, exactly as in the [previous chapter](/basic-operations/tokens/kwic).


``` r
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
##      [Labour, 338:339]        economy and the values of | British citizenship | , and step up our
```

Most later analyses, including a document-feature matrix, only count individual tokens and have no way of knowing that two adjacent words belong together. To preserve multi-word expressions like these in that kind of "bag-of-words" analysis, you need to glue them together into a single token first, using `tokens_compound()`. As with `kwic()` in the [previous chapter](/basic-operations/tokens/kwic), `tokens_compound()` only recognises a pattern as a multi-word phrase if you wrap it in `phrase()`. Without it, "asylum seeker" is treated as one long, literal pattern that never matches anything, so the function silently compounds nothing, rather than raising an error.


``` r
# without phrase(), nothing gets compounded, silently
toks_nophrase <- tokens_compound(toks, pattern = "asylum seeker")
identical(toks, toks_nophrase)
```

```
## [1] TRUE
```

Wrapping the same pattern in `phrase()` tells `tokens_compound()` that "asylum seeker" is two consecutive words to search for, not one unmatchable string, so it can find and glue together the correct sequence.


``` r
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
##       [Labour, 338]        economy and the values of | British_citizenship | , and step up our
```

Notice that "asylum seeker" and "British citizen" now appear in the output joined by an underscore, as `asylum_seeker` and `british_citizen`. From this point on, **quanteda** treats each of these as a single token, so a document-feature matrix built from `toks_comp` would count "asylum_seeker" as one feature rather than counting "asylum" and "seeker" separately.

{{% notice tip %}}
You can discover multi-word expressions in your tokens using `textstat_collocations()`. See [Compounding multi-word expressions](../../../advanced-operations/compound-mutiword-expressions/) to learn how to do it.
{{% /notice %}}



