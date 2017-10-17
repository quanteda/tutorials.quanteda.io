
### 1. Load the package

```r
library(quanteda)
```

```
## quanteda version 0.99.9000
```

```
## Using 7 of 8 threads for parallel computing
```

```
## 
## Attaching package: 'quanteda'
```

```
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
```

```
## Corpus consisting of 9 documents and 1 docvar.
```

```r
summary(uk2010immigCorpus)
```

```
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
## Created: Tue Oct 17 08:23:47 2017
## Notes:   Immigration-related sections of 2010 UK party manifestos
```
### 3. Check how keywords are used

```r
kwic(uk2010immigCorpus, "deport*", window = 5)
```

```
##                                                                   
##      [BNP, 81]       all further immigration, the | deportation  |
##     [BNP, 157]                    .- The BNP will |    deport    |
##    [BNP, 1946]         resettlement programme. 2. |    Deport    |
##    [BNP, 1952]    all illegal immigrants We shall |    deport    |
##    [BNP, 1974] under the current unacceptably lax | deportation  |
##    [BNP, 1981]          , thousands of people are |   deported   |
##    [BNP, 2563]       Britain, enforced by instant | deportation  |
##    [BNP, 2578]               immigration laws. 8. | Deportation  |
##    [BNP, 2585]     all Foreign Criminals We shall |    deport    |
##    [BNP, 2599]          status. This includes the | deportation  |
##  [Greens, 631]          not be subject to summary | deportation  |
##  [LibDem, 217]        illegal labour.- Prioritise | deportation  |
##  [LibDem, 444]                 flight risks.- End | deportations |
##    [UKIP, 362]           respect our laws or face | deportation  |
##                                          
##  of all illegal immigrants,              
##  all foreigners convicted of crimes      
##  all illegal immigrants We shall         
##  all illegal immigrants and bogus        
##  policies, thousands of people           
##  from the UK annually without            
##  , for anyone found guilty               
##  of all Foreign Criminals We             
##  all criminal entrants, regardless       
##  of all Muslim extremists,               
##  . They should receive a                 
##  efforts on criminals, people-traffickers
##  of refugees to countries where          
##  . Such citizens will not
```

### 4. Create a document-feature matrix, removing gramatical words

```r
mydfm <- dfm(uk2010immigCorpus, remove = stopwords("english"), remove_punct = TRUE)
mydfm
```

```
## Document-feature matrix of: 9 documents, 1,547 features (83.8% sparse).
```

```r
topfeatures(mydfm, 20)  # 20 top words
```

```
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

<img src="/overview/example_main.en_files/figure-html/quanteda_example-1.svg" width="768" />
