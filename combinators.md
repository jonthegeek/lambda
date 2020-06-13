Lambda Calculus
================
Jon Harmon
6/13/2020

## Idiot

Take input and return it.

``` r
I <- function(x) x
I(5) == 5
```

    ## [1] TRUE

## Mockingbird

Take input and return that input called on itself.

``` r
M <- function(f) f(f)
M(I)(5) == I(5)
```

    ## [1] TRUE

## Kestrel

Take input and return a function that takes new input and returns the
original input.

``` r
K <- function(c) function(i) c
identical(K(I)(2), I)
```

    ## [1] TRUE

``` r
K(I)(2)(3) == 3
```

    ## [1] TRUE

## Kite

Take input and return a function that ignores that input and returns its
own input.

``` r
KI <- function(x) K(I)(x)
KI(2)(3) == 3
```

    ## [1] TRUE

## Aside: Booleans

``` r
T <- K
F <- KI
# Mess with the srcref so returning F shows FALSE and T shows TRUE
attr(T, "srcref") <- TRUE
attr(F, "srcref") <- FALSE
T
```

    ## TRUE
    ## <bytecode: 0x00000000143793c0>

``` r
F
```

    ## FALSE

``` r
NOT <- function(p) p(F)(T)
NOT(T)
```

    ## FALSE

``` r
NOT(F)
```

    ## TRUE
    ## <bytecode: 0x00000000143793c0>

``` r
AND <- function(p) function(q) p(q)(p)
AND(F)(F)
```

    ## FALSE
    ## <bytecode: 0x0000000014b72250>

``` r
AND(T)(T)
```

    ## TRUE
    ## <bytecode: 0x00000000143793c0>

``` r
AND(F)(T)
```

    ## FALSE
    ## <bytecode: 0x0000000014b72250>

``` r
AND(T)(F)
```

    ## FALSE
    ## <bytecode: 0x0000000014b72250>

``` r
OR <- function(p) function(q) p(p)(q)
OR(F)(F)
```

    ## FALSE
    ## <bytecode: 0x0000000014b72250>

``` r
OR(F)(T)
```

    ## TRUE
    ## <bytecode: 0x00000000143793c0>

``` r
OR(T)(F)
```

    ## TRUE
    ## <bytecode: 0x00000000143793c0>

``` r
OR(T)(T)
```

    ## TRUE
    ## <bytecode: 0x00000000143793c0>

OR is a special case of the mockingbird\!

``` r
M(F)(F)
```

    ## FALSE
    ## <bytecode: 0x0000000014b72250>

``` r
M(F)(T)
```

    ## TRUE
    ## <bytecode: 0x00000000143793c0>

``` r
M(T)(F)
```

    ## TRUE
    ## <bytecode: 0x00000000143793c0>

``` r
M(T)(T)
```

    ## TRUE
    ## <bytecode: 0x00000000143793c0>

``` r
EQUAL <- function(p) function(q) p(q)(NOT(q))
EQUAL(T)(T)
```

    ## TRUE
    ## <bytecode: 0x00000000143793c0>

``` r
EQUAL(T)(F)
```

    ## FALSE
    ## <bytecode: 0x0000000014b72250>

``` r
EQUAL(F)(T)
```

    ## FALSE
    ## <bytecode: 0x0000000014b72250>

``` r
EQUAL(F)(F)
```

    ## TRUE
    ## <bytecode: 0x00000000143793c0>

## Cardinal

Take a 2-argument function and return that function but with its
arguments reversed. All of the sudden I need waaaay more complicated R
code. OH\! Except the cardinal *is* NOT, so I can get closer.

``` r
C <- function(f) {
  function(a) function(b) f(b)(a)
}
```

This works for any function written in the same formal way, so, for
example:

``` r
testing <- function(a) function(b) paste(a, b)
testing(1)(2)
```

    ## [1] "1 2"

``` r
testing(2)(1)
```

    ## [1] "2 1"

``` r
C(testing)(1)(2) == testing(2)(1)
```

    ## [1] TRUE

I’d like to test these equalities using `EQUAL`, but I need everything
to be functions (and accept functions) for that to work, so… not yet.
