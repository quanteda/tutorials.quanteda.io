---
title: English and German
weight: 10
draft: false
bibliography: ../references.bib
---

{{% author %}}By Kohei Watanabe and Stefan Müller{{% /author %}}

Here we apply the general multilingual workflow from the Overview page to two languages written with the Latin alphabet: English and German. Both follow the same three steps, tokenise, remove stopwords, then keep only genuine word characters, but the exact patterns differ slightly between the two, as the sections below show.

``` r
library(quanteda)
library(quanteda.corpora)
```

## English

The **stopwords** package, which **quanteda**’s `stopwords()` function draws on, bundles stopword lists for many languages from several different sources. After tokenisation, we remove so-called “stopwords” using `stopwords("en", source = "marimo")`, where the `source` argument picks which of these bundled lists to use; Marimo tends to include more words than the older Snowball lists you may see used elsewhere. If you want tokens to consist only of the English alphabet, you can select them with the regular expression `"^[a-zA-Z]+$"`, which discards any stray digits or symbols that survived tokenisation. You can find more details on stopwords on the [website](http://stopwords.quanteda.io) of the **stopwords** package.

{{% notice warning %}}
Be careful when pre-processing or removing tokens: as Denny and Spirling (2018) show, these choices might influence subsequent results, and the choice of stopword list is exactly this kind of decision. The [Select tokens](/basic-operations/tokens/tokens_select) page shows how to check this for your own corpus.
{{% /notice %}}

``` r
# reshape corpus to the level of paragraphs
corp_eng <- corpus_reshape(data_corpus_udhr["eng"], to = "paragraphs")

# tokenize corpus and apply pre-processing
toks_eng <- tokens(corp_eng, remove_punct = TRUE, remove_numbers = TRUE) |> 
  tokens_remove(pattern = stopwords("en", source = "marimo")) |> 
  tokens_keep(pattern = "^[a-zA-Z]+$", valuetype = "regex")
print(toks_eng[2], max_ndoc = 1, max_ntoken = -1)
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

``` r
# construct a document-feature matrix
dfmat_eng <- dfm(toks_eng)
print(dfmat_eng)
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

## German

Pre-processing of German texts follows the same three steps as English: tokenise, remove stopwords, then keep only genuine word characters. The one difference is the pattern used in that last step. A plain `a-zA-Z` pattern would silently strip out accented characters, so instead we use the [Unicode character class](http://www.unicode.org/reports/tr31/#Table_Recommended_Scripts) `"^[\\p{script=Latn}]+$"`, which matches any character in the Latin script, to correctly include characters with umlauts (ä/ö/ü).

``` r
# reshape document to the level of paragraphs
corp_ger <- corpus_reshape(data_corpus_udhr["deu_1996"], to = "paragraphs")

# tokenize corpus and apply pre-processing
toks_ger <- tokens(corp_ger, remove_punct = TRUE, remove_numbers = TRUE) |> 
  tokens_remove(pattern = stopwords("de", source = "marimo")) |> 
  tokens_keep(pattern = "^[\\p{script=Latn}]+$", valuetype = "regex")
print(toks_ger[2], max_ndoc = 1, max_ntoken = -1)
```

    ## Tokens consisting of 1 document and 4 docvars.
    ## deu_1996.2 :
    ##   [1] "Anerkennung"        "angeborenen"        "gleichen"           "unveräußerlichen"   "Rechte"            
    ##   [6] "Mitglieder"         "Gemeinschaft"       "Menschen"           "Grundlage"          "Freiheit"          
    ##  [11] "Gerechtigkeit"      "Frieden"            "Welt"               "bildet"             "Nichtanerkennung"  
    ##  [16] "Verachtung"         "Menschenrechte"     "Akten"              "Barbarei"           "geführt"           
    ##  [21] "Gewissen"           "Menschheit"         "Empörung"           "erfüllen"           "verkündet"         
    ##  [26] "worden"             "Welt"               "Menschen"           "Rede"               "Glaubensfreiheit"  
    ##  [31] "Freiheit"           "Furcht"             "Not"                "genießen"           "höchste"           
    ##  [36] "Streben"            "Menschen"           "gilt"               "notwendig"          "Menschenrechte"    
    ##  [41] "Herrschaft"         "Rechtes"            "schützen"           "Mensch"             "gezwungen"         
    ##  [46] "letztes"            "Mittel"             "Aufstand"           "Tyrannei"           "Unterdrückung"     
    ##  [51] "greifen"            "notwendig"          "Entwicklung"        "freundschaftlicher" "Beziehungen"       
    ##  [56] "Nationen"           "fördern"            "Völker"             "Vereinten"          "Nationen"          
    ##  [61] "Charta"             "ihren"              "Glauben"            "grundlegenden"      "Menschenrechte"    
    ##  [66] "Wert"               "menschlichen"       "Person"             "Gleichberechtigung" "Mann"              
    ##  [71] "Frau"               "erneut"             "bekräftigt"         "beschlossen"        "sozialen"          
    ##  [76] "Fortschritt"        "bessere"            "Lebensbedingungen"  "größerer"           "Freiheit"          
    ##  [81] "fördern"            "Mitgliedstaaten"    "verpflichtet"       "Zusammenarbeit"     "Vereinten"         
    ##  [86] "Nationen"           "allgemeine"         "Achtung"            "Einhaltung"         "Menschenrechte"    
    ##  [91] "Grundfreiheiten"    "hinzuwirken"        "gemeinsames"        "Verständnis"        "Rechte"            
    ##  [96] "Freiheiten"         "größter"            "Wichtigkeit"        "volle"              "Erfüllung"         
    ## [101] "Verpflichtung"      "verkündet"          "Generalversammlung" "Allgemeine"         "Erklärung"         
    ## [106] "Menschenrechte"     "Völkern"            "Nationen"           "erreichende"        "gemeinsame"        
    ## [111] "Ideal"              "einzelne"           "Organe"             "Gesellschaft"       "Erklärung"         
    ## [116] "gegenwärtig"        "halten"             "bemühen"            "Unterricht"         "Erziehung"         
    ## [121] "Achtung"            "Rechten"            "Freiheiten"         "fördern"            "fortschreitende"   
    ## [126] "nationale"          "internationale"     "Maßnahmen"          "allgemeine"         "tatsächliche"      
    ## [131] "Anerkennung"        "Einhaltung"         "Bevölkerung"        "Mitgliedstaaten"    "Bevölkerung"       
    ## [136] "ihrer"              "Hoheitsgewalt"      "unterstehenden"     "Gebiete"            "gewährleisten"

``` r
# construct document-feature matrix
dfmat_ger <- dfm(toks_ger)
print(dfmat_ger)
```

    ## Document-feature matrix of: 82 documents, 500 features (98.18% sparse) and 4 docvars.
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
    ## [ reached max_ndoc ... 76 more documents, reached max_nfeat ... 490 more features ]

Reuse this pattern for most other languages written with white-space-separated words, swapping only the stopword language and the Unicode script class, as the following pages show.

## References

<div id="refs" class="references csl-bib-body hanging-indent">

<div id="ref-denny2018" class="csl-entry">

Denny, Matthew J., and Arthur Spirling. 2018. “Text Preprocessing for Unsupervised Learning: Why It Matters, When It Misleads, and What to Do about It.” *Political Analysis* 26 (2): 168–89. <https://doi.org/10.1017/pan.2017.44>.

</div>

</div>
