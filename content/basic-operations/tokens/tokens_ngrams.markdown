---
title: Genarate n-grams
weight: 60
draft: false
---


```r
require(quanteda)
```


```r
toks <- tokens(data_char_ukimmig2010)
```

You can generate n-grams in any lengths from a tokens using `tokens_ngrams()`. N-grams are a sequence of tokens from already tokenized text objects.


```r
toks_ngram <- tokens_ngrams(toks, n = 2:4)
head(toks_ngram[[1]], 50)
```

```
##  [1] "IMMIGRATION_:"       ":_AN"                "AN_UNPARALLELED"    
##  [4] "UNPARALLELED_CRISIS" "CRISIS_WHICH"        "WHICH_ONLY"         
##  [7] "ONLY_THE"            "THE_BNP"             "BNP_CAN"            
## [10] "CAN_SOLVE"           "SOLVE_."             "._-"                
## [13] "-_At"                "At_current"          "current_immigration"
## [16] "immigration_and"     "and_birth"           "birth_rates"        
## [19] "rates_,"             ",_indigenous"        "indigenous_British" 
## [22] "British_people"      "people_are"          "are_set"            
## [25] "set_to"              "to_become"           "become_a"           
## [28] "a_minority"          "minority_well"       "well_within"        
## [31] "within_50"           "50_years"            "years_."            
## [34] "._-"                 "-_This"              "This_will"          
## [37] "will_result"         "result_in"           "in_the"             
## [40] "the_extinction"      "extinction_of"       "of_the"             
## [43] "the_British"         "British_people"      "people_,"           
## [46] ",_culture"           "culture_,"           ",_heritage"         
## [49] "heritage_and"        "and_identity"
```

```r
tail(toks_ngram[[1]], 50)
```

```
##  [1] "runs_through_their_veins"     "through_their_veins_."       
##  [3] "their_veins_._Being"          "veins_._Being_British"       
##  [5] "._Being_British_is"           "Being_British_is_more"       
##  [7] "British_is_more_than"         "is_more_than_merely"         
##  [9] "more_than_merely_possessing"  "than_merely_possessing_a"    
## [11] "merely_possessing_a_modern"   "possessing_a_modern_document"
## [13] "a_modern_document_known"      "modern_document_known_as"    
## [15] "document_known_as_a"          "known_as_a_passport"         
## [17] "as_a_passport_."              "a_passport_._It"             
## [19] "passport_._It_runs"           "._It_runs_far"               
## [21] "It_runs_far_deeper"           "runs_far_deeper_than"        
## [23] "far_deeper_than_that"         "deeper_than_that_:"          
## [25] "than_that_:_it"               "that_:_it_is"                
## [27] ":_it_is_to"                   "it_is_to_belong"             
## [29] "is_to_belong_to"              "to_belong_to_a"              
## [31] "belong_to_a_special"          "to_a_special_chain"          
## [33] "a_special_chain_of"           "special_chain_of_unique"     
## [35] "chain_of_unique_people"       "of_unique_people_who"        
## [37] "unique_people_who_have"       "people_who_have_the"         
## [39] "who_have_the_natural"         "have_the_natural_law"        
## [41] "the_natural_law_right"        "natural_law_right_to"        
## [43] "law_right_to_remain"          "right_to_remain_a"           
## [45] "to_remain_a_majority"         "remain_a_majority_in"        
## [47] "a_majority_in_their"          "majority_in_their_ancestral" 
## [49] "in_their_ancestral_homeland"  "their_ancestral_homeland_."
```

`tokens_ngrams()` also supports skip to generate skip-grams.


```r
skipgram <- tokens_ngrams(toks, n = 2, skip = 1:2)
head(skipgram[[1]], 50)
```

```
##  [1] "IMMIGRATION_AN"           "IMMIGRATION_UNPARALLELED"
##  [3] ":_UNPARALLELED"           ":_CRISIS"                
##  [5] "AN_CRISIS"                "AN_WHICH"                
##  [7] "UNPARALLELED_WHICH"       "UNPARALLELED_ONLY"       
##  [9] "CRISIS_ONLY"              "CRISIS_THE"              
## [11] "WHICH_THE"                "WHICH_BNP"               
## [13] "ONLY_BNP"                 "ONLY_CAN"                
## [15] "THE_CAN"                  "THE_SOLVE"               
## [17] "BNP_SOLVE"                "BNP_."                   
## [19] "CAN_."                    "CAN_-"                   
## [21] "SOLVE_-"                  "SOLVE_At"                
## [23] "._At"                     "._current"               
## [25] "-_current"                "-_immigration"           
## [27] "At_immigration"           "At_and"                  
## [29] "current_and"              "current_birth"           
## [31] "immigration_birth"        "immigration_rates"       
## [33] "and_rates"                "and_,"                   
## [35] "birth_,"                  "birth_indigenous"        
## [37] "rates_indigenous"         "rates_British"           
## [39] ",_British"                ",_people"                
## [41] "indigenous_people"        "indigenous_are"          
## [43] "British_are"              "British_set"             
## [45] "people_set"               "people_to"               
## [47] "are_to"                   "are_become"              
## [49] "set_become"               "set_a"
```

## Selective ngrams

While `tokens_ngrams()` generates n-grams or skip-grams in all possible combinations of tokens, `tokens_compound()` generates n-grams more selectively. For example, you can make negation bi-grams using `phrase()` and a wild card (`*`).


```r
neg_bigram <- tokens_compound(toks, pattern = phrase('not *'))
neg_bigram <- tokens_select(neg_bigram, pattern = phrase('not_*'))
head(neg_bigram[[1]], 50)
```

```
## [1] "not_born"     "not_\""       "not_include"  "not_from"    
## [5] "not_share"    "not_the"      "not_become"   "not_merely"  
## [9] "not_products"
```

{{% notice tip %}}
`tokens_ngrans()` is an efficient function, but it returns a large object if multiple values are given to `n` or `skip`. Since n-grams inflates the size of objects without adding much information, we recommend to generate n-grams more selectively using `tokens_compound()`.
{{% /notice %}}
