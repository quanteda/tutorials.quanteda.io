  ---
title: Chinese
weight: 30
draft: false
---

{{% author %}}By Yuan Zhou{{% /author %}} 

For Chinese texts, I highly recommend you to use the [jieba](https://github.com/fxsjy/jieba) algorithm to achieve a better text segmentation. The algorithm can be applied through the [jiebaR](https://github.com/qinwf/jiebaR) package in R.


```r
require(quanteda)
require(jiebaR)
options(width = 110)
```

The following is a comparison of results produced by `jieba::segment()` and `quanteda::tokens()`.


```r
text1 <- "小熊维尼"
text2 <- "联合国大会一九四八年十二月十日第217A(III)号决议通过并颁布"
text3 <- "张华考上了北京大学，在化学系学习；李萍进了中等技术学校，读机械制造专业；我在百货公司当售货员：我们都有光明的前途。"
text4 <- '中国共产党以马克思列宁主义、毛泽东思想、邓小平理论、“三个代表”重要思想和科学发展观、习近平新时代中国特色社会主义思想作为自己的行动指南。'

cc <- worker() # create a jieba tokenizer

tokens(text1)
```

```
## Tokens consisting of 1 document.
## text1 :
## [1] "小熊" "维"   "尼"
```

```r
segment(text1, cc)
```

```
## [1] "小熊维尼"
```

```r
print(tokens(text2), max_ntok = -1)
```

```
## Tokens consisting of 1 document.
## text1 :
##  [1] "联合"   "国"     "大会"   "一九"   "四"     "八年"   "十二月" "十日"   "第"     "217A"   "("     
## [12] "III"    ")"      "号"     "决议"   "通过"   "并"     "颁布"
```

```r
segment(text2, cc)
```

```
##  [1] "联合国大会" "一九四八年" "十二月"     "十日"       "第"         "217"        "A"          "III"       
##  [9] "号"         "决议"       "通过"       "并"         "颁布"
```

```r
print(tokens(text3), max_ntok = -1)
```

```
## Tokens consisting of 1 document.
## text1 :
##  [1] "张"   "华"   "考"   "上了" "北京" "大学" "，"   "在"   "化学" "系"   "学习" "；"   "李"   "萍"   "进"  
## [16] "了"   "中等" "技术" "学校" "，"   "读"   "机械" "制造" "专业" "；"   "我在" "百货" "公司" "当"   "售货"
## [31] "员"   "："   "我们" "都有" "光明" "的"   "前途" "。"
```

```r
segment(text3, cc)
```

```
##  [1] "张华"     "考上"     "了"       "北京大学" "在"       "化学系"   "学习"     "李"       "萍"      
## [10] "进"       "了"       "中等"     "技术学校" "读"       "机械制造" "专业"     "我"       "在"      
## [19] "百货公司" "当"       "售货员"   "我们"     "都"       "有"       "光明"     "的"       "前途"
```

```r
print(tokens(text4), max_ntok = -1)
```

```
## Tokens consisting of 1 document.
## text1 :
##  [1] "中国"   "共产"   "党"     "以"     "马克"   "思"     "列宁"   "主义"   "、"     "毛泽东" "思想"  
## [12] "、"     "邓小平" "理论"   "、"     "\""     "三"     "个"     "代表"   "\""     "重要"   "思想"  
## [23] "和"     "科学"   "发展"   "观"     "、"     "习"     "近平"   "新时代" "中国"   "特色"   "社会"  
## [34] "主义"   "思想"   "作为"   "自己"   "的"     "行动"   "指南"   "。"
```

```r
segment(text4, cc)
```

```
##  [1] "中国共产党"     "以"             "马克思列宁主义" "毛泽东思想"     "邓小平理论"     "三个代表"      
##  [7] "重要"           "思想"           "和"             "科学"           "发展观"         "习近平"        
## [13] "新"             "时代"           "中国"           "特色"           "社会主义"       "思想"          
## [19] "作为"           "自己"           "的"             "行动指南"
```

The jieba algorithm removes the punctuation by default. If you want to keep the punctuation, you can set `worker()$symbol` as `TRUE`.


```r
cc$symbol <- TRUE
segment(text3, cc)
```

```
##  [1] "张华"     "考上"     "了"       "北京大学" "，"       "在"       "化学系"   "学习"     "；"      
## [10] "李"       "萍"       "进"       "了"       "中等"     "技术学校" "，"       "读"       "机械制造"
## [19] "专业"     "；"       "我"       "在"       "百货公司" "当"       "售货员"   "："       "我们"    
## [28] "都"       "有"       "光明"     "的"       "前途"     "。"
```

```r
cc$symbol <- FALSE
```

The results produced by jieba segmentation can be converted to quanteda tokens.


```r
cc$bylines <- TRUE
toks_jieba <- c(text1, text2, text3, text4) %>% segment(cc) %>% as.tokens()

toks_jieba
```

```
## Tokens consisting of 4 documents.
## text1 :
## [1] "小熊维尼"
## 
## text2 :
##  [1] "联合国大会" "一九四八年" "十二月"     "十日"       "第"         "217"        "A"          "III"       
##  [9] "号"         "决议"       "通过"       "并"        
## [ ... and 1 more ]
## 
## text3 :
##  [1] "张华"     "考上"     "了"       "北京大学" "在"       "化学系"   "学习"     "李"       "萍"      
## [10] "进"       "了"       "中等"    
## [ ... and 15 more ]
## 
## text4 :
##  [1] "中国共产党"     "以"             "马克思列宁主义" "毛泽东思想"     "邓小平理论"     "三个代表"      
##  [7] "重要"           "思想"           "和"             "科学"           "发展观"         "习近平"        
## [ ... and 10 more ]
```

There does not exist an authoritative Chinese stopword list. You can use either [Baidu stopwords](http://www.baiduguide.com/baidu-stopwords/) or [marimo stopwords](https://github.com/koheiw/marimo) to remove the meaningless tokens.


```r
head(stopwords("zh", source = "misc"), 50) # Baidu stopwords
```

```
##  [1] "按"     "按照"   "俺"     "们"     "阿"     "别"     "别人"   "别处"   "是"     "别的"   "别管"  
## [12] "说"     "不"     "不仅"   "不但"   "不光"   "不单"   "不只"   "不外乎" "不如"   "不妨"   "不尽"  
## [23] "然"     "不得"   "不怕"   "不惟"   "不成"   "不拘"   "料"     "不是"   "不比"   "不然"   "特"    
## [34] "不独"   "不管"   "不至于" "若"     "不论"   "不过"   "不问"   "比"     "方"     "比如"   "及"    
## [45] "本身"   "本着"   "本地"   "本人"   "本"     "巴巴"
```

```r
head(stopwords("zh_cn", source = "marimo"), 50)
```

```
##  [1] "我"       "把我"     "对我"     "我自己"   "我们"     "把我们"   "对我们"   "我们自己" "你"      
## [10] "把你"     "对你"     "你自己"   "你们"     "把你们"   "对你们"   "你们自己" "您"       "把您"    
## [19] "对您"     "您自己"   "您们"     "把您们"   "对您们"   "您们自己" "他"       "把他"     "对他"    
## [28] "他自己"   "她"       "把她"     "对她"     "她自己"   "它"       "把它"     "对它"     "它自己"  
## [37] "你们"     "把你们"   "对你们"   "你们自己" "这"       "那"       "这些"     "那些"     "该"      
## [46] "这个"     "这么"     "这样"     "这是"     "我的"
```

```r
toks_jieba <- tokens_remove(toks_jieba, stopwords("zh", source = "misc"))
print((toks_jieba[[3]]), max_ntok = -1)
```

```
##  [1] "张华"     "考上"     "北京大学" "化学系"   "学习"     "李"       "萍"       "进"       "中等"    
## [10] "技术学校" "读"       "机械制造" "专业"     "百货公司" "售货员"   "光明"     "前途"
```

You can also create your own new words or stop words.


```r
new_user_word(cc, c("李萍", "中等技术学校"))
```

```
## [1] TRUE
```

```r
segment(text3, cc)
```

```
## [[1]]
##  [1] "张华"         "考上"         "了"           "北京大学"     "在"           "化学系"       "学习"        
##  [8] "李萍"         "进"           "了"           "中等技术学校" "读"           "机械制造"     "专业"        
## [15] "我"           "在"           "百货公司"     "当"           "售货员"       "我们"         "都"          
## [22] "有"           "光明"         "的"           "前途"
```

```r
stwords <- c("了", "在")
segment(text3, cc) %>% as.tokens() %>% tokens_remove(stwords)
```

```
## Tokens consisting of 1 document.
## text1 :
##  [1] "张华"         "考上"         "北京大学"     "化学系"       "学习"         "李萍"         "进"          
##  [8] "中等技术学校" "读"           "机械制造"     "专业"         "我"          
## [ ... and 9 more ]
```
