---
title: "Install packages"
weight: 5
draft: false
---

## R Studio

**quanteda** runs solely on [base R](https://cran.r-project.org/), but [RStudio](https://www.rstudio.com/products/rstudio/download/) makes it easy to write your code and inspect your objects. You will need to have [base R](https://cran.r-project.org/) installed, and we also recommend to install the latest version of [RStudio](https://www.rstudio.com/products/rstudio/download/).

## Quanteda

First, you need to have **quanteda** installed. You can do this from inside RStudio, from the Tools > Install Packages, or executing a command:

```r
install.packages("quanteda")
```

If you are feeling adventurous, you can install the latest build of **quanteda** from its [GitHub code page](https://github.com/quanteda/quanteda).

Note that on **Windows platforms**, it is also recommended that you install the [RTools suite](https://cran.r-project.org/bin/windows/Rtools/), and for **OS X** that you install [XCode](https://itunes.apple.com/gb/app/xcode/id497799835?mt=12) from the App Store.


## Other packages

We will use the **readtext** package to read in different types of text data in this tutorials. Again, you can do this using RStudio menu (Tools > Install Packages), or executing the following command:


```r
install.packages("readtext")
```

We will also use extra datasets in tutorials that are available in **quanteda.corpora**:


```r
install.packages("devtools")
devtools::install_github("quanteda/quanteda.corpora")
```

## Extra packages

The tutorials do not cover syntactical analysis, but you should install **spacyr** for  part-of-speech tagging, entity recognition, and dependency parsing. It provides an interface to the spaCy library and works well with **quanteda**. Note that you need to have Python installed to use the **spacyr** package. See the [package description](https://github.com/quanteda/spacyr/blob/master/README.md) for more information.


```r
install.packages("spacyr")
```
