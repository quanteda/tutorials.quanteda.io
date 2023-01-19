---
title: Generate n-grams
weight: 60
draft: false
---


```r
require(quanteda)
options(width = 110)
```


```r
toks <- tokens(data_char_ukimmig2010, remove_punct = TRUE)
```

You can generate n-grams in any lengths from a tokens using `tokens_ngrams()`. N-grams are a sequence of tokens from already tokenized text objects.


```r
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

```r
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

`tokens_ngrams()` also supports skip to generate skip-grams.


```r
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

While `tokens_ngrams()` generates n-grams or skip-grams in all possible combinations of tokens, `tokens_compound()` generates n-grams more selectively. For example, you can make negation bi-grams using `phrase()` and a wild card (`*`).


```r
toks_neg_bigram <- tokens_compound(toks, pattern = phrase("not *"))
toks_neg_bigram_select <- tokens_select(toks_neg_bigram, pattern = phrase("not_*"))
head(toks_neg_bigram_select[[1]], 30)
```

```
## [1] "not_born"     "not_white"    "not_include"  "not_from"     "not_share"    "not_the"      "not_become"  
## [8] "not_merely"   "not_products"
```

{{% notice tip %}}
`tokens_ngrams()` is an efficient function, but it returns a large object if multiple values are given to `n` or `skip`. Since n-grams inflates the size of objects without adding much information, we recommend generating n-grams more selectively using `tokens_compound()`.
{{% /notice %}}
