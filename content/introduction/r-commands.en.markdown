---
title: "R commands"
weight: 5
draft: true
---

You do not need to write an R program to perfrom text analysis with **quanteda**, becasue the package has wide range of functions. However, you still have to understand the basic R commands.

## Basic R objects and operations

R has three types of objects vector, data frame and matrix. Since many of the **quanteda** objects behaves similarly to these objects, it is essential for you to understand how to interact with them.

### Vectors

As a language for statistical analysis, R's most basic objects are vectors. Vectors containe a set values, and `num_vec` is a *numeric vector* and `char_vec` is a *chracter vector* in the example below. We use `c()` to combine elements of a vector and `<-` to assign a vector to a variable. 


```r
num_vec <- c(1, 5, 6, 3)
print(num_vec)
```

```
## [1] 1 5 6 3
```

```r
char_vec <- c('apple', 'banana', 'mandarin', 'melon')
print(char_vec)
```

```
## [1] "apple"    "banana"   "mandarin" "melon"
```

Once a vector is created, you can extract elements of vectors by the `[]` operator with index numbers of desired elements.


```r
print(num_vec[1])
```

```
## [1] 1
```

```r
print(num_vec[1:2])
```

```
## [1] 1 5
```

```r
print(char_vec[c(1, 3)])
```

```
## [1] "apple"    "mandarin"
```

You can apply alrithmatic operartions such as addition, subtraction, multiplication or division On numeric vectors. If only a single value is given for multiplication, for example, each elelent of the vector will be multiplied by the same value.  


```r
num_vec2 <- num_vec * 2
print(num_vec2)
```

```
## [1]  2 10 12  6
```

You can also compare elements of a vector by relational operators such as `==`, `>=`, `>`, `<=`, `<`. The ressult of these operations will be a *logical vector* that contains either `TRUE` or `FALSE`.


```r
logi_gt5_vec <- num_vec >= 5
print(logi_gt5_vec)
```

```
## [1] FALSE  TRUE  TRUE FALSE
```

You cannot apply alrithmatic operations on character vectors, but can apply equality operator.


```r
logi_apple_vec <- char_vec == 'apple'
print(logi_apple_vec)
```

```
## [1]  TRUE FALSE FALSE FALSE
```

You can also concatenate elements of character vectors by `paste()`. Since the two vectors in the example have the same length, elements at the same positions of the vectors are concatenated. 


```r
char_vec2 <- paste(c('red', 'yellow', 'orange', 'green'), char_vec)
print(char_vec2)
```

```
## [1] "red apple"       "yellow banana"   "orange mandarin" "green melon"
```

Finally, you can set names to elements of a numeric vector using `names()`.


```r
names(num_vec) <- char_vec
print(num_vec)
```

```
##    apple   banana mandarin    melon 
##        1        5        6        3
```

### Data frames

A data frame combines multiple vectors to construct a dataset. Vectors for a data frame must have the same lengths but can be different types. `nrow()` and `ncol()` show the number of records and variables in a data frame.


```r
fruit_df <- data.frame(name = char_vec, count = num_vec )
print(fruit_df)
```

```
##              name count
## apple       apple     1
## banana     banana     5
## mandarin mandarin     6
## melon       melon     3
```

```r
print(nrow(fruit_df))
```

```
## [1] 4
```

```r
print(ncol(fruit_df))
```

```
## [1] 2
```

You can use `subset()` to select records in the data frame. 


```r
fruit_df2 <- subset(fruit_df, count >= 5)
print(fruit_df2)
```

```
##              name count
## banana     banana     5
## mandarin mandarin     6
```

```r
print(nrow(fruit_df2))
```

```
## [1] 2
```

```r
print(ncol(fruit_df2))
```

```
## [1] 2
```

{{% notice tip %}}
We use `print()` to show values and structures of objects in the examples, but you do not need to use the command in the console, becasue it is triggered automatically when objects are returned to the global environment.
{{% /notice %}}

### Matrices

Similar to a data frame, a matrix contains multi-dimensional data but its values are all in the same type.


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
colnames(mat) <- char_vec
print(mat)
```

```
##      apple banana mandarin melon
## [1,]     1      6        3     2
## [2,]     3      8        5     7
```

```r
rownames(mat) <- c('bag1', 'bag2') 
print(mat)
```

```
##      apple banana mandarin melon
## bag1     1      6        3     2
## bag2     3      8        5     7
```

You can obtaine the size of a matrix by `dim()` that returns a two-element numeric vector.


```r
print(dim(mat))
```

```
## [1] 2 4
```

If a matrix has column and row names, you can extract rows or columns by their names.


```r
print(mat['bag1',])
```

```
##    apple   banana mandarin    melon 
##        1        6        3        2
```

```r
print(mat[,'banana'])
```

```
## bag1 bag2 
##    6    8
```

Finally, you can obtaine marginals of matrix by `colSums()` or `rowSums()`.


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

{{% notice info %}}
If you want to know the dtails of R commands, prepend `?` to the command and execute. For example, `?subset()` will show you how to use it with different types of objects.
{{% /notice %}}
