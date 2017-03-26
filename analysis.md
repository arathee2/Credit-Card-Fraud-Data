Credit Card Fradu Data--Handling unbalanced class
================

data input and a little cleaning
--------------------------------

``` r
library(readr)
cc <- read_csv("creditcard.csv")
```

    ## Parsed with column specification:
    ## cols(
    ##   .default = col_double(),
    ##   Time = col_integer(),
    ##   Class = col_integer()
    ## )

    ## See spec(...) for full column specifications.

``` r
str(cc) # 284807 rows and 31 columns
```

    ## Classes 'tbl_df', 'tbl' and 'data.frame':    284807 obs. of  31 variables:
    ##  $ Time  : int  0 0 1 1 2 2 4 7 7 9 ...
    ##  $ V1    : num  -1.36 1.192 -1.358 -0.966 -1.158 ...
    ##  $ V2    : num  -0.0728 0.2662 -1.3402 -0.1852 0.8777 ...
    ##  $ V3    : num  2.536 0.166 1.773 1.793 1.549 ...
    ##  $ V4    : num  1.378 0.448 0.38 -0.863 0.403 ...
    ##  $ V5    : num  -0.3383 0.06 -0.5032 -0.0103 -0.4072 ...
    ##  $ V6    : num  0.4624 -0.0824 1.8005 1.2472 0.0959 ...
    ##  $ V7    : num  0.2396 -0.0788 0.7915 0.2376 0.5929 ...
    ##  $ V8    : num  0.0987 0.0851 0.2477 0.3774 -0.2705 ...
    ##  $ V9    : num  0.364 -0.255 -1.515 -1.387 0.818 ...
    ##  $ V10   : num  0.0908 -0.167 0.2076 -0.055 0.7531 ...
    ##  $ V11   : num  -0.552 1.613 0.625 -0.226 -0.823 ...
    ##  $ V12   : num  -0.6178 1.0652 0.0661 0.1782 0.5382 ...
    ##  $ V13   : num  -0.991 0.489 0.717 0.508 1.346 ...
    ##  $ V14   : num  -0.311 -0.144 -0.166 -0.288 -1.12 ...
    ##  $ V15   : num  1.468 0.636 2.346 -0.631 0.175 ...
    ##  $ V16   : num  -0.47 0.464 -2.89 -1.06 -0.451 ...
    ##  $ V17   : num  0.208 -0.115 1.11 -0.684 -0.237 ...
    ##  $ V18   : num  0.0258 -0.1834 -0.1214 1.9658 -0.0382 ...
    ##  $ V19   : num  0.404 -0.146 -2.262 -1.233 0.803 ...
    ##  $ V20   : num  0.2514 -0.0691 0.525 -0.208 0.4085 ...
    ##  $ V21   : num  -0.01831 -0.22578 0.248 -0.1083 -0.00943 ...
    ##  $ V22   : num  0.27784 -0.63867 0.77168 0.00527 0.79828 ...
    ##  $ V23   : num  -0.11 0.101 0.909 -0.19 -0.137 ...
    ##  $ V24   : num  0.0669 -0.3398 -0.6893 -1.1756 0.1413 ...
    ##  $ V25   : num  0.129 0.167 -0.328 0.647 -0.206 ...
    ##  $ V26   : num  -0.189 0.126 -0.139 -0.222 0.502 ...
    ##  $ V27   : num  0.13356 -0.00898 -0.05535 0.06272 0.21942 ...
    ##  $ V28   : num  -0.0211 0.0147 -0.0598 0.0615 0.2152 ...
    ##  $ Amount: num  149.62 2.69 378.66 123.5 69.99 ...
    ##  $ Class : int  0 0 0 0 0 0 0 0 0 0 ...
    ##  - attr(*, "problems")=Classes 'tbl_df', 'tbl' and 'data.frame': 1 obs. of  4 variables:
    ##   ..$ row     : int 153759
    ##   ..$ col     : chr "Time"
    ##   ..$ expected: chr "no trailing characters"
    ##   ..$ actual  : chr "e+05"
    ##  - attr(*, "spec")=List of 2
    ##   ..$ cols   :List of 31
    ##   .. ..$ Time  : list()
    ##   .. .. ..- attr(*, "class")= chr  "collector_integer" "collector"
    ##   .. ..$ V1    : list()
    ##   .. .. ..- attr(*, "class")= chr  "collector_double" "collector"
    ##   .. ..$ V2    : list()
    ##   .. .. ..- attr(*, "class")= chr  "collector_double" "collector"
    ##   .. ..$ V3    : list()
    ##   .. .. ..- attr(*, "class")= chr  "collector_double" "collector"
    ##   .. ..$ V4    : list()
    ##   .. .. ..- attr(*, "class")= chr  "collector_double" "collector"
    ##   .. ..$ V5    : list()
    ##   .. .. ..- attr(*, "class")= chr  "collector_double" "collector"
    ##   .. ..$ V6    : list()
    ##   .. .. ..- attr(*, "class")= chr  "collector_double" "collector"
    ##   .. ..$ V7    : list()
    ##   .. .. ..- attr(*, "class")= chr  "collector_double" "collector"
    ##   .. ..$ V8    : list()
    ##   .. .. ..- attr(*, "class")= chr  "collector_double" "collector"
    ##   .. ..$ V9    : list()
    ##   .. .. ..- attr(*, "class")= chr  "collector_double" "collector"
    ##   .. ..$ V10   : list()
    ##   .. .. ..- attr(*, "class")= chr  "collector_double" "collector"
    ##   .. ..$ V11   : list()
    ##   .. .. ..- attr(*, "class")= chr  "collector_double" "collector"
    ##   .. ..$ V12   : list()
    ##   .. .. ..- attr(*, "class")= chr  "collector_double" "collector"
    ##   .. ..$ V13   : list()
    ##   .. .. ..- attr(*, "class")= chr  "collector_double" "collector"
    ##   .. ..$ V14   : list()
    ##   .. .. ..- attr(*, "class")= chr  "collector_double" "collector"
    ##   .. ..$ V15   : list()
    ##   .. .. ..- attr(*, "class")= chr  "collector_double" "collector"
    ##   .. ..$ V16   : list()
    ##   .. .. ..- attr(*, "class")= chr  "collector_double" "collector"
    ##   .. ..$ V17   : list()
    ##   .. .. ..- attr(*, "class")= chr  "collector_double" "collector"
    ##   .. ..$ V18   : list()
    ##   .. .. ..- attr(*, "class")= chr  "collector_double" "collector"
    ##   .. ..$ V19   : list()
    ##   .. .. ..- attr(*, "class")= chr  "collector_double" "collector"
    ##   .. ..$ V20   : list()
    ##   .. .. ..- attr(*, "class")= chr  "collector_double" "collector"
    ##   .. ..$ V21   : list()
    ##   .. .. ..- attr(*, "class")= chr  "collector_double" "collector"
    ##   .. ..$ V22   : list()
    ##   .. .. ..- attr(*, "class")= chr  "collector_double" "collector"
    ##   .. ..$ V23   : list()
    ##   .. .. ..- attr(*, "class")= chr  "collector_double" "collector"
    ##   .. ..$ V24   : list()
    ##   .. .. ..- attr(*, "class")= chr  "collector_double" "collector"
    ##   .. ..$ V25   : list()
    ##   .. .. ..- attr(*, "class")= chr  "collector_double" "collector"
    ##   .. ..$ V26   : list()
    ##   .. .. ..- attr(*, "class")= chr  "collector_double" "collector"
    ##   .. ..$ V27   : list()
    ##   .. .. ..- attr(*, "class")= chr  "collector_double" "collector"
    ##   .. ..$ V28   : list()
    ##   .. .. ..- attr(*, "class")= chr  "collector_double" "collector"
    ##   .. ..$ Amount: list()
    ##   .. .. ..- attr(*, "class")= chr  "collector_double" "collector"
    ##   .. ..$ Class : list()
    ##   .. .. ..- attr(*, "class")= chr  "collector_integer" "collector"
    ##   ..$ default: list()
    ##   .. ..- attr(*, "class")= chr  "collector_guess" "collector"
    ##   ..- attr(*, "class")= chr "col_spec"

``` r
summary(cc) # only one missing value
```

    ##       Time              V1                  V2           
    ##  Min.   :     0   Min.   :-56.40751   Min.   :-72.71573  
    ##  1st Qu.: 54201   1st Qu.: -0.92037   1st Qu.: -0.59855  
    ##  Median : 84692   Median :  0.01811   Median :  0.06549  
    ##  Mean   : 94814   Mean   :  0.00000   Mean   :  0.00000  
    ##  3rd Qu.:139321   3rd Qu.:  1.31564   3rd Qu.:  0.80372  
    ##  Max.   :172792   Max.   :  2.45493   Max.   : 22.05773  
    ##  NA's   :1                                               
    ##        V3                 V4                 V5            
    ##  Min.   :-48.3256   Min.   :-5.68317   Min.   :-113.74331  
    ##  1st Qu.: -0.8904   1st Qu.:-0.84864   1st Qu.:  -0.69160  
    ##  Median :  0.1799   Median :-0.01985   Median :  -0.05434  
    ##  Mean   :  0.0000   Mean   : 0.00000   Mean   :   0.00000  
    ##  3rd Qu.:  1.0272   3rd Qu.: 0.74334   3rd Qu.:   0.61193  
    ##  Max.   :  9.3826   Max.   :16.87534   Max.   :  34.80167  
    ##                                                            
    ##        V6                 V7                 V8           
    ##  Min.   :-26.1605   Min.   :-43.5572   Min.   :-73.21672  
    ##  1st Qu.: -0.7683   1st Qu.: -0.5541   1st Qu.: -0.20863  
    ##  Median : -0.2742   Median :  0.0401   Median :  0.02236  
    ##  Mean   :  0.0000   Mean   :  0.0000   Mean   :  0.00000  
    ##  3rd Qu.:  0.3986   3rd Qu.:  0.5704   3rd Qu.:  0.32735  
    ##  Max.   : 73.3016   Max.   :120.5895   Max.   : 20.00721  
    ##                                                           
    ##        V9                 V10                 V11          
    ##  Min.   :-13.43407   Min.   :-24.58826   Min.   :-4.79747  
    ##  1st Qu.: -0.64310   1st Qu.: -0.53543   1st Qu.:-0.76249  
    ##  Median : -0.05143   Median : -0.09292   Median :-0.03276  
    ##  Mean   :  0.00000   Mean   :  0.00000   Mean   : 0.00000  
    ##  3rd Qu.:  0.59714   3rd Qu.:  0.45392   3rd Qu.: 0.73959  
    ##  Max.   : 15.59500   Max.   : 23.74514   Max.   :12.01891  
    ##                                                            
    ##       V12                V13                V14          
    ##  Min.   :-18.6837   Min.   :-5.79188   Min.   :-19.2143  
    ##  1st Qu.: -0.4056   1st Qu.:-0.64854   1st Qu.: -0.4256  
    ##  Median :  0.1400   Median :-0.01357   Median :  0.0506  
    ##  Mean   :  0.0000   Mean   : 0.00000   Mean   :  0.0000  
    ##  3rd Qu.:  0.6182   3rd Qu.: 0.66251   3rd Qu.:  0.4931  
    ##  Max.   :  7.8484   Max.   : 7.12688   Max.   : 10.5268  
    ##                                                          
    ##       V15                V16                 V17           
    ##  Min.   :-4.49894   Min.   :-14.12985   Min.   :-25.16280  
    ##  1st Qu.:-0.58288   1st Qu.: -0.46804   1st Qu.: -0.48375  
    ##  Median : 0.04807   Median :  0.06641   Median : -0.06568  
    ##  Mean   : 0.00000   Mean   :  0.00000   Mean   :  0.00000  
    ##  3rd Qu.: 0.64882   3rd Qu.:  0.52330   3rd Qu.:  0.39968  
    ##  Max.   : 8.87774   Max.   : 17.31511   Max.   :  9.25353  
    ##                                                            
    ##       V18                 V19                 V20           
    ##  Min.   :-9.498746   Min.   :-7.213527   Min.   :-54.49772  
    ##  1st Qu.:-0.498850   1st Qu.:-0.456299   1st Qu.: -0.21172  
    ##  Median :-0.003636   Median : 0.003735   Median : -0.06248  
    ##  Mean   : 0.000000   Mean   : 0.000000   Mean   :  0.00000  
    ##  3rd Qu.: 0.500807   3rd Qu.: 0.458949   3rd Qu.:  0.13304  
    ##  Max.   : 5.041069   Max.   : 5.591971   Max.   : 39.42090  
    ##                                                             
    ##       V21                 V22                  V23           
    ##  Min.   :-34.83038   Min.   :-10.933144   Min.   :-44.80774  
    ##  1st Qu.: -0.22839   1st Qu.: -0.542350   1st Qu.: -0.16185  
    ##  Median : -0.02945   Median :  0.006782   Median : -0.01119  
    ##  Mean   :  0.00000   Mean   :  0.000000   Mean   :  0.00000  
    ##  3rd Qu.:  0.18638   3rd Qu.:  0.528554   3rd Qu.:  0.14764  
    ##  Max.   : 27.20284   Max.   : 10.503090   Max.   : 22.52841  
    ##                                                              
    ##       V24                V25                 V26          
    ##  Min.   :-2.83663   Min.   :-10.29540   Min.   :-2.60455  
    ##  1st Qu.:-0.35459   1st Qu.: -0.31715   1st Qu.:-0.32698  
    ##  Median : 0.04098   Median :  0.01659   Median :-0.05214  
    ##  Mean   : 0.00000   Mean   :  0.00000   Mean   : 0.00000  
    ##  3rd Qu.: 0.43953   3rd Qu.:  0.35072   3rd Qu.: 0.24095  
    ##  Max.   : 4.58455   Max.   :  7.51959   Max.   : 3.51735  
    ##                                                           
    ##       V27                  V28                Amount        
    ##  Min.   :-22.565679   Min.   :-15.43008   Min.   :    0.00  
    ##  1st Qu.: -0.070840   1st Qu.: -0.05296   1st Qu.:    5.60  
    ##  Median :  0.001342   Median :  0.01124   Median :   22.00  
    ##  Mean   :  0.000000   Mean   :  0.00000   Mean   :   88.35  
    ##  3rd Qu.:  0.091045   3rd Qu.:  0.07828   3rd Qu.:   77.17  
    ##  Max.   : 31.612198   Max.   : 33.84781   Max.   :25691.16  
    ##                                                             
    ##      Class         
    ##  Min.   :0.000000  
    ##  1st Qu.:0.000000  
    ##  Median :0.000000  
    ##  Mean   :0.001728  
    ##  3rd Qu.:0.000000  
    ##  Max.   :1.000000  
    ## 

``` r
# change class from numerical to categorical variable
cc$Class <- factor(cc$Class)

# remove missing value
cc <- na.omit(cc)
```

Anaysing data
-------------

``` r
library(ggplot2)

# time histogram
ggplot(data = cc, aes(x = Time)) + geom_histogram(color = "black", bindwidth = 5000) # bimodal histogram
```

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

![](github_md_files/figure-markdown_github/unnamed-chunk-2-1.png)

``` r
# look at class variable
table(cc$Class) # very unbalanced data
```

    ## 
    ##      0      1 
    ## 284314    492

``` r
prop.table(table(cc$Class))*100 # 99.83% negative class, 0.17% positive class
```

    ## 
    ##          0          1 
    ## 99.8272508  0.1727492

``` r
# class bar plot
ggplot(data = cc, aes(x = Class)) + geom_bar()
```

![](github_md_files/figure-markdown_github/unnamed-chunk-2-2.png)

### correlation matrix

``` r
library(corrplot)

numeric <- sapply(cc, is.numeric)
cordata <- cc[ ,numeric]
cordata <- na.omit(cordata)
cor_matrix <- cor(cordata) # to see correlation table
cor_matrix
```

    ##                Time            V1            V2            V3
    ## Time    1.000000000  1.173967e-01 -1.059297e-02 -4.196209e-01
    ## V1      0.117396691  1.000000e+00 -2.484442e-06  4.242962e-06
    ## V2     -0.010592968 -2.484442e-06  1.000000e+00  5.804742e-06
    ## V3     -0.419620905  4.242962e-06  5.804742e-06  1.000000e+00
    ## V4     -0.105260326  6.878257e-07  9.410056e-07 -1.607061e-06
    ## V5      0.173072028  9.198432e-07  1.258426e-06 -2.149156e-06
    ## V6     -0.063016634  9.613155e-07  1.315164e-06 -2.246053e-06
    ## V7      0.084715213 -3.560709e-06 -4.871361e-06  8.319373e-06
    ## V8     -0.036949706  1.608068e-06  2.199977e-06 -3.757149e-06
    ## V9     -0.008661595  6.899299e-06  9.438844e-06 -1.611978e-05
    ## V10     0.030617100 -2.697316e-06 -3.690163e-06  6.302108e-06
    ## V11    -0.247689655  9.735153e-07  1.331854e-06 -2.274557e-06
    ## V12     0.124349683 -4.993952e-06 -6.832162e-06  1.166805e-05
    ## V13    -0.065902120  5.867951e-07  8.027869e-07 -1.371010e-06
    ## V14    -0.098757084  1.387653e-06  1.898431e-06 -3.242164e-06
    ## V15    -0.183455940 -8.917771e-06 -1.220029e-05  2.083581e-05
    ## V16     0.011902858  6.291063e-08  8.606724e-08 -1.469867e-07
    ## V17    -0.073297498  1.545702e-06  2.114655e-06 -3.611435e-06
    ## V18     0.090437906  3.393537e-06  4.642654e-06 -7.928785e-06
    ## V19     0.028975266  2.481883e-07  3.395433e-07 -5.798763e-07
    ## V20    -0.050866130  6.846773e-07  9.366983e-07 -1.599705e-06
    ## V21     0.044735621  7.482008e-07  1.023604e-06 -1.748124e-06
    ## V22     0.144058933  2.638378e-06  3.609532e-06 -6.164405e-06
    ## V23     0.051142212  1.137308e-06  1.555936e-06 -2.657248e-06
    ## V24    -0.016182377  3.071154e-06  4.201607e-06 -7.175559e-06
    ## V25    -0.233083078 -3.591448e-06 -4.913415e-06  8.391192e-06
    ## V26    -0.041407325  1.337113e-06  1.829287e-06 -3.124080e-06
    ## V27    -0.005134727  8.880047e-07  1.214868e-06 -2.074767e-06
    ## V28    -0.009412961  1.745476e-06  2.387963e-06 -4.078194e-06
    ## Amount -0.010596315 -2.277093e-01 -5.314104e-01 -2.108806e-01
    ##                   V4            V5            V6            V7
    ## Time   -1.052603e-01  1.730720e-01 -6.301663e-02  8.471521e-02
    ## V1      6.878257e-07  9.198432e-07  9.613155e-07 -3.560709e-06
    ## V2      9.410056e-07  1.258426e-06  1.315164e-06 -4.871361e-06
    ## V3     -1.607061e-06 -2.149156e-06 -2.246053e-06  8.319373e-06
    ## V4      1.000000e+00 -3.483992e-07 -3.641072e-07  1.348652e-06
    ## V5     -3.483992e-07  1.000000e+00 -4.869279e-07  1.803579e-06
    ## V6     -3.641072e-07 -4.869279e-07  1.000000e+00  1.884896e-06
    ## V7      1.348652e-06  1.803579e-06  1.884896e-06  1.000000e+00
    ## V8     -6.090707e-07 -8.145226e-07 -8.512464e-07  3.153014e-06
    ## V9     -2.613174e-06 -3.494650e-06 -3.652211e-06  1.352778e-05
    ## V10     1.021634e-06  1.366251e-06  1.427850e-06 -5.288753e-06
    ## V11    -3.687280e-07 -4.931074e-07 -5.153398e-07  1.908817e-06
    ## V12     1.891506e-06  2.529549e-06  2.643597e-06 -9.791874e-06
    ## V13    -2.222541e-07 -2.972249e-07 -3.106257e-07  1.150556e-06
    ## V14    -5.255866e-07 -7.028776e-07 -7.345678e-07  2.720836e-06
    ## V15     3.377689e-06  4.517052e-06  4.720709e-06 -1.748549e-05
    ## V16    -2.382799e-08 -3.186565e-08 -3.330236e-08  1.233518e-07
    ## V17    -5.854491e-07 -7.829329e-07 -8.182324e-07  3.030730e-06
    ## V18    -1.285334e-06 -1.718903e-06 -1.796402e-06  6.653865e-06
    ## V19    -9.400364e-08 -1.257130e-07 -1.313809e-07  4.866344e-07
    ## V20    -2.593279e-07 -3.468045e-07 -3.624406e-07  1.342479e-06
    ## V21    -2.833880e-07 -3.789805e-07 -3.960674e-07  1.467032e-06
    ## V22    -9.993104e-07 -1.336398e-06 -1.396651e-06  5.173191e-06
    ## V23    -4.307659e-07 -5.760719e-07 -6.020449e-07  2.229972e-06
    ## V24    -1.163228e-06 -1.555609e-06 -1.625745e-06  6.021755e-06
    ## V25     1.360295e-06  1.819149e-06  1.901168e-06 -7.041920e-06
    ## V26    -5.064440e-07 -6.772778e-07 -7.078138e-07  2.621740e-06
    ## V27    -3.363401e-07 -4.497943e-07 -4.700739e-07  1.741152e-06
    ## V28    -6.611152e-07 -8.841227e-07 -9.239845e-07  3.422435e-06
    ## Amount  9.873183e-02 -3.863562e-01  2.159815e-01  3.973119e-01
    ##                   V8            V9           V10           V11
    ## Time   -3.694971e-02 -8.661595e-03  3.061710e-02 -2.476897e-01
    ## V1      1.608068e-06  6.899299e-06 -2.697316e-06  9.735153e-07
    ## V2      2.199977e-06  9.438844e-06 -3.690163e-06  1.331854e-06
    ## V3     -3.757149e-06 -1.611978e-05  6.302108e-06 -2.274557e-06
    ## V4     -6.090707e-07 -2.613174e-06  1.021634e-06 -3.687280e-07
    ## V5     -8.145226e-07 -3.494650e-06  1.366251e-06 -4.931074e-07
    ## V6     -8.512464e-07 -3.652211e-06  1.427850e-06 -5.153398e-07
    ## V7      3.153014e-06  1.352778e-05 -5.288753e-06  1.908817e-06
    ## V8      1.000000e+00 -6.109340e-06  2.388477e-06 -8.620494e-07
    ## V9     -6.109340e-06  1.000000e+00  1.024759e-05 -3.698561e-06
    ## V10     2.388477e-06  1.024759e-05  1.000000e+00  1.445971e-06
    ## V11    -8.620494e-07 -3.698561e-06  1.445971e-06  1.000000e+00
    ## V12     4.422152e-06  1.897293e-05 -7.417560e-06  2.677146e-06
    ## V13    -5.196080e-07 -2.229341e-06  8.715719e-07 -3.145678e-07
    ## V14    -1.228769e-06 -5.271945e-06  2.061094e-06 -7.438900e-07
    ## V15     7.896701e-06  3.388022e-05 -1.324564e-05  4.780618e-06
    ## V16    -5.570747e-08 -2.390089e-07  9.344172e-08 -3.372499e-08
    ## V17    -1.368722e-06 -5.872401e-06  2.295845e-06 -8.286164e-07
    ## V18    -3.004982e-06 -1.289266e-05  5.040450e-06 -1.819199e-06
    ## V19    -2.197711e-07 -9.429122e-07  3.686363e-07 -1.330482e-07
    ## V20    -6.062828e-07 -2.601213e-06  1.016957e-06 -3.670402e-07
    ## V21    -6.625330e-07 -2.842550e-06  1.111309e-06 -4.010938e-07
    ## V22    -2.336288e-06 -1.002368e-05  3.918806e-06 -1.414376e-06
    ## V23    -1.007088e-06 -4.320837e-06  1.689253e-06 -6.096853e-07
    ## V24    -2.719512e-06 -1.166787e-05  4.561612e-06 -1.646377e-06
    ## V25     3.180233e-06  1.364456e-05 -5.334409e-06  1.925295e-06
    ## V26    -1.184016e-06 -5.079934e-06  1.986026e-06 -7.167965e-07
    ## V27    -7.863295e-07 -3.373690e-06  1.318961e-06 -4.760395e-07
    ## V28    -1.545621e-06 -6.631377e-06  2.592570e-06 -9.357106e-07
    ## Amount -1.030788e-01 -4.424469e-02 -1.015029e-01  1.041873e-04
    ##                  V12           V13           V14           V15
    ## Time    1.243497e-01 -6.590212e-02 -9.875708e-02 -1.834559e-01
    ## V1     -4.993952e-06  5.867951e-07  1.387653e-06 -8.917771e-06
    ## V2     -6.832162e-06  8.027869e-07  1.898431e-06 -1.220029e-05
    ## V3      1.166805e-05 -1.371010e-06 -3.242164e-06  2.083581e-05
    ## V4      1.891506e-06 -2.222541e-07 -5.255866e-07  3.377689e-06
    ## V5      2.529549e-06 -2.972249e-07 -7.028776e-07  4.517052e-06
    ## V6      2.643597e-06 -3.106257e-07 -7.345678e-07  4.720709e-06
    ## V7     -9.791874e-06  1.150556e-06  2.720836e-06 -1.748549e-05
    ## V8      4.422152e-06 -5.196080e-07 -1.228769e-06  7.896701e-06
    ## V9      1.897293e-05 -2.229341e-06 -5.271945e-06  3.388022e-05
    ## V10    -7.417560e-06  8.715719e-07  2.061094e-06 -1.324564e-05
    ## V11     2.677146e-06 -3.145678e-07 -7.438900e-07  4.780618e-06
    ## V12     1.000000e+00  1.613674e-06  3.816017e-06 -2.452368e-05
    ## V13     1.613674e-06  1.000000e+00 -4.483864e-07  2.881561e-06
    ## V14     3.816017e-06 -4.483864e-07  1.000000e+00  6.814316e-06
    ## V15    -2.452368e-05  2.881561e-06  6.814316e-06  1.000000e+00
    ## V16     1.730029e-07 -2.032804e-08 -4.807176e-08  3.089337e-07
    ## V17     4.250647e-06 -4.994560e-07 -1.181114e-06  7.590442e-06
    ## V18     9.332153e-06 -1.096539e-06 -2.593095e-06  1.666456e-05
    ## V19     6.825125e-07 -8.019601e-08 -1.896475e-07  1.218772e-06
    ## V20     1.882848e-06 -2.212368e-07 -5.231809e-07  3.362228e-06
    ## V21     2.057536e-06 -2.417629e-07 -5.717209e-07  3.674172e-06
    ## V22     7.255484e-06 -8.525277e-07 -2.016058e-06  1.295622e-05
    ## V23     3.127572e-06 -3.674933e-07 -8.690482e-07  5.584949e-06
    ## V24     8.445609e-06 -9.923687e-07 -2.346754e-06  1.508144e-05
    ## V25    -9.876405e-06  1.160489e-06  2.744325e-06 -1.763644e-05
    ## V26     3.677032e-06 -4.320555e-07 -1.021725e-06  6.566129e-06
    ## V27     2.441994e-06 -2.869371e-07 -6.785489e-07  4.360703e-06
    ## V28     4.800021e-06 -5.640079e-07 -1.333766e-06  8.571466e-06
    ## Amount -9.542947e-03  5.293536e-03  3.375149e-02 -2.987840e-03
    ##                  V16           V17           V18           V19
    ## Time    1.190286e-02 -7.329750e-02  9.043791e-02  2.897527e-02
    ## V1      6.291063e-08  1.545702e-06  3.393537e-06  2.481883e-07
    ## V2      8.606724e-08  2.114655e-06  4.642654e-06  3.395433e-07
    ## V3     -1.469867e-07 -3.611435e-06 -7.928785e-06 -5.798763e-07
    ## V4     -2.382799e-08 -5.854491e-07 -1.285334e-06 -9.400364e-08
    ## V5     -3.186565e-08 -7.829329e-07 -1.718903e-06 -1.257130e-07
    ## V6     -3.330236e-08 -8.182324e-07 -1.796402e-06 -1.313809e-07
    ## V7      1.233518e-07  3.030730e-06  6.653865e-06  4.866344e-07
    ## V8     -5.570747e-08 -1.368722e-06 -3.004982e-06 -2.197711e-07
    ## V9     -2.390089e-07 -5.872401e-06 -1.289266e-05 -9.429122e-07
    ## V10     9.344172e-08  2.295845e-06  5.040450e-06  3.686363e-07
    ## V11    -3.372499e-08 -8.286164e-07 -1.819199e-06 -1.330482e-07
    ## V12     1.730029e-07  4.250647e-06  9.332153e-06  6.825125e-07
    ## V13    -2.032804e-08 -4.994560e-07 -1.096539e-06 -8.019601e-08
    ## V14    -4.807176e-08 -1.181114e-06 -2.593095e-06 -1.896475e-07
    ## V15     3.089337e-07  7.590442e-06  1.666456e-05  1.218772e-06
    ## V16     1.000000e+00 -5.354696e-08 -1.175605e-07 -8.597858e-09
    ## V17    -5.354696e-08  1.000000e+00 -2.888439e-06 -2.112477e-07
    ## V18    -1.175605e-07 -2.888439e-06  1.000000e+00 -4.637873e-07
    ## V19    -8.597858e-09 -2.112477e-07 -4.637873e-07  1.000000e+00
    ## V20    -2.371892e-08 -5.827693e-07 -1.279450e-06 -9.357336e-08
    ## V21    -2.591953e-08 -6.368379e-07 -1.398156e-06 -1.022550e-07
    ## V22    -9.139998e-08 -2.245680e-06 -4.930314e-06 -3.605814e-07
    ## V23    -3.939916e-08 -9.680297e-07 -2.125277e-06 -1.554334e-07
    ## V24    -1.063924e-07 -2.614041e-06 -5.739038e-06 -4.197280e-07
    ## V25     1.244167e-07  3.056894e-06  6.711307e-06  4.908354e-07
    ## V26    -4.632091e-08 -1.138096e-06 -2.498651e-06 -1.827403e-07
    ## V27    -3.076269e-08 -7.558332e-07 -1.659406e-06 -1.213617e-07
    ## V28    -6.046761e-08 -1.485677e-06 -3.261755e-06 -2.385504e-07
    ## Amount -3.909513e-03  7.309381e-03  3.565119e-02 -5.615074e-02
    ##                  V20           V21           V22           V23
    ## Time   -5.086613e-02  4.473562e-02  1.440589e-01  5.114221e-02
    ## V1      6.846773e-07  7.482008e-07  2.638378e-06  1.137308e-06
    ## V2      9.366983e-07  1.023604e-06  3.609532e-06  1.555936e-06
    ## V3     -1.599705e-06 -1.748124e-06 -6.164405e-06 -2.657248e-06
    ## V4     -2.593279e-07 -2.833880e-07 -9.993104e-07 -4.307659e-07
    ## V5     -3.468045e-07 -3.789805e-07 -1.336398e-06 -5.760719e-07
    ## V6     -3.624406e-07 -3.960674e-07 -1.396651e-06 -6.020449e-07
    ## V7      1.342479e-06  1.467032e-06  5.173191e-06  2.229972e-06
    ## V8     -6.062828e-07 -6.625330e-07 -2.336288e-06 -1.007088e-06
    ## V9     -2.601213e-06 -2.842550e-06 -1.002368e-05 -4.320837e-06
    ## V10     1.016957e-06  1.111309e-06  3.918806e-06  1.689253e-06
    ## V11    -3.670402e-07 -4.010938e-07 -1.414376e-06 -6.096853e-07
    ## V12     1.882848e-06  2.057536e-06  7.255484e-06  3.127572e-06
    ## V13    -2.212368e-07 -2.417629e-07 -8.525277e-07 -3.674933e-07
    ## V14    -5.231809e-07 -5.717209e-07 -2.016058e-06 -8.690482e-07
    ## V15     3.362228e-06  3.674172e-06  1.295622e-05  5.584949e-06
    ## V16    -2.371892e-08 -2.591953e-08 -9.139998e-08 -3.939916e-08
    ## V17    -5.827693e-07 -6.368379e-07 -2.245680e-06 -9.680297e-07
    ## V18    -1.279450e-06 -1.398156e-06 -4.930314e-06 -2.125277e-06
    ## V19    -9.357336e-08 -1.022550e-07 -3.605814e-07 -1.554334e-07
    ## V20     1.000000e+00 -2.820909e-07 -9.947363e-07 -4.287941e-07
    ## V21    -2.820909e-07  1.000000e+00 -1.087027e-06 -4.685771e-07
    ## V22    -9.947363e-07 -1.087027e-06  1.000000e+00 -1.652342e-06
    ## V23    -4.287941e-07 -4.685771e-07 -1.652342e-06  1.000000e+00
    ## V24    -1.157904e-06 -1.265333e-06 -4.461939e-06 -1.923378e-06
    ## V25     1.354068e-06  1.479697e-06  5.217850e-06  2.249223e-06
    ## V26    -5.041259e-07 -5.508980e-07 -1.942630e-06 -8.373962e-07
    ## V27    -3.348005e-07 -3.658629e-07 -1.290141e-06 -5.561323e-07
    ## V28    -6.580891e-07 -7.191457e-07 -2.535922e-06 -1.093142e-06
    ## Amount  3.394036e-01  1.059991e-01 -6.480020e-02 -1.126324e-01
    ##                  V24           V25           V26           V27
    ## Time   -1.618238e-02 -2.330831e-01 -4.140732e-02 -5.134727e-03
    ## V1      3.071154e-06 -3.591448e-06  1.337113e-06  8.880047e-07
    ## V2      4.201607e-06 -4.913415e-06  1.829287e-06  1.214868e-06
    ## V3     -7.175559e-06  8.391192e-06 -3.124080e-06 -2.074767e-06
    ## V4     -1.163228e-06  1.360295e-06 -5.064440e-07 -3.363401e-07
    ## V5     -1.555609e-06  1.819149e-06 -6.772778e-07 -4.497943e-07
    ## V6     -1.625745e-06  1.901168e-06 -7.078138e-07 -4.700739e-07
    ## V7      6.021755e-06 -7.041920e-06  2.621740e-06  1.741152e-06
    ## V8     -2.719512e-06  3.180233e-06 -1.184016e-06 -7.863295e-07
    ## V9     -1.166787e-05  1.364456e-05 -5.079934e-06 -3.373690e-06
    ## V10     4.561612e-06 -5.334409e-06  1.986026e-06  1.318961e-06
    ## V11    -1.646377e-06  1.925295e-06 -7.167965e-07 -4.760395e-07
    ## V12     8.445609e-06 -9.876405e-06  3.677032e-06  2.441994e-06
    ## V13    -9.923687e-07  1.160489e-06 -4.320555e-07 -2.869371e-07
    ## V14    -2.346754e-06  2.744325e-06 -1.021725e-06 -6.785489e-07
    ## V15     1.508144e-05 -1.763644e-05  6.566129e-06  4.360703e-06
    ## V16    -1.063924e-07  1.244167e-07 -4.632091e-08 -3.076269e-08
    ## V17    -2.614041e-06  3.056894e-06 -1.138096e-06 -7.558332e-07
    ## V18    -5.739038e-06  6.711307e-06 -2.498651e-06 -1.659406e-06
    ## V19    -4.197280e-07  4.908354e-07 -1.827403e-07 -1.213617e-07
    ## V20    -1.157904e-06  1.354068e-06 -5.041259e-07 -3.348005e-07
    ## V21    -1.265333e-06  1.479697e-06 -5.508980e-07 -3.658629e-07
    ## V22    -4.461939e-06  5.217850e-06 -1.942630e-06 -1.290141e-06
    ## V23    -1.923378e-06  2.249223e-06 -8.373962e-07 -5.561323e-07
    ## V24     1.000000e+00  6.073740e-06 -2.261282e-06 -1.501765e-06
    ## V25     6.073740e-06  1.000000e+00  2.644373e-06  1.756183e-06
    ## V26    -2.261282e-06  2.644373e-06  1.000000e+00 -6.538352e-07
    ## V27    -1.501765e-06  1.756183e-06 -6.538352e-07  1.000000e+00
    ## V28    -2.951892e-06  3.451981e-06 -1.285189e-06 -8.535206e-07
    ## Amount  5.146894e-03 -4.783781e-02 -3.207750e-03  2.882566e-02
    ##                  V28        Amount
    ## Time   -9.412961e-03 -0.0105963145
    ## V1      1.745476e-06 -0.2277092615
    ## V2      2.387963e-06 -0.5314104015
    ## V3     -4.078194e-06 -0.2108806131
    ## V4     -6.611152e-07  0.0987318319
    ## V5     -8.841227e-07 -0.3863561639
    ## V6     -9.239845e-07  0.2159814519
    ## V7      3.422435e-06  0.3973119129
    ## V8     -1.545621e-06 -0.1030788261
    ## V9     -6.631377e-06 -0.0442446936
    ## V10     2.592570e-06 -0.1015029310
    ## V11    -9.357106e-07  0.0001041873
    ## V12     4.800021e-06 -0.0095429467
    ## V13    -5.640079e-07  0.0052935365
    ## V14    -1.333766e-06  0.0337514908
    ## V15     8.571466e-06 -0.0029878397
    ## V16    -6.046761e-08 -0.0039095133
    ## V17    -1.485677e-06  0.0073093810
    ## V18    -3.261755e-06  0.0356511882
    ## V19    -2.385504e-07 -0.0561507370
    ## V20    -6.580891e-07  0.3394036106
    ## V21    -7.191457e-07  0.1059991101
    ## V22    -2.535922e-06 -0.0648002035
    ## V23    -1.093142e-06 -0.1126323529
    ## V24    -2.951892e-06  0.0051468943
    ## V25     3.451981e-06 -0.0478378108
    ## V26    -1.285189e-06 -0.0032077501
    ## V27    -8.535206e-07  0.0288256627
    ## V28     1.000000e+00  0.0102586019
    ## Amount  1.025860e-02  1.0000000000

``` r
corrplot(cor_matrix, method = "square", type = "lower") # to visualize correlation matrix
```

![](github_md_files/figure-markdown_github/unnamed-chunk-3-1.png)

Predictive Modelling
--------------------

### Split data 80:20

``` r
library("caTools")
set.seed(10)
split <- sample.split(cc$Class, SplitRatio = 0.8)
train <- subset(cc,split == TRUE)
cv <- subset(cc, split == FALSE)

table(train$Class)
```

    ## 
    ##      0      1 
    ## 227451    394

``` r
table(cv$Class)
```

    ## 
    ##     0     1 
    ## 56863    98

### Baseline Model

``` r
table(cv$Class)
```

    ## 
    ##     0     1 
    ## 56863    98

``` r
56863/nrow(cv)
```

    ## [1] 0.9982795

### Decision Tree Model

``` r
library("rpart")
library("rpart.plot")
library(caret)
```

    ## Loading required package: lattice

``` r
library(ROCR)
```

    ## Loading required package: gplots

    ## 
    ## Attaching package: 'gplots'

    ## The following object is masked from 'package:stats':
    ## 
    ##     lowess

``` r
library(pROC)
```

    ## Type 'citation("pROC")' for a citation.

    ## 
    ## Attaching package: 'pROC'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     cov, smooth, var

``` r
tree.model <- rpart(Class ~ ., data = train, method = "class", minbucket = 20)
prp(tree.model) 
```

![](github_md_files/figure-markdown_github/unnamed-chunk-6-1.png)

``` r
tree.predict <- predict(tree.model, cv, type = "class")
confusionMatrix(cv$Class, tree.predict)
```

    ## Confusion Matrix and Statistics
    ## 
    ##           Reference
    ## Prediction     0     1
    ##          0 56854     9
    ##          1    25    73
    ##                                           
    ##                Accuracy : 0.9994          
    ##                  95% CI : (0.9992, 0.9996)
    ##     No Information Rate : 0.9986          
    ##     P-Value [Acc > NIR] : 1.603e-09       
    ##                                           
    ##                   Kappa : 0.8108          
    ##  Mcnemar's Test P-Value : 0.0101          
    ##                                           
    ##             Sensitivity : 0.9996          
    ##             Specificity : 0.8902          
    ##          Pos Pred Value : 0.9998          
    ##          Neg Pred Value : 0.7449          
    ##              Prevalence : 0.9986          
    ##          Detection Rate : 0.9981          
    ##    Detection Prevalence : 0.9983          
    ##       Balanced Accuracy : 0.9449          
    ##                                           
    ##        'Positive' Class : 0               
    ## 

``` r
tree.predict <- predict(tree.model, cv)
tree.ROCR <- prediction(tree.predict[,2], cv$Class)
tree.auc <- as.numeric(performance(tree.ROCR,"auc")@y.values)
tree.auc
```

    ## [1] 0.9028721

##### To handle the imbalanced class over sampling and under sampling is used further.

###### Oversampling and undersampling are opposite and roughly equivalent techniques. They both involve using a bias to select more samples from one class than from another.

###### The usual reason for oversampling is to correct for a bias in the original dataset. One scenario where it is useful is when training a classifier using labelled training data from a biased source, since labelled training data is valuable but often comes from un-representative sources.

###### For example, suppose we have a sample of 1000 people of which 66.7% are male (perhaps the sample was collected at a football match). We know the general population is 50% female, and we may wish to adjust our dataset to represent this. Simple oversampling will select each female example twice, and this copying will produce a balanced dataset of 1333 samples with 50% female. Simple undersampling will drop some of the male samples at random to give a balanced dataset of 667 samples, again with 50% female

### Over Sampling

``` r
library(ROSE)
```

    ## Loaded ROSE 0.0-3

``` r
oversampled_train_data <- ovun.sample(Class ~ ., data = train, method = "over",
                                      N = 2*nrow(subset(train, train$Class == 0)))$data
table(oversampled_train_data$Class)
```

    ## 
    ##      0      1 
    ## 227451 227451

``` r
tree.model.over <- rpart(Class ~ ., data = oversampled_train_data, method = "class", minbucket = 20)
prp(tree.model.over) 
```

![](github_md_files/figure-markdown_github/unnamed-chunk-7-1.png)

``` r
tree.predict.over <- predict(tree.model.over, cv, type = "class")
confusionMatrix(cv$Class, tree.predict.over)
```

    ## Confusion Matrix and Statistics
    ## 
    ##           Reference
    ## Prediction     0     1
    ##          0 53915  2948
    ##          1    11    87
    ##                                           
    ##                Accuracy : 0.9481          
    ##                  95% CI : (0.9462, 0.9499)
    ##     No Information Rate : 0.9467          
    ##     P-Value [Acc > NIR] : 0.07908         
    ##                                           
    ##                   Kappa : 0.0524          
    ##  Mcnemar's Test P-Value : < 2e-16         
    ##                                           
    ##             Sensitivity : 0.99980         
    ##             Specificity : 0.02867         
    ##          Pos Pred Value : 0.94816         
    ##          Neg Pred Value : 0.88776         
    ##              Prevalence : 0.94672         
    ##          Detection Rate : 0.94652         
    ##    Detection Prevalence : 0.99828         
    ##       Balanced Accuracy : 0.51423         
    ##                                           
    ##        'Positive' Class : 0               
    ## 

``` r
tree.predict.over <- predict(tree.model.over, cv)
tree.ROCR.over <- prediction(tree.predict.over[,2], cv$Class)
tree.auc.over <- as.numeric(performance(tree.ROCR.over,"auc")@y.values)
tree.auc.over
```

    ## [1] 0.9380361

### Under sampling

``` r
undersampled_train_data <- ovun.sample(Class ~ ., data = train, method = "under",
                                      N = 2*nrow(subset(train, train$Class == 1)), seed = 1)$data
table(undersampled_train_data$Class)
```

    ## 
    ##   0   1 
    ## 394 394

``` r
tree.model.under <- rpart(Class ~ ., data = undersampled_train_data, method = "class", minbucket = 20)
prp(tree.model.under) 
```

![](github_md_files/figure-markdown_github/unnamed-chunk-8-1.png)

``` r
tree.predict.under <- predict(tree.model.under, cv, type = "class")
confusionMatrix(cv$Class, tree.predict.under)
```

    ## Confusion Matrix and Statistics
    ## 
    ##           Reference
    ## Prediction     0     1
    ##          0 51761  5102
    ##          1     8    90
    ##                                           
    ##                Accuracy : 0.9103          
    ##                  95% CI : (0.9079, 0.9126)
    ##     No Information Rate : 0.9088          
    ##     P-Value [Acc > NIR] : 0.1176          
    ##                                           
    ##                   Kappa : 0.0308          
    ##  Mcnemar's Test P-Value : <2e-16          
    ##                                           
    ##             Sensitivity : 0.99985         
    ##             Specificity : 0.01733         
    ##          Pos Pred Value : 0.91028         
    ##          Neg Pred Value : 0.91837         
    ##              Prevalence : 0.90885         
    ##          Detection Rate : 0.90871         
    ##    Detection Prevalence : 0.99828         
    ##       Balanced Accuracy : 0.50859         
    ##                                           
    ##        'Positive' Class : 0               
    ## 

``` r
tree.predict.under <- predict(tree.model.under, cv)
tree.ROCR.under <- prediction(tree.predict.under[,2], cv$Class)
tree.auc.under <- as.numeric(performance(tree.ROCR.under,"auc")@y.values)
tree.auc.under
```

    ## [1] 0.9381045

### Mix of both, over and under sampling

``` r
mix_sampled_train_data <- ovun.sample(Class ~ ., data = train, method = "both", p=0.5, 
                                  N=nrow(train), seed = 1)$data
table(mix_sampled_train_data$Class)
```

    ## 
    ##      0      1 
    ## 113715 114130

``` r
tree.model.both <- rpart(Class ~ ., data = mix_sampled_train_data, method = "class", minbucket = 20)
prp(tree.model.both) 
```

![](github_md_files/figure-markdown_github/unnamed-chunk-9-1.png)

``` r
tree.predict.both <- predict(tree.model.both, cv, type = "class")
confusionMatrix(cv$Class, tree.predict.both)
```

    ## Confusion Matrix and Statistics
    ## 
    ##           Reference
    ## Prediction     0     1
    ##          0 53915  2948
    ##          1    11    87
    ##                                           
    ##                Accuracy : 0.9481          
    ##                  95% CI : (0.9462, 0.9499)
    ##     No Information Rate : 0.9467          
    ##     P-Value [Acc > NIR] : 0.07908         
    ##                                           
    ##                   Kappa : 0.0524          
    ##  Mcnemar's Test P-Value : < 2e-16         
    ##                                           
    ##             Sensitivity : 0.99980         
    ##             Specificity : 0.02867         
    ##          Pos Pred Value : 0.94816         
    ##          Neg Pred Value : 0.88776         
    ##              Prevalence : 0.94672         
    ##          Detection Rate : 0.94652         
    ##    Detection Prevalence : 0.99828         
    ##       Balanced Accuracy : 0.51423         
    ##                                           
    ##        'Positive' Class : 0               
    ## 

``` r
tree.predict.both <- predict(tree.model.both, cv)
tree.ROCR.both <- prediction(tree.predict.both[,2], cv$Class)
tree.auc.both <- as.numeric(performance(tree.ROCR.both,"auc")@y.values)
tree.auc.both
```

    ## [1] 0.9380361

### ROSE: Generation of synthetic data by --&gt; Randomly Over Sampling Examples

``` r
rose_train_data <- ROSE(Class ~ ., data = train, seed = 1)$data
table(rose_train_data$Class)
```

    ## 
    ##      0      1 
    ## 113715 114130

``` r
tree.model.rose <- rpart(Class ~ ., data = rose_train_data, method = "class", minbucket = 20)
prp(tree.model) 
```

![](github_md_files/figure-markdown_github/unnamed-chunk-10-1.png)

``` r
tree.predict.rose <- predict(tree.model.rose, cv, type = "class")
confusionMatrix(cv$Class, tree.predict.rose)
```

    ## Confusion Matrix and Statistics
    ## 
    ##           Reference
    ## Prediction     0     1
    ##          0 55566  1297
    ##          1    14    84
    ##                                           
    ##                Accuracy : 0.977           
    ##                  95% CI : (0.9757, 0.9782)
    ##     No Information Rate : 0.9758          
    ##     P-Value [Acc > NIR] : 0.02841         
    ##                                           
    ##                   Kappa : 0.1107          
    ##  Mcnemar's Test P-Value : < 2e-16         
    ##                                           
    ##             Sensitivity : 0.99975         
    ##             Specificity : 0.06083         
    ##          Pos Pred Value : 0.97719         
    ##          Neg Pred Value : 0.85714         
    ##              Prevalence : 0.97576         
    ##          Detection Rate : 0.97551         
    ##    Detection Prevalence : 0.99828         
    ##       Balanced Accuracy : 0.53029         
    ##                                           
    ##        'Positive' Class : 0               
    ## 

``` r
tree.predict.rose <- predict(tree.model.rose, cv)
tree.ROCR.rose <- prediction(tree.predict.rose[,2], cv$Class)
tree.auc.rose <- as.numeric(performance(tree.ROCR.rose,"auc")@y.values)
tree.auc.rose
```

    ## [1] 0.9190395

Results
-------

``` r
result.auc <- data.frame(normal.auc = tree.auc, over.auc = tree.auc.over, under.auc = tree.auc.under, mix.auc = tree.auc.both, rose.auc = tree.auc.rose)
result.auc  
```

    ##   normal.auc  over.auc under.auc   mix.auc  rose.auc
    ## 1  0.9028721 0.9380361 0.9381045 0.9380361 0.9190395

###### In this case over, under and mix sampling gives the best AUC values.

Conclusion
----------

###### The data had a highly imbalanced class in which normal machine learning algorithms fail to be as efficient as in case of a balanced class.

###### Different sampling techniques can be used to generate/ remove data to balance the class/outcome variabele.
