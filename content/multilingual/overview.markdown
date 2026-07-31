---
title: Overview
weight: 5
draft: false
---

Every example so far in this tutorial has used English text. That was a simplification for teaching purposes.   **quanteda** can analyse text in almost any language. Thanks to the **stringi** package and the [International Component for Unicode (ICU)](http://site.icu-project.org/home), on which **stringi** is built, **quanteda** can process all the major languages in Unicode.


``` r
library(quanteda)
library(quanteda.corpora)
```



The corpus `data_corpus_udhr` contains the Universal Declaration of Human Rights translated into over 400 languages, which makes it convenient for comparing the same text across languages. We can process European languages such as English and German, Middle Eastern languages such as Arabic and Hebrew, and East Asian languages such as Chinese and Japanese. First, we subset the languages relevant for the following tutorial pages.


``` r
corp <- data_corpus_udhr[c("eng", "deu_1996", "arb", "heb", "cmn_hans", "jpn")]
print(corp)
```

```
## Corpus consisting of 6 documents and 4 docvars.
## eng :
## "Preamble Whereas recognition of the inherent dignity and of ..."
## 
## deu_1996 :
## "Präambel Da die Anerkennung der angeborenen Würde und der gl..."
## 
## arb :
## "الديباجة لمّا كان الاعتراف بالكرامة المتأصلة في جميع أعضاء ا..."
## 
## heb :
## "הכרזה לכל באי עולם בדבר זכויות האדם הואיל והכרה בכבוד הטבעי ..."
## 
## cmn_hans :
## "序言 鉴于对人类家庭所有成员的固有尊严及其平等的和不移的权利的承认,乃是世界自由、正义与和平的基础, 鉴于对人权的无视和..."
## 
## jpn :
## "〈前文〉 人類社会のすべての構成員の固有の尊厳と平等で譲ることのできない権利とを承認することは、世界における自由、正義及..."
```

Words are separated by white space or punctuation marks in the first four languages here (English, German, Arabic, Hebrew), but not in Chinese or Japanese, which have no spaces between words at all. Morphological analysis tools such as Jieba or MeCab have traditionally been needed to segment Chinese and Japanese text into words, but `tokens()` needs no such external tool: the underlying ICU library already knows how to find word boundaries in these languages.


``` r
toks <- tokens(corp)
print(toks)
```

```
## Tokens consisting of 6 documents and 4 docvars.
## eng :
##  [1] "Preamble"    "Whereas"     "recognition" "of"          "the"         "inherent"    "dignity"    
##  [8] "and"         "of"          "the"         "equal"       "and"        
## [ ... and 1,889 more ]
## 
## deu_1996 :
##  [1] "Präambel"         "Da"               "die"              "Anerkennung"      "der"             
##  [6] "angeborenen"      "Würde"            "und"              "der"              "gleichen"        
## [11] "und"              "unveräußerlichen"
## [ ... and 1,805 more ]
## 
## arb :
##  [1] "الديباجة" "لمّا"      "كان"      "الاعتراف" "بالكرامة" "المتأصلة" "في"       "جميع"     "أعضاء"   
## [10] "الأسرة"   "البشرية"  "وبحقوقهم"
## [ ... and 1,409 more ]
## 
## heb :
##  [1] "הכרזה"  "לכל"    "באי"    "עולם"   "בדבר"   "זכויות" "האדם"   "הואיל"  "והכרה"  "בכבוד"  "הטבעי" 
## [12] "אשר"   
## [ ... and 1,459 more ]
## 
## cmn_hans :
##  [1] "序言" "鉴于" "对"   "人类" "家庭" "所有" "成员" "的"   "固有" "尊严" "及其" "平等"
## [ ... and 1,701 more ]
## 
## jpn :
##  [1] "〈"     "前文"   "〉"     "人類"   "社会"   "の"     "すべて" "の"     "構成"   "員"     "の"    
## [12] "固有"  
## [ ... and 2,415 more ]
```

The chapter explains how to preprocess each of these languages before constructing a DFM. The preprocessing details differ from language to language, mainly around stopwords and which characters count as "letters", but once a DFM is constructed, you can apply the same statistical analysis and machine learning techniques covered elsewhere in this tutorial, ignoring the syntactic and lexical differences between languages.

{{% notice note %}}
`tokens()` tokenizes Chinese and Japanese texts using a dictionary in the ICU library. The library also [detects boundaries](http://userguide.icu-project.org/boundaryanalysis) between words and other elements such as symbols and numbers, which is why the function separates punctuation marks from words even without the white space between them.
{{% /notice %}}

{{% notice warning %}}
The pages in this chapter are only a starting point for any particular language. Each one shows one reasonable way to tokenise, remove stopwords and filter characters for that language. Scripts, stopword lists and conventions vary enough between languages that no single set of defaults suits every corpus or research question. Treat these examples as a demonstration of the choices available, not a definitive workflow, and always inspect and validate your own output, as described in the [Select tokens](/basic-operations/tokens/tokens_select) page.
{{% /notice %}}
