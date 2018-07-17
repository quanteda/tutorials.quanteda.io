
---
title: Korean
weight: 10
draft: false
---

{{% author %}}By Oul Han{{% /author %}} 


```r
require(quanteda)
require(KoNLP)
```

## Tokenization

You can use `tokens()` or a morphological analysis tool such as [KoNLP](https://cran.r-project.org/web/packages/KoNLP/index.html) to tokenize Korean texts. The sample corpus contains all 17 transcripts of commencement speeches by Korean presidents from 1948 to 2008. [Click to download the corpus if you want to take a look in the traditional way. ](https://www.dropbox.com/s/1cluzpgjjje9icd/data_corpus_korean-presidential-speeches.RDS?dl=1)


```r
corp <- download("data_corpus_korean_presidential-speeches")
txt <- tail(texts(corp), 1000)
```



### Dictionary-based boundary detection

`tokens()` can segment Korean texts, but fails to slice off many morphemes that are not necessary for text (content) analysis. However, you may experiment and find that `tokens()` suffices, especially if supplemented with stop words that you tailor for the corpus in question. Thus, `tokens()` is a quick and dirty method for tokenizing Korean without using additional tools and based on the rules defined in the [ICU library](http://site.icu-project.org/home), which is available via the **stringi** package. ICU detects boundaries of words using [a dictionary with frequency information](http://source.icu-project.org/repos/icu/icu/tags/release-58-rc/source/data/brkitr/dictionaries/). As you can see there, ICU doesn't yet support Korean--which explains why the results below are just "kind of okay".


```r
icu_toks <- tokens(txt)
head(icu_toks[[10]], 50)
```

```
##  [1] "친애하는"         "국민"             "여러분"          
##  [4] "!"                "이"               "자리에"          
##  [7] "참석하신"         "내외귀빈"         "여러분"          
## [10] "!"                "오늘"             "본인은"          
## [13] "대한민국"         "제"               "10"              
## [16] "대"               "대통령으로"       "취임함에"        
## [19] "즈음하여"         ","                "먼저"            
## [22] "본인을"           "대통령으로"       "선출하여"        
## [25] "주신"             "통일주체국민회의" "대의원"          
## [28] "여러분과"         "국민"             "여러분에게"      
## [31] "깊은"             "사의를"           "표하고자"        
## [34] "합니다"           "."                "방금"            
## [37] "본인은"           "헌법이"           "규정한"          
## [40] "바에"             "따라"             "선서를"          
## [43] "하면서"           ","                "숙연한"          
## [46] "마음으로"         "대통령으로서의"   "막중한"          
## [49] "책임을"           "다시"
```

### Morphological analysis 

If you want to perform more accurate tokenization, you need to install a morphological analysis tool, and call it from R. [KoNLP](https://cran.r-project.org/web/packages/KoNLP/index.html) is one of the most popular tools for tokenizing Korean texts via **noun extraction**. This function is a slightly more radical form of boundary analysis for dropping unneeded morpheme types that trail behind Korean words without whitespace ("post-positions"). We can use this function from R with **KoNLP**. The package is available on CRAN. 




```r
konlp_toks <- extractNoun(txt)
```

The object `konlp_toks` is a list. Turn it into a `tokens` object that is an innate format of **quanteda**.

```r
noun_toks <- as.tokens(konlp_toks) 
```


```r
head(noun_toks[[10]], 50)
```

```
##  [1] "친애"             "국민"             "여러분"          
##  [4] "자리"             "참석"             "내외"            
##  [7] "귀빈"             "여러분"           "오늘"            
## [10] "본인"             "대한"             "민국"            
## [13] "10대"             "대통령"           "취임"            
## [16] "함"               "즈음"             "본인"            
## [19] "대통령"           "선출"             "통일주체국민회의"
## [22] "대의원"           "여러분"           "국민"            
## [25] "여러분"           "사의"             "표"              
## [28] "본인"             "헌법"             "규정"            
## [31] "한"               "바"               "선서"            
## [34] "숙연"             "한"               "마음"            
## [37] "대통령"           "막중"             "한"              
## [40] "책임"             "한"               "번"              
## [43] "통감"             "10"               "월"              
## [46] "26"               "박정희"           "대통령"          
## [49] "각하"             "돌연"
```

### Refining tokens

Firstly, we can remove numbers with `tokens()`. 


```r
head(noun_toks[[7]], 40)
```

```
##  [1] "사랑"    "5"       "천"      "국내외"  "동포"    "여러분"  "내외"   
##  [8] "귀빈"    "여러분"  "저"      "2"       "차"      "세계"    "대전의" 
## [15] "포화"    "지"      "4반세기" "오늘"    "우리"    "인류"    "이상"   
## [22] "평화"    "번영"    "다짐"    "시대"    "문턱"    "나"      "시"     
## [29] "인류"    "대화"    "협조"    "윤리"    "존중"    "공존"    "공영"   
## [36] "세계"    "평화"    "질"      "확립"    "기회"
```


```r
noun_toks <- tokens(noun_toks, remove_numbers = TRUE)
```

Note that numbers with combined words are not filtered (such as "4반세기" = four half centuries). 


```r
head(noun_toks[[7]], 40)
```

```
##  [1] "사랑"     "천"       "국내외"   "동포"     "여러분"   "내외"    
##  [7] "귀빈"     "여러분"   "저"       "차"       "세계"     "대전의"  
## [13] "포화"     "지"       "4반세기"  "오늘"     "우리"     "인류"    
## [19] "이상"     "평화"     "번영"     "다짐"     "시대"     "문턱"    
## [25] "나"       "시"       "인류"     "대화"     "협조"     "윤리"    
## [31] "존중"     "공존"     "공영"     "세계"     "평화"     "질"      
## [37] "확립"     "기회"     "요"       "아시아인"
```

Secondly, we can remove Chinese words that occur in older or highly specialized Korean text. In this sample corpus, they tend to appear in brackets after their Korean phonetization. This makes them safe to remove without losing content. We will use regular expressions and the unicode character property ["Han"](http://unicode.org/faq/han_cjk.html) to filter Chinese characters that are either in, or not in, brackets.


```r
head(noun_toks[[2]], 40)
```

```
##  [1] "대통령취임사(大統領就任辭)" "오늘"                      
##  [3] "취임"                       "就任式"                    
##  [5] "나"                         "책"                        
##  [7] "責任"                       "나"                        
##  [9] "수"                         "것임니다"                  
## [11] "사년(四年)동안에"           "행(行)한"                  
## [13] "정부(政府)일은"             "일"                        
## [15] "아니였던"                   "것임니다"                  
## [17] "앞"                         "일"                        
## [19] "수업"                       "터임니다"                  
## [21] "우리"                       "사랑"                      
## [23] "민국(民國)이"               "위험(危險)한"              
## [25] "때"                         "당(當)해서"                
## [27] "정부관료(政府官僚)나"       "일반평민(一般平民)이나"    
## [29] "너"                         "나"                        
## [31] "물론(勿論)하고"             "누구"                      
## [33] "각각(各各)"                 "나라"                      
## [35] "직책(職責)과"               "민족(民族)"                
## [37] "사명(使命)"                 "외(外)에는"                
## [39] "것"                         "감(敢)히"
```


```r
refi_noun_toks <- noun_toks
refi_pure_korean <- tokens_remove(refi_noun_toks, "\\(?\\p{Han}+\\)?", valuetype = "regex") 
```


```r
head(refi_pure_korean[[2]], 40)
```

```
##  [1] "오늘"     "취임"     "나"       "책"       "나"       "수"      
##  [7] "것임니다" "일"       "아니였던" "것임니다" "앞"       "일"      
## [13] "수업"     "터임니다" "우리"     "사랑"     "때"       "너"      
## [19] "나"       "누구"     "나라"     "것"       "것임니다" "우리"    
## [25] "우리"     "것"       "아님니다" "우리"     "앞"       "우리들"  
## [31] "우리"     "몸"       "평"       "마음"     "것"       "수"      
## [37] "것임니다" "수"       "우리"     "것임니다"
```

{{% notice info %}}
In this example, R regex doesn't recognize the escape character (such as `\p` or `\(`) so you need to escape the escape (as in `\\p`).
{{% /notice%}}

### Feature selection

The results include a number of monosyllabic morphemes that are almost meaningless for content analysis (such as "한" and "바"). Note that these aren't syllables per se, but words that occupy one character space. Thus, we will extract only words with more than two characters. 


```r
refi_pure_korean <- tokens_remove(refi_pure_korean, min_nchar = 2) 
```


```r
head(refi_pure_korean[[2]], 40)
```

```
##  [1] "오늘"     "취임"     "것임니다" "아니였던" "것임니다" "수업"    
##  [7] "터임니다" "우리"     "사랑"     "누구"     "나라"     "것임니다"
## [13] "우리"     "우리"     "아님니다" "우리"     "우리들"   "우리"    
## [19] "마음"     "것임니다" "우리"     "것임니다" "우리"     "것임니다"
## [25] "이때"     "우리"     "희생"     "때임니다" "다같이"   "것임니다"
## [31] "우리"     "희생"     "우리"     "우리"     "우리"     "살길"    
## [37] "우리"     "우리"     "것임니다" "우리나라"
```

Stop words act as additional filter, which you can write down as a list then call from quanteda. There is no standard list of stop words for Korean. For now, you should create your own list by identifying and filtering stop words step by step. 

