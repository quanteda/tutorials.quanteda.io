---
title: Newsmap
weight: 60
draft: false
bibliography: ../references.bib
---

Newsmap (Watanabe 2018) introduces an alternative to the fully supervised classifiers (Naive Bayes, regularised regression) and fully unsupervised topic models covered earlier in this chapter: a semi-supervised model for geographical document classification. Rather than needing a large set of documents that a human has manually labelled by country, as full supervision would, it learns from a small dictionary of “seed words”, such as country and city names, and expands on that starting point automatically. You can classify documents by country even when no one has hand-labelled a training set for you.

Install the **newsmap** package from CRAN.

``` r
install.packages("newsmap")
```

``` r
library(quanteda)
library(quanteda.corpora)
library(newsmap)
library(maps)
library(ggplot2)
```

Download a corpus with news articles using **quanteda.corpora**’s `download()` function, in the same way you downloaded the Guardian corpus for earlier chapters, though here again by direct URL rather than by a registered short name.

``` r
corp_news <- download(url = "https://www.dropbox.com/s/r8zhsu8zvjzhnml/data_corpus_yahoonews.rds?dl=1")
```

`corp_news` contains 10,000 news summaries downloaded from Yahoo News in 2014.

``` r
ndoc(corp_news)
```

    ## [1] 10000

``` r
range(corp_news$date)
```

    ## [1] "2014-01-01" "2014-12-31"

Proper nouns, such as country and city names, are the most useful features of documents for geographical classification. However, not every capitalised word is a proper noun; month names, day names and news agency abbreviations are capitalised too, and would otherwise be mistaken for place names. We therefore define custom stopword lists for these categories to exclude them.

``` r
month <- c("January", "February", "March", "April", "May", "June",
           "July", "August", "September", "October", "November", "December")
day <- c("Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday")
agency <- c("AP", "AFP", "Reuters")
```

``` r
toks_news <- tokens(corp_news, remove_punct = TRUE) |>
             tokens_remove(pattern = c(stopwords("en"), month, day, agency),
                           valuetype = "fixed", padding = TRUE)
```

**newsmap** contains [seed geographical dictionaries](https://github.com/koheiw/newsmap/tree/master/dict) in English, German, Spanish, Japanese and Russian. `data_dictionary_newsmap_en` is the seed dictionary for English texts; like the geographical dictionary you used with `tokens_lookup()` in the [Basic Operations chapter](/basic-operations/tokens/tokens_lookup), it is organised hierarchically from continents down to individual countries.

We now build two separate DFMs from the same tokens: one that labels documents by country, using the seed dictionary at the country level (`levels = 3`), and one of ordinary capitalised word features. The model will learn, from this labelled subset, which capitalised words tend to co-occur with each country label, so that it can later predict a country even for documents that never explicitly mention a country’s name.

``` r
toks_label <- tokens_lookup(toks_news, dictionary = data_dictionary_newsmap_en,
                            levels = 3) # level 3 is countries
dfmat_label <- dfm(toks_label, tolower = FALSE)

dfmat_feat <- dfm(toks_news, tolower = FALSE)
dfmat_feat_select <- dfm_select(dfmat_feat, pattern = "^[A-Z][A-Za-z0-9]+", 
                                valuetype = "regex", case_insensitive = FALSE) |> 
                     dfm_trim(min_termfreq = 10)

tmod_nm <- textmodel_newsmap(dfmat_feat_select, y = dfmat_label)
```

The seed dictionary contains only names of countries and capital cities, but the fitted model also learns other features associated with each country, such as the names of politicians or currencies. `coef()` shows the features most strongly associated with a given country, identified by its two-letter code as defined in [ISO 3166-1](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2).

``` r
coef(tmod_nm, n = 15)[c("US", "GB", "FR", "BR", "JP")]
```

    ## $US
    ## WASHINGTON         US   American Washington       YORK     States  Americans 
    ##   7.153809   7.036355   6.829401   6.605344   6.369564   6.054140   5.359407 
    ##       York Brunnstrom      Kirby   Platinum      Anglo    Stewart   Keystone 
    ##   4.992858   3.781222   3.701179   3.614168   3.567648   3.162183   3.088075 
    ##    Admiral 
    ##   3.008032 
    ## 
    ## $GB
    ##   British    LONDON    London   Britain Britain's        UK      UKIP   Kingdom 
    ##  7.877447  7.847349  7.563364  7.264550  6.670776  5.487606  4.905684  4.774656 
    ##     Tesco     Hamza Cameron's   Osborne   Salmond     Clegg   Cameron 
    ##  4.446152  4.359141  3.858365  3.753005  3.695846  3.665993  3.595376 
    ## 
    ## $FR
    ##        French        France         PARIS         Paris      Hollande 
    ##      8.183414      8.089341      7.541560      7.303602      6.532896 
    ##    Hollande's        Fabius         Valls      Francois Saint-Germain 
    ##      5.401494      5.296133      5.296133      5.277441      4.970711 
    ##        Froome            Le      France's       Renault           Pen 
    ##      4.803657      3.755688      3.734898      3.705045      3.504374 
    ## 
    ## $BR
    ##    Brazil       SAO     PAULO       RIO   JANEIRO Brazilian       Rio        DE 
    ##  8.175085  7.261538  7.247745  7.048616  7.048616  6.996430  6.922322  6.355469 
    ##   Janeiro       Sao     Paulo      BELO HORIZONTE  BRASILIA     Dilma 
    ##  6.303283  5.966811  5.966811  5.915517  5.915517  5.804292  5.306453 
    ## 
    ## $JP
    ##        Japan     Japanese        TOKYO          Abe        Tokyo       Shinzo 
    ##     8.191612     7.895346     7.762799     7.061747     6.961663     6.789813 
    ##        Abe's      Tokyo's    Fukushima      Japan's       Nikkei       Toyota 
    ##     5.808984     5.316507     5.169904     4.389464     3.834903     3.478228 
    ##    Pyongyang Asia-Pacific        Honda 
    ##     3.180976     3.155001     3.141756

{{% notice tip %}}
Names of people, organizations and places are often multi-word expressions. To distinguish between “New York” and “York”, for example, it is useful to compound tokens using `tokens_compound()` as explained in [Advanced Operations](/advanced-operations/compound-mutiword-expressions).
{{% /notice %}}

With the model fitted and inspected, you can predict the most strongly associated country for every document using `predict()`, and then count how often each country was predicted using `table()`.

``` r
pred_nm <- predict(tmod_nm)
head(pred_nm, 20)
```

    ##  text1  text2  text3  text4  text5  text6  text7  text8  text9 text10 text11 
    ##     KP     SY     IQ     RU     TH     CN     UA     SY     GB     US     SY 
    ## text12 text13 text14 text15 text16 text17 text18 text19 text20 
    ##     US     UA     SY     LK     ES     AU     CR     ID     BH 
    ## 204 Levels: BI DJ ER ET KE MG MU MW MZ RE RW SO TZ UG ZM ZW AO CD CF CG ... WS

By default, `table()` only reports countries predicted at least once, silently dropping any country with a count of zero. Setting factor levels explicitly, using the same country codes that appear in `dfmat_label`, ensures that countries which did not appear in the corpus show up in the result with a count of zero, rather than being omitted entirely.

``` r
count <- sort(table(factor(pred_nm, levels = colnames(dfmat_label))), decreasing = TRUE)
head(count, 20)
```

    ## 
    ##  GB  US  RU  UA  AU  CN  CA  FR  IQ  BR  SY  DE  ZA  NZ  JP  IL  IN  ES  EG  PS 
    ## 622 578 516 440 367 363 319 311 295 278 262 250 237 228 197 197 187 182 157 155

You can visualise the distribution of global news attention geographically using **ggplot2**’s `geom_map()`, which colours each country on a world map according to how often it was predicted. The result is a visual summary of which parts of the world this news corpus covers most.

``` r
dat_country <- as.data.frame(count, stringsAsFactors = FALSE)
colnames(dat_country) <- c("id", "frequency")

world_map <- map_data(map = "world")
world_map$region <- iso.alpha(world_map$region) # convert country name to ISO code

ggplot(dat_country, aes(map_id = id)) +
      geom_map(aes(fill = frequency), map = world_map) +
      expand_limits(x = world_map$long, y = world_map$lat) +
      scale_fill_continuous(name = "Frequency") +
      theme_void() +
      coord_fixed()
```

<img src="/machine-learning/newsmap.en_files/figure-html/unnamed-chunk-12-1.png" alt="" width="672" />

## References

<div id="refs" class="references csl-bib-body hanging-indent">

<div id="ref-watanabe2018" class="csl-entry">

Watanabe, Kohei. 2018. “Newsmap: A Semi-Supervised Approach to Geographical News Classification.” *Digital Journalism* 6 (3): 294–309. <https://doi.org/10.1080/21670811.2017.1293487>.

</div>

</div>
