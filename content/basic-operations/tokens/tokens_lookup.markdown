---
title: Look up dictionary
weight: 50
draft: false
---

A dictionary is a predefined list of words grouped into categories, such as a list of country names grouped by continent, or a list of positive and negative words for sentiment analysis. `tokens_lookup()` scans a tokens object for the words in a dictionary and replaces each match with the name of the category it belongs to, which lets you count categories of meaning rather than individual words. It's the most flexible dictionary look-up function in **quanteda**. We use a [geographical dictionary](https://raw.githubusercontent.com/quanteda/tutorials.quanteda.io/master/content/dictionary/newsmap.yml) from the [**newsmap**](https://cran.r-project.org/web/packages/newsmap/index.html) package as an example. Using `dictionary()`, you can import dictionary files in the Wordstat, LIWC, Yoshicoder, Lexicoder and YAML formats, so you are not limited to dictionaries built specifically for R.


``` r
library(quanteda)
```




``` r
toks <- tokens(data_char_ukimmig2010)
```


``` r
dict_newsmap <- dictionary(file = "../../dictionary/newsmap.yml")
```

Note that you can access the dictionary in various languages (currently English, German, Japanese, Russian, and Spanish) with the **newsmap** package.

The geographical dictionary comprises names of countries and cities, and their demonyms (such as "French" for France), arranged in a hierarchical structure where countries are nested within world regions and sub-regions. You can explore this structure the same way you would explore a nested list in R.


``` r
length(dict_newsmap)
```

```
## [1] 5
```

``` r
names(dict_newsmap)
```

```
## [1] "AFRICA"  "AMERICA" "ASIA"    "EUROPE"  "OCEANIA"
```

``` r
names(dict_newsmap[["AFRICA"]])
```

```
## [1] "EAST"   "MIDDLE" "NORTH"  "SOUTH"  "WEST"
```

``` r
dict_newsmap[["AFRICA"]][["NORTH"]]
```

```
## Dictionary object with 8 key entries.
## - [DZ]:
##   - algeria, algerian*, algiers
## - [EG]:
##   - egypt, egyptian*, cairo
## - [LY]:
##   - libya, libyan*, tripoli
## - [MA]:
##   - morocco, moroccan*, rabat
## - [SD]:
##   - sudan, sudanese, khartoum
## - [SS]:
##   - south sudan, s sudan, s sudanese, juba
## [ reached max_nkey ... 2 more keys ]
```

Because the dictionary is hierarchical, you must tell `tokens_lookup()` which level of the hierarchy to use, with the `levels` argument. Level 1 is the broadest, continents, while level 3 is the most specific, individual countries.


``` r
# use level of continents
toks_region <- tokens_lookup(toks, dictionary = dict_newsmap, levels = 1)
print(toks_region)
```

```
## Tokens consisting of 9 documents.
## BNP :
##  [1] "EUROPE" "EUROPE" "EUROPE" "EUROPE" "EUROPE" "EUROPE" "EUROPE" "EUROPE" "EUROPE" "EUROPE" "EUROPE"
## [12] "EUROPE"
## [ ... and 62 more ]
## 
## Coalition :
## [1] "EUROPE"
## 
## Conservative :
## [1] "EUROPE" "EUROPE" "EUROPE" "EUROPE" "EUROPE"
## 
## Greens :
## [1] "EUROPE" "EUROPE"
## 
## Labour :
##  [1] "EUROPE"  "OCEANIA" "OCEANIA" "OCEANIA" "EUROPE"  "OCEANIA" "EUROPE"  "EUROPE"  "EUROPE"  "EUROPE" 
## [11] "EUROPE" 
## 
## LibDem :
## [1] "EUROPE"  "EUROPE"  "EUROPE"  "EUROPE"  "EUROPE"  "EUROPE"  "AMERICA"
## 
## [ reached max_ndoc ... 3 more documents ]
```


``` r
# use level of countries
toks_country <- tokens_lookup(toks, dictionary = dict_newsmap, levels = 3)
print(toks_country)
```

```
## Tokens consisting of 9 documents.
## BNP :
##  [1] "GB" "GB" "GB" "GB" "GB" "GB" "GB" "GB" "GB" "GB" "GB" "GB"
## [ ... and 62 more ]
## 
## Coalition :
## [1] "GB"
## 
## Conservative :
## [1] "GB" "GB" "GB" "GB" "GB"
## 
## Greens :
## [1] "GB" "GB"
## 
## Labour :
##  [1] "GB" "AU" "AU" "AU" "GB" "AU" "GB" "GB" "GB" "GB" "GB"
## 
## LibDem :
## [1] "GB" "GB" "GB" "GB" "GB" "GB" "CA"
## 
## [ reached max_ndoc ... 3 more documents ]
```

Compare the two outputs: `toks_region` collapses every matched place name down to a handful of continent labels, while `toks_country` keeps the finer-grained country labels. Everything that is not a place name has disappeared entirely, since `tokens_lookup()` only keeps tokens that match an entry in the dictionary.

You can also run a keyword-in-context analysis by looking up the mentions of all African countries and the words surrounding them, combining what you learned in the [previous chapter](/basic-operations/tokens/kwic) with the dictionary itself.


``` r
kwic(toks, dict_newsmap["AFRICA"])
```

```
## Keyword-in-context with 2 matches.                                                                            
##  [BNP, 3116] , with Rome and Ancient | Egypt  | being well known examples of
##  [BNP, 3149]   purpose. The Balkans, | Rwanda | , Indonesia, Ulster,
```

You are not limited to imported dictionaries such as **newsmap**'s. You can define your own dictionary by passing a named list of characters to `dictionary()`, where each name becomes a category and each character vector lists the words, or word patterns, that belong to it.


``` r
dict <- dictionary(list(refugee = c("refugee*", "asylum*"),
                        worker = c("worker*", "employee*")))
print(dict)
```

```
## Dictionary object with 2 key entries.
## - [refugee]:
##   - refugee*, asylum*
## - [worker]:
##   - worker*, employee*
```

``` r
dict_toks <- tokens_lookup(toks, dictionary = dict)
print(dict_toks)
```

```
## Tokens consisting of 9 documents.
## BNP :
##  [1] "refugee" "worker"  "refugee" "refugee" "refugee" "refugee" "refugee" "refugee" "refugee" "refugee"
## [11] "refugee" "refugee"
## [ ... and 2 more ]
## 
## Coalition :
## [1] "refugee"
## 
## Conservative :
## character(0)
## 
## Greens :
## [1] "worker"  "refugee" "refugee" "refugee" "refugee"
## 
## Labour :
## [1] "refugee" "refugee" "refugee" "refugee" "worker"  "worker"  "worker"  "worker" 
## 
## LibDem :
## [1] "refugee" "refugee" "refugee" "refugee" "refugee" "refugee"
## 
## [ reached max_ndoc ... 3 more documents ]
```

``` r
dfm(dict_toks)
```

```
## Document-feature matrix of: 9 documents, 2 features (38.89% sparse) and 0 docvars.
##               features
## docs           refugee worker
##   BNP               12      2
##   Coalition          1      0
##   Conservative       0      0
##   Greens             4      1
##   Labour             4      4
##   LibDem             6      0
## [ reached max_ndoc ... 3 more documents ]
```

{{% notice tip %}}
`tokens_lookup()` ignores multiple matches of dictionary values for the same key with the same token to avoid double counting. For example, if `US = c("United States of America", "United States")` is in your dictionary, you get "US" only once for a sequence of tokens `"United" "States" "of" "America"`.
{{% /notice %}}

{{% notice note %}}
`dfm_lookup()` performs the same category look-up, but starting from a document-feature matrix instead of a tokens object, which works fine for single-word dictionary entries. But a DFM only stores feature counts, not the order words appeared in, so `dfm_lookup()` cannot detect multi-word expressions such as "New York": each word is matched, or not, on its own, independently of its neighbours. `tokens_lookup()` works on the tokens themselves, so it matches multi-word entries as complete phrases, and also gives you access to options like `nested_scope` for controlling how overlapping multi-word matches are resolved, something a DFM has no way to represent. If your dictionary includes any multi-word entries, use `tokens_lookup()`, or run `tokens_compound()` before building a DFM so the phrases survive as single features.
{{% /notice %}}

## Nested matches from different categories

The tip above covers duplicate matches within a single key. A different problem arises when one dictionary entry sits entirely inside another entry that belongs to a *different* key. Consider a dictionary that treats "New York Times" as a newspaper and "New York" as a city: the phrase "New York Times" contains "New York" as a nested match, so a `paper` match and a `city` match can both, in principle, fire over the same three words.

`nested_scope` controls whether that nested match survives. With `nested_scope = "key"`, the default, nesting is only resolved within the same key, so a shorter match belonging to a different key is kept even when it sits entirely inside a longer match. With `nested_scope = "dictionary"`, the longest match wins across the whole dictionary, and any shorter match nested inside it is suppressed, regardless of which key it belongs to.


``` r
# nested matching differences
dict4 <- dictionary(list(paper = "New York Times", city = "New York"))
toks4 <- tokens("The New York Times is a New York paper.")
tokens_lookup(toks4, dict4, nested_scope = "key", exclusive = FALSE)
```

```
## Tokens consisting of 1 document.
## text1 :
## [1] "The"   "PAPER" "CITY"  "is"    "a"     "CITY"  "paper" "."
```

``` r
tokens_lookup(toks4, dict4, nested_scope = "dictionary", exclusive = FALSE)
```

```
## Tokens consisting of 1 document.
## text1 :
## [1] "The"   "PAPER" "is"    "a"     "CITY"  "paper" "."
```

With `nested_scope = "key"`, "New York Times" is tagged `PAPER`. But the "New York" nested inside it is *also* tagged `CITY`, on top of the second, freestanding "New York" later in the sentence, so `CITY` ends up counted twice. With `nested_scope = "dictionary"`, the nested "New York" inside "New York Times" is suppressed, since a longer match from another key already covers it; only the freestanding "New York" is left tagged as `CITY`.

{{% notice warning %}}
The distinction matters whenever your dictionary mixes entries that can contain one another, which is common with entity names, since organisation, place and person names frequently overlap. The default, `nested_scope = "key"`, can silently double-count a shorter category inside a longer match from an unrelated key, inflating that category's frequency. If your own dictionary has this kind of overlap, check both settings against a few examples you understand well, rather than assuming the default does what you expect.
{{% /notice %}}
