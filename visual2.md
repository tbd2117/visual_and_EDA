Visualization\_ partII
================
10/02/2020

``` r
library(tidyverse)
```

    ## ── Attaching packages ─────────────────────────────────────────────────────────────────────── tidyverse 1.3.0 ──

    ## ✓ ggplot2 3.3.2     ✓ purrr   0.3.4
    ## ✓ tibble  3.0.3     ✓ dplyr   1.0.2
    ## ✓ tidyr   1.1.2     ✓ stringr 1.4.0
    ## ✓ readr   1.3.1     ✓ forcats 0.5.0

    ## ── Conflicts ────────────────────────────────────────────────────────────────────────── tidyverse_conflicts() ──
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

``` r
library(patchwork)
```

``` r
weather_df = 
  rnoaa::meteo_pull_monitors(
    c("USW00094728", "USC00519397", "USS0023B17S"),
    var = c("PRCP", "TMIN", "TMAX"), 
    date_min = "2017-01-01",
    date_max = "2017-12-31") %>%
  mutate(
    name = recode(
      id, 
      USW00094728 = "CentralPark_NY", 
      USC00519397 = "Waikiki_HA",
      USS0023B17S = "Waterhole_WA"),
    tmin = tmin / 10,
    tmax = tmax / 10) %>%
  select(name, id, everything())
```

    ## Registered S3 method overwritten by 'hoardr':
    ##   method           from
    ##   print.cache_info httr

    ## using cached file: /Users/thiagoaraujo/Library/Caches/R/noaa_ghcnd/USW00094728.dly

    ## date created (size, mb): 2020-10-02 07:31:47 (7.52)

    ## file min/max dates: 1869-01-01 / 2020-09-30

    ## using cached file: /Users/thiagoaraujo/Library/Caches/R/noaa_ghcnd/USC00519397.dly

    ## date created (size, mb): 2020-10-02 07:31:54 (1.699)

    ## file min/max dates: 1965-01-01 / 2020-03-31

    ## using cached file: /Users/thiagoaraujo/Library/Caches/R/noaa_ghcnd/USS0023B17S.dly

    ## date created (size, mb): 2020-10-02 07:31:58 (0.877)

    ## file min/max dates: 1999-09-01 / 2020-09-30

``` r
weather_df
```

    ## # A tibble: 1,095 x 6
    ##    name           id          date        prcp  tmax  tmin
    ##    <chr>          <chr>       <date>     <dbl> <dbl> <dbl>
    ##  1 CentralPark_NY USW00094728 2017-01-01     0   8.9   4.4
    ##  2 CentralPark_NY USW00094728 2017-01-02    53   5     2.8
    ##  3 CentralPark_NY USW00094728 2017-01-03   147   6.1   3.9
    ##  4 CentralPark_NY USW00094728 2017-01-04     0  11.1   1.1
    ##  5 CentralPark_NY USW00094728 2017-01-05     0   1.1  -2.7
    ##  6 CentralPark_NY USW00094728 2017-01-06    13   0.6  -3.8
    ##  7 CentralPark_NY USW00094728 2017-01-07    81  -3.2  -6.6
    ##  8 CentralPark_NY USW00094728 2017-01-08     0  -3.8  -8.8
    ##  9 CentralPark_NY USW00094728 2017-01-09     0  -4.9  -9.9
    ## 10 CentralPark_NY USW00094728 2017-01-10     0   7.8  -6  
    ## # … with 1,085 more rows

## remember this plot

``` r
weather_df %>% 
  ggplot(aes(x = tmin, y = tmax, color = name)) + 
  geom_point(alpha = .5)
```

    ## Warning: Removed 15 rows containing missing values (geom_point).

![](visual2_files/figure-gfm/unnamed-chunk-2-1.png)<!-- -->

## labels

``` r
weather_df %>% 
  ggplot(aes(x = tmin, y = tmax, color = name)) + 
  geom_point(alpha = .5) +
  labs(
    title = "Tempaerature Plot",
    x = "Minimum daily Temperature (ºC)",
    y = "Maximum daily Temperature (ºC)",
      caption = "Data from rnoaa package; temperatures in 2017"
  )
```

    ## Warning: Removed 15 rows containing missing values (geom_point).

![](visual2_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

## scales

Start with the same plot

``` r
weather_df %>% 
  ggplot(aes(x = tmin, y = tmax, color = name)) + 
  geom_point(alpha = .5) +
  labs(
    title = "Tempaerature Plot",
    x = "Minimum daily Temperature (ºC)",
    y = "Maximum daily Temperature (ºC)",
      caption = "Data from rnoaa package; temperatures in 2017"
  ) + 
  scale_x_continuous(
    breaks = c(-15, 0, 15),
    labels = c("-15ºC", "0", "15")
  ) +
  scale_y_continuous(
    trans = "log",  #sqrt could be an option
    position = "right"
  )
```

    ## Warning in self$trans$transform(x): NaNs produced

    ## Warning: Transformation introduced infinite values in continuous y-axis

    ## Warning: Removed 90 rows containing missing values (geom_point).

![](visual2_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

Lets look at color scales

``` r
weather_df %>% 
  ggplot(aes(x = tmin, y = tmax, color = name)) + 
  geom_point(alpha = .5) +
  labs(
    title = "Tempaerature Plot",
    x = "Minimum daily Temperature (ºC)",
    y = "Maximum daily Temperature (ºC)",
      caption = "Data from rnoaa package; temperatures in 2017"
  ) +
  scale_color_hue(
    name = "Location",
    h = c(100, 300)
  )
```

    ## Warning: Removed 15 rows containing missing values (geom_point).

![](visual2_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

``` r
weather_df %>% 
  ggplot(aes(x = tmin, y = tmax, color = name)) + 
  geom_point(alpha = .5) +
  labs(
    title = "Tempaerature Plot",
    x = "Minimum daily Temperature (ºC)",
    y = "Maximum daily Temperature (ºC)",
      caption = "Data from rnoaa package; temperatures in 2017"
  ) +
  viridis::scale_color_viridis( #good for colorblind people AND is better gray scale print than default
    name = "Location",
    discrete = TRUE
  )
```

    ## Warning: Removed 15 rows containing missing values (geom_point).

![](visual2_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->

## themes

shift the legend

``` r
weather_df %>% 
  ggplot(aes(x = tmin, y = tmax, color = name)) + 
  geom_point(alpha = .5) +
  labs(
    title = "Tempaerature Plot",
    x = "Minimum daily Temperature (ºC)",
    y = "Maximum daily Temperature (ºC)",
      caption = "Data from rnoaa package; temperatures in 2017"
  ) +
  viridis::scale_color_viridis( #good for colorblind people AND is better gray scale print than default
    name = "Location",
    discrete = TRUE
  ) +
  theme(legend.position = "bottom")
```

    ## Warning: Removed 15 rows containing missing values (geom_point).

![](visual2_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

change the overall theme

``` r
weather_df %>% 
  ggplot(aes(x = tmin, y = tmax, color = name)) + 
  geom_point(alpha = .5) +
  labs(
    title = "Tempaerature Plot",
    x = "Minimum daily Temperature (ºC)",
    y = "Maximum daily Temperature (ºC)",
      caption = "Data from rnoaa package; temperatures in 2017"
  ) +
  viridis::scale_color_viridis(  #good for colorblind people AND is better gray scale print than default
    name = "Location",
    discrete = TRUE
  ) + 
  theme_minimal()
```

    ## Warning: Removed 15 rows containing missing values (geom_point).

![](visual2_files/figure-gfm/unnamed-chunk-8-1.png)<!-- -->

``` r
weather_df %>% 
  ggplot(aes(x = tmin, y = tmax, color = name)) + 
  geom_point(alpha = .5) +
  labs(
    title = "Tempaerature Plot",
    x = "Minimum daily Temperature (ºC)",
    y = "Maximum daily Temperature (ºC)",
      caption = "Data from rnoaa package; temperatures in 2017"
  ) +
  viridis::scale_color_viridis(  #good for colorblind people AND is better gray scale print than default
    name = "Location",
    discrete = TRUE
  ) + 
  ggthemes::theme_economist() +
  theme(legend.position = "bottom") ## be careful because once you use a theme is overrides all theme elements before
```

    ## Warning: Removed 15 rows containing missing values (geom_point).

![](visual2_files/figure-gfm/unnamed-chunk-9-1.png)<!-- -->

## setting options

Put this in the begginign to set as default option througout the rmkd
file

``` r
library(tidyverse)

knitr::opts_chunk$set(
  fig.width = 6,
  fig.asp = .6,
  out.width = "90%"
)

theme_set(theme_minimal() + theme(legend.position = "bottom"))

options(
  ggplot2.continuous.color = "viridis",
  ggplot2.continuous.fill = "viridis"
)

scale_colour_discrete = scale_color_viridis_d
scale_fill_discrete = scale_fill_viridis_d
```

## Data args in `geom`

``` r
central_park = 
  weather_df %>% 
  filter(name == "CentralPark_NY")

waikiki = 
  weather_df %>% 
  filter(name == "Waikiki_HA")

ggplot(data = waikiki, aes(x = date, y = tmax, color = name))+
  geom_point() + 
  geom_line(data = central_park)
```

    ## Warning: Removed 3 rows containing missing values (geom_point).

![](visual2_files/figure-gfm/unnamed-chunk-11-1.png)<!-- -->
