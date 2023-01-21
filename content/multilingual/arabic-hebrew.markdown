---
title: Arabic and Hebrew
weight: 20
draft: false
---

{{% author %}}By Dai Yamao and Elad Segev{{% /author %}} 


```r
require(quanteda)
require(quanteda.corpora)
options(width = 110)
```

## Arabic

It is challenging to deal with the right-to-left languages in R because of the design of its console, but it is still possible to analyze Arabic texts.

We use the Arabic stopwords list in [Marimo](https://github.com/koheiw/marimo) `stopwords("ar", source = "marimo")`. You can also remove all the non-Arabic words with `"^[\\p{script=Arab}]+$"`.


```r
# reshape document to the level of paragraphs
corp_arb <- corpus_reshape(data_corpus_udhr["arb"], to = "paragraphs")

# tokenize corpus and apply pre-processing
toks_arb <- tokens(corp_arb, remove_punct = TRUE, remove_numbers = TRUE) %>% 
  tokens_keep(pattern = "^[\\p{script=Arab}]+$", valuetype = "regex") %>% 
  tokens_remove(pattern = stopwords("ar", source = "marimo"))
print(toks_arb[2], max_ndoc = 1, max_ntoken = -1)
```

```
## Tokens consisting of 1 document and 4 docvars.
## arb.2 :
##   [1] "الاعتراف"  "بالكرامة"  "المتأصلة"  "أعضاء"     "الأسرة"    "البشرية"   "وبحقوقهم"  "المتساوية"
##   [9] "الثابتة"   "أساس"      "الحرية"    "والعدل"    "والسلام"   "العالم"    "ولما"      "تناسي"    
##  [17] "حقوق"      "الإنسان"   "وازدراؤها" "أفضيا"     "أعمال"     "همجية"     "آذت"       "الضمير"   
##  [25] "الإنساني"  "غاية"      "يرنو"      "إليه"      "عامة"      "البشر"     "انبثاق"    "عالم"     
##  [33] "يتمتع"     "الفرد"     "بحرية"     "القول"     "والعقيدة"  "ويتحرر"    "الفزع"     "والفاقة"  
##  [41] "ولما"      "الضروري"   "يتولى"     "القانون"   "حماية"     "حقوق"      "الإنسان"   "لكيلا"    
##  [49] "يضطر"      "المرء"     "الأمر"     "التمرد"    "الاستبداد" "والظلم"    "ولما"      "شعوب"     
##  [57] "الأمم"     "المتحدة"   "أكدت"      "الميثاق"   "جديد"      "إيمانها"   "بحقوق"     "الإنسان"  
##  [65] "الأساسية"  "وبكرامة"   "الفرد"     "وقدره"     "للرجال"    "والنساء"   "حقوق"      "متساوية"  
##  [73] "وحزمت"     "أمرها"     "تدفع"      "بالرقي"    "الاجتماعي" "ترفع"      "مستوى"     "الحياة"   
##  [81] "جو"        "الحرية"    "أفسح"      "ولما"      "الدول"     "الأعضاء"   "تعهدت"     "بالتعاون" 
##  [89] "الأمم"     "المتحدة"   "ضمان"      "إطراد"     "مراعاة"    "حقوق"      "الإنسان"   "والحريات" 
##  [97] "الأساسية"  "واحترامها" "ولما"      "للإدراك"   "العام"     "الحقوق"    "والحريات"  "الأهمية"  
## [105] "الكبرى"    "للوفاء"    "التام"     "التعهد"    "الجمعية"   "العامة"    "تنادي"     "الإعلان"  
## [113] "العالمي"   "لحقوق"     "الإنسان"   "المستوى"   "المشترك"   "تستهدفه"   "كافة"      "الشعوب"   
## [121] "والأمم"    "يسعى"      "فرد"       "وهيئة"     "المجتمع"   "واضعين"    "الدوام"    "الإعلان"  
## [129] "نصب"       "أعينهم"    "توطيد"     "احترام"    "الحقوق"    "والحريات"  "طريق"      "التعليم"  
## [137] "والتربية"  "واتخاذ"    "إجراءات"   "مطردة"     "قومية"     "وعالمية"   "لضمان"     "الإعتراف" 
## [145] "ومراعاتها" "بصورة"     "عالمية"    "فعالة"     "الدول"     "الأعضاء"   "وشعوب"     "البقاع"   
## [153] "الخاضعة"   "لسلطانها"
```


```r
dfmat_arb <- dfm(toks_arb)
print(dfmat_arb)
```

```
## Document-feature matrix of: 82 documents, 611 features (98.31% sparse) and 4 docvars.
##        features
## docs    الديباجة الاعتراف بالكرامة المتأصلة أعضاء الأسرة البشرية وبحقوقهم المتساوية الثابتة
##   arb.1        1        0        0        0     0      0       0        0         0       0
##   arb.2        0        1        1        1     1      1       1        1         1       1
##   arb.3        0        0        0        0     0      0       0        0         0       0
##   arb.4        0        0        0        0     0      0       0        0         0       0
##   arb.5        0        0        0        0     0      0       0        0         0       0
##   arb.6        0        0        0        0     0      0       0        0         0       0
## [ reached max_ndoc ... 76 more documents, reached max_nfeat ... 601 more features ]
```

## Hebrew

We resort to the Hebrew stopwords list (`stopwords("he", source = "marimo")`) and the length of words (`min_nchar = 2`) to remove function words. You can also remove all the non-Hebrew words with `"^[\\p{script=Hebr}]+$"`.


```r
# reshape document to the level of paragraphs
corp_heb <- corpus_reshape(data_corpus_udhr["heb"], to = "paragraphs")

# tokenize corpus and apply pre-processing
toks_heb <- tokens(corp_heb, remove_punct = TRUE, remove_numbers = TRUE) %>% 
  tokens_select(pattern = "^[\\p{script=Hebr}]+$", valuetype = "regex") %>% 
  tokens_remove(pattern = stopwords("he", source = "marimo"), min_nchar = 2)
print(toks_heb[2], max_ndoc = 1, max_ntoken = -1)
```

```
## Tokens consisting of 1 document and 4 docvars.
## heb.2 :
##   [1] "הואיל"       "והכרה"       "בכבוד"       "הטבעי"       "אשר"         "בני"         "משפהת"      
##   [8] "האדם"        "ובזכויותיהם" "השוות"       "והבלתי"      "נפקעות"      "יסוד"        "החופש"      
##  [15] "הצדק"        "והשלום"      "בעולם"       "הואיל"       "והזלזול"     "בזכויות"     "האדם"       
##  [22] "וביזוין"     "הבשילו"      "מעשים"       "פראיים"      "שפגעו"       "קשה"         "במצפונה"    
##  [29] "האנושות"     "ובנין"       "עולם"        "ייהנו"       "יצורי"       "אנוש"        "מחירות"     
##  [36] "הדיבור"      "והאמונה"     "ומן"         "החירות"      "מפחד"        "וממחסור"     "הוכרז"      
##  [43] "כראש"        "שאיפותיו"    "אדם"         "הואיל"       "והכרח"       "שזכויות"     "האדם"       
##  [50] "תהיינה"      "מוגנות"      "בכוח"        "שלטונו"      "החוק"        "יהא"         "האדם"       
##  [57] "אנוס"        "כמפלט"       "אחרון"       "להשליך"      "יהבו"        "מרידה"       "בעריצות"    
##  [64] "ובדיכזי"     "הואיל"       "והכרח"       "לקדם"        "התפתחותם"    "יחסי"        "ידידות"     
##  [71] "האומות"      "הואיל"       "והעמים"      "המאוגדים"    "בארגון"      "האומות"      "המאוחדות"   
##  [78] "חזרו"        "ואישרו"      "במגילה"      "אמונתם"      "בזכויות"     "היסוד"       "האדם"       
##  [85] "בכבודה"      "ובערכה"      "אישיותו"     "ובזכות"      "שווה"        "לגבר"        "ולאשה"      
##  [92] "ומנוי"       "וגמור"       "לסייע"       "לקדמה"       "חברתית"      "ולהעלאת"     "רמת"        
##  [99] "החיים"       "יתר"         "חירות"       "הואיל"       "והמדינות"    "החברות"      "התחייבו"    
## [106] "לפעול"       "בשיתוף"      "ארגון"       "האומות"      "המאוחדות"    "לטיפול"      "יחס"        
## [113] "כבוד"        "כללי"        "זכויות"      "האדם"        "חירויות"     "היסוד"       "והקפדה"     
## [120] "קיומן"       "הואיל"       "והבנה"       "משותפת"      "במהותן"      "זכויות"      "וחירויות"   
## [127] "תנאי"        "לקיומה"      "השלם"        "התחייבות"    "לפיכך"       "מכריזש"      "העצרת"      
## [134] "באזני"       "באי"         "העולם"       "ההכרזש"      "הזאת"        "בדבר"        "זכויות"     
## [141] "האדם"        "כרמת"        "הישגים"      "כללית"       "העמים"       "והאומות"     "כדי"        
## [148] "יחיד"        "גוף"         "חברתי"       "ישווה"       "תמיד"        "עיניו"       "וישאף"      
## [155] "לטפח"        "לימוד"       "וחינוך"      "יחס"         "כבוד"        "הזכויות"     "החירויות"   
## [162] "ולהבטיח"     "באמצעים"     "הדרגתיים"    "לאומיים"     "ובינלאומיים" "שההכרה"      "בעקרונות"   
## [169] "וההקפדה"     "עליהם"       "תהא"         "כללית"       "ויעילה"      "בקרב"        "אוכלוסי"    
## [176] "המדינות"     "החברות"      "ובקרב"       "האוכלוסים"   "שבארצות"     "שיפוטם"
```


```r
# construct document-feature matrix
dfmat_heb <- dfm(toks_heb)
print(dfmat_heb)
```

```
## Document-feature matrix of: 82 documents, 690 features (98.40% sparse) and 4 docvars.
##        features
## docs    הכרזה באי עולם בדבר זכויות האדם הואיל והכרה בכבוד הטבעי
##   heb.1     1   1    1    1      1    1     0     0     0     0
##   heb.2     0   1    1    1      3    7     7     1     1     1
##   heb.3     0   0    0    0      0    0     0     0     0     0
##   heb.4     0   0    0    0      0    0     0     0     0     0
##   heb.5     0   0    0    0      0    0     0     0     0     0
##   heb.6     0   0    0    0      0    0     0     0     0     0
## [ reached max_ndoc ... 76 more documents, reached max_nfeat ... 680 more features ]
```

{{% notice note %}}
Analysis of right-to-left language is easiest on Linux because it supports Unicode better than Windows and in Mac.
{{% /notice %}}
