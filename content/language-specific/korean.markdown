
---
title: Korean
weight: 10
draft: true
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


```r
head(konlp_toks[[10]], 50)
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

## Stop words

As you can see above, the results are better. However, the results include morphemes that are legit within Korean syntax, but almost completely meaningless for the purpose of content analysis (such as "한" or "바"). This is where stop words act as additional filter, which you can write down as a list then call from quanteda. There is no standard list of stop words for Korean, which may be something to work on in the future. For now, you should create your own by identifying and filtering words step by step that have passed through the above described methods. 

Lastly, `tokens(remove_punct = TRUE)` is quanteda's standard command for removing punctuations, which might not be necessary if you've used KoNLP's `extractNoun()`.

