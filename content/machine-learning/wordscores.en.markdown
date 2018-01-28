---
title: Wordscores
weight: 10
draft: false
---

Wordscores is a scaling model for estimating the positions (mostly of political actors) for dimensions that are specified a priori. Wordscores was introduced in Laver, Benoit and Garry (2003) and is widely used among political scientists.


```r
require(quanteda)
require(quanteda.corpora)
require(ggplot2)
```

Traininga a Wordscores model reference scores that are stored in a 'Scoring'. Afterwards, Wordscores estimates the positions for the remaining "virgin" texts.

We use texts of amicus curiae briefs labelled as being either pro-petitioner or pro-respondent in US Supreme court cases on affirmative action to exemplify Wordscores. 


```r
refs <- docvars(data_corpus_amicus, "trainclass")
print(refs)
```

```
##   [1] P    P    R    R    <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA>
##  [15] <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA>
##  [29] <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA>
##  [43] <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA>
##  [57] <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA>
##  [71] <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA>
##  [85] <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA>
##  [99] <NA> <NA> <NA> <NA>
## Levels: P R
```

We see that the first two documents are labelled as _P_ (petitioner), the third and fourth documents are labelled as _R_ (respondent).

The `docvar` contains the class of the remaining 96 documents.

Next, we transform the reference scores to a numeric variable that takes the value -1 for the petitioners and +1 for the respondents.


```r
refs <- (as.numeric(refs) - 1.5) * 2
```

Now we can apply the Wordscores algorithm to the document-feature matrix.


```r
amicus_dfm <- dfm(data_corpus_amicus)

ws <- textmodel_wordscores(amicus_dfm, y = refs)
```

Next, we predict the Wordscores for the unknown virgin texts.


```r
ws_preds <- predict(ws, newdata = amicus_dfm)
summary(ws_preds)
```

```
##     Min.  1st Qu.   Median     Mean  3rd Qu.     Max. 
## -0.20673  0.02659  0.07694  0.05667  0.09922  0.23308
```

Finally, we can plot the distribution of the Wordscores for the virgin texts with **ggplot2**. 


```r
# create data frame
pred_data <- data.frame(
  predictions = ws_preds,
  document_class = docvars(amicus_dfm, "testclass")
)

# remove training texts
pred_data <- subset(pred_data, !is.na(document_class))

ggplot(pred_data, aes(x = document_class, y = predictions)) +
  geom_boxplot() +
  coord_flip() + 
  labs(x = "Test class", y = "Predicted document class") + 
  theme_minimal()
```

<img src="/machine-learning/wordscores.en_files/figure-html/unnamed-chunk-6-1.svg" width="768" />


{{% notice info %}}
If you want to learn more about Wordscores, see Laver, Michael, Kenneth R Benoit, and John Garry. 2003. "Extracting Policy Positions From Political Texts Using Words as Data." _American Political Science Review_ 97(02): 311-31.
{{% /notice%}}
