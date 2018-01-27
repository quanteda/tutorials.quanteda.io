---
title: Pre-formatted files
weight: 10
draft: false
---


```r
require(quanteda)
require(readtext)
```

`data_dir`is the location of sample files in your computer.


```r
data_dir <- system.file("extdata/", package = "readtext")
```

If your text data is stored in a pre-formatted file where one column contains the text and additional columns might store document-level variables (e.g. year, author, or language), you can use `read.csv()` to import.


```r
inaug_data <- read.csv(paste0(data_dir, "/csv/inaugCorpus.csv"))
```

Alternatively, you can use the **readtext** package to import character (comma or tab) separated values. **readtext** reads files containing text, along with any associated document-level variables.


```r
inaug_data <- readtext(paste0(data_dir, "/tsv/dailsample.tsv"), text_field = "speech")
```
