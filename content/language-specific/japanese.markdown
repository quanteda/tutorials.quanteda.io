---
title: Japanese
weight: 10
draft: false
---

You can use either **quanteda**'s `tokens()` or a morphorogical analysis tool to tokenize Japanese texts. 



```r
require(quanteda)
require(quanteda.corpora)
```

## Tokenization


```r
corp <- download("data_corpus_foreignaffairscommittee")
```



### Dictionary-based boundary detection

Tokenization by `tokens()` is based on rules defined by the ICU library


```r
icu_toks <- tokens(txt)
head(icu_toks[[20]], 100)
```

```
##   [1] "○"        "森"       "政府"     "参考"     "人"       "お答え"  
##   [7] "申"       "し"       "上げ"     "ます"     "。"       "ＡＣＳＡ"
##  [13] "は"       "、"       "自衛隊"   "と"       "相手"     "国"      
##  [19] "の"       "軍隊"     "と"       "の"       "間"       "の"      
##  [25] "物品"     "、"       "役務"     "の"       "相互"     "提供"    
##  [31] "に"       "適用"     "さ"       "れる"     "決済"     "手続"    
##  [37] "等"       "の"       "枠組み"   "を"       "定める"   "もの"    
##  [43] "で"       "ご"       "ざ"       "い"       "ます"     "。"      
##  [49] "これ"     "を"       "締結"     "する"     "こと"     "により"  
##  [55] "、"       "自衛隊"   "と"       "相手"     "国"       "の"      
##  [61] "軍隊"     "と"       "の"       "間"       "の"       "物品"    
##  [67] "、"       "役務"     "の"       "相互"     "提供"     "を"      
##  [73] "円滑"     "かつ"     "迅速"     "に"       "行う"     "こと"    
##  [79] "が"       "可能"     "と"       "なる"     "。"       "我が国"  
##  [85] "を"       "取り巻く" "安全"     "保障"     "環境"     "が"      
##  [91] "一層"     "厳"       "し"       "さ"       "を"       "増す"    
##  [97] "中"       "で"       "、"       "今回"
```


### Morphorogical analysis




```r
install.packages("RMeCab", repos = "http://rmecab.jp/R")
mecab_toks <- 
  txt %>% 
  lapply(function(x) unlist(RMeCab::RMeCabC(x))) %>% 
  as.tokens()
```


```r
head(mecab_toks[[20]], 100)
```

```
##   [1] "○"        "森"       "政府"     "参考"     "人"       "　"      
##   [7] "お答え"   "申し上げ" "ます"     "。"       "　"       "ＡＣＳＡ"
##  [13] "は"       "、"       "自衛隊"   "と"       "相手"     "国"      
##  [19] "の"       "軍隊"     "と"       "の"       "間"       "の"      
##  [25] "物品"     "、"       "役務"     "の"       "相互"     "提供"    
##  [31] "に"       "適用"     "さ"       "れる"     "決済"     "手続"    
##  [37] "等"       "の"       "枠組み"   "を"       "定める"   "もの"    
##  [43] "で"       "ござい"   "ます"     "。"       "これ"     "を"      
##  [49] "締結"     "する"     "こと"     "により"   "、"       "自衛隊"  
##  [55] "と"       "相手"     "国"       "の"       "軍隊"     "と"      
##  [61] "の"       "間"       "の"       "物品"     "、"       "役務"    
##  [67] "の"       "相互"     "提供"     "を"       "円滑"     "かつ"    
##  [73] "迅速"     "に"       "行う"     "こと"     "が"       "可能"    
##  [79] "と"       "なる"     "。"       "　"       "我が国"   "を"      
##  [85] "取り巻く" "安全"     "保障"     "環境"     "が"       "一層"    
##  [91] "厳し"     "さ"       "を"       "増す"     "中"       "で"      
##  [97] "、"       "今回"     "の"       "日"
```


### Refine tokenization


```r
refi_toks <- icu_toks
seqs_kanji <- icu_toks %>% 
              tokens_select('^[一-龠]+$', valuetype = 'regex', padding = TRUE) %>% 
              textstat_collocations(min_count = 5, tolower = FALSE)
refi_toks <- tokens_compound(refi_toks, seqs_kanji[seqs_kanji$z > 2], concatenator = '', join = TRUE)

seqs_kana <- icu_toks %>% 
             tokens_select('^[ァ-ヶー]+$', valuetype = 'regex', padding = TRUE) %>% 
             textstat_collocations(min_count = 5, tolower = FALSE)
refi_toks <- tokens_compound(refi_toks, seqs_kana[seqs_kana$z > 2], concatenator = '', join = TRUE)

seqs_any <- icu_toks %>% 
            tokens_select('^[０-９ァ-ヶー一-龠]+$', valuetype = 'regex', padding = TRUE) %>% 
            textstat_collocations(min_count = 5, tolower = FALSE)
refi_toks <- tokens_compound(refi_toks, seqs_any[seqs_any$z > 2], concatenator = '', join = TRUE)
```


```r
head(refi_toks[[20]], 100)
```

```
##   [1] "○"            "森政府参考人" "お答え"       "申"          
##   [5] "し"           "上げ"         "ます"         "。"          
##   [9] "ＡＣＳＡ"     "は"           "、"           "自衛隊"      
##  [13] "と"           "相手国"       "の"           "軍隊"        
##  [17] "と"           "の"           "間"           "の"          
##  [21] "物品"         "、"           "役務"         "の"          
##  [25] "相互提供"     "に"           "適用"         "さ"          
##  [29] "れる"         "決済手続等"   "の"           "枠組み"      
##  [33] "を"           "定める"       "もの"         "で"          
##  [37] "ご"           "ざ"           "い"           "ます"        
##  [41] "。"           "これ"         "を"           "締結"        
##  [45] "する"         "こと"         "により"       "、"          
##  [49] "自衛隊"       "と"           "相手国"       "の"          
##  [53] "軍隊"         "と"           "の"           "間"          
##  [57] "の"           "物品"         "、"           "役務"        
##  [61] "の"           "相互提供"     "を"           "円滑"        
##  [65] "かつ"         "迅速"         "に"           "行う"        
##  [69] "こと"         "が"           "可能"         "と"          
##  [73] "なる"         "。"           "我が国"       "を"          
##  [77] "取り巻く"     "安全保障環境" "が"           "一層"        
##  [81] "厳"           "し"           "さ"           "を"          
##  [85] "増す"         "中"           "で"           "、"          
##  [89] "今回"         "の"           "日米"         "ＡＣＳＡ"    
##  [93] "の"           "締結"         "は"           "、"          
##  [97] "昨年"         "三月"         "に"           "施行"
```

{{% notice note %}}
Compounding makes data more sparse
{{% /notice %}}

## Feature selection


```r
toks <- tokens_select(refi_toks, '^[０-９ぁ-んァ-ヶー一-龠]+$', valuetype = 'regex')
toks <- tokens_remove(refi_toks, "^[ぁ-ん]+$", valuetype = "regex")
head(toks[[20]], 100)
```

```
##  [1] "○"            "森政府参考人" "お答え"       "申"          
##  [5] "上げ"         "。"           "ＡＣＳＡ"     "、"          
##  [9] "自衛隊"       "相手国"       "軍隊"         "間"          
## [13] "物品"         "、"           "役務"         "相互提供"    
## [17] "適用"         "決済手続等"   "枠組み"       "定める"      
## [21] "。"           "締結"         "、"           "自衛隊"      
## [25] "相手国"       "軍隊"         "間"           "物品"        
## [29] "、"           "役務"         "相互提供"     "円滑"        
## [33] "迅速"         "行う"         "可能"         "。"          
## [37] "我が国"       "取り巻く"     "安全保障環境" "一層"        
## [41] "厳"           "増す"         "中"           "、"          
## [45] "今回"         "日米"         "ＡＣＳＡ"     "締結"        
## [49] "、"           "昨年"         "三月"         "施行"        
## [53] "平和安全法制" "幅"           "広"           "日米間"      
## [57] "安全保障"     "協力"         "円滑"         "実施"        
## [61] "貢献"         "、"           "協力"         "実効性"      
## [65] "一層"         "高める"       "上"           "大きな"      
## [69] "意義"         "考え"         "。"
```

{{% notice info %}}
If you want to learn more about how to analyze speeches at at Japan's Committee on Foreign Affairs and Defense of the lower house (Shugiin), see\
https://docs.quanteda.io/articles/pkgdown/examples/japanese_speech_ja.html
{{% /notice%}}
