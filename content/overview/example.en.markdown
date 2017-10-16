---
title: Example
weight: 10
draft: false
output: slidy_presentation
---



## Analyze UK party platforms
With **quanteda**, it takes only five steps to create wordcloud in R.

### 1. Load the package

```r
library(quanteda)
## quanteda version 0.99.9000
## Using 7 of 8 threads for parallel computing
## 
## Attaching package: 'quanteda'
## The following object is masked from 'package:utils':
## 
##     View
```

### 2. Create a corpus from manifestos

```r
uk2010immigCorpus <- 
    corpus(data_char_ukimmig2010,
           docvars = data.frame(party = names(data_char_ukimmig2010)),
           metacorpus = list(notes = "Immigration-related sections of 2010 UK party manifestos"))
uk2010immigCorpus
## Corpus consisting of 9 documents and 1 docvar.
summary(uk2010immigCorpus)
## Corpus consisting of 9 documents:
## 
##          Text Types Tokens Sentences        party
##           BNP  1125   3280        88          BNP
##     Coalition   142    260         4    Coalition
##  Conservative   251    499        15 Conservative
##        Greens   322    679        21       Greens
##        Labour   298    683        29       Labour
##        LibDem   251    483        14       LibDem
##            PC    77    114         5           PC
##           SNP    88    134         4          SNP
##          UKIP   346    723        27         UKIP
## 
## Source:  C:/Users/Kohei/Documents/R/quanteda_tutorials/content/overview/* on x86-64 by Kohei
## Created: Mon Oct 16 19:09:59 2017
## Notes:   Immigration-related sections of 2010 UK party manifestos
```
### 3. Check how keywords are used

```r
kwic(uk2010immigCorpus, "deport*", window = 5)
##                                                                                                            
##      [BNP, 81]       all further immigration, the | deportation  | of all illegal immigrants,              
##     [BNP, 157]                    .- The BNP will |    deport    | all foreigners convicted of crimes      
##    [BNP, 1946]         resettlement programme. 2. |    Deport    | all illegal immigrants We shall         
##    [BNP, 1952]    all illegal immigrants We shall |    deport    | all illegal immigrants and bogus        
##    [BNP, 1974] under the current unacceptably lax | deportation  | policies, thousands of people           
##    [BNP, 1981]          , thousands of people are |   deported   | from the UK annually without            
##    [BNP, 2563]       Britain, enforced by instant | deportation  | , for anyone found guilty               
##    [BNP, 2578]               immigration laws. 8. | Deportation  | of all Foreign Criminals We             
##    [BNP, 2585]     all Foreign Criminals We shall |    deport    | all criminal entrants, regardless       
##    [BNP, 2599]          status. This includes the | deportation  | of all Muslim extremists,               
##  [Greens, 631]          not be subject to summary | deportation  | . They should receive a                 
##  [LibDem, 217]        illegal labour.- Prioritise | deportation  | efforts on criminals, people-traffickers
##  [LibDem, 444]                 flight risks.- End | deportations | of refugees to countries where          
##    [UKIP, 362]           respect our laws or face | deportation  | . Such citizens will not
```

### 4. Create a document-feature matrix, removing gramatical words

```r
mydfm <- dfm(uk2010immigCorpus, remove = stopwords("english"), remove_punct = TRUE)
mydfm
## Document-feature matrix of: 9 documents, 1,547 features (83.8% sparse).
topfeatures(mydfm, 20)  # 20 top words
## immigration     british      people      asylum     britain          uk      system  population     country         new 
##          66          37          35          29          28          27          27          21          20          19 
##  immigrants      ensure       shall citizenship      social    national         bnp     illegal        work     percent 
##          17          17          17          16          14          14          13          13          13          12
```

### 5. Plot a word cloud

```r
set.seed(100)
textplot_wordcloud(mydfm, min.freq = 6, random.order = FALSE,
                   rot.per = .25, 
                   colors = RColorBrewer::brewer.pal(8,"Dark2"))
```

![plot of chunk quanteda_example](figure/quanteda_example-1.png)
