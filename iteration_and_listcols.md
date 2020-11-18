Iteration and listcols
================

## Lists

You can put anything in a list.

``` r
l = list(
  vec_numeric = 5:8,
  vec_logical = c(TRUE, TRUE, FALSE, TRUE, FALSE, FALSE),
  mat = matrix(1:8, nrow = 2, ncol = 4),
  summary = summary(rnorm(100))
)
```

``` r
l
```

    ## $vec_numeric
    ## [1] 5 6 7 8
    ## 
    ## $vec_logical
    ## [1]  TRUE  TRUE FALSE  TRUE FALSE FALSE
    ## 
    ## $mat
    ##      [,1] [,2] [,3] [,4]
    ## [1,]    1    3    5    7
    ## [2,]    2    4    6    8
    ## 
    ## $summary
    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ## -2.7277 -0.4213  0.1957  0.2281  0.9296  2.2504

``` r
l$vec_numeric
```

    ## [1] 5 6 7 8

``` r
l[[1]]
```

    ## [1] 5 6 7 8

``` r
mean(l[["vec_numeric"]])
```

    ## [1] 6.5

## `for` loop

Create a new list.

``` r
list_norm = 
  list(
    a = rnorm(20, mean = 3, sd = 1),
    b = rnorm(30, mean = 0, sd = 5),
    c = rnorm(40, mean = 10, sd = .2),
    d = rnorm(20, mean = -3, sd = 1)
  )
```

``` r
list_norm
```

    ## $a
    ##  [1] 2.263155 2.380943 1.996792 1.824141 2.703546 2.869567 3.051443 3.426094
    ##  [9] 1.677299 4.153675 3.328096 2.726894 3.040464 3.530404 3.632910 3.409436
    ## [17] 2.039146 2.634202 3.454234 5.238637
    ## 
    ## $b
    ##  [1]   9.1361392   0.1051339   4.3670180  -0.8161387   0.8698341  -1.5068419
    ##  [7]  -3.9462708   2.7524730   2.7136311   6.1085049   2.1153668  -3.8372511
    ## [13]   2.1690129   0.6698434  -3.1875320   3.6992668  -7.3804453   1.5809081
    ## [19] -13.8422072  -9.1452365 -10.6303106  -1.4316344   3.5428809   6.0113354
    ## [25]  -0.9841755  -3.0883375   1.8296405  -2.0736876  -1.5108675   4.4202428
    ## 
    ## $c
    ##  [1] 10.352462  9.450501  9.763063 10.231698  9.906867 10.364547  9.888313
    ##  [8] 10.462587  9.999173  9.710174  9.956751 10.025543 10.159846 10.024204
    ## [15]  9.958346  9.808001  9.835207 10.213972 10.048837 10.069432  9.920080
    ## [22] 10.077900 10.192284 10.077815 10.273194  9.798353 10.055302 10.151529
    ## [29]  9.777820 10.064470 10.105193 10.139750  9.871712 10.176637 10.166235
    ## [36] 10.179278  9.864395  9.921223  9.926951 10.183006
    ## 
    ## $d
    ##  [1] -1.9718792 -2.5983391 -1.9132083 -2.9376735 -3.4184694 -3.0572191
    ##  [7] -4.8156731 -1.3960893 -0.3996826 -2.4183168 -2.7735044 -2.4839078
    ## [13] -3.4093465 -1.3549033 -3.0607135 -3.0674723 -2.7137140 -4.2899659
    ## [19] -2.4558765 -2.0901582

Pause and get my old function.

``` r
mean_and_sd = function(x) {
  
  if (!is.numeric(x)) {
    stop("Input must be numeric")
  }
  
  if (length(x) < 3) {
    stop("Input must have at least three numbers")
  }
  
  mean_x = mean(x)
  sd_x = sd(x)
  
  tibble(
    mean = mean_x,
    sd = sd_x
  )
  
}
```

I can apply that function to each list element.

``` r
mean_and_sd(list_norm[[1]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  2.97 0.858

``` r
mean_and_sd(list_norm[[2]])
```

    ## # A tibble: 1 x 2
    ##     mean    sd
    ##    <dbl> <dbl>
    ## 1 -0.376  5.10

``` r
mean_and_sd(list_norm[[3]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  10.0 0.200

``` r
mean_and_sd(list_norm[[4]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -2.63 0.997

Let’s use a for loop:

``` r
output = vector("list", length = 4)

for (i in 1:4) {
  
  output[[i]] = mean_and_sd(list_norm[[i]])
  
}
```
