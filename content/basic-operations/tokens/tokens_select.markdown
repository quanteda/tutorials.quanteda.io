---
title: Select tokens
weight: 30
draft: false
bibliography: ../../references.bib
---

Not every token in a text is useful for analysis. Function words, or stopwords, such as “the” or “and” appear constantly in any English text and rarely tell you anything about its content. `tokens_select()` lets you remove tokens like these, or, the other way round, keep only the tokens you are interested in, whether that’s a set of words, numbers, or punctuation. `stopwords()` returns a pre-defined list of function words for a given language.

``` r
library(quanteda)
```

``` r
toks <- tokens(data_char_ukimmig2010)
```

Setting `selection = "remove"` drops every token that matches the pattern, here the built-in English stopword list, from each document.

``` r
toks_nostop <- tokens_select(toks, pattern = stopwords("en"), selection = "remove")
print(toks_nostop)
```

    ## Tokens consisting of 9 documents.
    ## BNP :
    ##  [1] "IMMIGRATION"  ":"            "UNPARALLELED" "CRISIS"       "BNP"          "CAN"          "SOLVE"       
    ##  [8] "."            "-"            "current"      "immigration"  "birth"       
    ## [ ... and 2,109 more ]
    ## 
    ## Coalition :
    ##  [1] "IMMIGRATION"  "."            "Government"   "believes"     "immigration"  "enriched"     "culture"     
    ##  [8] "strengthened" "economy"      ","            "must"         "controlled"  
    ## [ ... and 146 more ]
    ## 
    ## Conservative :
    ##  [1] "Attract"     "brightest"   "best"        "country"     "."           "Immigration" "enriched"   
    ##  [8] "nation"      "years"       "want"        "attract"     "brightest"  
    ## [ ... and 277 more ]
    ## 
    ## Greens :
    ##  [1] "Immigration" "."           "Migration"   "fact"        "life"        "."           "People"     
    ##  [8] "always"      "moved"       "one"         "country"     "another"    
    ## [ ... and 377 more ]
    ## 
    ## Labour :
    ##  [1] "Crime"            "immigration"      "challenge"        "Britain"          "control"         
    ##  [6] "immigration"      "new"              "Australian-style" "points-based"     "system"          
    ## [11] "-"                "unlike"          
    ## [ ... and 391 more ]
    ## 
    ## LibDem :
    ##  [1] "firm"        "fair"        "immigration" "system"      "Britain"     "always"      "open"       
    ##  [8] ","           "welcoming"   "country"     ","           "thousands"  
    ## [ ... and 285 more ]
    ## 
    ## [ reached max_ndoc ... 3 more documents ]

Because removing stopwords is such a common step, **quanteda** provides `tokens_remove()` as a shortcut, an alias for `tokens_select(selection = "remove")`, so the code above and below produce identical results.

``` r
toks_nostop2 <- tokens_remove(toks, pattern = stopwords("en"))
print(toks_nostop2)
```

    ## Tokens consisting of 9 documents.
    ## BNP :
    ##  [1] "IMMIGRATION"  ":"            "UNPARALLELED" "CRISIS"       "BNP"          "CAN"          "SOLVE"       
    ##  [8] "."            "-"            "current"      "immigration"  "birth"       
    ## [ ... and 2,109 more ]
    ## 
    ## Coalition :
    ##  [1] "IMMIGRATION"  "."            "Government"   "believes"     "immigration"  "enriched"     "culture"     
    ##  [8] "strengthened" "economy"      ","            "must"         "controlled"  
    ## [ ... and 146 more ]
    ## 
    ## Conservative :
    ##  [1] "Attract"     "brightest"   "best"        "country"     "."           "Immigration" "enriched"   
    ##  [8] "nation"      "years"       "want"        "attract"     "brightest"  
    ## [ ... and 277 more ]
    ## 
    ## Greens :
    ##  [1] "Immigration" "."           "Migration"   "fact"        "life"        "."           "People"     
    ##  [8] "always"      "moved"       "one"         "country"     "another"    
    ## [ ... and 377 more ]
    ## 
    ## Labour :
    ##  [1] "Crime"            "immigration"      "challenge"        "Britain"          "control"         
    ##  [6] "immigration"      "new"              "Australian-style" "points-based"     "system"          
    ## [11] "-"                "unlike"          
    ## [ ... and 391 more ]
    ## 
    ## LibDem :
    ##  [1] "firm"        "fair"        "immigration" "system"      "Britain"     "always"      "open"       
    ##  [8] ","           "welcoming"   "country"     ","           "thousands"  
    ## [ ... and 285 more ]
    ## 
    ## [ reached max_ndoc ... 3 more documents ]

Removing tokens shortens each document, since the matched words disappear, which is a problem if you later want to measure the distance between two words in the original text: removed words would artificially pull the remaining words closer together. Setting `padding = TRUE` avoids this by leaving an empty placeholder behind instead of shrinking the document, which keeps the position of the remaining tokens unchanged.

``` r
toks_nostop_pad <- tokens_remove(toks, pattern = stopwords("en"), padding = TRUE)
print(toks_nostop_pad)
```

    ## Tokens consisting of 9 documents.
    ## BNP :
    ##  [1] "IMMIGRATION"  ":"            ""             "UNPARALLELED" "CRISIS"       ""             ""            
    ##  [8] ""             "BNP"          "CAN"          "SOLVE"        "."           
    ## [ ... and 3,268 more ]
    ## 
    ## Coalition :
    ##  [1] "IMMIGRATION" "."           ""            "Government"  "believes"    ""            "immigration"
    ##  [8] ""            "enriched"    ""            "culture"     ""           
    ## [ ... and 248 more ]
    ## 
    ## Conservative :
    ##  [1] "Attract"     ""            "brightest"   ""            "best"        ""            ""           
    ##  [8] "country"     "."           "Immigration" ""            "enriched"   
    ## [ ... and 487 more ]
    ## 
    ## Greens :
    ##  [1] "Immigration" "."           "Migration"   ""            ""            "fact"        ""           
    ##  [8] "life"        "."           "People"      ""            "always"     
    ## [ ... and 667 more ]
    ## 
    ## Labour :
    ##  [1] "Crime"       ""            "immigration" ""            "challenge"   ""            "Britain"    
    ##  [8] ""            ""            "control"     "immigration" ""           
    ## [ ... and 671 more ]
    ## 
    ## LibDem :
    ##  [1] "firm"        ""            "fair"        "immigration" "system"      "Britain"     ""           
    ##  [8] "always"      ""            ""            "open"        ","          
    ## [ ... and 471 more ]
    ## 
    ## [ reached max_ndoc ... 3 more documents ]

So far we have removed words we are not interested in. You can flip this around and keep only the words you are interested in, so that everything else is discarded, useful when your analysis focuses on a small set of terms.

``` r
toks_immig <- tokens_select(toks, pattern = c("immig*", "migra*"), padding = TRUE)
print(toks_immig)
```

    ## Tokens consisting of 9 documents.
    ## BNP :
    ##  [1] "IMMIGRATION" ""            ""            ""            ""            ""            ""           
    ##  [8] ""            ""            ""            ""            ""           
    ## [ ... and 3,268 more ]
    ## 
    ## Coalition :
    ##  [1] "IMMIGRATION" ""            ""            ""            ""            ""            "immigration"
    ##  [8] ""            ""            ""            ""            ""           
    ## [ ... and 248 more ]
    ## 
    ## Conservative :
    ##  [1] ""            ""            ""            ""            ""            ""            ""           
    ##  [8] ""            ""            "Immigration" ""            ""           
    ## [ ... and 487 more ]
    ## 
    ## Greens :
    ##  [1] "Immigration" ""            "Migration"   ""            ""            ""            ""           
    ##  [8] ""            ""            ""            ""            ""           
    ## [ ... and 667 more ]
    ## 
    ## Labour :
    ##  [1] ""            ""            "immigration" ""            ""            ""            ""           
    ##  [8] ""            ""            ""            "immigration" ""           
    ## [ ... and 671 more ]
    ## 
    ## LibDem :
    ##  [1] ""            ""            ""            "immigration" ""            ""            ""           
    ##  [8] ""            ""            ""            ""            ""           
    ## [ ... and 471 more ]
    ## 
    ## [ reached max_ndoc ... 3 more documents ]

If you want to analyse the words that appear around your keywords, rather than the keywords themselves, add the `window` argument, keeping a band of surrounding tokens on either side of every match, the same idea behind the `window` argument you saw in `kwic()`.

``` r
toks_immig_window <- tokens_select(toks, pattern = c("immig*", "migra*"), padding = TRUE, window = 5)
print(toks_immig_window)
```

    ## Tokens consisting of 9 documents.
    ## BNP :
    ##  [1] "IMMIGRATION"  ":"            "AN"           "UNPARALLELED" "CRISIS"       "WHICH"        ""            
    ##  [8] ""             ""             ""             "SOLVE"        "."           
    ## [ ... and 3,268 more ]
    ## 
    ## Coalition :
    ##  [1] "IMMIGRATION" "."           "The"         "Government"  "believes"    "that"        "immigration"
    ##  [8] "has"         "enriched"    "our"         "culture"     "and"        
    ## [ ... and 248 more ]
    ## 
    ## Conservative :
    ##  [1] ""            ""            ""            ""            "best"        "to"          "our"        
    ##  [8] "country"     "."           "Immigration" "has"         "enriched"   
    ## [ ... and 487 more ]
    ## 
    ## Greens :
    ##  [1] "Immigration" "."           "Migration"   "is"          "a"           "fact"        "of"         
    ##  [8] "life"        ""            ""            ""            ""           
    ## [ ... and 667 more ]
    ## 
    ## Labour :
    ##  [1] "Crime"       "and"         "immigration" "The"         "challenge"   "for"         "Britain"    
    ##  [8] "We"          "will"        "control"     "immigration" "with"       
    ## [ ... and 671 more ]
    ## 
    ## LibDem :
    ##  [1] "firm"        "but"         "fair"        "immigration" "system"      "Britain"     "has"        
    ##  [8] "always"      "been"        ""            ""            ""           
    ## [ ... and 471 more ]
    ## 
    ## [ reached max_ndoc ... 3 more documents ]

{{% notice tip %}}
We keep using *glob* patterns in this tutorials, but almost all the functions in **quanteda** support *regular expression* patterns. See `?pattern` for details and this [cheatsheet](https://www.cheatography.com/davechild/cheat-sheets/regular-expressions/pdf_bw/) for a concise introduction to frequently used regular expressions.
{{% /notice %}}

## Does it matter which preprocessing choices you make?

Choices like removing stopwords, choosing a `window`, and, in previous chapters, removing punctuation and lowercasing, can feel like routine housekeeping. Denny and Spirling (2018) show that it is not: different, individually reasonable combinations of preprocessing decisions can noticeably change downstream results, and some corpora are far more sensitive to these choices than others.

{{% notice info %}}
Denny and Spirling (2018)’s key insight is that preprocessing is not a single “correct” pipeline but a set of largely independent binary choices, remove punctuation, remove numbers, lowercase, stem, remove stopwords, trim infrequent terms, and each one is individually defensible. When they compared documents processed under every combination of these choices, some decisions barely moved the results. Others, not always the ones you would expect, consistently pushed a corpus’s results away from what most other combinations agreed on. Because there is no way to know in advance which choices will matter for a given corpus and research question, treat your own preprocessing pipeline as a set of assumptions worth checking rather than as settled housekeeping. Beyond checking, let your research question decide what to remove, not the default settings. A text about government budgets loses information if you strip out currency symbols and numbers. In research on the temporal focus of speeches (Müller 2022), removing “will” as a stopword would delete the main marker of future-oriented language, even though most stopword lists remove it by default.
{{% /notice %}}

## References

<div id="refs" class="references csl-bib-body hanging-indent">

<div id="ref-denny2018" class="csl-entry">

Denny, Matthew J., and Arthur Spirling. 2018. “Text Preprocessing for Unsupervised Learning: Why It Matters, When It Misleads, and What to Do about It.” *Political Analysis* 26 (2): 168–89. <https://doi.org/10.1017/pan.2017.44>.

</div>

<div id="ref-mueller2022" class="csl-entry">

Müller, Stefan. 2022. “The Temporal Focus of Campaign Communication.” *The Journal of Politics* 84 (1): 585–90. <https://doi.org/10.1086/715165>.

</div>

</div>
