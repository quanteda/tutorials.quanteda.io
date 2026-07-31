---
title: Construct a tokens object
weight: 10
draft: false
---

A corpus stores whole documents, but most quantitative text analysis works with smaller units. "Tokenising" is the process of splitting text into these units, called tokens: usually a single word, but a token can also be a punctuation mark, a number, or, once you compound tokens together, a multiword expression. `tokens()` performs this splitting on the texts in a corpus, using rules for word boundaries such as spaces and punctuation.


``` r
library(quanteda)
```




``` r
corp_immig <- corpus(data_char_ukimmig2010)
toks_immig <- tokens(corp_immig)
```

`tokens()` also works directly on a plain character vector, without you needing to build a corpus first, which is convenient for a quick, one-off look at some text.


``` r
toks_immig2 <- tokens(data_char_ukimmig2010)
print(toks_immig2[1], max_ndoc = 1, max_ntoken = 20)
```

```
## Tokens consisting of 1 document.
## BNP :
##  [1] "IMMIGRATION"  ":"            "AN"           "UNPARALLELED" "CRISIS"       "WHICH"        "ONLY"        
##  [8] "THE"          "BNP"          "CAN"          "SOLVE"        "."            "-"            "At"          
## [15] "current"      "immigration"  "and"          "birth"        "rates"        ","           
## [ ... and 3,260 more ]
```

Each document in a tokens object is a separate list of words, in the order they appeared in the original text. In contrast to a document-feature matrix (covered in later chapters), a tokens object still remembers word order and position.

By default, `tokens()` only removes separators (typically white spaces), keeping punctuation marks and numbers as tokens in their own right. Most analyses do not need these, so `tokens()` has arguments to strip them out at the same time as splitting the text: `remove_punct = TRUE` and, similarly, `remove_numbers = TRUE` or `remove_symbols = TRUE`.


``` r
toks_nopunct <- tokens(data_char_ukimmig2010, remove_punct = TRUE)
print(toks_nopunct)
```

```
## Tokens consisting of 9 documents.
## BNP :
##  [1] "IMMIGRATION"  "AN"           "UNPARALLELED" "CRISIS"       "WHICH"        "ONLY"         "THE"         
##  [8] "BNP"          "CAN"          "SOLVE"        "At"           "current"     
## [ ... and 2,839 more ]
## 
## Coalition :
##  [1] "IMMIGRATION"  "The"          "Government"   "believes"     "that"         "immigration"  "has"         
##  [8] "enriched"     "our"          "culture"      "and"          "strengthened"
## [ ... and 219 more ]
## 
## Conservative :
##  [1] "Attract"     "the"         "brightest"   "and"         "best"        "to"          "our"        
##  [8] "country"     "Immigration" "has"         "enriched"    "our"        
## [ ... and 440 more ]
## 
## Greens :
##  [1] "Immigration" "Migration"   "is"          "a"           "fact"        "of"          "life"       
##  [8] "People"      "have"        "always"      "moved"       "from"       
## [ ... and 598 more ]
## 
## Labour :
##  [1] "Crime"       "and"         "immigration" "The"         "challenge"   "for"         "Britain"    
##  [8] "We"          "will"        "control"     "immigration" "with"       
## [ ... and 608 more ]
## 
## LibDem :
##  [1] "firm"        "but"         "fair"        "immigration" "system"      "Britain"     "has"        
##  [8] "always"      "been"        "an"          "open"        "welcoming"  
## [ ... and 423 more ]
## 
## [ reached max_ndoc ... 3 more documents ]
```

Compare the two tokens objects above: `toks_immig` still contains punctuation marks as separate tokens, while `toks_nopunct` does not, which is usually what you want before counting words. Removing punctuation and numbers at the tokenisation stage, rather than afterwards, is both faster and less error-prone than trying to strip them out later.

## Choosing a tokenizer version

`what = "word"`, the default you have been using so far, is not a single fixed set of rules, but tracks whichever version of **quanteda**'s built-in word tokenizer is current, set by `quanteda_options("tokens_tokenizer_word")`. That mapping has changed as the package has evolved. For most everyday work this is exactly what you want, since the current tokenizer is also the most accurate one. But it means the same code, `tokens(x)`, can silently produce different tokens after you or a collaborator updates **quanteda**.

`tokens()` also accepts several older, explicitly named versions of the tokenizer, so you can pin down and reproduce a specific rule set rather than relying on whichever version happens to be current. `"word1"` reproduces the pre-version-2 behaviour, `"word2"` and `"word3"` reproduce the versions used in **quanteda** 2 and 3, and `"word4"`, current at the time of writing, is what `what = "word"` currently maps onto.


``` r
txt_abbrev <- "Self-funding U.S.A. isnt gr8, right?"
tokens(txt_abbrev, what = "word2")
```

```
## Tokens consisting of 1 document.
## text1 :
## [1] "Self-funding" "U.S.A"        "."            "isnt"         "gr8"          ","            "right"       
## [8] "?"
```

``` r
tokens(txt_abbrev, what = "word4")
```

```
## Tokens consisting of 1 document.
## text1 :
## [1] "Self-funding" "U.S.A"        "."            "isnt"         "gr8"          ","            "right"       
## [8] "?"
```

If reproducibility across **quanteda** versions matters for your project, specify the exact version you used directly, `what = "word4"`, rather than the version-tracking `what = "word"`. That way the same code keeps producing the same tokens even after **quanteda** itself moves on to a newer tokenizer.

## Changing case

Text almost always contains a mix of capitalisation that you usually want treated as the same word rather than as different ones: "Immigration" at the start of a sentence and "immigration" in the middle of one are still the same word. `tokens_tolower()` converts every token to lower case, and its counterpart `tokens_toupper()` does the reverse.


``` r
toks_case <- tokens(c(example = "The Quick BROWN fox"))
print(toks_case)
```

```
## Tokens consisting of 1 document.
## example :
## [1] "The"   "Quick" "BROWN" "fox"
```

``` r
print(tokens_tolower(toks_case))
```

```
## Tokens consisting of 1 document.
## example :
## [1] "the"   "quick" "brown" "fox"
```

{{% notice tip %}}
`dfm()` lowercases text automatically by default (its `tolower` argument defaults to `TRUE`). Most of the time, you do not need to call `tokens_tolower()` before building a document-feature matrix. You can switch it off with `dfm(x, tolower = FALSE)`.
{{% /notice %}}

## Inspecting the structure of a tokens object

A tokens object is, underneath its printed display, a list with one element per document, each holding a character vector of that document's tokens in order. Converting it with `as.list()` makes this structure explicit, useful early on for building an accurate mental model of what a tokens object contains.


``` r
str(as.list(toks_immig["BNP"]))
```

```
## List of 1
##  $ BNP: chr [1:3280] "IMMIGRATION" ":" "AN" "UNPARALLELED" ...
```

You can also summarise a tokens object numerically, without inspecting the tokens directly: `ntoken()` gives the total number of tokens per document, and `ntype()` gives the number of distinct token types (unique words) per document. The difference between the two tells you something about repetition: a document with many tokens but comparatively few types is repeating the same words often.


``` r
toks_immig3 <- tokens(data_char_ukimmig2010, remove_punct = TRUE)
head(ntoken(toks_immig3))
```

```
##          BNP    Coalition Conservative       Greens       Labour       LibDem 
##         2851          231          452          610          620          435
```

``` r
head(ntype(toks_immig3))
```

```
##          BNP    Coalition Conservative       Greens       Labour       LibDem 
##         1114          139          246          317          293          246
```

