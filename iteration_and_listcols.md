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
    ##     Min.  1st Qu.   Median     Mean  3rd Qu.     Max. 
    ## -3.11222 -0.66180  0.08355  0.05176  0.79450  2.36830

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
    ##  [1] 1.389118 2.004420 1.258836 1.422562 2.765247 2.437385 3.127733 2.528927
    ##  [9] 1.904243 1.884481 1.766997 2.479501 3.134922 1.862297 4.026221 2.449091
    ## [17] 2.481770 4.932850 3.310308 2.923762
    ## 
    ## $b
    ##  [1]  -3.8875255  -4.6821221 -12.0801679   7.7955927  -8.1411187  -2.1769073
    ##  [7]  -1.3008177   1.8866762   6.1298495  -9.8531164  -0.7935069  -1.9570439
    ## [13]  -4.4140534   0.4124367   0.3231695   2.5117484   5.7981123  -6.4059567
    ## [19]   3.0442003  -0.6368922  -0.7709309   4.0930131  -1.2023796   8.6503044
    ## [25]  -2.9278547   9.7973447  -3.3973081   4.0257761  15.9507632   1.5939547
    ## 
    ## $c
    ##  [1]  9.722237  9.780379 10.172277 10.009278  9.641480  9.837087 10.186060
    ##  [8]  9.715848  9.666944 10.006332 10.007212  9.811223 10.021544 10.134908
    ## [15]  9.748902 10.059377  9.925988 10.158805  9.858473  9.808294 10.079845
    ## [22] 10.133562  9.948770 10.031425 10.061596 10.054855  9.949216  9.799677
    ## [29]  9.991240  9.933373  9.798560  9.865882  9.928914 10.212773  9.994537
    ## [36]  9.735452  9.925775  9.905138  9.515386 10.174192
    ## 
    ## $d
    ##  [1] -1.933217 -3.104516 -3.077417 -2.377372 -3.054325 -3.404076 -2.068465
    ##  [8] -5.682631 -1.984550 -1.381948 -3.710477 -2.957743 -2.215913 -3.465898
    ## [15] -4.176443 -2.317095 -4.018002 -2.458400 -3.567725 -3.202606

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
    ## 1  2.50 0.912

``` r
mean_and_sd(list_norm[[2]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 0.246  6.02

``` r
mean_and_sd(list_norm[[3]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  9.93 0.168

``` r
mean_and_sd(list_norm[[4]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -3.01 0.981

Let’s use a for loop:

``` r
output = vector("list", length = 4)

for (i in 1:4) {
  
  output[[i]] = mean_and_sd(list_norm[[i]])
  
}
```

## Let’s try map\!

``` r
output = map(list_norm, mean_and_sd)
```

what if you want a different function..?

``` r
output = map(list_norm, median)
```

``` r
output = map_dbl(list_norm, median, .id = "input")
```

``` r
output = map_df(list_norm, mean_and_sd, .id = "input")
```

## List columns\!

``` r
listcol_df = 
  tibble(
    name = c("a", "b", "c", "d"),
    samp = list_norm
  )
```

``` r
listcol_df %>% pull(name)
```

    ## [1] "a" "b" "c" "d"

``` r
listcol_df %>% pull(samp)
```

    ## $a
    ##  [1] 1.389118 2.004420 1.258836 1.422562 2.765247 2.437385 3.127733 2.528927
    ##  [9] 1.904243 1.884481 1.766997 2.479501 3.134922 1.862297 4.026221 2.449091
    ## [17] 2.481770 4.932850 3.310308 2.923762
    ## 
    ## $b
    ##  [1]  -3.8875255  -4.6821221 -12.0801679   7.7955927  -8.1411187  -2.1769073
    ##  [7]  -1.3008177   1.8866762   6.1298495  -9.8531164  -0.7935069  -1.9570439
    ## [13]  -4.4140534   0.4124367   0.3231695   2.5117484   5.7981123  -6.4059567
    ## [19]   3.0442003  -0.6368922  -0.7709309   4.0930131  -1.2023796   8.6503044
    ## [25]  -2.9278547   9.7973447  -3.3973081   4.0257761  15.9507632   1.5939547
    ## 
    ## $c
    ##  [1]  9.722237  9.780379 10.172277 10.009278  9.641480  9.837087 10.186060
    ##  [8]  9.715848  9.666944 10.006332 10.007212  9.811223 10.021544 10.134908
    ## [15]  9.748902 10.059377  9.925988 10.158805  9.858473  9.808294 10.079845
    ## [22] 10.133562  9.948770 10.031425 10.061596 10.054855  9.949216  9.799677
    ## [29]  9.991240  9.933373  9.798560  9.865882  9.928914 10.212773  9.994537
    ## [36]  9.735452  9.925775  9.905138  9.515386 10.174192
    ## 
    ## $d
    ##  [1] -1.933217 -3.104516 -3.077417 -2.377372 -3.054325 -3.404076 -2.068465
    ##  [8] -5.682631 -1.984550 -1.381948 -3.710477 -2.957743 -2.215913 -3.465898
    ## [15] -4.176443 -2.317095 -4.018002 -2.458400 -3.567725 -3.202606

``` r
listcol_df %>% 
  filter(name == "a")
```

    ## # A tibble: 1 x 2
    ##   name  samp        
    ##   <chr> <named list>
    ## 1 a     <dbl [20]>

Let’s try some operations.

``` r
mean_and_sd(listcol_df$samp[[1]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  2.50 0.912

``` r
mean_and_sd(listcol_df$samp[[2]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 0.246  6.02

Can I just … map?

``` r
map(listcol_df$samp, mean_and_sd)
```

    ## $a
    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  2.50 0.912
    ## 
    ## $b
    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 0.246  6.02
    ## 
    ## $c
    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  9.93 0.168
    ## 
    ## $d
    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -3.01 0.981

So … can I add a list column??

``` r
listcol_df = 
  listcol_df %>% 
  mutate(
    summary = map(samp, mean_and_sd),
    medians = map_dbl(samp, median))
```
