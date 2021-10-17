# Date lubridate

## Baisc

as.Date()用于执行这种转化。其语法为as.Date(x, “input_format”)，其中x是字符型数 据，input_format则给出了用于读入日期的适当格式

Sys.Date()可以返回当天的日期，而date() 则返回当前的日期和时间

函数format(x, format=“output_format”)来输出指定格式的日期值，并且则使用mm/dd/yyyy的格式读取数据。

函数difftime()来计算时间间隔，并以星期、天、时、分、秒来表示

```
# Converting character values to dates
strDates <- c("01/05/1965", "08/16/1975")
dates <- as.Date(strDates, "%m/%d/%Y")
dates
## [1] "1965-01-05" "1975-08-16"
```

|            |                                                                                                                      |
| ---------- | -------------------------------------------------------------------------------------------------------------------- |
| Sys.Date() | "2021-02-04"                                                                                                         |
| date()     | "Thu Feb 04 11:36:46 2021"                                                                                           |
| format     | <p>Date functions and formatted printing <br><br>today &#x3C;- Sys.Date() <br>format(today, format = "%B %d %Y")</p> |

## Package lubridate

R扩展包lubridate提供了多个方便函数， 可以更容易地生成、转换、管理日期型和日期时间型数据。

* 函数lubridate::today()返回当前日期：
* 函数lubridate::now()返回当前日期时间：结果显示中出现的CST是\* 时区， 这里使用了操作系统提供的当前时区
* 函数lubridate::ymd(), lubridate::mdy(), lubridate::dmy()将字符型向量转换为日期型向量
* 函数lubridate::make_date(year, month, day) 构成日期向量
* 函数lubridate::make_datetime(year, month, day, hour, min, sec) 可以从最多六个数值组成日期时间， 其中时分秒缺省值都是0
* 函数lubridate::as_date()可以将日期时间型转换为日期型
* 函数lubridate::as_datetime()可以将日期型数据转换为日期时间型



```
as_datetime(as.Date("1998-03-16"))
```

```
## [1] "1998-03-16 UTC"
```

```
## lubridate的这些成分函数还允许被赋值， 结果就修改了相应元素的值，如
x <- as.POSIXct("2018-1-17 13:15:40")
year(x) <- 2000
month(x) <- 1
mday(x) <- 1
x
```

```
## [1] "2000-01-01 13:15:40 CET"
```

```
## update()函数中可以用year, month, mday, hour, minute, second等参数修改日期的组成部分
x <- as.POSIXct("2018-1-17 13:15:40")
y <- update(x, year=2000)
y
```

```
## [1] "2000-01-17 13:15:40 CET"
```

lubridate包提供了floor_date(), round_date(), ceiling_date()等函数， 对日期可以用unit=指定一个时间单位进行舍入。 时间单位为字符串， 如seconds, 5 seconds, minutes, 2 minutes, hours, days, weeks, months, years等。

比如，以10 minutes为单位， floor_date()将时间向前归一化到10分钟的整数倍， ceiling_date()将时间向后归一化到10分钟的整数倍， round_date()将时间归一化到最近的10分钟的整数倍， 时间恰好是5分钟倍数时按照类似四舍五入的原则向上取整。 例如

```
x <- ymd_hms("2018-01-11 08:32:44")
floor_date(x, unit="10 minutes")
```

```
## [1] "2018-01-11 08:30:00 UTC"
```

日期计算 在lubridate的支持下日期可以相减， 可以进行加法、除法。 lubridate包提供了如下的三种与时间长短有关的数据类型：

```
时间长度(duration)，按整秒计算
时间周期(period)，如日、周
时间区间(interval)，包括一个开始时间和一个结束时间
```
