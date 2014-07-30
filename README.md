

# rtype

[![Build Status](https://travis-ci.org/renkun-ken/rtype.png?branch=master)](https://travis-ci.org/renkun-ken/rtype)

rtype is a strong type system for R which supports symbol declaration and assignment with type checking.

[Release notes](https://github.com/renkun-ken/rtype/releases)

## Installation

```r
devtools::install_github("rtype","renkun-ken")
```

## Getting started

Declare symbols Before using them.


```r
declare(x,y=numeric(),z=logical())
ls.str()
```

```
x :  NULL
y :  num(0) 
z :  logi(0) 
```

Assign values with type checking


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
Error: Symbol is not integer
```

```r
# the assignment fails if the value does not pass type checking
# is.logical(z) = TRUE, but is.logical(c(1,2,3)) = FALSE (violation)
logical(z) <- c(1,2,3)
```

```
Error: Value is not logical
```

## Help overview

```r
help(package = rtype)
```

## License

This package is under [MIT License](http://opensource.org/licenses/MIT).
