---
title: Overview
weight: 5
draft: false
---

{{% author %}}By Kohei Watanabe{{% /author %}} 

The examples in the previous tutorials use a corpus of English texts, but we can easily analyze texts in other languages. Thanks to the creation of the **stringi** package and the recent development in [International Component for Unicode (ICU)](http://site.icu-project.org/home), we can process all the major languages in Unicode.


```r
require(quanteda)
require(quanteda.corpora)
options(width = 110)
```

The corpus `data_corpus_udhr` contains the Universal Declaration of Human Rights in over 400 languages. We can process European languages (English and German), but also Middle Eastern (Arabic and Hebrew) and East Asian (Chinese and Japanese) languages appropriately. First, we will subset languages relevant for the following tutorial pages.


```r
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

Words are segmented by the white space or punctuation marks in the first four languages (English, German, Arabic, Hebrew), but they are not in the last two languages. For this reason, morphological analysis tools such as Jieba or Mecab have been used for tokenizing Chinese and Japanese texts, but `tokens()` does not require such tools. The ability to tokenize texts in different languages makes it possible to perform multilingual quantitative text analysis.


```r
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
## [ ... and 1,801 more ]
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

This chapter explains how to preprocess these languages before constructing a DFM. Once a DFM is constructed, we can perform the statistical analysis and machine learning techniques, largely ignoring syntactical and lexical differences between languages.

{{% notice note %}}
`tokens()` tokenizes Chinese and Japanese texts using a dictionary in the ICU library. The library also [detects boundaries](http://userguide.icu-project.org/boundaryanalysis) between words and other elements such symbols and numbers. This is why the function separates punctuation marks from words even without the white space between them.
{{% /notice %}}
