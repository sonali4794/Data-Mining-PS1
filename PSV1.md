##Q1\] Looking at top 10 traffic Lanes flying out of Austin

    ## `summarise()` has grouped output by 'lane', 'Origin'. You can override using the `.groups` argument.

    ## Reading layer `cb_2018_us_state_20m' from data source 
    ##   `C:\Users\hp\Documents\cb_2018_us_state_20m\cb_2018_us_state_20m.shp' 
    ##   using driver `ESRI Shapefile'
    ## Simple feature collection with 52 features and 9 fields
    ## Geometry type: MULTIPOLYGON
    ## Dimension:     XY
    ## Bounding box:  xmin: -179.1743 ymin: 17.91377 xmax: 179.7739 ymax: 71.35256
    ## Geodetic CRS:  NAD83

##Q2\] Billboard Top 100

    ## `summarise()` has grouped output by 'song'. You can override using the `.groups` argument.

    ## # A tibble: 10 x 3
    ## # Groups:   song [10]
    ##    song                              performer                             count
    ##    <chr>                             <chr>                                 <int>
    ##  1 Radioactive                       Imagine Dragons                          87
    ##  2 Sail                              AWOLNATION                               79
    ##  3 Blinding Lights                   The Weeknd                               76
    ##  4 I'm Yours                         Jason Mraz                               76
    ##  5 How Do I Live                     LeAnn Rimes                              69
    ##  6 Counting Stars                    OneRepublic                              68
    ##  7 Party Rock Anthem                 LMFAO Featuring Lauren Bennett & Goo~    68
    ##  8 Foolish Games/You Were Meant For~ Jewel                                    65
    ##  9 Rolling In The Deep               Adele                                    65
    ## 10 Before He Cheats                  Carrie Underwood                         64

    ## `summarise()` has grouped output by 'year'. You can override using the `.groups` argument.

![](PSV1_files/figure-markdown_strict/unnamed-chunk-2-1.png)

    ## `summarise()` has grouped output by 'performer'. You can override using the `.groups` argument.

![](PSV1_files/figure-markdown_strict/unnamed-chunk-2-2.png)

##Q3\] Olympics Wrangling

    ## 95% 
    ## 183

    ## [1] "The 95th percentile height of female athletes is 183"

    ## [1] 10.86549

    ## [1] "Highest variation (measured in terms of std dev) is in Rowing Women's Coxed Fours event followed by Women's basketball"

![](PSV1_files/figure-markdown_strict/unnamed-chunk-3-1.png)

##Q4\]K-nearest Neighhbours

    ## Rows: 29466 Columns: 17

    ## -- Column specification --------------------------------------------------------
    ## Delimiter: ","
    ## chr (11): trim, subTrim, condition, color, displacement, fuel, state, region...
    ## dbl  (5): id, mileage, year, featureCount, price
    ## lgl  (1): isOneOwner

    ## 
    ## i Use `spec()` to retrieve the full column specification for this data.
    ## i Specify the column types or set `show_col_types = FALSE` to quiet this message.

![](PSV1_files/figure-markdown_strict/unnamed-chunk-4-1.png)

    ##            k        e
    ## result.26 27 8085.804

![](PSV1_files/figure-markdown_strict/unnamed-chunk-4-2.png)![](PSV1_files/figure-markdown_strict/unnamed-chunk-4-3.png)

    ##              k        e
    ## result.101 102 14020.15

![](PSV1_files/figure-markdown_strict/unnamed-chunk-4-4.png)
