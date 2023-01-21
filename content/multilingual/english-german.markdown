---
title: English and German
weight: 10
draft: false
---

{{% author %}}By Kohei Watanabe and Stefan Müller{{% /author %}} 


```r
require(quanteda)
require(quanteda.corpora)
options(width = 110)
```

## English 

After tokenization, we remove so called "stopwords" using `stopwords("en", source = "marimo")`. If you want tokens to comprise only of the English alphabet, you can select them by `"^[a-zA-Z]+$"`. You can find more details on stopwords on the [website](http://stopwords.quanteda.io) of the **stopwords** package.  Please be very careful when pre-processing or removing tokens since these choices [might influence subsequent results](https://doi.org/10.1017/pan.2017.44).


```r
# reshape corpus to the level of paragraphs
corp_eng <- corpus_reshape(data_corpus_udhr["eng"], to = "paragraphs")

# tokenize corpus and apply pre-processing
toks_eng <- tokens(corp_eng, remove_punct = TRUE, remove_numbers = TRUE) %>% 
  tokens_remove(pattern = stopwords("en", source = "marimo")) %>% 
  tokens_keep(pattern = "^[a-zA-Z]+$", valuetype = "regex")
print(toks_eng[2], max_ndoc = 1, max_ntoken = -1)
```

```
## Tokens consisting of 1 document and 4 docvars.
## eng.2 :
##   [1] "Whereas"       "recognition"   "inherent"      "dignity"       "equal"         "inalienable"  
##   [7] "rights"        "members"       "human"         "family"        "foundation"    "freedom"      
##  [13] "justice"       "peace"         "world"         "Whereas"       "disregard"     "contempt"     
##  [19] "human"         "rights"        "resulted"      "barbarous"     "acts"          "outraged"     
##  [25] "conscience"    "mankind"       "advent"        "world"         "human"         "beings"       
##  [31] "shall"         "enjoy"         "freedom"       "speech"        "belief"        "freedom"      
##  [37] "fear"          "want"          "proclaimed"    "highest"       "aspiration"    "common"       
##  [43] "people"        "Whereas"       "essential"     "man"           "compelled"     "recourse"     
##  [49] "last"          "resort"        "rebellion"     "tyranny"       "oppression"    "human"        
##  [55] "rights"        "protected"     "rule"          "law"           "Whereas"       "essential"    
##  [61] "promote"       "development"   "friendly"      "relations"     "nations"       "Whereas"      
##  [67] "peoples"       "United"        "Nations"       "Charter"       "reaffirmed"    "faith"        
##  [73] "fundamental"   "human"         "rights"        "dignity"       "worth"         "human"        
##  [79] "person"        "equal"         "rights"        "men"           "women"         "determined"   
##  [85] "promote"       "social"        "progress"      "better"        "standards"     "life"         
##  [91] "larger"        "freedom"       "Whereas"       "Member"        "States"        "pledged"      
##  [97] "achieve"       "United"        "Nations"       "promotion"     "universal"     "respect"      
## [103] "observance"    "human"         "rights"        "fundamental"   "freedoms"      "Whereas"      
## [109] "common"        "understanding" "rights"        "freedoms"      "greatest"      "importance"   
## [115] "full"          "realization"   "pledge"        "Now"           "therefore"     "General"      
## [121] "Assembly"      "Proclaims"     "Universal"     "Declaration"   "Human"         "Rights"       
## [127] "common"        "standard"      "achievement"   "peoples"       "nations"       "end"          
## [133] "every"         "individual"    "every"         "organ"         "society"       "keeping"      
## [139] "Declaration"   "constantly"    "mind"          "shall"         "strive"        "teaching"     
## [145] "education"     "promote"       "respect"       "rights"        "freedoms"      "progressive"  
## [151] "measures"      "national"      "international" "secure"        "universal"     "effective"    
## [157] "recognition"   "observance"    "among"         "peoples"       "Member"        "States"       
## [163] "among"         "peoples"       "territories"   "jurisdiction"
```


```r
# construct a document-feature matrix
dfmat_eng <- dfm(toks_eng)
print(dfmat_eng)
```

```
## Document-feature matrix of: 82 documents, 431 features (97.82% sparse) and 4 docvars.
##        features
## docs    preamble whereas recognition inherent dignity equal inalienable rights members human
##   eng.1        1       0           0        0       0     0           0      0       0     0
##   eng.2        0       7           2        1       2     2           1      9       1     8
##   eng.3        0       0           0        0       0     0           0      0       0     0
##   eng.4        0       0           0        0       1     1           0      1       0     1
##   eng.5        0       0           0        0       0     0           0      0       0     0
##   eng.6        0       0           0        0       0     0           0      1       0     0
## [ reached max_ndoc ... 76 more documents, reached max_nfeat ... 421 more features ]
```

## German

Pre-processing of German texts is very similar to English texts, but we have to use [Unicode character class](http://www.unicode.org/reports/tr31/#Table_Recommended_Scripts) `"^[\\p{script=Latn}]+$"` to include characters with umlauts (ä/ö/ü).


```r
# reshape document to the level of paragraphs
corp_ger <- corpus_reshape(data_corpus_udhr["deu_1996"], to = "paragraphs")

# tokenize corpus and apply pre-processing
toks_ger <- tokens(corp_ger, remove_punct = TRUE, remove_numbers = TRUE) %>% 
  tokens_remove(pattern = stopwords("de", source = "marimo")) %>% 
  tokens_keep(pattern = "^[\\p{script=Latn}]+$", valuetype = "regex")
print(toks_ger[2], max_ndoc = 1, max_ntoken = -1)
```

```
## Tokens consisting of 1 document and 4 docvars.
## deu_1996.2 :
##   [1] "Anerkennung"        "angeborenen"        "gleichen"           "unveräußerlichen"   "Rechte"            
##   [6] "Mitglieder"         "Gemeinschaft"       "Menschen"           "Grundlage"          "Freiheit"          
##  [11] "Gerechtigkeit"      "Frieden"            "Welt"               "bildet"             "Nichtanerkennung"  
##  [16] "Verachtung"         "Menschenrechte"     "Akten"              "Barbarei"           "geführt"           
##  [21] "Gewissen"           "Menschheit"         "Empörung"           "erfüllen"           "verkündet"         
##  [26] "worden"             "Welt"               "Menschen"           "Glaubensfreiheit"   "Freiheit"          
##  [31] "Furcht"             "Not"                "genießen"           "höchste"            "Streben"           
##  [36] "Menschen"           "gilt"               "notwendig"          "Menschenrechte"     "Herrschaft"        
##  [41] "Rechtes"            "schützen"           "Mensch"             "gezwungen"          "letztes"           
##  [46] "Mittel"             "Aufstand"           "Tyrannei"           "Unterdrückung"      "greifen"           
##  [51] "notwendig"          "Entwicklung"        "freundschaftlicher" "Beziehungen"        "Nationen"          
##  [56] "fördern"            "Völker"             "Vereinten"          "Nationen"           "Charta"            
##  [61] "ihren"              "Glauben"            "grundlegenden"      "Menschenrechte"     "Wert"              
##  [66] "menschlichen"       "Person"             "Gleichberechtigung" "Mann"               "Frau"              
##  [71] "erneut"             "bekräftigt"         "beschlossen"        "sozialen"           "Fortschritt"       
##  [76] "bessere"            "Lebensbedingungen"  "größerer"           "Freiheit"           "fördern"           
##  [81] "Mitgliedstaaten"    "verpflichtet"       "Zusammenarbeit"     "Vereinten"          "Nationen"          
##  [86] "allgemeine"         "Achtung"            "Einhaltung"         "Menschenrechte"     "Grundfreiheiten"   
##  [91] "hinzuwirken"        "gemeinsames"        "Verständnis"        "Rechte"             "Freiheiten"        
##  [96] "größter"            "Wichtigkeit"        "volle"              "Erfüllung"          "Verpflichtung"     
## [101] "verkündet"          "Generalversammlung" "Allgemeine"         "Erklärung"          "Menschenrechte"    
## [106] "Völkern"            "Nationen"           "erreichende"        "gemeinsame"         "Ideal"             
## [111] "einzelne"           "Organe"             "Gesellschaft"       "Erklärung"          "gegenwärtig"       
## [116] "halten"             "bemühen"            "Unterricht"         "Erziehung"          "Achtung"           
## [121] "Rechten"            "Freiheiten"         "fördern"            "fortschreitende"    "nationale"         
## [126] "internationale"     "Maßnahmen"          "allgemeine"         "tatsächliche"       "Anerkennung"       
## [131] "Einhaltung"         "Bevölkerung"        "Mitgliedstaaten"    "Bevölkerung"        "ihrer"             
## [136] "Hoheitsgewalt"      "unterstehenden"     "Gebiete"            "gewährleisten"
```


```r
# construct document-feature matrix
dfmat_ger <- dfm(toks_ger)
print(dfmat_ger)
```

```
## Document-feature matrix of: 82 documents, 496 features (98.18% sparse) and 4 docvars.
##             features
## docs         präambel anerkennung angeborenen gleichen unveräußerlichen rechte mitglieder gemeinschaft
##   deu_1996.1        1           0           0        0                0      0          0            0
##   deu_1996.2        0           2           1        1                1      2          1            1
##   deu_1996.3        0           0           0        0                0      0          0            0
##   deu_1996.4        0           0           0        0                0      0          0            0
##   deu_1996.5        0           0           0        0                0      0          0            0
##   deu_1996.6        0           0           0        0                0      1          0            0
##             features
## docs         menschen grundlage
##   deu_1996.1        0         0
##   deu_1996.2        3         1
##   deu_1996.3        0         0
##   deu_1996.4        1         0
##   deu_1996.5        0         0
##   deu_1996.6        0         0
## [ reached max_ndoc ... 76 more documents, reached max_nfeat ... 486 more features ]
```
