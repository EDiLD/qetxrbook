

## Data types in R

Before starting with data analyses we need to know which types of data R can handle.
Here we discuss only the basic types `numeric`, `integer`, `character` and `logical`.


### Numerics
We already encountered the *numeric* data type in the previous section:


```r
a <- 2.5
```

We can check what type an object is with the class function


```r
class(a)
```

```
## [1] "numeric"
```

Note that integers are also stored as numeric:


```r
b <- 2
class(b)
```

```
## [1] "numeric"
```

We can also confirm the type of an object using the `is.*()` functions (where * is a placeholder for `numeric`, `integer`, `logical` or `character`).


```r
is.numeric(b)
```

```
## [1] TRUE
```

```r
is.integer(b)
```

```
## [1] FALSE
```


### Integers

There isn't a big practical difference between integers and numeric in R. However, it is useful to know about them.

To specify an integer object we need to add `L` to the integer:


```r
c <- 2L
class(c)
```

```
## [1] "integer"
```

We can also convert a numeric to integer using the `as.integer()` function.


```r
b
```

```
## [1] 2
```

```r
class(b)
```

```
## [1] "numeric"
```

```r
d <- as.integer(b)
d
```

```
## [1] 2
```

```r
class(d)
```

```
## [1] "integer"
```

There are also equivalent `as.*()` functions for `numeric`, `logical` or `character`.
However, be careful we converting numerics:

```r
as.integer(6.23)
as.integer(6.7)
```

```
## [1] 6
## [1] 6
```

Also converting strings make no sense

```r
as.integer('Hello World')
```

```
## Warning: NAs introduced by coercion
```

```
## [1] NA
```

and R returns `NA` accomnied with a warning that it cannot coerce to a integer.


### Characters

Character objects are used to represent string values in R:


```r
a <- 'Hello World'
class(a)
```

```
## [1] "character"
```

We can always coerce integers and numerics to characters:



```r
as.character(2L)
as.character(2.6)
```

```
## [1] "2"
## [1] "2.6"
```

and also sometimes (when possible) the way round:


```r
as.numeric(as.character(2.6))
```

```
## [1] 2.6
```

```r
as.numeric('Hello World')
```

```
## Warning: NAs introduced by coercion
```

```
## [1] NA
```

With `numeric` and `integers` we can do arithmetics,

```r
2 + 1
2L + 1
```

```
## [1] 3
## [1] 3
```

, but not with `charactars` this is meaningless and gives this error


```r
'Hello world' + 1
```

```
## Error in "Hello world" + 1: non-numeric argument to binary operator
```



### Logicals

We already encoutered logicals when checking objects with the `is.*()` function. 
This function returned either `TRUE` or `FALSE`. 
Logicals are also returned when comparing two numerics


```r
2 < 3  # is 2 smaller then 3?
5 > 6  # is 5 bigger the 6
5 > 5 # is 5 greater then 5
5 >= 5 # is 5 greater or equal then 5
```

```
## [1] TRUE
## [1] FALSE
## [1] FALSE
## [1] TRUE
```

Logical operators in R are `&` (AND), `|` OR and `!` (negotiation):



```r
TRUE & TRUE
TRUE & FALSE
FALSE & TRUE
FALSE & FALSE
```

```
## [1] TRUE
## [1] FALSE
## [1] FALSE
## [1] FALSE
```


```r
TRUE | TRUE
TRUE | FALSE
FALSE | TRUE
FALSE | FALSE
```

```
## [1] TRUE
## [1] TRUE
## [1] TRUE
## [1] FALSE
```


```r
TRUE
!TRUE
```

```
## [1] TRUE
## [1] FALSE
```


Logicals can be coerced to *integers* or *numerics*, with `TRUE` beeing mapped to 1 and `FALSE` to 0.


```r
as.numeric(TRUE)
```

```
## [1] 1
```

This is handy because we can also calculated the sum:


```r
TRUE + TRUE
```

```
## [1] 2
```


```r
3*TRUE + FALSE + TRUE
```

```
## [1] 4
```


### Factors
Factors are a fourth, more special, data type in R. 
You can think of factors as special character vectors with some nice additional functions.

You can create factors using the `factor` function:

```r
fac <- factor(c('A', 'A', 'B', 'B'))
class(fac)
```

```
## [1] "factor"
```

```r
fac
```

```
## [1] A A B B
## Levels: A B
```

From the output you see that factors store another information: the `levels` of the data. 

In fact R stores them internally as integers and only labels these integers according to it's levels.
Therefore, `as.numeric() return a vector of these internal integers:


```r
as.numeric(fac)
```

```
## [1] 1 1 2 2
```

and not an error like above with character vectors.

Be careful with numeric factors!
While for character vectors that can be coerced to numeric, ´as.numeric` returns the desired result



```r
char <- c('2', '2', '3', '3')
as.numeric(char)
```

```
## [1] 2 2 3 3
```

this is not the case for factors:


```r
as.numeric(factor(char))
```

```
## [1] 1 1 2 2
```

Here it returns the internal integer value (these are ordered by alphabet).

To return the desired result, first coerce to character (making R to forget about the levels) and then to numeric.


```r
as.numeric(as.character(factor(char)))
```

```
## [1] 2 2 3 3
```


You can access the levels of a factor using the `levels()` fucntion:


```r
levels(fac)
```

```
## [1] "A" "B"
```


Factors are useful for categorical data and we will encounter them later on when dealing with linear models. For now just keep in mind that factors are special characters, with special properties.




