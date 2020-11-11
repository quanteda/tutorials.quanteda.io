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
##   [1] "人類"     "社会"     "の"       ""         "の"       "構成"     "員"       "の"       "固有"    
##  [10] "の"       "尊厳"     "と"       "平等"     "で"       "譲る"     "こと"     "の"       ""        
##  [19] ""         "権利"     "と"       "を"       "承認"     ""         "こと"     "は"       ""        
##  [28] "世界"     "における" "自由"     ""         "正義"     "及び"     "平和"     "の"       "基礎"    
##  [37] "で"       ""         "ので"     ""         "人権"     "の"       "無視"     "及び"     "軽侮"    
##  [46] "が"       ""         "人類"     "の"       "良心"     "を"       "踏"       "み"       "に"      
##  [55] "じ"       "っ"       "た"       "野蛮"     "行為"     "を"       "も"       "たら"     "し"      
##  [64] ""         "言論"     "及び"     "信仰"     "の"       "自由"     "が"       "受け"     "ら"      
##  [73] "れ"       ""         "恐怖"     "及び"     "欠乏"     "の"       ""         "世界"     "の"      
##  [82] "到来"     "が"       ""         "一般"     "の"       "人々"     "の"       "最高"     "の"      
##  [91] "願望"     ""         "宣言"     "さ"       "れ"       "た"       "ので"     ""         "人間"    
## [100] "が"       "専制"     "と"       "圧迫"     "と"       ""         "最後"     "の"       "手段"    
## [109] ""         "反逆"     "に"       "訴える"   "こと"     "が"       ""         ""         "に"      
## [118] ""         ""         "に"       "は"       ""         "法"       "の"       "支配"     ""        
## [127] "人権"     "を"       "保護"     ""         "こと"     "が"       "肝要"     "で"       ""        
## [136] "ので"     ""         "諸国"     ""         "の"       "友好"     "関係"     "の"       "発展"    
## [145] "を"       "促進"     ""         "こと"     "が"       "肝要"     "で"       ""         "ので"    
## [154] ""         "国際"     "連合"     "の"       "諸"       "国民"     "は"       ""         "国"      
## [163] "連"       "憲章"     "において" ""         "基本"     "的"       "人権"     ""         "人間"    
## [172] "の"       "尊厳"     "及び"     "価値"     "並びに"   "男女"     "の"       "同権"     ""        
## [181] "の"       "信念"     "を"       "再"       "確認"     "し"       ""         "かつ"     ""        
## [190] ""         "大きな"   "自由"     "の"       "うち"     "で"       "社会"     "的"       "進歩"    
## [199] "と"       "生活"     "水準"     "の"       "向上"     "と"       "を"       "促進"     ""        
## [208] "こと"     "を"       "決意"     ""         "ので"     ""         "加盟"     "国"       "は"      
## [217] ""         "国際"     "連合"     "と"       "協力"     "し"       "て"       ""         "人権"    
## [226] "及び"     "基本"     "的"       "自由"     "の"       "普遍"     "的"       "な"       "尊重"    
## [235] "及び"     "遵守"     "の"       "促進"     "を"       "達成"     ""         "こと"     "を"      
## [244] "誓約"     ""         "ので"     ""         ""         "権利"     "及び"     "自由"     ""        
## [253] "共通"     "の"       "理解"     "は"       ""         ""         "誓約"     "を"       "完全"    
## [262] "に"       ""         ""         "に"       "もっとも" "重要"     "で"       ""         "ので"    
## [271] ""         ""         ""         ""         "に"       ""         "国"       "連"       "総会"    
## [280] "は"       ""         "社会"     "の"       "各"       "個人"     "及び"     "各"       "機関"    
## [289] "が"       ""         ""         "世界"     "人権"     "宣言"     "を"       "常に"     "念頭"    
## [298] "に"       "置"       "き"       ""         ""         "加盟"     "国"       ""         "の"      
## [307] "人民"     "の"       ""         "に"       "も"       ""         ""         ""         "加盟"    
## [316] "国"       "の"       "管轄"     ""         "に"       ""         "地域"     "の"       "人民"    
## [325] "の"       ""         "に"       "も"       ""         ""         "権利"     "と"       "自由"    
## [334] "と"       "の"       "尊重"     "を"       "指導"     "及び"     "教育"     ""         "促進"    
## [343] ""         "こと"     "並びに"   ""         "普遍"     "的"       "措置"     ""         "確保"    
## [352] ""         "ことに"   "努力"     ""         ""         "に"       ""         ""         "の"      
## [361] "人民"     "と"       ""         "の"       "国"       "と"       "が"       "達成"     ""        
## [370] "き"       "共通"     "の"       "基準"     ""         ""         ""         "人権"     "宣言"    
## [379] "を"       "公布"     ""         ""
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
## 1      社会 的     7            0      2  4.086332 7.837802
## 2    人権 宣言     2            0      2  5.094436 5.937858
## 3    国際 連合     5            0      2  8.650227 5.578507
## 4    生活 水準     2            0      2  8.114795 4.951230
## 5    友好 関係     2            0      2 10.061986 4.796631
## 6      加盟 国     3            0      2  7.349231 4.762417
## 7      基礎 的     2            0      2  4.173954 4.585371
## 8        国 連     2            0      2  6.921787 4.388809
## 9      基本 的     4            0      2  6.430032 4.284303
## 10     国際 的     2            0      2  3.073171 4.099615
## 11     的 保護     2            0      2  2.835913 3.881293
## 12     普遍 的     2            0      2  5.784258 3.711671
## 13     的 権利     3            0      2  1.512286 2.613049
## 14     的 自由     2            0      2  1.729004 2.539099
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
##   [1] "人類"     "社会"     "構成"     "固有"     "尊厳"     "平等"     "譲る"     "こと"     "権利"    
##  [10] "承認"     "こと"     "世界"     "における" "自由"     "正義"     "及び"     "平和"     "基礎"    
##  [19] "ので"     "人権"     "無視"     "及び"     "軽侮"     "人類"     "良心"     "野蛮"     "行為"    
##  [28] "たら"     "言論"     "及び"     "信仰"     "自由"     "受け"     "恐怖"     "及び"     "欠乏"    
##  [37] "世界"     "到来"     "一般"     "人々"     "最高"     "願望"     "宣言"     "ので"     "人間"    
##  [46] "専制"     "圧迫"     "最後"     "手段"     "反逆"     "訴える"   "こと"     "支配"     "人権"    
##  [55] "保護"     "こと"     "肝要"     "ので"     "諸国"     "友好関係" "発展"     "促進"     "こと"    
##  [64] "肝要"     "ので"     "国際連合" "国民"     "国連"     "憲章"     "において" "基本的"   "人権"    
##  [73] "人間"     "尊厳"     "及び"     "価値"     "並びに"   "男女"     "同権"     "信念"     "確認"    
##  [82] "かつ"     "大きな"   "自由"     "うち"     "社会的"   "進歩"     "生活水準" "向上"     "促進"    
##  [91] "こと"     "決意"     "ので"     "加盟国"   "国際連合" "協力"     "人権"     "及び"     "基本的"  
## [100] "自由"     "普遍的"   "尊重"     "及び"     "遵守"     "促進"     "達成"     "こと"     "誓約"    
## [109] "ので"     "権利"     "及び"     "自由"     "共通"     "理解"     "誓約"     "完全"     "もっとも"
## [118] "重要"     "ので"     "国連"     "総会"     "社会"     "個人"     "及び"     "機関"     "世界"    
## [127] "人権宣言" "常に"     "念頭"     "加盟国"   "人民"     "加盟国"   "管轄"     "地域"     "人民"    
## [136] "権利"     "自由"     "尊重"     "指導"     "及び"     "教育"     "促進"     "こと"     "並びに"  
## [145] "普遍的"   "措置"     "確保"     "ことに"   "努力"     "人民"     "達成"     "共通"     "基準"    
## [154] "人権宣言" "公布"
```


```r
# create a document-feature matrix
dfmat <- dfm(toks_comp)

print(dfmat)
```

```
## Document-feature matrix of: 82 documents, 393 features (97.4% sparse) and 4 docvars.
##        features
## docs    前文 人類 社会 構成 固有 尊厳 平等 譲る こと 権利
##   jpn.1    1    0    0    0    0    0    0    0    0    0
##   jpn.2    0    2    2    1    1    2    1    1    8    3
##   jpn.3    0    0    0    0    0    0    0    0    0    0
##   jpn.4    0    0    0    0    0    1    1    0    0    1
##   jpn.5    0    0    0    0    0    0    0    0    0    0
##   jpn.6    0    0    0    0    0    0    0    0    2    1
## [ reached max_ndoc ... 76 more documents, reached max_nfeat ... 383 more features ]
```

