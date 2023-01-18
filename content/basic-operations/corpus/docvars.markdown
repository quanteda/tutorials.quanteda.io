---
title: Document-level variables
weight: 15
draft: false
---


```r
require(quanteda)
```

**quanteda**'s objects keep information associated with documents. They are called "document-level variables", or "docvars", and are accessed using `docvars()`.


```r
corp <- data_corpus_inaugural
head(docvars(corp))
```

```
##   Year  President FirstName                 Party
## 1 1789 Washington    George                  none
## 2 1793 Washington    George                  none
## 3 1797      Adams      John            Federalist
## 4 1801  Jefferson    Thomas Democratic-Republican
## 5 1805  Jefferson    Thomas Democratic-Republican
## 6 1809    Madison     James Democratic-Republican
```

## Extracting document-level variables

If you want to extract individual elements of document variables, you can specify `field`.


```r
docvars(corp, field = "Year")
```

```
##  [1] 1789 1793 1797 1801 1805 1809 1813 1817 1821 1825 1829 1833 1837 1841 1845
## [16] 1849 1853 1857 1861 1865 1869 1873 1877 1881 1885 1889 1893 1897 1901 1905
## [31] 1909 1913 1917 1921 1925 1929 1933 1937 1941 1945 1949 1953 1957 1961 1965
## [46] 1969 1973 1977 1981 1985 1989 1993 1997 2001 2005 2009 2013 2017 2021
```

You can also access to individual document-level variables using the `$` operator. 


```r
corp$Year
```

```
##  [1] 1789 1793 1797 1801 1805 1809 1813 1817 1821 1825 1829 1833 1837 1841 1845
## [16] 1849 1853 1857 1861 1865 1869 1873 1877 1881 1885 1889 1893 1897 1901 1905
## [31] 1909 1913 1917 1921 1925 1929 1933 1937 1941 1945 1949 1953 1957 1961 1965
## [46] 1969 1973 1977 1981 1985 1989 1993 1997 2001 2005 2009 2013 2017 2021
```

## Assigning document-level variables

`docvars()` also allows you to create or update document variables.


```r
docvars(corp, field = "Century") <- floor(docvars(corp, field = "Year") / 100) + 1
head(docvars(corp))
```

```
##   Year  President FirstName                 Party Century
## 1 1789 Washington    George                  none      18
## 2 1793 Washington    George                  none      18
## 3 1797      Adams      John            Federalist      18
## 4 1801  Jefferson    Thomas Democratic-Republican      19
## 5 1805  Jefferson    Thomas Democratic-Republican      19
## 6 1809    Madison     James Democratic-Republican      19
```

Alternatively, you can create the document-level variable using the `$` operator


```r
corp$Century <- floor(corp$Year / 100) + 1
```

{{% notice tip %}}
`docvars()` is explained only in this section, but it works on other **quanteda** objects (tokens and dfm) in the same way.
{{% /notice %}}
