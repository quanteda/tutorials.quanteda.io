---
title: "R commands"
weight: 10
draft: false
bibliography: ../references.bib
---

You do not need advanced knowledge of the R programming language to analyse text with **quanteda**, but you do need to understand several basic R commands. If you have never used R before, this page introduces the small set of commands you will see reused throughout the rest of these tutorials.

## Basic R objects and commands

R has three types of objects that you will meet again and again in this tutorial: *vector*, *data frame* and *matrix*. Many of **quanteda**’s own objects, such as the document-feature matrix you will meet in a later chapter, behave in a similar way to these objects, so it helps to get comfortable with them here first.

### Vectors

As a language for statistical analysis, R’s most basic objects are vectors. A vector is an ordered set of values of the same type, such as a list of numbers or a list of words. In the examples below, `vec_num` is a *numeric vector*, while `vec_char` is a *character vector*. We use `c()` (short for “combine”) to combine elements into a vector and `<-` to assign the result to a variable name so that we can use it again later.

``` r
vec_num <- c(1, 5, 6, 3)
print(vec_num)
```

    ## [1] 1 5 6 3

``` r
vec_char <- c("apple", "banana", "mandarin", "melon")
print(vec_char)
```

    ## [1] "apple"    "banana"   "mandarin" "melon"

Once a vector is created, you can extract elements of vectors with the `[]` operator and index numbers of desired elements. Index numbers start at 1 in R, not 0, so `vec_num[1]` gives you the first element, not the second.

``` r
print(vec_num[1])
```

    ## [1] 1

``` r
print(vec_num[1:2])
```

    ## [1] 1 5

``` r
print(vec_char[c(1, 3)])
```

    ## [1] "apple"    "mandarin"

You can apply arithmetical operations such as addition, subtraction, multiplication or division on numeric vectors. If only a single value is given for multiplication, for example, each element of the vector will be multiplied by the same value.

``` r
vec_num2 <- vec_num * 2
print(vec_num2)
```

    ## [1]  2 10 12  6

You can also compare elements of a vector by relational operators such as `==`, `>=`, `>`, `<=`, `<`. The result of these operations will be a *logical vector* that contains either `TRUE` or `FALSE`. You will use this kind of comparison later to select subsets of texts or documents in **quanteda**.

``` r
vec_logi_gt5 <- vec_num >= 5
print(vec_logi_gt5)
```

    ## [1] FALSE  TRUE  TRUE FALSE

You cannot apply arithmetical operations on character vectors, since it makes no sense to “add” two words together. But you can apply the equality operator to check whether elements match a given piece of text.

``` r
vec_logi_apple <- vec_char == "apple"
print(vec_logi_apple)
```

    ## [1]  TRUE FALSE FALSE FALSE

You can also concatenate elements of character vectors using `paste()`. Since the two vectors in the example have the same length, elements in the same position are pasted together. So the first element of `vec_char2` combines the first elements of both input vectors, and so on.

``` r
vec_char2 <- paste(c("red", "yellow", "orange", "green"), vec_char)
print(vec_char2)
```

    ## [1] "red apple"       "yellow banana"   "orange mandarin" "green melon"

Finally, you can attach a name to each element of a numeric vector using `names()`. Naming elements keeps track of what each number refers to, exactly how **quanteda** keeps track of document names in a corpus.

``` r
names(vec_num) <- vec_char
print(vec_num)
```

    ##    apple   banana mandarin    melon 
    ##        1        5        6        3

### Data frames

A data frame combines multiple vectors side by side to construct a dataset, much like a spreadsheet with rows and columns. You can only combine vectors into a data frame if they have the same length, since every row needs a value in every column. But the columns can hold different types of data, such as text in one column and numbers in another. `nrow()` and `ncol()` show the number of rows (observations) and columns (variables) in a data frame.

``` r
dat_fruit <- data.frame(name = vec_char, count = vec_num)
print(dat_fruit)
```

    ##              name count
    ## apple       apple     1
    ## banana     banana     5
    ## mandarin mandarin     6
    ## melon       melon     3

``` r
print(nrow(dat_fruit))
```

    ## [1] 4

``` r
print(ncol(dat_fruit))
```

    ## [1] 2

You can use `subset()` to keep only the rows that meet some condition, in the same way that you compared elements of a vector above.

``` r
dat_fruit_sub <- subset(dat_fruit, count >= 5)
print(dat_fruit_sub)
```

    ##              name count
    ## banana     banana     5
    ## mandarin mandarin     6

``` r
print(nrow(dat_fruit_sub))
```

    ## [1] 2

``` r
print(ncol(dat_fruit_sub))
```

    ## [1] 2

Notice that `dat_fruit_sub` has fewer rows than `dat_fruit`, because only the fruits with a count of five or more were kept, though it still keeps the same two columns. `corpus_subset()` and `dfm_subset()`, two functions you will use later to keep only the documents you are interested in, work the same way.

{{% notice tip %}}
We use `print()` to show values and structures of objects in the examples. You do not need it in the console, since output is triggered automatically when objects are returned to the global environment.
{{% /notice %}}

### Matrices

Similar to a data frame, a matrix arranges data into rows and columns. In contrast to a data frame, every value in a matrix must be the same type, usually numbers. A document-feature matrix in **quanteda** has exactly this shape: documents as rows, words as columns, and the number of times each word appears as the values in between.

``` r
mat <- matrix(c(1, 3, 6, 8, 3, 5, 2, 7), nrow = 2)
print(mat)
```

    ##      [,1] [,2] [,3] [,4]
    ## [1,]    1    6    3    2
    ## [2,]    3    8    5    7

You can use `colnames()` or `rownames()` to set or retrieve names for the rows or columns of a matrix, which makes it much easier to read than a matrix of bare numbers.

``` r
colnames(mat) <- vec_char
print(mat)
```

    ##      apple banana mandarin melon
    ## [1,]     1      6        3     2
    ## [2,]     3      8        5     7

``` r
rownames(mat) <- c("bag1", "bag2")
print(mat)
```

    ##      apple banana mandarin melon
    ## bag1     1      6        3     2
    ## bag2     3      8        5     7

You can obtain the size of a matrix with `dim()`, which returns a two-element numeric vector giving the number of rows and the number of columns, in that order.

``` r
print(dim(mat))
```

    ## [1] 2 4

If a matrix has column and row names, you can extract rows or columns by their names instead of having to remember their position.

``` r
print(mat["bag1", ])
```

    ##    apple   banana mandarin    melon 
    ##        1        6        3        2

``` r
print(mat[, "banana"])
```

    ## bag1 bag2 
    ##    6    8

Finally, you can obtain the row totals or column totals of a matrix with `rowSums()` or `colSums()`. When you build a document-feature matrix later in this tutorial, these same two functions will tell you how many words are in each document. They will also tell you how often each word appears across the whole collection of documents.

``` r
print(rowSums(mat))
```

    ## bag1 bag2 
    ##   12   23

``` r
print(colSums(mat))
```

    ##    apple   banana mandarin    melon 
    ##        4       14        8        9

{{% notice tip %}}
If you want to know the details of R commands, prepend `?` to the command and execute. For example, `?subset()` will show you how to use the subset function with different types of objects.
{{% /notice %}}

You are ready to start using **quanteda** in the next chapter.

## References
