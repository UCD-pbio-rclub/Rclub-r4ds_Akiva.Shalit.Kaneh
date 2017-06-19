# Untitled
Akiva Shalit-Kaneh  
May 31, 2017  




```r
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
## Chapter 11 Data Import

# 11.2.2 Excercises

1.What function would you use to read a file where fields were separated with
“|”?


```r
#read_delim(file, delim = "|")
```

2.Apart from file, skip, and comment, what other arguments do read_csv() and read_tsv() have in common?


read_csv(file, col_names = TRUE, col_types = NULL,
  locale = default_locale(), na = c("", "NA"), quoted_na = TRUE,
  quote = "\"", comment = "", trim_ws = TRUE, skip = 0, n_max = Inf,
  guess_max = min(1000, n_max), progress = show_progress())


read_tsv(file, col_names = TRUE, col_types = NULL,
  locale = default_locale(), na = c("", "NA"), quoted_na = TRUE,
  quote = "\"", comment = "", trim_ws = TRUE, skip = 0, n_max = Inf,
  guess_max = min(1000, n_max), progress = show_progress())


Arguments

file	
Either a path to a file, a connection, or literal data (either a single string or a raw vector).
Files ending in .gz, .bz2, .xz, or .zip will be automatically uncompressed. Files starting with http://, https://, ftp://, or ftps:// will be automatically downloaded. Remote gz files can also be automatically downloaded and decompressed.
Literal data is most useful for examples and tests. It must contain at least one new line to be recognised as data (instead of a path).

delim	
Single character used to separate fields within a record.

quote	
Single character used to quote strings.

escape_backslash	
Does the file use backslashes to escape special characters? This is more general than escape_double as backslashes can be used to escape the delimiter character, the quote character, or to add special characters like \n.

escape_double	
Does the file escape quotes by doubling them? i.e. If this option is TRUE, the value """" represents a single quote, \".

col_names	
Either TRUE, FALSE or a character vector of column names.
If TRUE, the first row of the input will be used as the column names, and will not be included in the data frame. If FALSE, column names will be generated automatically: X1, X2, X3 etc.
If col_names is a character vector, the values will be used as the names of the columns, and the first row of the input will be read into the first row of the output data frame.
Missing (NA) column names will generate a warning, and be filled in with dummy names X1, X2 etc. Duplicate column names will generate a warning and be made unique with a numeric prefix.

col_types	
One of NULL, a cols() specification, or a string. See vignette("column-types") for more details.
If NULL, all column types will be imputed from the first 1000 rows on the input. This is convenient (and fast), but not robust. If the imputation fails, you'll need to supply the correct types yourself.
If a column specification created by cols(), it must contain one column specification for each column. If you only want to read a subset of the columns, use cols_only().
Alternatively, you can use a compact string representation where each character represents one column: c = character, i = integer, n = number, d = double, l = logical, D = date, T = date time, t = time, ? = guess, or _/- to skip the column.

locale	
The locale controls defaults that vary from place to place. The default locale is US-centric (like R), but you can use locale() to create your own locale that controls things like the default time zone, encoding, decimal mark, big mark, and day/month names.

na	
Character vector of strings to use for missing values. Set this option to character() to indicate no missing values.

quoted_na	
Should missing values inside quotes be treated as missing values (the default) or strings.

comment	
A string used to identify comments. Any text after the comment characters will be silently ignored.

trim_ws	
Should leading and trailing whitespace be trimmed from each field before parsing it?

skip	
Number of lines to skip before reading data.

n_max	
Maximum number of records to read.

guess_max	
Maximum number of records to use for guessing column types.

progress	
Display a progress bar? By default it will only display in an interactive session and not while knitting a document. The display is updated every 50,000 values and will only display if estimated reading time is 5 seconds or more. The automatic progress bar can be disabled by setting option readr.show_progress to FALSE.


3.What are the most important arguments to read_fwf()?

read_fwf(file, col_positions, col_types = NULL, locale = default_locale(),
  na = c("", "NA"), comment = "", skip = 0, n_max = Inf,
  guess_max = min(n_max, 1000), progress = show_progress())
  
col_positions - supply the last column position as NA is the width of the last column is variable.

col_types - if NULL - types will be imputed based on first 1000 rows.

If a column specification created by cols(), it must contain one column specification for each column. If you only want to read a subset of the columns, use cols_only().

Alternatively, you can use a compact string representation where each character represents one column: c = character, i = integer, n = number, d = double, l = logical, D = date, T = date time, t = time, ? = guess, or _/- to skip the column.

na - Character vector of strings to use for missing values. Set this option to character() to indicate no missing values.

skip - Number of lines to skip before reading data.

n_max - Maximum number of records to read.

guess_max - Maximum number of records to use for guessing column types.

progress - displays progress bar (not while knitting).

4.Sometimes strings in a CSV file contain commas. To prevent them from causing problems they need to be surrounded by a quoting character, like " or '. By convention, read_csv() assumes that the quoting character will be ", and if you want to change it you’ll need to use read_delim() instead. What arguments do you need to specify to read the following text into a data frame?



```r
x <- "x,y\n1,'a,b'"
read_delim(x, ",", quote = "'")
```

```
## # A tibble: 1 × 2
##       x     y
##   <int> <chr>
## 1     1   a,b
```

5.Identify what is wrong with each of the following inline CSV files. What happens when you run the code?


```r
read_csv("a,b\n1,2,3\n4,5,6")
```

```
## Warning: 2 parsing failures.
## row col  expected    actual         file
##   1  -- 2 columns 3 columns literal data
##   2  -- 2 columns 3 columns literal data
```

```
## # A tibble: 2 × 2
##       a     b
##   <int> <int>
## 1     1     2
## 2     4     5
```

```r
# Third column was not specified so the last column is dropped.
# Fixed read_csv("a,b,c\n1,2,3\n4,5,6")
```


```r
read_csv("a,b,c\n1,2\n1,2,3,4")
```

```
## Warning: 2 parsing failures.
## row col  expected    actual         file
##   1  -- 3 columns 2 columns literal data
##   2  -- 3 columns 4 columns literal data
```

```
## # A tibble: 2 × 3
##       a     b     c
##   <int> <int> <int>
## 1     1     2    NA
## 2     1     2     3
```

```r
# Three columns specified, third column's first value is not specified and gets an automatic NA. There is not 4th column so the fourth number in the second row is dropped.
# Fix read_csv("a,b,c,d\n1,2,3,4\n1,2,3,4")
```


```r
read_csv("a,b\n\"1")
```

```
## Warning: 2 parsing failures.
## row col                     expected    actual         file
##   1  a  closing quote at end of file           literal data
##   1  -- 2 columns                    1 columns literal data
```

```
## # A tibble: 1 × 2
##       a     b
##   <int> <chr>
## 1     1  <NA>
```

```r
# I don't understand this.
```


```r
read_csv("a,b\n1,2\na,b")
```

```
## # A tibble: 2 × 2
##       a     b
##   <chr> <chr>
## 1     1     2
## 2     a     b
```

```r
# 1 and 2 are read as 'chr' and not 'int'
```


```r
read_csv("a;b\n1;3")
```

```
## # A tibble: 1 × 1
##   `a;b`
##   <chr>
## 1   1;3
```

```r
# a;b is seen as the column name and 1;3 as the 'chr' data.
# Fix read_csv2("a;b\n1;3")
```



```r
library(hms)
```


# Exercises 11.3.5

1.What are the most important arguments to locale()?

Arguments

date_names	
Character representations of day and month names. Either the language code as string (passed on to date_names_lang()) or an object created by date_names().
date_format, time_format	
Default date and time formats.
decimal_mark, grouping_mark	
Symbols used to indicate the decimal place, and to chunk larger numbers. Decimal mark can only be , or ..
tz	
Default tz. This is used both for input (if the time zone isn't present in individual strings), and for output (to control the default display). The default is to use "UTC", a time zone that does not use daylight savings time (DST) and hence is typically most useful for data. The absence of time zones makes it approximately 50x faster to generate UTC times than any other time zone.
Use "" to use the system default time zone, but beware that this will not be reproducible across systems.
For a complete list of possible time zones, see OlsonNames(). Americans, note that "EST" is a Canadian time zone that does not have DST. It is not Eastern Standard Time. It's better to use "US/Eastern", "US/Central" etc.
encoding	
Default encoding. This only affects how the file is read - readr always converts the output to UTF-8.
asciify	
Should diacritics be stripped from date names and converted to ASCII? This is useful if you're dealing with ASCII data where the correct spellings have been lost. Requires the stringi package.

2.What happens if you try and set 'decimal_mark' and 'grouping_mark' to the same character? What happens to the default value of grouping_mark when you set decimal_mark to “,”? What happens to the default value of decimal_mark when you set the grouping_mark to “.”?


```r
#locale(decimal_mark = ".", grouping_mark = ".")
```

# These cannot be the same!


```r
locale(decimal_mark = ",")
```

```
## <locale>
## Numbers:  123.456,78
## Formats:  %AD / %AT
## Timezone: UTC
## Encoding: UTF-8
## <date_names>
## Days:   Sunday (Sun), Monday (Mon), Tuesday (Tue), Wednesday (Wed),
##         Thursday (Thu), Friday (Fri), Saturday (Sat)
## Months: January (Jan), February (Feb), March (Mar), April (Apr), May
##         (May), June (Jun), July (Jul), August (Aug), September
##         (Sep), October (Oct), November (Nov), December (Dec)
## AM/PM:  AM/PM
```

# Grouping mark is set to period.


```r
locale(grouping_mark = ".")
```

```
## <locale>
## Numbers:  123.456,78
## Formats:  %AD / %AT
## Timezone: UTC
## Encoding: UTF-8
## <date_names>
## Days:   Sunday (Sun), Monday (Mon), Tuesday (Tue), Wednesday (Wed),
##         Thursday (Thu), Friday (Fri), Saturday (Sat)
## Months: January (Jan), February (Feb), March (Mar), April (Apr), May
##         (May), June (Jun), July (Jul), August (Aug), September
##         (Sep), October (Oct), November (Nov), December (Dec)
## AM/PM:  AM/PM
```
# Decimal mark is set to comma

3.I didn’t discuss the date_format and time_format options to locale(). What do they do? Construct an example that shows when they might be useful.

Default date and time formats. These could be changes:


```r
parse_date("1 janvier 2015", "%d %B %Y", locale = locale("fr"))
```

```
## [1] "2015-01-01"
```

```r
#> [1] "2015-01-01"
parse_date("14 oct. 1979", "%d %b %Y", locale = locale("fr"))
```

```
## [1] "1979-10-14"
```

```r
#> [1] "1979-10-14"
```

Year
%Y (4 digits).
%y (2 digits); 00-69 -> 2000-2069, 70-99 -> 1970-1999.
Month
%m (2 digits).
%b (abbreviated name, like “Jan”).
%B (full name, “January”).
Day
%d (2 digits).
%e (optional leading space).

Time
%H 0-23 hour.
%I 0-12, must be used with %p.
%p AM/PM indicator.
%M minutes.
%S integer seconds.
%OS real seconds.
%Z Time zone (as name, e.g. America/Chicago). Beware of abbreviations: if you’re American, note that “EST” is a Canadian time zone that does not have daylight savings time. It is not Eastern Standard Time! We’ll come back to this time zones.



```r
parse_time("20:10:10", "%H %M %OS", locale = locale("fr"))
```

```
## Warning: 1 parsing failure.
## row col            expected   actual
##   1  -- time like %H %M %OS 20:10:10
```

```
## NA
```
# This did not work!

4.If you live outside the US, create a new locale object that encapsulates the settings for the types of file you read most commonly.


```r
maori <- locale(date_names(
  day = c("Rātapu", "Rāhina", "Rātū", "Rāapa", "Rāpare", "Rāmere", "Rāhoroi"),
  mon = c("Kohi-tātea", "Hui-tanguru", "Poutū-te-rangi", "Paenga-whāwhā",
    "Haratua", "Pipiri", "Hōngongoi", "Here-turi-kōkā", "Mahuru",
    "Whiringa-ā-nuku", "Whiringa-ā-rangi", "Hakihea")
))
```

5.What’s the difference between read_csv() and read_csv2()?

csv reads "," delimited data and csv2 reads ";" delimited data.

6.What are the most common encodings used in Europe? What are the most common encodings used in Asia? Do some googling to find out.

ISO 8859-1 Western Europe
ISO 8859-2 Western and Central Europe
ISO 8859-3 Western Europe and South European (Turkish, Maltese plus Esperanto)
ISO 8859-4 Western Europe and Baltic countries (Lithuania, Estonia, Latvia and Lapp)
ISO 8859-5 Cyrillic alphabet
ISO 8859-6 Arabic
ISO 8859-7 Greek
ISO 8859-8 Hebrew
ISO 8859-9 Western Europe with amended Turkish character set
ISO 8859-10 Western Europe with rationalised character set for Nordic 



Chinese Guobiao
GB 2312
GBK (Microsoft Code page 936)
GB 18030
Taiwan Big5 (a more famous variant is Microsoft Code page 950)
Hong Kong HKSCS
Korean
KS X 1001 is a Korean double-byte character encoding standard
EUC-KR
ISO-2022-KR

7.Generate the correct format string to parse each of the following dates and times:


```r
d1 <- "January 1, 2010"
parse_date(d1, "%B %d, %Y", locale = locale("en"))
```

```
## [1] "2010-01-01"
```

# Adding the comma after %d, enabled the function to work and parse d1.


```r
d2 <- "2015-Mar-07"
parse_date(d2, "%Y- %b- %d")
```

```
## [1] "2015-03-07"
```



```r
d3 <- "06-Jun-2017"
parse_date(d3, "%d- %b -%Y")
```

```
## [1] "2017-06-06"
```


```r
d4 <- c("August 19 (2015)", "July 1 (2015)")
parse_date(d4, "%B %d (%Y)", locale = locale("en"))
```

```
## [1] "2015-08-19" "2015-07-01"
```



```r
d5 <- "12/30/14" # Dec 30, 2014
parse_date(d5, "%m/%d/%y", locale = locale("en"))
```

```
## [1] "2014-12-30"
```


```r
t1 <- "1705"
parse_time(t1, "%H%M")
```

```
## 17:05:00
```


```r
t2 <- "11:15:10.12 PM"
parse_time(t2,"%H: %M: %OS %p")
```

```
## 23:15:10.12
```

