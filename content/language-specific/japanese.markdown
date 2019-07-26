---
title: Japanese
weight: 10
draft: false
---

{{% author %}}By Kohei Watanabe{{% /author %}} 


```r
require(quanteda)
require(quanteda.corpora)
```

## Tokenization

You can use `tokens()` or a morphological analysis tool such as [Mecab](http://taku910.github.io/mecab/) to tokenize Japanese texts. The sample corpus contains transcripts of all the speeches at Japan's Committee on Foreign Affairs and Defense of the lower house (Shugiin) from 1947 to 2017


```r
corp <- download("data_corpus_foreignaffairscommittee")

# use the latest 1000 speeches
txt <- tail(texts(corp), 1000)
```



### Dictionary-based boundary detection

`tokens()` can segment Japanese texts without additional tools based on the rules defined in the [ICU library](http://site.icu-project.org/home), which is available via the **stringi** package. ICU detects boundaries of Japanese words using [a dictonary with frequency information](http://source.icu-project.org/repos/icu/icu/tags/release-58-rc/source/data/brkitr/dictionaries/).


```r
toks_icu <- tokens(txt)
head(toks_icu[[20]], 100)
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

### Morphological analysis

If you want to perform more accurate tokenization, you need to install a morphological analysis tool, and call it from R. [Mecab](https://en.wikipedia.org/wiki/MeCab) is one of the most popular tools for tokenizing Japanese texts, and we can use it from R with **RMecab**. The package is not available on CRAN, so you have to install the library from the [author's repository](http://rmecab.jp).




```r
install.packages("RMeCab", repos = "http://rmecab.jp/R")
library(RMeCab)
toks_mecab <- txt %>% 
  lapply(function(x) unlist(RMeCab::RMeCabC(x))) %>% 
  as.tokens()
```


```r
head(toks_mecab[[20]], 100)
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


## Refining tokens

Even if you use a morphological analysis tool, tokenization of Japanese text is far from perfect. However, you can refine tokens by compounding sequence of the same character class using `textstat_collocations()` and `tokens_compound()`. `^[一-龠]+$` and `^[ァ-ヶー]+$` regular expressions that match tokens are compromised only of kanji and katakana, respectively. `^[０-９ァ-ヶー一-龠]+$` is for tokens that is a combination of numbers, katakana, or kanji.


```r
toks_icu_refi <- toks_icu
tstat_kanji <- toks_icu %>% 
    tokens_select('^[一-龠]+$', valuetype = 'regex', padding = TRUE) %>% 
    textstat_collocations(min_count = 5, tolower = FALSE)
toks_icu_refi <- tokens_compound(toks_icu_refi, tstat_kanji[tstat_kanji$z > 2],
                                 concatenator = '', join = TRUE)

tstat_kana <- toks_icu_refi %>% 
    tokens_select('^[ァ-ヶー]+$', valuetype = 'regex', padding = TRUE) %>% 
    textstat_collocations(min_count = 5, tolower = FALSE)
toks_icu_refi <- tokens_compound(toks_icu_refi, tstat_kana[tstat_kana$z > 2],
                                 concatenator = '', join = TRUE)

tstast_any <- toks_icu_refi %>% 
            tokens_select('^[０-９ァ-ヶー一-龠]+$', valuetype = 'regex', padding = TRUE) %>% 
            textstat_collocations(min_count = 5, tolower = FALSE)
toks_icu_refi <- tokens_compound(toks_icu_refi, tstast_any[tstast_any$z > 2],
                                 concatenator = '', join = TRUE)
```


```r
head(toks_icu_refi[[20]], 100)
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
You have to be careful not to compound too many tokens to reduce their semantic ambiguity, because large ngrams increase sparsity of the data and make statistical analysis more difficult.
{{% /notice %}}

## Feature selection

We did not remove punctuation marks in the initial tokenization, but we do it here by `tokens(remove_punct = TRUE)`. There is no standard list of stop words for Japanese, but we can hiragana-only tokens are usually function words that we can remove.


```r
toks_icu_refi <- tokens(toks_icu_refi, remove_punct = TRUE)
toks_icu_refi <- tokens_remove(toks_icu_refi, "^[ぁ-ん]+$", valuetype = "regex")
head(toks_icu_refi[[20]], 100)
```

```
##  [1] "森政府参考人" "お答え"       "申"           "上げ"        
##  [5] "ＡＣＳＡ"     "自衛隊"       "相手国"       "軍隊"        
##  [9] "間"           "物品"         "役務"         "相互提供"    
## [13] "適用"         "決済手続等"   "枠組み"       "定める"      
## [17] "締結"         "自衛隊"       "相手国"       "軍隊"        
## [21] "間"           "物品"         "役務"         "相互提供"    
## [25] "円滑"         "迅速"         "行う"         "可能"        
## [29] "我が国"       "取り巻く"     "安全保障環境" "一層"        
## [33] "厳"           "増す"         "中"           "今回"        
## [37] "日米"         "ＡＣＳＡ"     "締結"         "昨年"        
## [41] "三月"         "施行"         "平和安全法制" "幅"          
## [45] "広"           "日米間"       "安全保障"     "協力"        
## [49] "円滑"         "実施"         "貢献"         "協力"        
## [53] "実効性"       "一層"         "高める"       "上"          
## [57] "大きな"       "意義"         "考え"
```

Even after hiragana-only tokens are removed, there are many one-character tokens, many of which are very common verbs in Japanese texts. These tokens can be removed by `dfm_select(min_nchar = 2)`.


```r
dfmat_icu_refi <- dfm(toks_icu_refi, tolower = FALSE)
topfeatures(dfmat_icu_refi, 20)
```

```
##     思い       私     考え       言       中       今     問題   我が国 
##      658      567      381      363      317      314      303      280 
##     上げ     日本       申       行     大臣   北朝鮮       方       思 
##      278      273      253      248      246      242      238      235 
##     議論 ＡＣＳＡ       等 に対して 
##      219      214      208      194
```

```r
dfmat_icu_refi <- dfm_select(dfmat_icu_refi, min_nchar = 2)
topfeatures(dfmat_icu_refi, 20)
```

```
##     思い     考え     問題   我が国     上げ     日本     大臣   北朝鮮 
##      658      381      303      280      278      273      246      242 
##     議論 ＡＣＳＡ に対して     状況     委員     活動     政府   自衛隊 
##      219      214      194      192      187      184      183      178 
##     確認     質問   先ほど     今回 
##      175      169      169      158
```

{{% notice info %}}
If you want to learn more about how to analyze speeches at at Japan's Committee on Foreign Affairs and Defense of the lower house, see the [example](https://docs.quanteda.io/articles/pkgdown/examples/japanese_speech_ja.html) on the documentation site.
{{% /notice%}}
