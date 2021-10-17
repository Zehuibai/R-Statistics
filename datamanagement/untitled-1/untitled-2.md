# Import/Export

## Baisc Import 

### import csv data

默认情况下，字符型变量将转换为因子。我们并不总是希望程序这样做(例如处理一个含有被调查者评论的变量时)。有许多方法可以禁止这种转换行为。其中包括设置选项 stringsAsFactors=FALSE

```
```

当固定格式文件很大并且有很多变量时，这特别方便，键入所有变量名称变得不切实际。 在这种情况下，width选项用于指定每个变量的宽度，而col.name选项用于指定包含变量名称的文件。

```
test.fixed <- read.fwf("https://stats.idre.ucla.edu/wp-content/uploads/2016/02/testfixed.txt", 
                       col.names=names, 
                       width = c(5, 7, 2, 4, 4))
print(test.fixed)
```

### Scan

scan与read.table函数返回数据帧不同，scan函数返回列表或向量。这使得扫描功能对于输入“矩形”数据的用处不大

```
x <- scan("https://stats.idre.ucla.edu/wp-content/uploads/2016/02/scan.txt", 
          what=list(age=0, name=""))
          
names <- scan("https://stats.idre.ucla.edu/wp-content/uploads/2016/02/names.txt", 
              what=character() )
```

### import txt data

```
test.semi <- read.table("https://stats.idre.ucla.edu/wp-content/uploads/2016/02/testsemicolon.txt", header=T, sep=";")
print(test.semi)

pfad <- "~/Desktop/SASUniversityEdition/myfolders/Daten"
mydata3 <- read.table(file.path(pfad, "CRP.csv"), 
                      header=TRUE, sep=";", dec = ",", skip=9) 
```

### import excel data

* read.xlsx slow for large data sets (worksheet with more than 100 000 cells) 大型数据集较
* read.xlsx2 is faster on big files compared to read.xlsx function.

```
```

### import stata data

```
```

### import SAS data

```
```

## Basic Export

### save R-Datei

save(): als R-Datei (.RData bzw. .rda) 

```
save(x,X,XX, file="~/Desktop/mysave.RData")
load()
```

### write

write.table(), write.csv(): als Text- oder csv-Datei 

```
```

(): als 

```
library(foreign)
write.foreign(df=data.frame(X), datafile="X.csv",
              codefile="X.sas", package="SAS")
```

write.xlsx: als Excel-Datei (Package xlsx)

```
library("xlsx")
# Write the first data set in a new workbook
write.xlsx(USArrests, file = "myworkbook.xlsx",
      sheetName = "USA-ARRESTS", append = FALSE)
# Add a second data set in a new worksheet
write.xlsx(mtcars, file = "myworkbook.xlsx", 
           sheetName="MTCARS", append=TRUE)
# Add a third data set
write.xlsx(iris, file = "myworkbook.xlsx",
           sheetName="IRIS", append=TRUE)
```

### Plot



```
```

```
# 1. Open jpeg file
jpeg("rplot.jpg", width = 350, height = "350")
# 2. Create the plot
plot(x = my_data$wt, y = my_data$mpg,
     pch = 16, frame = FALSE,
     xlab = "wt", ylab = "mpg", col = "#2E9FDF")
# 3. Close the file
dev.off()
```

##  **Package readr** 

### read_csv

运行read_csv（）时，它会打印出列规范 (column specification)，该规范给出了每一列的名称和类型。

```
library(readr)
read_csv("a,b,c
1,2,3
4,5,6")
```

有时，文件顶部会包含几行元数据。 您可以使用skip = n跳过前n行； 或使用comment =“＃”删除

```
read_csv("The first line of metadata
  The second line of metadata
  x,y,z
  1,2,3", skip = 2)

read_csv("# A comment I want to skip
  x,y,z
  1,2,3", comment = "#")

#> # A tibble: 1 x 3
#>       x     y     z
#>   <dbl> <dbl> <dbl>
#> 1     1     2     3
```

数据可能没有列名。 您可以使用col_names = FALSE来告诉read_csv（）不要将第一行视为标题，而是依次将它们从X1标记为Xn：

```
read_csv("1,2,3\n4,5,6", col_names = FALSE)
#> # A tibble: 2 x 3
#>      X1    X2    X3
#>   <dbl> <dbl> <dbl>
#> 1     1     2     3
```

通常需要调整的另一个选项是na：这指定了用于表示文件中缺失值的一个或多个值：

```
read_csv("a,b,c\n1,2,.", na = ".")
#> # A tibble: 1 x 3
#>       a     b c    
#>   <dbl> <dbl> <lgl>
#> 1     1     2 NA
```

### parse_\*() functions

|                                                                      |                                     |
| -------------------------------------------------------------------- | ----------------------------------- |
| [parse_atomic](https://rdrr.io/cran/readr/man/parse_atomic.html)     | Parse logicals, integers, and reals |
| [parse_datetime](https://rdrr.io/cran/readr/man/parse_datetime.html) | Parse date/times                    |
| [parse_factor](https://rdrr.io/cran/readr/man/parse_factor.html)     | Parse factors                       |
| [parse_guess](https://rdrr.io/cran/readr/man/parse_guess.html)       | Parse using the "best" type         |
| [parse_number](https://rdrr.io/cran/readr/man/parse_number.html)     | Parse numbers, flexibly             |
| [parse_vector](https://rdrr.io/cran/readr/man/parse_vector.html)     | Parse a character vector.           |

```
parse_vector(
  x,
  collector,
  na = c("", "NA"),
  locale = default_locale(),
  trim_ws = TRUE
)
```

```
str(parse_logical(c("TRUE", "FALSE", "NA")))
#>  logi [1:3] TRUE FALSE NA
str(parse_integer(c("1", "2", "3")))
#>  int [1:3] 1 2 3
str(parse_date(c("2010-01-01", "1979-10-14")))
#>  Date[1:2], format: "2010-01-01" "1979-10-14"
```

第一个参数是要解析的字符向量，而na参数指定应将哪些字符串视为丢失

```
parse_integer(c("1", "231", ".", "456"), na = ".")
#> [1]   1 231  NA 456
```

parse\_~~n~~umber（）忽略了数字前后的非数字字符。 这对于货币和百分比特别有用，但也可以提取文本中嵌入的数字。

```
parse_number("$100")
#> [1] 100
parse_number("20%")
#> [1] 20
parse_number("It cost $123.45")
#> [1] 123.45


parse_number("123'456'789", 
             locale = locale(grouping_mark = ","))
#> [1] 123

parse_number("123'456'789", 
             locale = locale(grouping_mark = "'"))
#> [1] 123456789
```

charToRaw（）获得字符串的基础表示形式：从十六进制数字到字符的映射称为编码，在这种情况下，编码称为ASCII。 ASCII在表示英文字符方面做得很好，因为它是美国信息交换标准代码。幸运的是，今天有几乎所有地方都支持的一种标准：UTF-8。 UTF-8可以编码当今人类使用的几乎每个字符，以及许多其他符号（如表情符号！）。readr在所有地方都使用UTF-8：它假定您的数据在读取时是UTF-8编码的，并且在写入时始终使用。这是一个很好的默认设置.

```
charToRaw("Zehui Bai")
[1] 5a 65 68 75 69 20 42 61 69
```

### Writing to a file

readr also comes with two useful functions for writing data back to disk: `write_csv()` and `write_tsv()`. Both functions increase the chances of the output file being read back in correctly by:

* Always encoding strings in UTF-8.
* Saving dates and date-times in ISO8601 format so they are easily parsed elsewhere.

```
write_csv(challenge, "challenge-2.csv")
read_csv("challenge-2.csv")
```

```
write_rds(challenge, "challenge.rds")
read_rds("challenge.rds")
```

### Other types of data

To get other types of data into R, we recommend starting with the tidyverse packages listed below. They’re certainly not perfect, but they are a good place to start. For rectangular data:

* **haven** reads SPSS, Stata, and SAS files.
* **readxl** reads excel files (both `.xls` and `.xlsx`).
* **DBI**, along with a database specific backend (e.g. **RMySQL**, **RSQLite**, **RPostgreSQL** etc) allows you to run SQL queries against a database and return a data frame.
