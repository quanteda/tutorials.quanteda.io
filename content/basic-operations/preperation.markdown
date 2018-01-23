---
title: Preparing texts
weight: 20
chapter: false
draft: true
---



Here we will step through the basic elements of preparing a text for analysis.  These are tokenization, conversion to lower case, stemming, removing or selecting features, and defining equivalency classes for features, including the use of dictionaries.

### 1. Tokenization

Tokenization in quanteda is very *conservative*: by default, it only removes separator characters.


```r
require(quanteda, quietly = TRUE, warn.conflicts = FALSE)
txt <- c(text1 = "This is $10 in 999 different ways,\n up and down; left and right!",
         text2 = "@kenbenoit working: on #quanteda 2day\t4ever, http://textasdata.com?page=123.")
tokens(txt)
## tokens from 2 documents.
## text1 :
##  [1] "This"      "is"        "$"         "10"        "in"       
##  [6] "999"       "different" "ways"      ","         "up"       
## [11] "and"       "down"      ";"         "left"      "and"      
## [16] "right"     "!"        
## 
## text2 :
##  [1] "@kenbenoit"     "working"        ":"              "on"            
##  [5] "#quanteda"      "2day"           "4ever"          ","             
##  [9] "http"           ":"              "/"              "/"             
## [13] "textasdata.com" "?"              "page"           "="             
## [17] "123"            "."
tokens(txt, verbose = TRUE)
## Starting tokenization...
## ...tokenizing 1 of 1 blocks
## ...preserving hyphens
## ...preserving Twitter characters (#, @)
## ...serializing tokens 34 unique types
## ...total elapsed:  0.127 seconds.
## Finished tokenizing and cleaning 2 texts.
## tokens from 2 documents.
## text1 :
##  [1] "This"      "is"        "$"         "10"        "in"       
##  [6] "999"       "different" "ways"      ","         "up"       
## [11] "and"       "down"      ";"         "left"      "and"      
## [16] "right"     "!"        
## 
## text2 :
##  [1] "@kenbenoit"     "working"        ":"              "on"            
##  [5] "#quanteda"      "2day"           "4ever"          ","             
##  [9] "http"           ":"              "/"              "/"             
## [13] "textasdata.com" "?"              "page"           "="             
## [17] "123"            "."
tokens(txt, remove_numbers = TRUE,  remove_punct = TRUE)
## tokens from 2 documents.
## text1 :
##  [1] "This"      "is"        "in"        "different" "ways"     
##  [6] "up"        "and"       "down"      "left"      "and"      
## [11] "right"    
## 
## text2 :
## [1] "@kenbenoit"     "working"        "on"             "#quanteda"     
## [5] "2day"           "4ever"          "http"           "textasdata.com"
## [9] "page"
tokens(txt, remove_numbers = FALSE, remove_punct = TRUE)
## tokens from 2 documents.
## text1 :
##  [1] "This"      "is"        "10"        "in"        "999"      
##  [6] "different" "ways"      "up"        "and"       "down"     
## [11] "left"      "and"       "right"    
## 
## text2 :
##  [1] "@kenbenoit"     "working"        "on"             "#quanteda"     
##  [5] "2day"           "4ever"          "http"           "textasdata.com"
##  [9] "page"           "123"
tokens(txt, remove_numbers = TRUE,  remove_punct = FALSE)
## tokens from 2 documents.
## text1 :
##  [1] "This"      "is"        "$"         "in"        "different"
##  [6] "ways"      ","         "up"        "and"       "down"     
## [11] ";"         "left"      "and"       "right"     "!"        
## 
## text2 :
##  [1] "@kenbenoit"     "working"        ":"              "on"            
##  [5] "#quanteda"      "2day"           "4ever"          ","             
##  [9] "http"           ":"              "/"              "/"             
## [13] "textasdata.com" "?"              "page"           "="             
## [17] "."
tokens(txt, remove_numbers = FALSE, remove_punct = FALSE)
## tokens from 2 documents.
## text1 :
##  [1] "This"      "is"        "$"         "10"        "in"       
##  [6] "999"       "different" "ways"      ","         "up"       
## [11] "and"       "down"      ";"         "left"      "and"      
## [16] "right"     "!"        
## 
## text2 :
##  [1] "@kenbenoit"     "working"        ":"              "on"            
##  [5] "#quanteda"      "2day"           "4ever"          ","             
##  [9] "http"           ":"              "/"              "/"             
## [13] "textasdata.com" "?"              "page"           "="             
## [17] "123"            "."
tokens(txt, remove_numbers = FALSE, remove_punct = FALSE, remove_separators = FALSE)
## tokens from 2 documents.
## text1 :
##  [1] "This"      " "         "is"        " "         "$"        
##  [6] "10"        " "         "in"        " "         "999"      
## [11] " "         "different" " "         "ways"      ","        
## [16] "\n"        " "         "up"        " "         "and"      
## [21] " "         "down"      ";"         " "         "left"     
## [26] " "         "and"       " "         "right"     "!"        
## 
## text2 :
##  [1] "@kenbenoit"     " "              "working"        ":"             
##  [5] " "              "on"             " "              "#quanteda"     
##  [9] " "              "2day"           "\t"             "4ever"         
## [13] ","              " "              "http"           ":"             
## [17] "/"              "/"              "textasdata.com" "?"             
## [21] "page"           "="              "123"            "."
```

There are several options to the `what` argument: 

```r
# sentence level
tokens(c("Kurt Vongeut said; only assholes use semi-colons.",
         "Today is Thursday in Canberra:  It is yesterday in London.",
         "Today is Thursday in Canberra:  \nIt is yesterday in London.",
         "To be?  Or\nnot to be?"),
       what = "sentence")
## tokens from 4 documents.
## text1 :
## [1] "Kurt Vongeut said; only assholes use semi-colons."
## 
## text2 :
## [1] "Today is Thursday in Canberra:  It is yesterday in London."
## 
## text3 :
## [1] "Today is Thursday in Canberra:   It is yesterday in London."
## 
## text4 :
## [1] "To be?"        "Or not to be?"
tokens(data_corpus_inaugural[2], what = "sentence")
## tokens from 1 document.
## 1793-Washington :
## [1] "Fellow citizens, I am again called upon by the voice of my country to execute the functions of its Chief Magistrate."                                                                                                                                                                                                                                       
## [2] "When the occasion proper for it shall arrive, I shall endeavor to express the high sense I entertain of this distinguished honor, and of the confidence which has been reposed in me by the people of united America."                                                                                                                                      
## [3] "Previous to the execution of any official act of the President the Constitution requires an oath of office."                                                                                                                                                                                                                                                
## [4] "This oath I am now about to take, and in your presence: That if it shall be found during my administration of the Government I have in any instance violated willingly or knowingly the injunctions thereof, I may (besides incurring constitutional punishment) be subject to the upbraidings of all who are now witnesses of the present solemn ceremony."

# character level
tokens("My big fat text package.", what = "character")
## tokens from 1 document.
## text1 :
##  [1] "M" "y" "b" "i" "g" "f" "a" "t" "t" "e" "x" "t" "p" "a" "c" "k" "a"
## [18] "g" "e" "."
tokens("My big fat text package.", what = "character", remove_separators = FALSE)
## tokens from 1 document.
## text1 :
##  [1] "M" "y" " " "b" "i" "g" " " "f" "a" "t" " " "t" "e" "x" "t" " " "p"
## [18] "a" "c" "k" "a" "g" "e" "."
```

Two other options, for really fast and simple tokenization are `"fastestword"` and `"fasterword"`, if performance is a key issue.  These are less intelligent than the boundary detection used in the default `"word"` method, which is based on stringi/ICU boundary detection.

### 2. Conversion to lower case

This is a tricky one in our workflow, since it is a form of equivalency declaration, rather than a tokenization step.  It turns out that it is more efficient to perform at the pre-tokenization stage. 

The **quanteda** functions `*_tolower` include options designed to preserve acronyms.

```r
test1 <- c(text1 = "England and France are members of NATO and UNESCO",
           text2 = "NASA sent a rocket into space.")
char_tolower(test1)
##                                               text1 
## "england and france are members of nato and unesco" 
##                                               text2 
##                    "nasa sent a rocket into space."
char_tolower(test1, keep_acronyms = TRUE)
##                                               text1 
## "england and france are members of NATO and UNESCO" 
##                                               text2 
##                    "NASA sent a rocket into space."

test2 <- tokens(test1, remove_punct = TRUE)
tokens_tolower(test2)
## tokens from 2 documents.
## text1 :
## [1] "england" "and"     "france"  "are"     "members" "of"      "nato"   
## [8] "and"     "unesco" 
## 
## text2 :
## [1] "nasa"   "sent"   "a"      "rocket" "into"   "space"
tokens_tolower(test2, keep_acronyms = TRUE)
## tokens from 2 documents.
## text1 :
## [1] "england" "and"     "france"  "are"     "members" "of"      "NATO"   
## [8] "and"     "UNESCO" 
## 
## text2 :
## [1] "NASA"   "sent"   "a"      "rocket" "into"   "space"
```

The **quanteda** `*_tolower` functions are based on `stringi::stri_trans_tolower()`, and is therefore nicely Unicode compliant.

```r
data(data_char_encodedtexts, package = "readtext")

# Russian
cat(iconv(data_char_encodedtexts[8], "windows-1251", "UTF-8"))
## "8-битные" oncodings являются частью прошлого. 0€. Дефис-ели. Тильда ~ тире - и ± § 50.
cat(tolower(iconv(data_char_encodedtexts[8], "windows-1251", "UTF-8")))
## "8-битные" oncodings являются частью прошлого. 0€. дефис-ели. тильда ~ тире - и ± § 50.
head(char_tolower(stopwords("russian")), 20)
##  [1] "и"   "в"   "во"  "не"  "что" "он"  "на"  "я"   "с"   "со"  "как"
## [12] "а"   "то"  "все" "она" "так" "его" "но"  "да"  "ты"
head(char_toupper(stopwords("russian")), 20)
##  [1] "И"   "В"   "ВО"  "НЕ"  "ЧТО" "ОН"  "НА"  "Я"   "С"   "СО"  "КАК"
## [12] "А"   "ТО"  "ВСЕ" "ОНА" "ТАК" "ЕГО" "НО"  "ДА"  "ТЫ"

# Arabic
cat(iconv(data_char_encodedtexts[6], "ISO-8859-6", "UTF-8"))
## ترميزات 8 بت هي موديل قديم. 0 . الواصلة أكل. تيلدا ~ م اندفاعة - و  50 .
cat(tolower(iconv(data_char_encodedtexts[6], "ISO-8859-6", "UTF-8")))
## ترميزات 8 بت هي موديل قديم. 0 . الواصلة أكل. تيلدا ~ م اندفاعة - و  50 .
head(char_toupper(stopwords("arabic")), 20)
## Warning: 'stopwords(language = "ar")' is deprecated.
## Use 'stopwords(language = "ar", source = "misc")' instead.
## See help("Deprecated")
##  [1] "فى"    "في"    "كل"    "لم"    "لن"    "له"    "من"    "هو"   
##  [9] "هي"    "قوة"   "كما"   "لها"   "منذ"   "وقد"   "ولا"   "نفسه" 
## [17] "لقاء"  "مقابل" "هناك"  "وقال"
```

**Note**: dfm, the Swiss army knife, converts to lower case by default, but this can be turned off using the `tolower = FALSE` argument.


### 3. Removing and selecting features

This can be done when creating a dfm:

```r
# with English stopwords and stemming
dfmsInaug2 <- dfm(corpus_subset(data_corpus_inaugural, Year > 1980),
                  remove = stopwords("en"), stem = TRUE)
dfmsInaug2
## Document-feature matrix of: 10 documents, 2,303 features (75.1% sparse).
head(dfmsInaug2)
## Document-feature matrix of: 6 documents, 2,303 features (75.3% sparse).
```


Or can be done **after** creating a dfm:

```r
myDfm <- dfm(c("My Christmas was ruined by your opposition tax plan.",
               "Does the United_States or Sweden have more progressive taxation?"),
             tolower = FALSE, verbose = TRUE)
## Creating a dfm from a character input...
##    ... found 2 documents, 20 features
##    ... created a 2 x 20 sparse dfm
##    ... complete. 
## Elapsed time: 0.074 seconds.
dfm_select(myDfm, c("s$", ".y"), selection = "keep", valuetype = "regex")
## Document-feature matrix of: 2 documents, 6 features (50% sparse).
## 2 x 6 sparse Matrix of class "dfm"
##        features
## docs    My Christmas was by Does United_States
##   text1  1         1   1  1    0             0
##   text2  0         0   0  0    1             1
dfm_select(myDfm, c("s$", ".y"), selection = "remove", valuetype = "regex")
## Document-feature matrix of: 2 documents, 14 features (50% sparse).
## 2 x 14 sparse Matrix of class "dfm"
##        features
## docs    ruined your opposition tax plan . the or Sweden have more
##   text1      1    1          1   1    1 1   0  0      0    0    0
##   text2      0    0          0   0    0 0   1  1      1    1    1
##        features
## docs    progressive taxation ?
##   text1           0        0 0
##   text2           1        1 1
dfm_select(myDfm, stopwords("en"), selection = "keep", valuetype = "fixed")
## Document-feature matrix of: 2 documents, 9 features (50% sparse).
## 2 x 9 sparse Matrix of class "dfm"
##        features
## docs    My was by your Does the or have more
##   text1  1   1  1    1    0   0  0    0    0
##   text2  0   0  0    0    1   1  1    1    1
dfm_select(myDfm, stopwords("en"), selection = "remove", valuetype = "fixed")
## Document-feature matrix of: 2 documents, 11 features (50% sparse).
## 2 x 11 sparse Matrix of class "dfm"
##        features
## docs    Christmas ruined opposition tax plan . United_States Sweden
##   text1         1      1          1   1    1 1             0      0
##   text2         0      0          0   0    0 0             1      1
##        features
## docs    progressive taxation ?
##   text1           0        0 0
##   text2           1        1 1
```

More examples:

```r
# removing stopwords
testText <- "The quick brown fox named Seamus jumps over the lazy dog also named Seamus, with
             the newspaper from a boy named Seamus, in his mouth."
testCorpus <- corpus(testText)
# note: "also" is not in the default stopwords("en")
featnames(dfm(testCorpus, remove = stopwords("en")))
##  [1] "quick"     "brown"     "fox"       "named"     "seamus"   
##  [6] "jumps"     "lazy"      "dog"       "also"      ","        
## [11] "newspaper" "boy"       "mouth"     "."
# for ngrams
featnames(dfm(testCorpus, ngrams = 2, remove = stopwords("en")))
##  [1] "the_quick"      "quick_brown"    "brown_fox"      "fox_named"     
##  [5] "named_seamus"   "seamus_jumps"   "jumps_over"     "over_the"      
##  [9] "the_lazy"       "lazy_dog"       "dog_also"       "also_named"    
## [13] "seamus_,"       ",_with"         "with_the"       "the_newspaper" 
## [17] "newspaper_from" "from_a"         "a_boy"          "boy_named"     
## [21] ",_in"           "in_his"         "his_mouth"      "mouth_."
featnames(dfm(testCorpus, ngrams = 1:2, remove = stopwords("en")))
##  [1] "quick"          "brown"          "fox"            "named"         
##  [5] "seamus"         "jumps"          "lazy"           "dog"           
##  [9] "also"           ","              "newspaper"      "boy"           
## [13] "mouth"          "."              "the_quick"      "quick_brown"   
## [17] "brown_fox"      "fox_named"      "named_seamus"   "seamus_jumps"  
## [21] "jumps_over"     "over_the"       "the_lazy"       "lazy_dog"      
## [25] "dog_also"       "also_named"     "seamus_,"       ",_with"        
## [29] "with_the"       "the_newspaper"  "newspaper_from" "from_a"        
## [33] "a_boy"          "boy_named"      ",_in"           "in_his"        
## [37] "his_mouth"      "mouth_."

## removing stopwords before constructing ngrams
tokensAll <- tokens(tolower(testText), remove_punct = TRUE)
tokensNoStopwords <- tokens_remove(tokensAll, stopwords("en"))
tokensNgramsNoStopwords <- tokens_ngrams(tokensNoStopwords, 2)
featnames(dfm(tokensNgramsNoStopwords, verbose = FALSE))
##  [1] "quick_brown"      "brown_fox"        "fox_named"       
##  [4] "named_seamus"     "seamus_jumps"     "jumps_lazy"      
##  [7] "lazy_dog"         "dog_also"         "also_named"      
## [10] "seamus_newspaper" "newspaper_boy"    "boy_named"       
## [13] "seamus_mouth"

# keep only certain words
dfm(testCorpus, select = "*s", verbose = FALSE)  # keep only words ending in "s"
## Document-feature matrix of: 1 document, 3 features (0% sparse).
## 1 x 3 sparse Matrix of class "dfm"
##        features
## docs    seamus jumps his
##   text1      3     1   1
dfm(testCorpus, select = "s$", valuetype = "regex", verbose = FALSE)
## Document-feature matrix of: 1 document, 3 features (0% sparse).
## 1 x 3 sparse Matrix of class "dfm"
##        features
## docs    seamus jumps his
##   text1      3     1   1

# testing Twitter functions
testTweets <- c("My homie @justinbieber #justinbieber shopping in #LA yesterday #beliebers",
                "2all the ha8ers including my bro #justinbieber #emabiggestfansjustinbieber",
                "Justin Bieber #justinbieber #belieber #fetusjustin #EMABiggestFansJustinBieber")
dfm(testTweets, select = "#*", remove_twitter = FALSE)  # keep only hashtags
## Document-feature matrix of: 3 documents, 6 features (50% sparse).
## 3 x 6 sparse Matrix of class "dfm"
##        features
## docs    #justinbieber #la #beliebers #emabiggestfansjustinbieber #belieber
##   text1             1   1          1                           0         0
##   text2             1   0          0                           1         0
##   text3             1   0          0                           1         1
##        features
## docs    #fetusjustin
##   text1            0
##   text2            0
##   text3            1
dfm(testTweets, select = "^#.*$", valuetype = "regex", remove_twitter = FALSE)
## Document-feature matrix of: 3 documents, 6 features (50% sparse).
## 3 x 6 sparse Matrix of class "dfm"
##        features
## docs    #justinbieber #la #beliebers #emabiggestfansjustinbieber #belieber
##   text1             1   1          1                           0         0
##   text2             1   0          0                           1         0
##   text3             1   0          0                           1         1
##        features
## docs    #fetusjustin
##   text1            0
##   text2            0
##   text3            1
```


One very nice feature is the ability to create a new dfm with the same feature set as the old.  This is very useful, for instance, if we train a model on one dfm, and need to predict on counts from another, but need the feature set to be equivalent.

```r
# selecting on a dfm
textVec1 <- c("This is text one.", "This, the second text.", "Here: the third text.")
textVec2 <- c("Here are new words.", "New words in this text.")
featnames(dfm1 <- dfm(textVec1))
##  [1] "this"   "is"     "text"   "one"    "."      ","      "the"   
##  [8] "second" "here"   ":"      "third"
featnames(dfm2a <- dfm(textVec2))
## [1] "here"  "are"   "new"   "words" "."     "in"    "this"  "text"
(dfm2b <- dfm_select(dfm2a, dfm1))
## Document-feature matrix of: 2 documents, 11 features (77.3% sparse).
## 2 x 11 sparse Matrix of class "dfm"
##        features
## docs    this is text one . , the second here : third
##   text1    0  0    0   0 1 0   0      0    1 0     0
##   text2    1  0    1   0 1 0   0      0    0 0     0
identical(featnames(dfm1), featnames(dfm2b))
## [1] TRUE
```

### 5. Stemming

Stemming relies on the `SnowballC` package's implementation of the Porter stemmer, and is available for the following languages:

```r
SnowballC::getStemLanguages()
##  [1] "danish"     "dutch"      "english"    "finnish"    "french"    
##  [6] "german"     "hungarian"  "italian"    "norwegian"  "porter"    
## [11] "portuguese" "romanian"   "russian"    "spanish"    "swedish"   
## [16] "turkish"
```

It's not perfect:

```r
char_wordstem(c("win", "winning", "wins", "won", "winner"))
## [1] "win"    "win"    "win"    "won"    "winner"
```
but it's fast.

Stemmed objects must be tokenized, but can be of many different quanteda classes:


```r
txt <- "From 10k packages, quanteda is an text analysis package, for analysing texts."
char_wordstem(txt)
(toks <- tokens_wordstem(tokens(txt, remove_punct = TRUE)))
dfm_wordstem(dfm(toks))
```

Some text operations can be conducted directly on the dfm:


```r
dfm1 <- dfm(data_corpus_inaugural[1:2], verbose = FALSE)
head(featnames(dfm1))
```

```
## [1] "fellow-citizens" "of"              "the"             "senate"         
## [5] "and"             "house"
```

```r
head(featnames(dfm_wordstem(dfm1)))
```

```
## [1] "fellow-citizen" "of"             "the"            "senat"         
## [5] "and"            "hous"
```

```r
# same as 
head(dfm(data_corpus_inaugural[1:2], stem = TRUE, verbose = FALSE))
```

```
## Document-feature matrix of: 2 documents, 580 features (44.3% sparse).
```

### 6. `dfm()` and its many options

Operates on `character` (vectors), `corpus`, or `tokens` objects,


```r
dfm(x, tolower = TRUE, stem = FALSE, select = NULL, remove = NULL,
  thesaurus = NULL, dictionary = NULL, 
  valuetype = c("glob", "regex", "fixed"), 
  groups = NULL, 
  verbose = quanteda_options("verbose"), ...)
```







