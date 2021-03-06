

# rtype

[![Build Status](https://travis-ci.org/renkun-ken/rtype.png?branch=master)](https://travis-ci.org/renkun-ken/rtype)

rtype is a strong type system for R which supports symbol declaration and assignment with type checking.

[Release notes](https://github.com/renkun-ken/rtype/releases)

## Installation

You can install the latest released version from [CRAN](http://cran.r-project.org/web/packages/rtype/) with

```r
install.packages("rtype")
```

or the latest development version from GitHub with

```r
devtools::install_github("rtype","renkun-ken")
```

## Examples

### Declaring symbols

Declare symbols Before using them.


```r
library(rtype)
declare(x,y=numeric(),z=logical())
ls.str()
```

```
x :  NULL
y :  num(0) 
z :  logi(0) 
```

### Assignment with type checking


```r
# NULL symbol can be assigned any value
numeric(x) <- c(1,2,3.5)

# numeric symbol can be assigned only numeric value
# is.numeric(y) = TRUE, and is.numeric(c(1,2,3)) = TRUE
numeric(y) <- c(1,2,3)

# the symbol does not take value that violates its type checking
# is.integer(y) = FALSE (violation)
integer(y) <- c(1L,2L)
```

```
Error: symbol fails type checking with .Primitive("is.integer")
```

```r
# the assignment fails if the value does not pass type checking
# is.logical(z) = TRUE, but is.logical(c(1,2,3)) = FALSE (violation)
logical(z) <- c(1,2,3)
```

```
Error: value fails type checking with .Primitive("is.logical")
```

### Assignment with condition checking

Checking single condition


```r
# assign value checking condition length = 3
numeric(x, length = 3) <- c(1,2,3)

# stop if condition is violated
numeric(x, length = 3) <- c(1,2,3,4)
```

```
Error: value [length = 4L] violates condition [length = 3]
```


Checking multiple conditions


```r
# assign value checking multiple conditions
declare(df)
data.frame(df, ncol=2, nrow=10) <- data.frame(x=1:10,y=letters[1:10])

# or equivalently
data.frame(df, dim=c(10,2)) <- data.frame(x=1:10,y=letters[1:10])

# stop if any condition is violated
data.frame(df, ncol=3, nrow=10) <- data.frame(x=1:10,y=letters[1:10])
```

```
Error: value [ncol = 2L] violates condition [ncol = 3]
```

Checking function conditions


```r
# checking function condition
declare(x)
cond1 <- function(x) mean(x) <= 5
numeric(x, length = 10, cond1) <- 0:9
numeric(x, length = 10, cond1) <- 1:10
```

```
Error: value violates condition [function (x) mean(x) <= 5]
```

General checking


```r
declare(x)
check(x, class="integer", length=10) <- 1:10
check(x, class="numeric", length=10) <- 1:10
```

```
Error: value [class = "integer"] violates condition [class = "numeric"]
```

## Help overview

```r
help(package = rtype)
```

## License

This package is under [MIT License](http://opensource.org/licenses/MIT).
