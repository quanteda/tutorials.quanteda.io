---
title: Pre-formatted files
weight: 10
draft: false
---

Every example so far in this tutorial has used text that was already loaded into R for you. In practice, your first task is almost always getting your own text files into R in the first place. The chapter and the two that follow cover the main ways to do that, starting with the simplest case.


``` r
library(quanteda)
library(readtext)
```

We will first show you how to import pre-formatted files that come in a "spreadsheet format": a table where one column holds the text of each document and other columns hold information about it. `path_data` is the location of sample files that come bundled with the **readtext** package, so that you can follow along without needing any files of your own.


``` r
path_data <- system.file("extdata/", package = "readtext")
```

If your text data is stored in a pre-formatted file, with one column containing the text and other columns holding document-level variables (e.g. year, author, or language), you can use base R's `read.csv()` to import it. You already did this when you built a corpus from a data frame in an earlier chapter, and it works the same way.


``` r
dat_inaug <- read.csv(paste0(path_data, "/csv/inaugCorpus.csv"))
```

Alternatively, you can use the **readtext** package to import character (comma- or tab-separated) values. **readtext** reads files containing text, along with any associated document-level variables. **readtext** can be more forgiving than `read.csv()` about file encoding and formatting quirks, so it is a reasonable default even for `.csv` files. Here we import a tab-separated (`.tsv`) file instead, telling `readtext()` that the actual speech text is stored in a column called `speech`.


``` r
dat_dail <- readtext(paste0(path_data, "/tsv/dailsample.tsv"), text_field = "speech")
```

Either `dat_inaug` or `dat_dail` can now be passed straight to `corpus()`, using `text_field` to point at the column that holds the text, just as you did in the [Basic Operations chapter](/basic-operations/corpus/corpus).

{{% notice warning %}}
The most common problem when loading data into R is a misspecified location for a file or directory. If a path is relative, check where you are using `getwd()` and set the root directory of your project using `setwd()`. On Windows, you also have to replace all `\` in a path with `/`.
{{% /notice%}}
/
