# R_club-May_10th
Akiva Shalit-Kaneh  
May 4, 2017  



## Data Transformation

Below is a fix for not being able to download nycflights13 from:
https://github.com/rstudio/webinars/issues/2



```r
install.packages("https://cran.r-project.org/src/contrib/nycflights13_0.2.2.tar.gz", repos=NULL, method="libcurl")
```

```
## Installing package into 'C:/Users/Akiva/Documents/R/win-library/3.3'
## (as 'lib' is unspecified)
```

To use view() you need to call on tidyverse first.

```r
library(nycflights13)
library(tidyverse)
```

```
## Loading tidyverse: ggplot2
## Loading tidyverse: tibble
## Loading tidyverse: tidyr
## Loading tidyverse: readr
## Loading tidyverse: purrr
## Loading tidyverse: dplyr
```

```
## Conflicts with tidy packages ----------------------------------------------
```

```
## filter(): dplyr, stats
## lag():    dplyr, stats
```

## 5.2.4 Exercises

1.1 Flights with an arrival delay of 2 or more hours:

```r
one_one <- filter(flights, arr_delay >= 120)
```

1.2 Flew to Houston:

```r
one_two <- filter(flights, dest == 'HOU' | dest == 'IAH')
```

1.3 Were operated by United, American, or Delta:


```r
one_three <- filter(flights, carrier == 'UA' | carrier == 'AA' | carrier == 'DL')
```
or 


```r
one_three <- filter(flights, carrier %in% c('UA', 'AA', 'DL'))
```

1.4 Departed in summer (July, August, and September):


```r
one_four <- filter(flights, month %in% c(7, 8, 9))
```

1.5 Arrived more than two hours late, but didn’t leave late: Note that early planes have negative minute values.


```r
one_five <- filter(flights, arr_delay >= 120 & dep_delay <= 0)
```

1.6 Were delayed by at least an hour, but made up over 30 minutes in flight:


```r
one_six <- filter(flights, dep_delay >= 60, dep_delay-arr_delay > 30)
```

1.7 Departed between midnight and 6am (inclusive):


```r
one_seven <- filter(flights, dep_time <=600 | dep_time == 2400)
```

2. between() is a shortcut for x >= left & x <= right, implemented efficiently in C++ for local values, and translated to the appropriate SQL for remote tables.


```r
two_seven <- filter(flights, !between(dep_time, 601, 2359))
```

3. 

```r
summary(flights)
```

```
##       year          month             day           dep_time   
##  Min.   :2013   Min.   : 1.000   Min.   : 1.00   Min.   :   1  
##  1st Qu.:2013   1st Qu.: 4.000   1st Qu.: 8.00   1st Qu.: 907  
##  Median :2013   Median : 7.000   Median :16.00   Median :1401  
##  Mean   :2013   Mean   : 6.549   Mean   :15.71   Mean   :1349  
##  3rd Qu.:2013   3rd Qu.:10.000   3rd Qu.:23.00   3rd Qu.:1744  
##  Max.   :2013   Max.   :12.000   Max.   :31.00   Max.   :2400  
##                                                  NA's   :8255  
##  sched_dep_time   dep_delay          arr_time    sched_arr_time
##  Min.   : 106   Min.   : -43.00   Min.   :   1   Min.   :   1  
##  1st Qu.: 906   1st Qu.:  -5.00   1st Qu.:1104   1st Qu.:1124  
##  Median :1359   Median :  -2.00   Median :1535   Median :1556  
##  Mean   :1344   Mean   :  12.64   Mean   :1502   Mean   :1536  
##  3rd Qu.:1729   3rd Qu.:  11.00   3rd Qu.:1940   3rd Qu.:1945  
##  Max.   :2359   Max.   :1301.00   Max.   :2400   Max.   :2359  
##                 NA's   :8255      NA's   :8713                 
##    arr_delay          carrier              flight       tailnum         
##  Min.   : -86.000   Length:336776      Min.   :   1   Length:336776     
##  1st Qu.: -17.000   Class :character   1st Qu.: 553   Class :character  
##  Median :  -5.000   Mode  :character   Median :1496   Mode  :character  
##  Mean   :   6.895                      Mean   :1972                     
##  3rd Qu.:  14.000                      3rd Qu.:3465                     
##  Max.   :1272.000                      Max.   :8500                     
##  NA's   :9430                                                           
##     origin              dest              air_time        distance   
##  Length:336776      Length:336776      Min.   : 20.0   Min.   :  17  
##  Class :character   Class :character   1st Qu.: 82.0   1st Qu.: 502  
##  Mode  :character   Mode  :character   Median :129.0   Median : 872  
##                                        Mean   :150.7   Mean   :1040  
##                                        3rd Qu.:192.0   3rd Qu.:1389  
##                                        Max.   :695.0   Max.   :4983  
##                                        NA's   :9430                  
##       hour           minute        time_hour                  
##  Min.   : 1.00   Min.   : 0.00   Min.   :2013-01-01 05:00:00  
##  1st Qu.: 9.00   1st Qu.: 8.00   1st Qu.:2013-04-04 13:00:00  
##  Median :13.00   Median :29.00   Median :2013-07-03 10:00:00  
##  Mean   :13.18   Mean   :26.23   Mean   :2013-07-03 05:02:36  
##  3rd Qu.:17.00   3rd Qu.:44.00   3rd Qu.:2013-10-01 07:00:00  
##  Max.   :23.00   Max.   :59.00   Max.   :2013-12-31 23:00:00  
## 
```

8255 departure times are missing. Other missing data are dep_delay 8255, 8713 arr_time, 9430 arr_delay.

4.All the examples have a known solution regardless of the NAs value.
Any number to the power of 0 is 1, any " " | True will return a vlue of true. Any False & "" will give false. The rule is that if the value is known regardless of NA that will be the default.

## Exercise 5.3.1

1. How could you use arrange() to sort all missing values to the start? (Hint: use is.na()).


```r
df <- tibble(x = c(5, 2, NA))
arrange(df, desc(is.na(x)))
```

```
## # A tibble: 3 × 1
##       x
##   <dbl>
## 1    NA
## 2     5
## 3     2
```

Or


```r
arrange(df, -(is.na(x)))
```

```
## # A tibble: 3 × 1
##       x
##   <dbl>
## 1    NA
## 2     5
## 3     2
```

I did not understand this, does it work because TRUE values are first?
In the second example TRUE gets the value of -1 and the others 0, is that the reason for the descending order?

2.Sort flights to find the most delayed flights. Find the flights that left earliest.

# _Most delayed_

```r
arrange(flights, desc(dep_delay))
```

```
## # A tibble: 336,776 × 19
##     year month   day dep_time sched_dep_time dep_delay arr_time
##    <int> <int> <int>    <int>          <int>     <dbl>    <int>
## 1   2013     1     9      641            900      1301     1242
## 2   2013     6    15     1432           1935      1137     1607
## 3   2013     1    10     1121           1635      1126     1239
## 4   2013     9    20     1139           1845      1014     1457
## 5   2013     7    22      845           1600      1005     1044
## 6   2013     4    10     1100           1900       960     1342
## 7   2013     3    17     2321            810       911      135
## 8   2013     6    27      959           1900       899     1236
## 9   2013     7    22     2257            759       898      121
## 10  2013    12     5      756           1700       896     1058
## # ... with 336,766 more rows, and 12 more variables: sched_arr_time <int>,
## #   arr_delay <dbl>, carrier <chr>, flight <int>, tailnum <chr>,
## #   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
## #   minute <dbl>, time_hour <dttm>
```
# _Left the earliest_

```r
arrange(flights, dep_delay)
```

```
## # A tibble: 336,776 × 19
##     year month   day dep_time sched_dep_time dep_delay arr_time
##    <int> <int> <int>    <int>          <int>     <dbl>    <int>
## 1   2013    12     7     2040           2123       -43       40
## 2   2013     2     3     2022           2055       -33     2240
## 3   2013    11    10     1408           1440       -32     1549
## 4   2013     1    11     1900           1930       -30     2233
## 5   2013     1    29     1703           1730       -27     1947
## 6   2013     8     9      729            755       -26     1002
## 7   2013    10    23     1907           1932       -25     2143
## 8   2013     3    30     2030           2055       -25     2213
## 9   2013     3     2     1431           1455       -24     1601
## 10  2013     5     5      934            958       -24     1225
## # ... with 336,766 more rows, and 12 more variables: sched_arr_time <int>,
## #   arr_delay <dbl>, carrier <chr>, flight <int>, tailnum <chr>,
## #   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
## #   minute <dbl>, time_hour <dttm>
```

3.Sort flights to find the fastest flights.


```r
arrange(flights, air_time)
```

```
## # A tibble: 336,776 × 19
##     year month   day dep_time sched_dep_time dep_delay arr_time
##    <int> <int> <int>    <int>          <int>     <dbl>    <int>
## 1   2013     1    16     1355           1315        40     1442
## 2   2013     4    13      537            527        10      622
## 3   2013    12     6      922            851        31     1021
## 4   2013     2     3     2153           2129        24     2247
## 5   2013     2     5     1303           1315       -12     1342
## 6   2013     2    12     2123           2130        -7     2211
## 7   2013     3     2     1450           1500       -10     1547
## 8   2013     3     8     2026           1935        51     2131
## 9   2013     3    18     1456           1329        87     1533
## 10  2013     3    19     2226           2145        41     2305
## # ... with 336,766 more rows, and 12 more variables: sched_arr_time <int>,
## #   arr_delay <dbl>, carrier <chr>, flight <int>, tailnum <chr>,
## #   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
## #   minute <dbl>, time_hour <dttm>
```

4.Which flights travelled the longest? Which travelled the shortest?


```r
arrange(flights, distance)
```

```
## # A tibble: 336,776 × 19
##     year month   day dep_time sched_dep_time dep_delay arr_time
##    <int> <int> <int>    <int>          <int>     <dbl>    <int>
## 1   2013     7    27       NA            106        NA       NA
## 2   2013     1     3     2127           2129        -2     2222
## 3   2013     1     4     1240           1200        40     1333
## 4   2013     1     4     1829           1615       134     1937
## 5   2013     1     4     2128           2129        -1     2218
## 6   2013     1     5     1155           1200        -5     1241
## 7   2013     1     6     2125           2129        -4     2224
## 8   2013     1     7     2124           2129        -5     2212
## 9   2013     1     8     2127           2130        -3     2304
## 10  2013     1     9     2126           2129        -3     2217
## # ... with 336,766 more rows, and 12 more variables: sched_arr_time <int>,
## #   arr_delay <dbl>, carrier <chr>, flight <int>, tailnum <chr>,
## #   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
## #   minute <dbl>, time_hour <dttm>
```

EWR to LGA 17 miles


```r
arrange(flights, desc(distance))
```

```
## # A tibble: 336,776 × 19
##     year month   day dep_time sched_dep_time dep_delay arr_time
##    <int> <int> <int>    <int>          <int>     <dbl>    <int>
## 1   2013     1     1      857            900        -3     1516
## 2   2013     1     2      909            900         9     1525
## 3   2013     1     3      914            900        14     1504
## 4   2013     1     4      900            900         0     1516
## 5   2013     1     5      858            900        -2     1519
## 6   2013     1     6     1019            900        79     1558
## 7   2013     1     7     1042            900       102     1620
## 8   2013     1     8      901            900         1     1504
## 9   2013     1     9      641            900      1301     1242
## 10  2013     1    10      859            900        -1     1449
## # ... with 336,766 more rows, and 12 more variables: sched_arr_time <int>,
## #   arr_delay <dbl>, carrier <chr>, flight <int>, tailnum <chr>,
## #   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
## #   minute <dbl>, time_hour <dttm>
```

JFK to HNL 4983 miles


## Exercise 5.4.1

1.

```r
select(flights, dep_time,  dep_delay, arr_time, arr_delay)
```

```
## # A tibble: 336,776 × 4
##    dep_time dep_delay arr_time arr_delay
##       <int>     <dbl>    <int>     <dbl>
## 1       517         2      830        11
## 2       533         4      850        20
## 3       542         2      923        33
## 4       544        -1     1004       -18
## 5       554        -6      812       -25
## 6       554        -4      740        12
## 7       555        -5      913        19
## 8       557        -3      709       -14
## 9       557        -3      838        -8
## 10      558        -2      753         8
## # ... with 336,766 more rows
```



```r
vars <- c("dep_time", "dep_delay", "arr_time", "arr_delay")
select(flights, one_of(vars))
```

```
## # A tibble: 336,776 × 4
##    dep_time dep_delay arr_time arr_delay
##       <int>     <dbl>    <int>     <dbl>
## 1       517         2      830        11
## 2       533         4      850        20
## 3       542         2      923        33
## 4       544        -1     1004       -18
## 5       554        -6      812       -25
## 6       554        -4      740        12
## 7       555        -5      913        19
## 8       557        -3      709       -14
## 9       557        -3      838        -8
## 10      558        -2      753         8
## # ... with 336,766 more rows
```



2.What happens if you include the name of a variable multiple times in a select() call?


```r
select(flights, dep_time,  dep_time, dep_time, arr_delay)
```

```
## # A tibble: 336,776 × 2
##    dep_time arr_delay
##       <int>     <dbl>
## 1       517        11
## 2       533        20
## 3       542        33
## 4       544       -18
## 5       554       -25
## 6       554        12
## 7       555        19
## 8       557       -14
## 9       557        -8
## 10      558         8
## # ... with 336,766 more rows
```

Select treats variables written twice or more as if they were written once.

3. one_of() is used with select as well but you have to first define a vector and put it into the vars varialble to use the function.



```r
vars <- c("year", "month", "day", "dep_delay", "arr_delay")
select(flights, one_of(vars))
```

```
## # A tibble: 336,776 × 5
##     year month   day dep_delay arr_delay
##    <int> <int> <int>     <dbl>     <dbl>
## 1   2013     1     1         2        11
## 2   2013     1     1         4        20
## 3   2013     1     1         2        33
## 4   2013     1     1        -1       -18
## 5   2013     1     1        -6       -25
## 6   2013     1     1        -4        12
## 7   2013     1     1        -5        19
## 8   2013     1     1        -3       -14
## 9   2013     1     1        -3        -8
## 10  2013     1     1        -2         8
## # ... with 336,766 more rows
```

4. Does the result of running the following code surprise you? How do the select helpers deal with case by default? How can you change that default?


```r
select(flights, contains("TIME"))
```

```
## # A tibble: 336,776 × 6
##    dep_time sched_dep_time arr_time sched_arr_time air_time
##       <int>          <int>    <int>          <int>    <dbl>
## 1       517            515      830            819      227
## 2       533            529      850            830      227
## 3       542            540      923            850      160
## 4       544            545     1004           1022      183
## 5       554            600      812            837      116
## 6       554            558      740            728      150
## 7       555            600      913            854      158
## 8       557            600      709            723       53
## 9       557            600      838            846      140
## 10      558            600      753            745      138
## # ... with 336,766 more rows, and 1 more variables: time_hour <dttm>
```

This was surprising, contains() was not case sensitive. By default several of the select helper functions are not case sensitive.

starts_with(match, ignore.case = TRUE, vars = current_vars())

ends_with(match, ignore.case = TRUE, vars = current_vars())

contains(match, ignore.case = TRUE, vars = current_vars())

matches(match, ignore.case = TRUE, vars = current_vars())
 
# This can be changes by setting _ignore.case = FALSE_

## 5.5.2 Exercises

1. Currently dep_time and sched_dep_time are convenient to look at, but hard to compute with because they’re not really continuous numbers. Convert them to a more convenient representation of number of minutes since midnight.


```r
New_flights <- flights
New_flights_min <- mutate(New_flights, dep_time_min = dep_time %/% 100 * 60 + dep_time %% 100, sched_dep_time_min = sched_dep_time %/% 100 * 60 + sched_dep_time %% 100)
New_flights_Min <- select(New_flights_min, -dep_time, -sched_dep_time)
```
How can I swap columns, or place new columns in place of existing ones (I know I can use select( wanted column, everything() ).

2.Compare air_time with arr_time - dep_time. What do you expect to see? What do you see? What do you need to do to fix it?


```r
flights_new <- mutate(flights, arr_dep = arr_time - dep_time)
select(flights_new, air_time, dep_time, arr_dep, everything())
```

```
## # A tibble: 336,776 × 20
##    air_time dep_time arr_dep  year month   day sched_dep_time dep_delay
##       <dbl>    <int>   <int> <int> <int> <int>          <int>     <dbl>
## 1       227      517     313  2013     1     1            515         2
## 2       227      533     317  2013     1     1            529         4
## 3       160      542     381  2013     1     1            540         2
## 4       183      544     460  2013     1     1            545        -1
## 5       116      554     258  2013     1     1            600        -6
## 6       150      554     186  2013     1     1            558        -4
## 7       158      555     358  2013     1     1            600        -5
## 8        53      557     152  2013     1     1            600        -3
## 9       140      557     281  2013     1     1            600        -3
## 10      138      558     195  2013     1     1            600        -2
## # ... with 336,766 more rows, and 12 more variables: arr_time <int>,
## #   sched_arr_time <int>, arr_delay <dbl>, carrier <chr>, flight <int>,
## #   tailnum <chr>, origin <chr>, dest <chr>, distance <dbl>, hour <dbl>,
## #   minute <dbl>, time_hour <dttm>
```

The variables dep_time and arr_time are in both hours and minutes and are not a continuous value. To fix this they should be converted to minutes as was done in question 1.

3. Compare dep_time, sched_dep_time, and dep_delay. How would you expect those three numbers to be related?


```r
(Compare <- select(flights, dep_time, sched_dep_time, dep_delay))
```

```
## # A tibble: 336,776 × 3
##    dep_time sched_dep_time dep_delay
##       <int>          <int>     <dbl>
## 1       517            515         2
## 2       533            529         4
## 3       542            540         2
## 4       544            545        -1
## 5       554            600        -6
## 6       554            558        -4
## 7       555            600        -5
## 8       557            600        -3
## 9       557            600        -3
## 10      558            600        -2
## # ... with 336,766 more rows
```

dep_delay is the result of subtracting sched_dep_time from dep_time.

4. Find the 10 most delayed flights using a ranking function. How do you want to handle ties? Carefully read the documentation for min_rank().


```r
filter(flights, min_rank(desc(dep_delay))<=10)
```

```
## # A tibble: 10 × 19
##     year month   day dep_time sched_dep_time dep_delay arr_time
##    <int> <int> <int>    <int>          <int>     <dbl>    <int>
## 1   2013     1     9      641            900      1301     1242
## 2   2013     1    10     1121           1635      1126     1239
## 3   2013    12     5      756           1700       896     1058
## 4   2013     3    17     2321            810       911      135
## 5   2013     4    10     1100           1900       960     1342
## 6   2013     6    15     1432           1935      1137     1607
## 7   2013     6    27      959           1900       899     1236
## 8   2013     7    22      845           1600      1005     1044
## 9   2013     7    22     2257            759       898      121
## 10  2013     9    20     1139           1845      1014     1457
## # ... with 12 more variables: sched_arr_time <int>, arr_delay <dbl>,
## #   carrier <chr>, flight <int>, tailnum <chr>, origin <chr>, dest <chr>,
## #   air_time <dbl>, distance <dbl>, hour <dbl>, minute <dbl>,
## #   time_hour <dttm>
```

using desc() in min_rank gived the largest values the smalest ranking and so asking for those with ranking <=10 is what we are looking for.

5. What does 1:3 + 1:10 return? Why?

```r
1:3 + 1:10
```

```
## Warning in 1:3 + 1:10: longer object length is not a multiple of shorter
## object length
```

```
##  [1]  2  4  6  5  7  9  8 10 12 11
```

This results in a error

> Warning message:
In 1:3 + 1:10 :
  longer object length is not a multiple of shorter object length
  
This makes two vectors of different sizes and so they cannot be added to each other.

6. What trigonometric functions does R provide?

>Trigonometric Functions

Description

These functions give the obvious trigonometric functions. They respectively compute the cosine, sine, tangent, arc-cosine, arc-sine, arc-tangent, and the two-argument arc-tangent.

cospi(x), sinpi(x), and tanpi(x), compute cos(pi*x), sin(pi*x), and tan(pi*x).
