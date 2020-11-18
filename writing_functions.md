Writing functions
================

## Do something simple

``` r
x_vec = rnorm(30, mean = 5, sd = 3)

(x_vec - mean(x_vec)) / sd(x_vec)
```

    ##  [1] -0.20544214  1.54094736  1.18031507 -0.15410133  0.93484467  0.38442871
    ##  [7]  0.78538699 -1.67644761 -0.92635146  0.53136133  0.13202635  0.07049412
    ## [13] -0.32550204  1.25577348 -1.30350777  0.11088164  1.95434886 -2.15669435
    ## [19]  0.64882559 -0.34634929 -0.40136524 -1.15606179  1.35660754 -0.46562665
    ## [25]  0.82411926  0.02596710 -0.40656600 -0.77391460  0.02747644 -1.46587425

I want a function to compute z-scores

``` r
z_scores = function(x) {
  
  if (!is.numeric(x)) {
    stop("Input must be numeric")
  }
  
  if (length(x) < 3) {
    stop("Input must have at least three numbers")
  }
  
  z = (x - mean(x)) / sd(x)
  
  return(z)
}

z_scores(x_vec)
```

    ##  [1] -0.20544214  1.54094736  1.18031507 -0.15410133  0.93484467  0.38442871
    ##  [7]  0.78538699 -1.67644761 -0.92635146  0.53136133  0.13202635  0.07049412
    ## [13] -0.32550204  1.25577348 -1.30350777  0.11088164  1.95434886 -2.15669435
    ## [19]  0.64882559 -0.34634929 -0.40136524 -1.15606179  1.35660754 -0.46562665
    ## [25]  0.82411926  0.02596710 -0.40656600 -0.77391460  0.02747644 -1.46587425

Try my function on some other things. These should give errors.

``` r
z_scores(3)
```

    ## Error in z_scores(3): Input must have at least three numbers

``` r
z_scores("my name is jeff")
```

    ## Error in z_scores("my name is jeff"): Input must be numeric

``` r
z_scores(mtcars)
```

    ## Error in z_scores(mtcars): Input must be numeric

``` r
z_scores(c(TRUE, TRUE, FALSE, TRUE))
```

    ## Error in z_scores(c(TRUE, TRUE, FALSE, TRUE)): Input must be numeric
