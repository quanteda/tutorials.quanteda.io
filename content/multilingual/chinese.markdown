  ---
title: Chinese
weight: 30
draft: false
---

{{% author %}}By Yuan Zhou{{% /author %}} 


```r
require(quanteda)
require(quanteda.corpora)
options(width = 110)
```

We resort to the [Marimo](https://github.com/koheiw/marimo) stopwords list (`stopwords("zh_cn", source = "marimo")`) and the length of words (`min_nchar = 2`) to remove function words. You can keep only Chinese characters with `"^\\p{script=Hani}+$"`.


```r
corp <- corpus_reshape(data_corpus_udhr["cmn_hans"], to = "paragraphs")
toks <- tokens(corp, remove_punct = TRUE, remove_numbers = TRUE) %>% 
  tokens_remove(pattern = stopwords("zh_cn", source = "marimo"), min_nchar = 2) %>% 
  tokens_keep(pattern = "^\\p{script=Hani}+$", valuetype = 'regex')
print(toks[2], max_ndoc = 1, max_ntok = -1)
```

```
## Tokens consisting of 1 document and 4 docvars.
## cmn_hans.2 :
##   [1] "鉴于"     "人类"     "家庭"     "成员"     "固有"     "尊严"     "及其"     "平等"     "不移"    
##  [10] "权利"     "承认"     "乃是"     "世界"     "自由"     "正义"     "和平"     "基础"     "鉴于"    
##  [19] "人权"     "无视"     "侮蔑"     "发展"     "野蛮"     "暴行"     "暴行"     "玷污"     "人类"    
##  [28] "良心"     "一个"     "人人"     "享有"     "言论"     "信仰"     "自由"     "恐惧"     "世界"    
##  [37] "来临"     "宣布"     "普通"     "人民"     "最高"     "愿望"     "鉴于"     "人类"     "不致"    
##  [46] "迫不得已" "暴政"     "压迫"     "进行"     "反叛"     "必要"     "人权"     "法治"     "保护"    
##  [55] "鉴于"     "必要"     "促进"     "各国"     "友好"     "关系"     "发展"     "鉴于"     "联合"    
##  [64] "国家"     "人民"     "联合"     "宪章"     "重申"     "他们"     "基本"     "人权"     "人格"    
##  [73] "尊严"     "价值"     "男女平等" "权利"     "信念"     "决心"     "促成"     "较大"     "自由"    
##  [82] "中的"     "社会"     "进步"     "生活"     "水平"     "改善"     "鉴于"     "会员"     "联合"    
##  [91] "合作"     "促进"     "人权"     "基本"     "自由"     "普遍"     "尊重"     "遵行"     "鉴于"    
## [100] "权利"     "自由"     "普遍"     "了解"     "充分"     "实现"     "具有"     "很大"     "重要性"  
## [109] "现在"     "大会"     "发布"     "世界"     "人权"     "宣言"     "人民"     "国家"     "努力"    
## [118] "实现"     "共同"     "标准"     "以期"     "每一个人" "社会"     "机构"     "经常"     "宣言"    
## [127] "努力"     "教诲"     "教育"     "促进"     "权利"     "自由"     "尊重"     "国家"     "国际"    
## [136] "渐进"     "措施"     "权利"     "自由"     "会员"     "本身"     "人民"     "管辖"     "领土"    
## [145] "人民"     "得到"     "普遍"     "有效"     "承认"     "遵行"
```


```r
# construct document-feature matrix
dfmat <- dfm(toks)
print(dfmat)
```

```
## Document-feature matrix of: 82 documents, 424 features (97.72% sparse) and 4 docvars.
##             features
## docs         序言 鉴于 人类 家庭 成员 固有 尊严 及其 平等 不移
##   cmn_hans.1    1    0    0    0    0    0    0    0    0    0
##   cmn_hans.2    0    7    3    1    1    1    2    1    1    1
##   cmn_hans.3    0    0    0    0    0    0    0    0    0    0
##   cmn_hans.4    0    0    0    0    0    0    1    0    1    0
##   cmn_hans.5    0    0    0    0    0    0    0    0    0    0
##   cmn_hans.6    0    0    0    0    0    0    0    0    0    0
## [ reached max_ndoc ... 76 more documents, reached max_nfeat ... 414 more features ]
```
