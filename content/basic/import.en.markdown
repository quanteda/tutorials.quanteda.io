---
title: Data import
weight: 10
chapter: false
draft: true
---



## Getting texts into R

In this section we will show how to load texts from different sources and create a `corpus` object in **quanteda**.

## Creating a `corpus` object

**quanteda can construct a `corpus` object** from several input sources:

###  a character vector object  

```r
require(quanteda, warn.conflicts = FALSE, quietly = TRUE)
myCorpus <- corpus(data_char_ukimmig2010, notes = "My first corpus")
## Warning in corpus.character(data_char_ukimmig2010, notes = "My first
## corpus"): Argument notes not used.
summary(myCorpus)
## Corpus consisting of 9 documents:
## 
##          Text Types Tokens Sentences
##           BNP  1125   3280        88
##     Coalition   142    260         4
##  Conservative   251    499        15
##        Greens   322    679        21
##        Labour   298    683        29
##        LibDem   251    483        14
##            PC    77    114         5
##           SNP    88    134         4
##          UKIP   346    723        27
## 
## Source:  C:/Users/Kohei/Documents/R/quanteda_tutorials/content/basic/* on x86-64 by Kohei
## Created: Wed Oct 18 11:31:31 2017
## Notes:
```
    
###  a `VCorpus` object from the **tm** package, and

```r
data(crude, package = "tm")
myTmCorpus <- corpus(crude)
summary(myTmCorpus, 5)
## Corpus consisting of 20 documents, showing 5 documents:
## 
##  Text Types Tokens Sentences       datetimestamp description
##   127    62    103         5 1987-02-26 17:00:56            
##   144   238    495        19 1987-02-26 17:34:11            
##   191    47     62         4 1987-02-26 18:18:00            
##   194    55     74         5 1987-02-26 18:21:01            
##   211    67     97         4 1987-02-26 19:00:57            
##                                          heading  id language
##         DIAMOND SHAMROCK (DIA) CUTS CRUDE PRICES 127       en
##  OPEC MAY HAVE TO MEET TO FIRM PRICES - ANALYSTS 144       en
##        TEXACO CANADA <TXC> LOWERS CRUDE POSTINGS 191       en
##        MARATHON PETROLEUM REDUCES CRUDE POSTINGS 194       en
##        HOUSTON OIL <HO> RESERVES STUDY COMPLETED 211       en
##             origin topics lewissplit     cgisplit oldid places
##  Reuters-21578 XML    YES      TRAIN TRAINING-SET  5670    usa
##  Reuters-21578 XML    YES      TRAIN TRAINING-SET  5687    usa
##  Reuters-21578 XML    YES      TRAIN TRAINING-SET  5734 canada
##  Reuters-21578 XML    YES      TRAIN TRAINING-SET  5737    usa
##  Reuters-21578 XML    YES      TRAIN TRAINING-SET  5754    usa
##                      author orgs people exchanges
##                        <NA> <NA>   <NA>      <NA>
##  BY TED D'AFFLISIO, Reuters opec   <NA>      <NA>
##                        <NA> <NA>   <NA>      <NA>
##                        <NA> <NA>   <NA>      <NA>
##                        <NA> <NA>   <NA>      <NA>
## 
## Source:  Converted from tm Corpus 'crude'
## Created: Wed Oct 18 11:31:31 2017
## Notes:
```

###  Texts read by the **readtext** package

The **quanteda** package works nicely with a companion package we have written named, descriptively, [**readtext**](https://github.com/kbenoit/readtext).  **readtext** is a one-function package that does exactly what it says on the tin: It reads files containing text, along with any associated document-level metadata, which we call "docvars", for document variables.  Plain text files do not have docvars, but other forms such as .csv, .tab, .xml, and .json files usually do.  

**readtext** accepts filemasks, so that you can specify a pattern to load multiple texts, and these texts can even be of multiple types.  **readtext** is smart enough to process them correctly, returning a data.frame with a primary field "text" containing a character vector of the texts, and additional columns of the data.frame as found in the document variables from the source files.

As encoding can also be a challenging issue for those reading in texts, we include functions for diagnosing encodings on a file-by-file basis, and allow you to specify vectorized input encodings to read in file types with individually set (and different) encodings.  (All ecnoding functions are handled by the **stringi** package.)

To install **readtext**, you will need to use the **devtools** package, and then issue this command:

```r
# devtools packaged required to install readtext from Github 
devtools::install_github("kbenoit/readtext") 
```


## Using `readtext()` to import texts

In the simplest case, we would like to load a set of texts in plain text files from a single directory. To do this, we use the `textfile` command, and use the 'glob' operator '*' to indicate that we want to load multiple files:


```r
require(readtext)
## Loading required package: readtext
myCorpus <- corpus(readtext("inaugural/*.txt"))
myCorpus <- corpus(readtext("sotu/*.txt"))
```

Often, we have metadata encoded in the names of the files. For example, the inaugural addresses contain the year and the president's name in the name of the file. With the `docvarsfrom` argument, we can instruct the `textfile` command to consider these elements as document variables.


```r
mytf <- readtext("inaugural/*.txt", docvarsfrom = "filenames", dvsep = "-", 
                 docvarnames = c("Year", "President"))
data_corpus_inaugural <- corpus(mytf)
summary(data_corpus_inaugural, 5)
## Corpus consisting of 57 documents, showing 5 documents:
## 
##   Text Types Tokens Sentences              doc_id Year  President
##  text1   625   1538        23 1789-Washington.txt 1789 Washington
##  text2    96    147         4 1793-Washington.txt 1793 Washington
##  text3   826   2578        37      1797-Adams.txt 1797      Adams
##  text4   717   1927        41  1801-Jefferson.txt 1801  Jefferson
##  text5   804   2381        45  1805-Jefferson.txt 1805  Jefferson
## 
## Source:  C:/Users/Kohei/Documents/R/quanteda_tutorials/content/basic/* on x86-64 by Kohei
## Created: Wed Oct 18 11:31:34 2017
## Notes:
```

If the texts and document variables are stored separately, we can easily add document variables to the corpus, as long as the data frame containing them is of the same length as the texts:


```r
SOTUdocvars <- read.csv("SOTU_metadata.csv", stringsAsFactors = FALSE)
SOTUdocvars$Date <- as.Date(SOTUdocvars$Date, "%B %d, %Y")
SOTUdocvars$delivery <- as.factor(SOTUdocvars$delivery)
SOTUdocvars$type <- as.factor(SOTUdocvars$type)
SOTUdocvars$party <- as.factor(SOTUdocvars$party)
SOTUdocvars$nwords <- NULL

sotuCorpus <- corpus(readtext("sotu/*.txt", encoding = "UTF-8-BOM"))
docvars(sotuCorpus) <- SOTUdocvars
```

On some non-English Windows operation systems, the `"%B %d, %Y"` pattern does not work. If it happens, change locale (language setting) temporarily:

```r
lct <- Sys.getlocale("LC_TIME") # get original locale
Sys.setlocale("LC_TIME", "C") # set C's basic locale
SOTUdocvars$Date <- as.Date(SOTUdocvars$Date, "%B %d, %Y")
Sys.setlocale("LC_TIME", lct) # set original locale

```

Another common case is that our texts are stored alongside the document variables in a structured file, such as a json, csv or excel file. The textfile command can read in the texts and document variables simultaneously from these files when the name of the field containing the texts is specified.

```r
tf1 <- readtext("inaugTexts.csv", textfield = "inaugSpeech")
## Warning in readtext("inaugTexts.csv", textfield = "inaugSpeech"): textfield
## is deprecated; use text_field instead
myCorpus <- corpus(tf1)


tf2 <- readtext("text_example.csv", textfield = "Title")
## Warning in readtext("text_example.csv", textfield = "Title"): textfield is
## deprecated; use text_field instead
myCorpus <- corpus(tf2)
head(docvars(tf2))
##   VOTE DocumentNo CouncilLink         Year Date.of.Meeting VotingDate
## 1    1    5728/99             Jan-Dec 1999      08/02/1999 08/02/1999
## 2    2    5728/99             Jan-Dec 1999      08/02/1999 08/02/1999
## 3    3    5728/99             Jan-Dec 1999      08/02/1999 08/02/1999
## 4    4    5728/99             Jan-Dec 1999      08/02/1999 08/02/1999
## 5    5    5728/99             Jan-Dec 1999      08/02/1999 08/02/1999
## 6    6    6327/99             Jan-Dec 1999      22/02/1999 22/02/1999
##                                       Policy.area Council.configuration
## 1 Employment, Education, Culture & Social Affairs                ECOFIN
## 2 Employment, Education, Culture & Social Affairs                ECOFIN
## 3 Employment, Education, Culture & Social Affairs                ECOFIN
## 4                         Agriculture & Fisheries                ECOFIN
## 5                  Transport & Telecommunications                ECOFIN
## 6                    Economic & Financial Affairs       General Affairs
##   Procedure
## 1         1
## 2         1
## 3         0
## 4         0
## 5         0
## 6         0
```

## Working with corpus objects

Once the we have loaded a corpus with some document level variables, we can subset the corpus using these variables, create document-feature matrices by aggregating on the variables, or extract the texts concatenated by variable.


```r
recentCorpus <- corpus_subset(data_corpus_inaugural, Year > 1980)
oldCorpus <- corpus_subset(data_corpus_inaugural, Year < 1880)

require(magrittr)
## Loading required package: magrittr
demCorpus <- corpus_subset(sotuCorpus, party == 'Democratic')
demFeatures <- dfm(demCorpus, remove = stopwords('english')) %>%
    dfm_trim(min_doc = 3, min_count = 5) %>% 
    dfm_weight(type='tfidf')
topfeatures(demFeatures)
##        -   mexico  tonight        $ treasury     jobs     1947     help 
## 427.9556 198.8482 182.1753 153.5178 148.0181 139.5461 122.5412 120.5252 
##      u.s   duties 
## 120.0403 118.3891

repCorpus <- corpus_subset(sotuCorpus, party == 'Republican') 
repFeatures <- dfm(repCorpus, remove = stopwords('english')) %>%
    dfm_trim(min_doc = 3, min_count = 5) %>% 
    dfm_weight(type = 'tfidf')
topfeatures(repFeatures)
##            -         upon      islands      tonight corporations 
##    246.36857    119.75093    110.59322    110.34032     99.33542 
##         cent       tariff         navy      program      subject 
##     97.52065     95.78987     92.06111     91.09733     90.93973
```

The **quanteda** corpus objects can be combined using the `+` operator:

```r
data_corpus_inaugural2 <- demCorpus + repCorpus
dfm(data_corpus_inaugural2, remove = stopwords('english'), verbose = FALSE) %>%
    dfm_trim(min_doc = 3, min_count = 5) %>% 
    dfm_weight(type = 'tfidf') %>% 
    topfeatures
##        -  tonight           mexico     jobs     upon  subject        $ 
## 659.9549 291.1009 238.4091 233.0129 217.3148 215.4849 207.0569 206.7664 
##  program     cent 
## 206.7522 195.8858
```

It should also be possible to load a zip file containing texts directly from a url. However, whether this operation succeeds or not can depend on access permission settings on your particular system (i.e. fails on Windows):


```r
immigfiles <- readtext("https://github.com/kbenoit/ME114/raw/master/day8/UKimmigTexts.zip")
mycorpus <- corpus(immigfiles)
summary(mycorpus)
```
