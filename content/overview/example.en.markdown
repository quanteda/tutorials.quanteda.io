---
title: Example
weight: 10
---



## Visualizing UK party platforms
With **quanted**, it takes only five steps to create wordcloud in R.

### 1. Load the package

```r
library(quanteda)
## quanteda version 0.99.13
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
## Source:  /home/kohei/packages/quanteda_tutorials/content/overview/* on x86_64 by kohei
## Created: Mon Oct  9 18:08:55 2017
## Notes:   Immigration-related sections of 2010 UK party manifestos
```
### 3. Check how keywords are used

```r
kwic(uk2010immigCorpus, "deport*", window = 3)
##                                                         
##      [BNP, 81]         immigration, the | deportation  |
##     [BNP, 157]             The BNP will |    deport    |
##    [BNP, 1946]                     . 2. |    Deport    |
##    [BNP, 1952]      immigrants We shall |    deport    |
##    [BNP, 1974] current unacceptably lax | deportation  |
##    [BNP, 1981]            of people are |   deported   |
##    [BNP, 2563]      enforced by instant | deportation  |
##    [BNP, 2578]                     . 8. | Deportation  |
##    [BNP, 2585]       Criminals We shall |    deport    |
##    [BNP, 2599]        This includes the | deportation  |
##  [Greens, 631]       subject to summary | deportation  |
##  [LibDem, 217]            .- Prioritise | deportation  |
##  [LibDem, 444]                   .- End | deportations |
##    [UKIP, 362]             laws or face | deportation  |
##                          
##  of all illegal          
##  all foreigners convicted
##  all illegal immigrants  
##  all illegal immigrants  
##  policies, thousands     
##  from the UK             
##  , for anyone            
##  of all Foreign          
##  all criminal entrants   
##  of all Muslim           
##  . They should           
##  efforts on criminals    
##  of refugees to          
##  . Such citizens
```

### 4. Create a document-feature matrix, removing gramatical words

```r
mydfm <- dfm(uk2010immigCorpus, remove = stopwords("english"), remove_punct = TRUE)
mydfm
## Document-feature matrix of: 9 documents, 1,547 features (83.8% sparse).
topfeatures(mydfm, 20)  # 20 top words
## immigration     british      people      asylum     britain          uk 
##          66          37          35          29          28          27 
##      system  population     country         new  immigrants      ensure 
##          27          21          20          19          17          17 
##       shall citizenship      social    national         bnp     illegal 
##          17          16          14          14          13          13 
##        work     percent 
##          13          12
```

### 5. Plot a word cloud

```r
set.seed(100)
textplot_wordcloud(mydfm, min.freq = 6, random.order = FALSE,
                   rot.per = .25, 
                   colors = RColorBrewer::brewer.pal(8,"Dark2"))
```

<img src="/overview/example.en_files/figure-html/quanteda_example-1.svg" width="768" />
