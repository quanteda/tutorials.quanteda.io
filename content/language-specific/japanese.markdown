  ---
title: Japanese
weight: 30
draft: false
---

{{% author %}}By Kohei Watanabe{{% /author %}} 

We usually do not need to use external tools such as Mecab to pre-process and analyze Japanese texts, because `tokens()` can segment Japanese texts based on rules defined in the Unicode library. 


```r
require(quanteda)
require(quanteda.corpora)
options(width = 110)
```

`data_corpus_udhr` contains the Universal Declaration of Human Rights in multiple languages including Japanese. After tokenization, we remove grammatical words using `stopwords("ja", source = "marimo")`.


```r
corp <- corpus_reshape(data_corpus_udhr["jpn"], to = "paragraphs")
toks <- tokens(corp, remove_punct = TRUE, remove_numbers = TRUE, padding = TRUE) %>% 
  tokens_remove(stopwords("ja", source = "marimo"), padding = TRUE)

print(toks[2], max_ndoc = 1, max_ntok = -1)
```

```
## Tokens consisting of 1 document and 4 docvars.
## jpn :
##   [1] ""         "前文"     ""         "人類"     "社会"     "の"       ""         "の"       "構成"    
##  [10] "員"       "の"       "固有"     "の"       "尊厳"     "と"       "平等"     "で"       "譲る"    
##  [19] "こと"     "の"       ""         ""         "権利"     "と"       "を"       "承認"     ""        
##  [28] "こと"     "は"       ""         "世界"     "における" "自由"     ""         "正義"     "及び"    
##  [37] "平和"     "の"       "基礎"     "で"       ""         "ので"     ""         "人権"     "の"      
##  [46] "無視"     "及び"     "軽侮"     "が"       ""         "人類"     "の"       "良心"     "を"      
##  [55] "踏"       "み"       "に"       "じ"       "っ"       "た"       "野蛮"     "行為"     "を"      
##  [64] "も"       "たら"     "し"       ""         "言論"     "及び"     "信仰"     "の"       "自由"    
##  [73] "が"       "受け"     "ら"       "れ"       ""         "恐怖"     "及び"     "欠乏"     "の"      
##  [82] ""         "世界"     "の"       "到来"     "が"       ""         "一般"     "の"       "人々"    
##  [91] "の"       "最高"     "の"       "願望"     ""         "宣言"     "さ"       "れ"       "た"      
## [100] "ので"     ""         "人間"     "が"       "専制"     "と"       "圧迫"     "と"       ""        
## [109] "最後"     "の"       "手段"     ""         "反逆"     "に"       "訴える"   "こと"     "が"      
## [118] ""         ""         "に"       ""         ""         "に"       "は"       ""         "法"      
## [127] "の"       "支配"     ""         "人権"     "を"       "保護"     ""         "こと"     "が"      
## [136] "肝要"     "で"       ""         "ので"     ""         "諸国"     ""         "の"       "友好"    
## [145] "関係"     "の"       "発展"     "を"       "促進"     ""         "こと"     "が"       "肝要"    
## [154] "で"       ""         "ので"     ""         "国際"     "連合"     "の"       "諸"       "国民"    
## [163] "は"       ""         "国"       "連"       "憲章"     "において" ""         "基本"     "的"      
## [172] "人権"     ""         "人間"     "の"       "尊厳"     "及び"     "価値"     "並びに"   "男女"    
## [181] "の"       "同権"     ""         "の"       "信念"     "を"       "再"       "確認"     "し"      
## [190] ""         "かつ"     ""         ""         "大きな"   "自由"     "の"       "うち"     "で"      
## [199] "社会"     "的"       "進歩"     "と"       "生活"     "水準"     "の"       "向上"     "と"      
## [208] "を"       "促進"     ""         "こと"     "を"       "決意"     ""         "ので"     ""        
## [217] "加盟"     "国"       "は"       ""         "国際"     "連合"     "と"       "協力"     "し"      
## [226] "て"       ""         "人権"     "及び"     "基本"     "的"       "自由"     "の"       "普遍"    
## [235] "的"       "な"       "尊重"     "及び"     "遵守"     "の"       "促進"     "を"       "達成"    
## [244] ""         "こと"     "を"       "誓約"     ""         "ので"     ""         ""         "権利"    
## [253] "及び"     "自由"     ""         "共通"     "の"       "理解"     "は"       ""         ""        
## [262] "誓約"     "を"       "完全"     "に"       ""         ""         "に"       "もっとも" "重要"    
## [271] "で"       ""         "ので"     ""         ""         ""         ""         "に"       ""        
## [280] "国"       "連"       "総会"     "は"       ""         "社会"     "の"       "各"       "個人"    
## [289] "及び"     "各"       "機関"     "が"       ""         ""         "世界"     "人権"     "宣言"    
## [298] "を"       "常に"     "念頭"     "に"       "置"       "き"       ""         ""         "加盟"    
## [307] "国"       ""         "の"       "人民"     "の"       ""         "に"       "も"       ""        
## [316] ""         ""         "加盟"     "国"       "の"       "管轄"     ""         "に"       ""        
## [325] "地域"     "の"       "人民"     "の"       ""         "に"       "も"       ""         ""        
## [334] "権利"     "と"       "自由"     "と"       "の"       "尊重"     "を"       "指導"     "及び"    
## [343] "教育"     ""         "促進"     ""         "こと"     "並びに"   ""         "普遍"     "的"      
## [352] "措置"     ""         "確保"     ""         "ことに"   "努力"     ""         ""         "に"      
## [361] ""         ""         "の"       "人民"     "と"       ""         "の"       "国"       "と"      
## [370] "が"       "達成"     ""         "き"       "共通"     "の"       "基準"     ""         ""        
## [379] ""         "人権"     "宣言"     "を"       "公布"     ""         ""
```

We can imporove tokenization by collocation analysis in a similar way as [compound multi-word expressions](advanced-operations/compound-mutiword-expressions/) in English texts. `"^[ァ-ヶー一-龠]+$"` is a regular expression to select only katakana and kanji. We set `padding = TRUE` to keep the distance between words.


```r
tstat_col <- toks %>% 
  tokens_select("^[ァ-ヶー一-龠]+$", valuetype = "regex", padding = TRUE) %>%  
  textstat_collocations()

print(tstat_col)
```

```
##    collocation count count_nested length    lambda        z
## 1      社会 的     7            0      2  4.100152 7.864396
## 2    人権 宣言     3            0      2  5.444112 6.904004
## 3    世界 人権     2            0      2  5.897788 6.037454
## 4    国際 連合     5            0      2  8.663830 5.587286
## 5        国 連     2            0      2  5.836377 5.432901
## 6      連 総会     2            0      2  8.976515 5.125599
## 7    生活 水準     2            0      2  8.128374 4.959521
## 8      条 何人     4            0      2  7.361164 4.857272
## 9    友好 関係     2            0      2 10.075548 4.803099
## 10     加盟 国     3            0      2  7.362857 4.771253
## 11     基礎 的     2            0      2  4.187732 4.600524
## 12     基本 的     4            0      2  6.443799 4.293481
## 13     国際 的     2            0      2  3.086980 4.118057
## 14     的 保護     2            0      2  2.849733 3.900229
## 15     普遍 的     2            0      2  5.798025 3.720510
## 16     的 権利     3            0      2  1.526356 2.637384
## 17     的 自由     2            0      2  1.742938 2.559579
```

After compounding of statistically significantly associated collocations (`tstat_col$z > 3`), we can remove single-character tokens (`min_nchar = 2`) to further remove grammatical words.


```r
toks_comp <- tokens_compound(toks, tstat_col[tstat_col$z > 3], concatenator = "") %>% 
  tokens_select(min_nchar = 2)

print(toks_comp[2], max_ndoc = 1, max_ntok = -1)
```

```
## Tokens consisting of 1 document and 4 docvars.
## jpn :
##   [1] "前文"         "人類"         "社会"         "構成"         "固有"         "尊厳"         "平等"        
##   [8] "譲る"         "こと"         "権利"         "承認"         "こと"         "世界"         "における"    
##  [15] "自由"         "正義"         "及び"         "平和"         "基礎"         "ので"         "人権"        
##  [22] "無視"         "及び"         "軽侮"         "人類"         "良心"         "野蛮"         "行為"        
##  [29] "たら"         "言論"         "及び"         "信仰"         "自由"         "受け"         "恐怖"        
##  [36] "及び"         "欠乏"         "世界"         "到来"         "一般"         "人々"         "最高"        
##  [43] "願望"         "宣言"         "ので"         "人間"         "専制"         "圧迫"         "最後"        
##  [50] "手段"         "反逆"         "訴える"       "こと"         "支配"         "人権"         "保護"        
##  [57] "こと"         "肝要"         "ので"         "諸国"         "友好関係"     "発展"         "促進"        
##  [64] "こと"         "肝要"         "ので"         "国際連合"     "国民"         "国連"         "憲章"        
##  [71] "において"     "基本的"       "人権"         "人間"         "尊厳"         "及び"         "価値"        
##  [78] "並びに"       "男女"         "同権"         "信念"         "確認"         "かつ"         "大きな"      
##  [85] "自由"         "うち"         "社会的"       "進歩"         "生活水準"     "向上"         "促進"        
##  [92] "こと"         "決意"         "ので"         "加盟国"       "国際連合"     "協力"         "人権"        
##  [99] "及び"         "基本的"       "自由"         "普遍的"       "尊重"         "及び"         "遵守"        
## [106] "促進"         "達成"         "こと"         "誓約"         "ので"         "権利"         "及び"        
## [113] "自由"         "共通"         "理解"         "誓約"         "完全"         "もっとも"     "重要"        
## [120] "ので"         "国連総会"     "社会"         "個人"         "及び"         "機関"         "世界人権宣言"
## [127] "常に"         "念頭"         "加盟国"       "人民"         "加盟国"       "管轄"         "地域"        
## [134] "人民"         "権利"         "自由"         "尊重"         "指導"         "及び"         "教育"        
## [141] "促進"         "こと"         "並びに"       "普遍的"       "措置"         "確保"         "ことに"      
## [148] "努力"         "人民"         "達成"         "共通"         "基準"         "人権宣言"     "公布"
```


```r
# create a document-feature matrix
dfmat <- dfm(toks_comp)

print(dfmat)
```

```
## Document-feature matrix of: 64 documents, 398 features (96.7% sparse) and 4 docvars.
##        features
## docs    世界 人権 宣言 回国 採択 前文 人類 社会 構成 固有
##   jpn.1    0    0    0    1    1    0    0    0    0    0
##   jpn.2    2    4    1    0    0    1    2    2    1    1
##   jpn.3    0    0    0    0    0    0    0    0    0    0
##   jpn.4    0    0    1    0    0    0    0    0    0    0
##   jpn.5    0    0    0    0    0    0    0    0    0    0
##   jpn.6    0    0    0    0    0    0    0    0    0    0
## [ reached max_ndoc ... 58 more documents, reached max_nfeat ... 388 more features ]
```

