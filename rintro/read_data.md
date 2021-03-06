



## Reading data into R


### Reading text files

One of the easiest ways to store data is to use text files (or comma separated files `.csv`). 
Nearly every tool can handle such data.

To read such data into R we use the function `read.table`.

`read.table` has a lot of arguments to control it's functionality:


```r
read.table(file, header = FALSE, sep = "", quote = "\"'",
           dec = ".", numerals = c("allow.loss", "warn.loss", "no.loss"),
           row.names, col.names, as.is = !stringsAsFactors,
           na.strings = "NA", colClasses = NA, nrows = -1,
           skip = 0, check.names = TRUE, fill = !blank.lines.skip,
           strip.white = FALSE, blank.lines.skip = TRUE,
           comment.char = "#",
           allowEscapes = FALSE, flush = FALSE,
           stringsAsFactors = default.stringsAsFactors(),
           fileEncoding = "", encoding = "unknown", text, skipNul = FALSE)
```



The most useful ones are:

* `file` : character, the path to the file (either absolute or relative to the working directory [`#! need link here`])
* `header` : logical (either `TRUE` or `FALSE`). Does the file include column names in the first row?
* `sep` : columns can be separated in text files by different characters (e.g. `,` or `;` or `|`)
* `dec` : the decimal separator. Default is `.`, but in some countries (like Germany) this can also be `,`
* `stringsAsFactors` : Should be strings interpreted as factors? It tend to use `stringsAsFactors = FALSE` (default is `TRUE`) and convert only if needed.

So a typical command to load data would be:


```r
setwd( "/home/edisz/Documents/Uni/Projects/qetxrbook")
df <- read.table('data/iris1.csv', sep = ';', header = TRUE, stringsAsFactors = FALSE)
```

After reading the data,

always check if all is correct.
You can use the `str()` function for this:


```r
str(df)
```

```
## 'data.frame':	150 obs. of  5 variables:
##  $ Sepal.Length: num  5.1 4.9 4.7 4.6 5 5.4 4.6 5 4.4 4.9 ...
##  $ Sepal.Width : num  3.5 3 3.2 NA 3.6 3.9 3.4 3.4 2.9 3.1 ...
##  $ Petal.Length: num  1.4 1.4 1.3 1.5 1.4 1.7 1.4 1.5 1.4 1.5 ...
##  $ Petal.Width : num  0.2 0.2 0.2 0.2 0.2 0.4 0.3 0.2 0.2 0.1 ...
##  $ Species     : chr  "setosa" "setosa" "setosa" "setosa" ...
```

Look at:

* correct number of rows (=obs.)
* correct number of columns (=variables)
* correct type of columns. Sometimes variable that should be numeric are recorded as factors. This is an indication that there is at least some non-numeric data in this column - check.
* correct column names


### Writing text files

Writing text files is done via `write.table`. It has a similar set of attributes as above. 
However, to others should be mentioned:

* `append` : If `TRUE` the data is appended to the file. Otherwise, the existing file is overwritten.
* `row.names` : Is always use `row.names=FALSE`, because row-names should not contain information and the columns are better aligned.

So to write the above data.frame to a file use:


```r
write.table(df, "/home/edisz/mytable.csv", sep = ';', row.names = FALSE)
```



### Excel files

Eventually you will have to work with Microsoft Excel files.
Excel file are [not a good format](http://www.win-vector.com/blog/2012/12/please-stop-using-Excel-like-formats-to-exchange-data/) to store data and have been hard to read into R.

Luckily, there is now the [`readxl`](https://cran.r-project.org/web/packages/readxl/index.html) package available on CRAN to make this easier. 




### Other files

R can also read a lot of other formats (SPSS, GIS-data, ...), however you'll certainly need additional packages. Just [search](../rintro/help.html) for it on the internet.



### Databases

R can connect to all major databases like [MySQL](https://cran.r-project.org/web/packages/RMySQL/index.html), [PostgreSQL](https://cran.r-project.org/web/packages/RPostgreSQL/index.html) or [SQLite](https://cran.r-project.org/web/packages/RSQLite/index.html).

The usage is similar for all:

1. Connect to the database


```r
drv <- dbDriver("PostgreSQL")
con <- dbConnect(drv, dbname = DBname, user = DBuser, host = DBhost, port = DBport)
```

where `DBname`, `DBuser`, `DBhost` and `DBport` are characters specifying the details of the connection.

2. Query the data you need:


```r
df <- dbGetQuery(con, 'SELECT * FROM public.df')
```


3. Close the connection when finished:


```r
dbDisconnect(con)
dbUnloadDriver(drv)
```



### Exercises

There are 3 additional data files in the data-directory (`iris2.csv`, `iris3.csv` and `iris4.csv`). Each of this files has different '*problems*'.

Read each into R using `read.table()` and specify appropriate arguments. 
It may be helpful to take a look at the files using a text-editor. 
The data should have the same structure as above.
(Hint: read the help for read.table carefully).
