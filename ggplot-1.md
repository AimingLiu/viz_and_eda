ggplot1
================
AimingLiu
9/26/2019

``` r
  weather_df = 
  rnoaa::meteo_pull_monitors(c("USW00094728", "USC00519397", "USS0023B17S"),#download weather from these three sets#
                      var = c("PRCP", "TMIN", "TMAX"), 
                      date_min = "2017-01-01",
                      date_max = "2017-12-31") %>%
  mutate(
    name = recode(id, USW00094728 = "CentralPark_NY", 
                      USC00519397 = "Waikiki_HA",
                      USS0023B17S = "Waterhole_WA"),
    tmin = tmin / 10,
    tmax = tmax / 10) %>%
  select(name, id, everything())
```

    ## Registered S3 method overwritten by 'crul':
    ##   method                 from
    ##   as.character.form_file httr

    ## Registered S3 method overwritten by 'hoardr':
    ##   method           from
    ##   print.cache_info httr

    ## file path:          /Users/aimingliu/Library/Caches/rnoaa/ghcnd/USW00094728.dly

    ## file last updated:  2019-09-26 10:27:03

    ## file min/max dates: 1869-01-01 / 2019-09-30

    ## file path:          /Users/aimingliu/Library/Caches/rnoaa/ghcnd/USC00519397.dly

    ## file last updated:  2019-09-26 10:27:12

    ## file min/max dates: 1965-01-01 / 2019-09-30

    ## file path:          /Users/aimingliu/Library/Caches/rnoaa/ghcnd/USS0023B17S.dly

    ## file last updated:  2019-09-26 10:27:15

    ## file min/max dates: 1999-09-01 / 2019-09-30

\#\#create a ggplot\#

``` r
ggplot(weather_df,aes(x = tmin,y = tmax))+geom_point()#add point#
```

    ## Warning: Removed 15 rows containing missing values (geom_point).

![](ggplot-1_files/figure-gfm/unnamed-chunk-1-1.png)<!-- -->

``` r
weather_df%>%
   ggplot(aes(x = tmin,y = tmax))+geom_point()
```

    ## Warning: Removed 15 rows containing missing values (geom_point).

![](ggplot-1_files/figure-gfm/unnamed-chunk-2-1.png)<!-- -->

saving initial plots\*(mostly don’t use this)

``` r
scatterplot = weather_df%>%
             ggplot(aes(x = tmin,y = tmax))+geom_point()

scatterplot
```

    ## Warning: Removed 15 rows containing missing values (geom_point).

![](ggplot-1_files/figure-gfm/unnamed-chunk-3-1.png)<!-- --> adding
color

``` r
weather_df%>%
   ggplot(aes(x = tmin,y = tmax))+
   geom_point(aes(color = name),alpha = .4)
```

    ## Warning: Removed 15 rows containing missing values (geom_point).

![](ggplot-1_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

``` r
  ggplot(weather_df, aes(x = tmin, y = tmax)) + 
  geom_point(aes(color = name), alpha = .5) +
  geom_smooth(se = FALSE)+
  facet_grid(. ~ name)#separate change to 3 plots#
```

    ## `geom_smooth()` using method = 'loess' and formula 'y ~ x'

    ## Warning: Removed 15 rows containing non-finite values (stat_smooth).

    ## Warning: Removed 15 rows containing missing values (geom_point).

![](ggplot-1_files/figure-gfm/unnamed-chunk-5-1.png)<!-- --> this is
fine but not interesting

``` r
weather_df%>%
   ggplot(aes(x = date,y = tmax,color = name))+
   geom_point(aes(size = prcp),alpha = .35)+
   geom_smooth(size = 2,se = FALSE)
```

    ## `geom_smooth()` using method = 'loess' and formula 'y ~ x'

    ## Warning: Removed 3 rows containing non-finite values (stat_smooth).

    ## Warning: Removed 3 rows containing missing values (geom_point).

![](ggplot-1_files/figure-gfm/unnamed-chunk-6-1.png)<!-- --> \#\#Some
extra stuff

``` r
  ggplot(weather_df, aes(x = date, y = tmax, color = name)) + 
  geom_smooth(se = FALSE) 
```

    ## `geom_smooth()` using method = 'loess' and formula 'y ~ x'

    ## Warning: Removed 3 rows containing non-finite values (stat_smooth).

![](ggplot-1_files/figure-gfm/unnamed-chunk-7-1.png)<!-- --> 2d density

``` r
#install package ("hexbin")
 weather_df%>%
  ggplot(aes(x = tmax, y = tmin)) + 
  geom_bin2d()
```

    ## Warning: Removed 15 rows containing non-finite values (stat_bin2d).

![](ggplot-1_files/figure-gfm/unnamed-chunk-8-1.png)<!-- --> \#\#More
kinds of plots\!

``` r
 weather_df%>%
  ggplot(aes(x = tmax,color = name))+#color is just outside the bar when you change color to fill it will change the color of bar#
  geom_histogram()
```

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

    ## Warning: Removed 3 rows containing non-finite values (stat_bin).

![](ggplot-1_files/figure-gfm/unnamed-chunk-9-1.png)<!-- -->

``` r
weather_df%>%
  ggplot(aes(x = tmax,fill = name))+
  geom_histogram(position = "dodge")
```

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

    ## Warning: Removed 3 rows containing non-finite values (stat_bin).

![](ggplot-1_files/figure-gfm/unnamed-chunk-10-1.png)<!-- -->

``` r
weather_df%>%
  ggplot(aes(x = tmax,fill = name))+
  geom_density(alpha = .3)
```

    ## Warning: Removed 3 rows containing non-finite values (stat_density).

![](ggplot-1_files/figure-gfm/unnamed-chunk-11-1.png)<!-- -->

``` r
weather_df%>%
  ggplot(aes(x = name,y = tmax))+
  geom_boxplot()
```

    ## Warning: Removed 3 rows containing non-finite values (stat_boxplot).

![](ggplot-1_files/figure-gfm/unnamed-chunk-12-1.png)<!-- -->

``` r
weather_df%>%
  ggplot(aes(x = name,y = tmax))+
  geom_violin()
```

    ## Warning: Removed 3 rows containing non-finite values (stat_ydensity).

![](ggplot-1_files/figure-gfm/unnamed-chunk-13-1.png)<!-- -->

ridge plots

``` r
 weather_df%>%
    ggplot(aes(x = tmax,y = name))+
    geom_density_ridges(scale = .85)
```

    ## Picking joint bandwidth of 1.84

    ## Warning: Removed 3 rows containing non-finite values (stat_density_ridges).

![](ggplot-1_files/figure-gfm/unnamed-chunk-14-1.png)<!-- -->
