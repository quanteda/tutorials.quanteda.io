---
title: Russian
weight: 10
draft: false
---

{{% author %}}By Kohei Watanabe and Lana Bilalova{{% /author %}} 

```{r, message=FALSE}
require(quanteda)
require(quanteda.corpora)
options(width = 110)
```

## Russian 

After tokenization, we remove so called "stopwords" using `stopwords("ru", source = "marimo")` or `stopwords("ru", source = "snowball")`. Note that using marimo helps drop wider list of stopwords. You can find more details on stopwords on the [website](http://stopwords.quanteda.io) of the **stopwords** package.  Please be very careful when pre-processing or removing tokens since these choices [might influence subsequent results](https://doi.org/10.1017/pan.2017.44). If you want tokens to comprise only of Cyrillic script, you can select them by `"^[а-яА-Я]+$"`.

```{r}
# reshape corpus to the level of paragraphs
corp_ru <- corpus_reshape(data_corpus_udhr["rus"], to = "paragraphs")

# tokenize corpus and apply pre-processing
# choosing marimo stopwords offers a wider list than snowball
toks_ru <- tokens(corp_ru, remove_punct = TRUE, remove_numbers = TRUE) %>% 
  tokens_remove(pattern = stopwords("ru", source = "snowball")) 
print(toks_ru[2], max_ndoc = 1, max_ntoken = -1)
```

```{r}
# construct a document-feature matrix
# note that now all the features are converted to lowercase by default
dfmat_ru <- dfm(toks_ru)
print(dfmat_ru)
```

