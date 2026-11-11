---
title: Generate n-grams
weight: 60
draft: false
---

An n-gram is a sequence of *n* consecutive tokens taken from a text, so a 2-gram (or "bigram") combines every pair of neighbouring words, and a 3-gram combines every three in a row. You can generate n-grams of any length from a tokens object using `tokens_ngrams()`, which builds every possible sequence of the requested length, sliding one token at a time through each document. Unlike `tokens_compound()`, which you met in the [previous chapter](/basic-operations/tokens/tokens_compound), `tokens_ngrams()` does not require you to know in advance which phrases matter, generating every combination and leaving you to work out afterwards which ones are meaningful.


``` r
library(quanteda)
```




``` r
toks <- tokens(data_char_ukimmig2010, remove_punct = TRUE)
```


``` r
toks_ngram <- tokens_ngrams(toks, n = 2:4)
head(toks_ngram[[1]], 30)
```

```
##  [1] "IMMIGRATION_AN"      "AN_UNPARALLELED"     "UNPARALLELED_CRISIS" "CRISIS_WHICH"       
##  [5] "WHICH_ONLY"          "ONLY_THE"            "THE_BNP"             "BNP_CAN"            
##  [9] "CAN_SOLVE"           "SOLVE_At"            "At_current"          "current_immigration"
## [13] "immigration_and"     "and_birth"           "birth_rates"         "rates_indigenous"   
## [17] "indigenous_British"  "British_people"      "people_are"          "are_set"            
## [21] "set_to"              "to_become"           "become_a"            "a_minority"         
## [25] "minority_well"       "well_within"         "within_50"           "50_years"           
## [29] "years_This"          "This_will"
```

``` r
tail(toks_ngram[[1]], 30)
```

```
##  [1] "a_passport_It_runs"          "passport_It_runs_far"        "It_runs_far_deeper"         
##  [4] "runs_far_deeper_than"        "far_deeper_than_that"        "deeper_than_that_it"        
##  [7] "than_that_it_is"             "that_it_is_to"               "it_is_to_belong"            
## [10] "is_to_belong_to"             "to_belong_to_a"              "belong_to_a_special"        
## [13] "to_a_special_chain"          "a_special_chain_of"          "special_chain_of_unique"    
## [16] "chain_of_unique_people"      "of_unique_people_who"        "unique_people_who_have"     
## [19] "people_who_have_the"         "who_have_the_natural"        "have_the_natural_law"       
## [22] "the_natural_law_right"       "natural_law_right_to"        "law_right_to_remain"        
## [25] "right_to_remain_a"           "to_remain_a_majority"        "remain_a_majority_in"       
## [28] "a_majority_in_their"         "majority_in_their_ancestral" "in_their_ancestral_homeland"
```

Setting `n = 2:4` generates bigrams, trigrams and four-grams all at once, which is why the output mixes sequences of different lengths, each joined with an underscore. The output is far larger than the words you started with, since almost every pair, triple and quadruple of neighbouring words now gets its own entry.

`tokens_ngrams()` also supports a `skip` argument to generate skip-grams, sequences that allow a gap between the words rather than requiring them to be strictly adjacent, useful for capturing relationships between words that are related but do not always sit next to each other.


``` r
toks_skip <- tokens_ngrams(toks, n = 2, skip = 1:2)
head(toks_skip[[1]], 30)
```

```
##  [1] "IMMIGRATION_UNPARALLELED" "IMMIGRATION_CRISIS"       "AN_CRISIS"               
##  [4] "AN_WHICH"                 "UNPARALLELED_WHICH"       "UNPARALLELED_ONLY"       
##  [7] "CRISIS_ONLY"              "CRISIS_THE"               "WHICH_THE"               
## [10] "WHICH_BNP"                "ONLY_BNP"                 "ONLY_CAN"                
## [13] "THE_CAN"                  "THE_SOLVE"                "BNP_SOLVE"               
## [16] "BNP_At"                   "CAN_At"                   "CAN_current"             
## [19] "SOLVE_current"            "SOLVE_immigration"        "At_immigration"          
## [22] "At_and"                   "current_and"              "current_birth"           
## [25] "immigration_birth"        "immigration_rates"        "and_rates"               
## [28] "and_indigenous"           "birth_indigenous"         "birth_British"
```

## Selective ngrams

Generating every possible n-gram, as above, produces a very large number of combinations, most of which are meaningless. While `tokens_ngrams()` generates n-grams or skip-grams from all possible combinations of tokens, `tokens_compound()` lets you generate n-grams more selectively, by specifying a pattern that the sequence must match. For example, you can make negation bigrams, such as "not good" or "not true", using `phrase()` together with a wildcard (`*`) that matches whatever word follows "not".


``` r
toks_neg_bigram <- tokens_compound(toks, pattern = phrase("not *"))
toks_neg_bigram_select <- tokens_select(toks_neg_bigram, pattern = phrase("not_*"))
head(toks_neg_bigram_select[[1]], 30)
```

```
## [1] "not_born"     "not_white"    "not_include"  "not_from"     "not_share"    "not_the"      "not_become"  
## [8] "not_merely"   "not_products"
```

The result is a much shorter, more targeted list than `tokens_ngrams()` produced, since only sequences beginning with "not" are compounded and then kept. Selective compounding like this is usually more useful in practice than generating every possible n-gram, because it lets you focus on the sequences you already have reason to think matter.

{{% notice tip %}}
`tokens_ngrams()` is an efficient function, but it returns a large object if multiple values are given to `n` or `skip`. Since n-grams inflates the size of objects without adding much information, we recommend generating n-grams more selectively using `tokens_compound()`.
{{% /notice %}}
