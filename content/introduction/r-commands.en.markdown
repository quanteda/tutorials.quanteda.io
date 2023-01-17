---
title: "R commands"
weight: 10
draft: false
---

You do not need to have advanced knowledge of the R programming language to perform text analysis with **quanteda** because the package has a wide range of functions. However, you still need to understand a number of basic R commands.

## Basic R objects and commands

R has three types of objects: *vector*, *data frame* and *matrix*. Since many of the **quanteda** objects behave similarly to these objects, you need to understand how to interact with them.

### Vectors

As a language for statistical analysis, R"s most basic objects are vectors. Vectors contain a set of values. In the examples below, `vec_num` is a *numeric vector*, while `vec_char` is a *chracter vector*. We use `c()` to combine elements of a vector and `<-` to assign a vector to a variable. 


```r
vec_num <- c(1, 5, 6, 3)
print(vec_num)
```

```
## [1] 1 5 6 3
```

```r
vec_char <- c("apple", "banana", "mandarin", "melon")
print(vec_char)
```

```
## [1] "apple"    "banana"   "mandarin" "melon"
```

Once a vector is created, you can extract elements of vectors with the `[]` operator and index numbers of desired elements.


```r
print(vec_num[1])
```

```
## [1] 1
```

```r
print(vec_num[1:2])
```

```
## [1] 1 5
```

```r
print(vec_char[c(1, 3)])
```

```
## [1] "apple"    "mandarin"
```

You can apply arithmetical operations such as addition, subtraction, multiplication or division on numeric vectors. If only a single value is given for multiplication, for example, each element of the vector will be multiplied by the same value.


```r
vec_num2 <- vec_num * 2
print(vec_num2)
```

```
## [1]  2 10 12  6
```

You can also compare elements of a vector by relational operators such as `==`, `>=`, `>`, `<=`, `<`. The result of these operations will be a *logical vector* that contains either `TRUE` or `FALSE`.


```r
vec_logi_gt5 <- vec_num >= 5
print(vec_logi_gt5)
```

```
## [1] FALSE  TRUE  TRUE FALSE
```

You cannot apply arithmetical operations on character vectors, but can apply the equality operator.


```r
vec_logi_apple <- vec_char == "apple"
print(vec_logi_apple)
```

```
## [1]  TRUE FALSE FALSE FALSE
```

You can also concatenate elements of character vectors using `paste()`. Since the two vectors in the example have the same length, elements in the same position of the vectors are concatenated. 


```r
vec_char2 <- paste(c("red", "yellow", "orange", "green"), vec_char)
print(vec_char2)
```

```
## [1] "red apple"       "yellow banana"   "orange mandarin" "green melon"
```

Finally, you can set names to elements of a numeric vector using `names()`.


```r
names(vec_num) <- vec_char
print(vec_num)
```

```
##    apple   banana mandarin    melon 
##        1        5        6        3
```

### Data frames

A data frame combines multiple vectors to construct a dataset. You can only combine vectors into a data frame if they have the same lengths. However, they can be different types. `nrow()` and `ncol()` show the number of rows (observations) and variables in a data frame.


```r
dat_fruit <- data.frame(name = vec_char, count = vec_num)
print(dat_fruit)
```

```
##              name count
## apple       apple     1
## banana     banana     5
## mandarin mandarin     6
## melon       melon     3
```

```r
print(nrow(dat_fruit))
```

```
## [1] 4
```

```r
print(ncol(dat_fruit))
```

```
## [1] 2
```

You can use `subset()` to select records in the data frame. 


```r
dat_fruit_sub <- subset(dat_fruit, count >= 5)
print(dat_fruit_sub)
```

```
##              name count
## banana     banana     5
## mandarin mandarin     6
```

```r
print(nrow(dat_fruit_sub))
```

```
## [1] 2
```

```r
print(ncol(dat_fruit_sub))
```

```
## [1] 2
```

{{% notice tip %}}
We use `print()` to show values and structures of objects in the examples, but you do not need to use the `print()` command in the console, because it is triggered automatically when objects are returned to the global environment.
{{% /notice %}}

### Matrices

Similar to a data frame, a matrix contains multi-dimensional data. In contrast to a data frame, its values must all be the same type.


```r
mat <- matrix(c(1, 3, 6, 8, 3, 5, 2, 7), nrow = 2)
print(mat)
```

```
##      [,1] [,2] [,3] [,4]
## [1,]    1    6    3    2
## [2,]    3    8    5    7
```

You can use `colnames()` or `rownames()` to set/retrieve names to rows or columns of a matrix.


```r
colnames(mat) <- vec_char
print(mat)
```

```
##      apple banana mandarin melon
## [1,]     1      6        3     2
## [2,]     3      8        5     7
```

```r
rownames(mat) <- c("bag1", "bag2") 
print(mat)
```

```
##      apple banana mandarin melon
## bag1     1      6        3     2
## bag2     3      8        5     7
```

You can obtain the size of a matrix by `dim()` that returns a two-element numeric vector.


```r
print(dim(mat))
```

```
## [1] 2 4
```

If a matrix has column and row names, you can extract rows or columns by their names.


```r
print(mat["bag1", ])
```

```
##    apple   banana mandarin    melon 
##        1        6        3        2
```

```r
print(mat[, "banana"])
```

```
## bag1 bag2 
##    6    8
```

Finally, you can obtain marginals of matrix by `colSums()` or `rowSums()`.


```r
print(rowSums(mat))
```

```
## bag1 bag2 
##   12   23
```

```r
print(colSums(mat))
```

```
##    apple   banana mandarin    melon 
##        4       14        8        9
```

{{% notice tip %}}
If you want to know the details of R commands, prepend `?` to the command and execute. For example, `?subset()` will show you how to use the subset function with different types of objects.
{{% /notice %}}
