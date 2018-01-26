---
title: "R commands"
weight: 5
draft: true
---

You do not need to write a program to perfrom text analysis with **quanteda**, becasue the package has wide range of functions. However, you still have to understand the basic R commands.

## Basic R objects and operations

R has three

### Vectors

As a language for statistical analysis, R's most basic objects are vectors that containe a set values. In the example below, `num_vec` is a *numeric vector* and `char_vec` is a *chracter vector*, and `c()` is used to combine elements of vectors and `<-` is to assign the vector to the variables. 


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

On numeric vectors, you can apply alrithmatic operartions such as addition, subtraction, multiplication or division. If only a single values is given for multiplication, for example, each elelent of the vector will be multiplied.  


```r
num_vec2 <- num_vec * 2
print(num_vec2)
```

```
## [1]  2 10 12  6
```

You can also evaluate the values of vectors by relational operators such as `==`, `>=`, `>`, `<=`, `<`. The ressult of the operation will be a *logical vector*.


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

You can also concatenate elements of character vectors by `paste()`. Since the two vectors have the same length, elements in the same positions of the vectors are concatenated. 


```r
char_vec2 <- paste(c('red', 'yellow', 'orange', 'green'), char_vec)
print(char_vec2)
```

```
## [1] "red apple"       "yellow banana"   "orange mandarin" "green melon"
```

Finally, you can set names to elements of numeric vectors using `names()`.


```r
names(num_vec) <- char_vec
print(num_vec)
```

```
##    apple   banana mandarin    melon 
##        1        5        6        3
```


### Data frames

R basi


```r
df <- data.frame(name = char_vec, count = num_vec )

subset(df, count >= 5)
```

```
##              name count
## banana     banana     5
## mandarin mandarin     6
```


### Matrices


```r
mat <- matrix(c(1,3,6,8,3,5,2,7), nrow = 2)
print(mat)
```

```
##      [,1] [,2] [,3] [,4]
## [1,]    1    6    3    2
## [2,]    3    8    5    7
```

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


```r
mat['bag1',]
```

```
##    apple   banana mandarin    melon 
##        1        6        3        2
```

```r
mat[,'banana']
```

```
## bag1 bag2 
##    6    8
```



```r
rowSums(mat)
```

```
## bag1 bag2 
##   12   23
```

```r
colSums(mat)
```

```
##    apple   banana mandarin    melon 
##        4       14        8        9
```



## Open help file



