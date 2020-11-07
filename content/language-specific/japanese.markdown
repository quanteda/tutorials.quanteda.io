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
corp_ja <- corpus_reshape(data_corpus_udhr["jpn"], to = "paragraphs")
toks_ja <- tokens(corp_ja, remove_punct = TRUE, padding = TRUE) %>% 
  tokens_remove(stopwords("ja", source = "marimo"), padding = TRUE)
print(toks_ja[4:5], max_ndoc = 1, max_ntok = -1)
```

```
## Tokens consisting of 2 documents and 2 docvars.
## jpn :
##   [1] "人類"     "社会"     "の"       ""         "の"       "構成"     "員"       "の"       "固有"    
##  [10] "の"       "尊厳"     "と"       "平等"     "で"       "譲る"     "こと"     "の"       ""        
##  [19] ""         "権利"     "と"       "を"       "承"       "認"       ""         "こと"     "は"      
##  [28] ""         "世界"     "における" "自由"     ""         "正義"     "及び"     "平和"     "の"      
##  [37] "基礎"     "で"       ""         "ので"     ""         "人権"     "の"       "無視"     "及び"    
##  [46] "軽侮"     "が"       ""         "人類"     "の"       "良心"     "を"       "踏"       "み"      
##  [55] "に"       "じ"       "っ"       "た"       "野蛮"     "行為"     "を"       "も"       "たら"    
##  [64] "し"       ""         "言論"     "及"       "び"       "信仰"     "の"       "自由"     "が"      
##  [73] "受け"     "ら"       "れ"       ""         "恐怖"     "及び"     "欠乏"     "の"       ""        
##  [82] "世界"     "の"       "到来"     "が"       ""         "一般"     "の"       "人々"     "の"      
##  [91] "最高"     "の"       "願望"     ""         "宣言"     "さ"       "れ"       "た"       "ので"    
## [100] ""         "人間"     "が"       "専制"     "と"       "圧迫"     "と"       ""         "最後"    
## [109] "の"       "手段"     ""         "反逆"     "に"       "訴える"   "こと"     "が"       ""        
## [118] ""         "に"       "す"       "る"       ""         "に"       "は"       ""         "法"      
## [127] "の"       "支配"     ""         "人権"     "を"       "保護"     ""         "こと"     "が"      
## [136] "肝要"     "で"       ""         "ので"     ""         "諸国"     ""         "の"       "友好"    
## [145] "関係"     "の"       "発展"     "を"       "促進"     ""         "こと"     "が"       "肝要"    
## [154] "で"       ""         "ので"     ""         "国際"     "連合"     "の"       "諸"       "国民"    
## [163] "は"       ""         "国"       "連"       "憲章"     "において" ""         "基本"     "的"      
## [172] "人権"     ""         "人間"     "の"       "尊厳"     "及び"     "価値"     "並び"     "に"      
## [181] "男女"     "の"       "同権"     ""         "の"       "信念"     "を"       "再"       "確認"    
## [190] "し"       ""         "かつ"     ""         ""         "大きな"   "自由"     "の"       "うち"    
## [199] "で"       "社会"     "的"       "進歩"     "と"       "生活"     "水準"     "の"       "向上"    
## [208] "と"       "を"       "促進"     ""         "こと"     "を"       "決意"     ""         "ので"    
## [217] ""         "加盟"     "国"       "は"       ""         "国際"     "連合"     "と"       "協力"    
## [226] "し"       "て"       ""         "人権"     "及び"     "基本"     "的"       "自由"     "の"      
## [235] "普遍"     "的"       "な"       "尊重"     "及び"     "遵守"     "の"       "促進"     "を"      
## [244] "達成"     ""         "こと"     "を"       "誓約"     ""         "ので"     ""         ""        
## [253] "権利"     "及び"     "自由"     ""         "共通"     "の"       "理解"     "は"       ""        
## [262] ""         "誓約"     "を"       "完全"     "に"       ""         ""         "に"       "も"      
## [271] "っ"       "とも"     "重要"     "で"       ""         "ので"     ""         ""         ""        
## [280] ""         "に"       ""         "国"       "連"       "総会"     "は"       ""         "社会"    
## [289] "の"       "各"       "個人"     "及び"     "各"       "機関"     "が"       ""         ""        
## [298] "世界"     "人権"     "宣言"     "を"       "常に"     "念頭"     "に"       "置"       "き"      
## [307] ""         ""         "加盟"     "国"      
## 
## [ reached max_ndoc ... 1 more document ]
```

We can imporove tokenization by collocation analysis in a similar way as [compound multi-word expressions](advanced-operations/compound-mutiword-expressions/) in English texts. `"^[ァ-ヶー一-龠]+$"` is a regular expression to select only katakana and kanji. We set `padding = TRUE` to keep the distance between words.


```r
tstat_col <- toks_ja %>% 
  tokens_select("^[ァ-ヶー一-龠]+$", valuetype = "regex", padding = TRUE) %>%  
  textstat_collocations()
print(tstat_col)
```

```
##    collocation count count_nested length    lambda        z
## 1      社会 的     6            0      2  3.960312 7.350570
## 2    人権 宣言     3            0      2  5.510407 6.988230
## 3    世界 人権     2            0      2  5.963975 6.105295
## 4      条 何人     3            0      2  4.337049 5.861139
## 5    国際 連合     5            0      2  8.729990 5.629984
## 6        国 連     2            0      2  5.994041 5.559816
## 7        権 利     4            0      2 10.728518 5.220942
## 8      連 総会     2            0      2  9.042513 5.163307
## 9    生活 水準     2            0      2  8.194427 4.999848
## 10   友好 関係     2            0      2 10.141520 4.834563
## 11     加盟 国     3            0      2  7.429125 4.814223
## 12     基礎 的     2            0      2  4.226534 4.645392
## 13     基本 的     4            0      2  6.480911 4.319065
## 14     国際 的     2            0      2  3.125919 4.172971
## 15     的 保護     2            0      2  3.000355 4.062534
## 16     普遍 的     2            0      2  5.836771 3.745989
## 17     的 権利     3            0      2  1.649864 2.847150
## 18     的 自由     2            0      2  1.782464 2.619885
```

After compounding of statistically significantly associated collocations (`tstat_col$z > 3`), we can remove single-character tokens (`min_nchar = 2`) to further remove grammatical words.


```r
toks_ja_comp <- tokens_compound(toks_ja, tstat_col[tstat_col$z > 3], concatenator = "") %>% 
  tokens_select(min_nchar = 2)
print(toks_ja_comp[4:5], max_ndoc = 1, max_ntok = -1)
```

```
## Tokens consisting of 2 documents and 2 docvars.
## jpn :
##   [1] "人類"         "社会"         "構成"         "固有"         "尊厳"         "平等"         "譲る"        
##   [8] "こと"         "権利"         "こと"         "世界"         "における"     "自由"         "正義"        
##  [15] "及び"         "平和"         "基礎"         "ので"         "人権"         "無視"         "及び"        
##  [22] "軽侮"         "人類"         "良心"         "野蛮"         "行為"         "たら"         "言論"        
##  [29] "信仰"         "自由"         "受け"         "恐怖"         "及び"         "欠乏"         "世界"        
##  [36] "到来"         "一般"         "人々"         "最高"         "願望"         "宣言"         "ので"        
##  [43] "人間"         "専制"         "圧迫"         "最後"         "手段"         "反逆"         "訴える"      
##  [50] "こと"         "支配"         "人権"         "保護"         "こと"         "肝要"         "ので"        
##  [57] "諸国"         "友好関係"     "発展"         "促進"         "こと"         "肝要"         "ので"        
##  [64] "国際連合"     "国民"         "国連"         "憲章"         "において"     "基本的"       "人権"        
##  [71] "人間"         "尊厳"         "及び"         "価値"         "並び"         "男女"         "同権"        
##  [78] "信念"         "確認"         "かつ"         "大きな"       "自由"         "うち"         "社会的"      
##  [85] "進歩"         "生活水準"     "向上"         "促進"         "こと"         "決意"         "ので"        
##  [92] "加盟国"       "国際連合"     "協力"         "人権"         "及び"         "基本的"       "自由"        
##  [99] "普遍的"       "尊重"         "及び"         "遵守"         "促進"         "達成"         "こと"        
## [106] "誓約"         "ので"         "権利"         "及び"         "自由"         "共通"         "理解"        
## [113] "誓約"         "完全"         "とも"         "重要"         "ので"         "国連総会"     "社会"        
## [120] "個人"         "及び"         "機関"         "世界人権宣言" "常に"         "念頭"         "加盟国"      
## 
## [ reached max_ndoc ... 1 more document ]
```
