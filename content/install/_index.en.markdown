---
title: Install
weight: 20
pre: "<b>2. </b>"
chapter: false
---



## Install R Studio

[download R Studio](https://www.rstudio.com/products/rstudio/download/)

## Install quanteda

First, you need to have **quanteda** installed.  You can do this from inside RStudio, from the Tools...Install Packages menu, or simply using

```r
install.packages("quanteda")
```

(Optional) You can install some additional corpus data from **quantedaData** using


```r
## the devtools package is required to install quanteda from Github
devtools::install_github("kbenoit/quantedaData")
```

If you are feeling adventurous, you can install the latest build of **quanteda** from its [GitHub code page](https://github.com/kbenoit/quanteda).

Note that on **Windows platforms**, it is also recommended that you install the [RTools suite](https://cran.r-project.org/bin/windows/Rtools/), and for **OS X**, that you install [XCode](https://itunes.apple.com/gb/app/xcode/id497799835?mt=12) from the App Store.


### Load your quanteda

Run the rest of this file to test your setup.  You must have quanteda installed in order for this next step to succeed.

```r
require(quanteda)
## Loading required package: quanteda
## quanteda version 0.99.9000
## Using 3 of 4 threads for parallel computing
## 
## Attaching package: 'quanteda'
## The following object is masked from 'package:utils':
## 
##     View
```







