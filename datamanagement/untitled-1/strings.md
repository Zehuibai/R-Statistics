---
description: Simple, Consistent Wrappers for Common String Operations
---

# Strings

## Stringr 

### String basics

To include a literal single or double quote in a string you can use `\` to “escape” it:

```
double_quote <- "\"" # or '"'
single_quote <- '\'' # or "'"
```

字符串的打印表示形式与字符串本身并不相同，因为打印的表示形式会显示转义符。 要查看字符串的原始内容，请使用 [`writeLines()`](https://rdrr.io/r/base/writeLines.html):

```
x <- c("\"", "\\")
x
#> [1] "\"" "\\"
writeLines(x)
#> "
#> \
```

其他特殊字符。 最常见的是“  n”（换行符）和“  t”（制表符），但是您可以通过请求帮助来查看完整列表。

```
?"'"

x <- "\u00b5"
x
#> [1] "µ"
```

### Package stringr Function

|                                                                        |                                                         |
| ---------------------------------------------------------------------- | ------------------------------------------------------- |
| [case](https://rdrr.io/cran/stringr/man/case.html)                     | Convert case of a string.                               |
| [invert_match](https://rdrr.io/cran/stringr/man/invert_match.html)     | Switch location of matches to location of non-matches.  |
| [modifiers](https://rdrr.io/cran/stringr/man/modifiers.html)           | 使用修饰符功能控制匹配行为。                                          |
| [str_c](https://rdrr.io/cran/stringr/man/str_c.html)                   | 将多个字符串连接成一个字符串。                                         |
| [str_conv](https://rdrr.io/cran/stringr/man/str_conv.html)             | 指定字符串的编码。                                               |
| [str_count](https://rdrr.io/cran/stringr/man/str_count.html)           | 计算字符串中的匹配数。                                             |
| [str_detect](https://rdrr.io/cran/stringr/man/str_detect.html)         | 检测字符串中是否存在模式。                                           |
| [str_dup](https://rdrr.io/cran/stringr/man/str_dup.html)               | <p>Duplicate and concatenate </p><p>在字符向量内复制和连接字符串。</p> |
| [str_extract](https://rdrr.io/cran/stringr/man/str_extract.html)       | 从字符串中提取匹配的模式                                            |
| [str_flatten](https://rdrr.io/cran/stringr/man/str_flatten.html)       | 展平字符串                                                   |
| [str_glue](https://rdrr.io/cran/stringr/man/str_glue.html)             | Format and interpolate a string with glue               |
| [str_interp](https://rdrr.io/cran/stringr/man/str_interp.html)         | 字符串插值。                                                  |
| [str_length](https://rdrr.io/cran/stringr/man/str_length.html)         | length of a string.                                     |
| [str_locate](https://rdrr.io/cran/stringr/man/str_locate.html)         | 在字符串中找到模式的位置                                            |
| [str_match](https://rdrr.io/cran/stringr/man/str_match.html)           | 从字符串中提取匹配的组。                                            |
| [str_order](https://rdrr.io/cran/stringr/man/str_order.html)           | Order or sort a character vector.                       |
| [str_remove](https://rdrr.io/cran/stringr/man/str_remove.html)         | Remove matched patterns in a string.                    |
| [str_replace](https://rdrr.io/cran/stringr/man/str_replace.html)       | Replace matched patterns in a string.                   |
| [str_replace_na](https://rdrr.io/cran/stringr/man/str_replace_na.html) | Turn NA into "NA"                                       |
| [str_split](https://rdrr.io/cran/stringr/man/str_split.html)           | 将一串切成小块。                                                |
| [str_starts](https://rdrr.io/cran/stringr/man/str_starts.html)         | 在开始时检测模式的存在或不存在...                                      |
| [str_sub](https://rdrr.io/cran/stringr/man/str_sub.html)               | 从字符向量中提取并替换子字符串。                                        |
| [str_subset](https://rdrr.io/cran/stringr/man/str_subset.html)         | 保持字符串与模式匹配，或找到位置。                                       |
| [str_trim](https://rdrr.io/cran/stringr/man/str_trim.html)             | 从字符串修剪空格                                                |
| [str_trunc](https://rdrr.io/cran/stringr/man/str_trunc.html)           | 截断字符串。                                                  |
| [str_wrap](https://rdrr.io/cran/stringr/man/str_wrap.html)             | 将字符串包装成格式正确的段落                                          |
| [word](https://rdrr.io/cran/stringr/man/word.html)                     | 从句子中提取单词。                                               |

### String length

```
str_length(c("a", "R for data science", NA))
#> [1]  1 18 NA
```

### Combining strings

```
str_c("x", "y", "z")
#> [1] "xyz"

str_c("x", "y", sep = ", ")
#> [1] "x, y"

x <- c("abc", NA)
str_c("|-", x, "-|")
#> [1] "|-abc-|" NA

str_c("|-", str_replace_na(x), "-|")
#> [1] "|-abc-|" "|-NA-|"

str_c("prefix-", c("a", "b", "c"), "-suffix")
#> [1] "prefix-a-suffix" "prefix-b-suffix" "prefix-c-suffix"

## 要将向量字符串折叠为单个字符串use collapse:
str_c(c("x", "y", "z"), collapse = ", ")
#> [1] "x, y, z"
```

### Subsetting strings

 `str_sub()` takes `start` and `end` arguments which give the (inclusive) position of the substring:

*  `str_to_lower()` to change the text to lower case
*  can also use `str_to_upper()` or `str_to_title()`

```
x <- c("Apple", "Banana", "Pear")
str_sub(x, 1, 3)
#> [1] "App" "Ban" "Pea"

## negative numbers count backwards from end
str_sub(x, -3, -1)
#> [1] "ple" "ana" "ear"

## 使用赋值形式 assignment 来修改字符串：
str_sub(x, 1, 1) <- str_to_lower(str_sub(x, 1, 1))
x
#> [1] "apple"  "banana" "pear"
```

Locale 指定语言环境

```
x <- c("apple", "eggplant", "banana")

str_sort(x, locale = "en")  # English
#> [1] "apple"    "banana"   "eggplant"

str_sort(x, locale = "haw") # Hawaiian
#> [1] "apple"    "eggplant" "banana"
```

## Matching patterns with regular expressions

### Html str_view

```
x <- c("apple", "banana", "pear")
str_view(x, "an")

## 匹配任何字符（换行符除外）
str_view(x, ".a.")

## 创建正则表达式
dot <- "\\."
writeLines(dot)
#> \.

str_view(c("abc", "a.c", "bef"), "a\\.c")

x <- "a\\b"
writeLines(x)
#> a\b

str_view(x, "\\\\")
```

### Anchors

* [`^`](https://rdrr.io/r/base/Arithmetic.html) to match the start of the string.
* [`$`](https://rdrr.io/r/base/Extract.html) to match the end of the string.

```
x <- c("apple", "banana", "pear")
str_view(x, "^a")   ## apple
str_view(x, "a$")   ## banana

x <- c("apple pie", "apple", "apple cake")
str_view(x, "apple")    ## All
str_view(x, "^apple$")  ## apple
```

* `\d`: matches any digit.
* `\s`: matches any whitespace (e.g. space, tab, newline).
* `[abc]`: matches a, b, or c.
* `[^abc]`: matches anything except a, b, or c.

```
# Look for a literal character that normally has special meaning in a regex
str_view(c("abc", "a.c", "a*c", "a c"), "a[.]c")
## "a.c"

str_view(c("abc", "a.c", "a*c", "a c"), ".[*]c")
## "a*c"

str_view(c("grey", "gray"), "gr(e|a)y")
## "grey", "gray"
```

### Repetition

* [`?`](https://rdrr.io/r/utils/Question.html): 0 or 1
* [`+`](https://rdrr.io/r/base/Arithmetic.html): 1 or more
* [`*`](https://rdrr.io/r/base/Arithmetic.html): 0 or more

```
x <- "1888 is the longest year in Roman numerals: MDCCCLXXXVIII"
str_view(x, "CC?")
## CC

str_view(x, "CC+")
## CCC

x <- "1888 is the longest year in Roman numerals: MDCCCLLXXXVIII"
str_view(x, 'C[LX]+')
## CLLXXX
```

can also specify the number of matches precisely:

* `{n}`: exactly n
* `{n,}`: n or more
* `{,m}`: at most m
* `{n,m}`: between n and m

```
str_view(x, "C{2,3}")
## CCC
```

### Grouping and backreferences

括号作为消除复杂表达式歧义的一种方法。 括号还会创建一个编号的捕获组（1、2等）。 捕获组将字符串的一部分与正则表达式的一部分匹配，存储在括号内。 您可以引用先前与带有反向引用的捕获组匹配的文本，例如 1， 2等

```
## 查找所有具有重复字母对的水果
str_view(fruit, "(..)\\1", match = TRUE)

banana
coconut
cucumber
jujube
papaya
salal berry

str_view(fruit, "(.)(.)\\2\\1", match = TRUE)

## eppe
bell pepper
chili pepper

str_view(fruit, "(.).\\1.\\1", match = TRUE)

banana
papaya
```

### Detect matches

```
x <- c("apple", "banana", "pear")
str_detect(x, "e")
#> [1]  TRUE FALSE  TRUE
```

在数字上下文中使用逻辑向量时，FALSE变为0，TRUE变为1。如果您想回答有关较大向量上的匹配问题 [`sum()`](https://rdrr.io/r/base/sum.html) and [`mean()`](https://rdrr.io/r/base/mean.html) useful

```
# How many common words start with t?
sum(str_detect(words, "^t"))
#> [1] 65

# What proportion of common words end with a vowel?
mean(str_detect(words, "[aeiou]$"))
#> [1] 0.2765306
```

```
＃找到所有至少包含一个元音 vowel 的单词，然后取反 negate
no_vowels_1 <- !str_detect(words, "[aeiou]")

＃查找仅由辅音consonants （非元音）组成的所有单词
no_vowels_2 <- str_detect(words, "^[^aeiou]+$")

identical(no_vowels_1, no_vowels_2)
#> [1] TRUE
```

选择与模式匹配的元素

```
words[str_detect(words, "x$")]
#> [1] "box" "sex" "six" "tax"
str_subset(words, "x$")
#> [1] "box" "sex" "six" "tax"

# Using tibble
df <- tibble(
  word = words, 
  i = seq_along(word)
)
df %>% 
  filter(str_detect(word, "x$"))
#> # A tibble: 4 x 2
#>   word      i
#>   <chr> <int>
#> 1 box     108
#> 2 sex     747
#> 3 six     772
#> 4 tax     841
```

字符串中有多少个匹配项：

```
x <- c("apple", "banana", "pear")
str_count(x, "a")
#> [1] 1 3 1

df %>% 
  mutate(
    vowels = str_count(word, "[aeiou]"),
    consonants = str_count(word, "[^aeiou]")
  )
#> # A tibble: 980 x 4
#>   word         i vowels consonants
#>   <chr>    <int>  <int>      <int>
#> 1 a            1      1          0
#> 2 able         2      2          2
#> 3 about        3      3          2
---
```

### Extract matches

```
length(sentences)
#> [1] 720
head(sentences)
#> [1] "The birch canoe slid on the smooth planks." 
#> [2] "Glue the sheet to the dark blue background."
#> [3] "It's easy to tell the depth of a well."     
#> [4] "These days a chicken leg is a rare dish."   
#> [5] "Rice is often served in round bowls."       
#> [6] "The juice of lemons makes fine punch."

colours <- c("red", "orange", "yellow", "green", "blue", "purple")
colour_match <- str_c(colours, collapse = "|")
colour_match
#> [1] "red|orange|yellow|green|blue|purple"

has_colour <- str_subset(sentences, colour_match)
head(has_colour)
[1] "Glue the sheet to the dark blue background." 
[2] "Two blue fish swam in the tank."            
[3] "The colt reared and threw the tall rider." 

## 第一个匹配项
matches <- str_extract(has_colour, colour_match)
head(matches)
[1] "blue"   "blue"   "red"   ...

## 选择所有具有多个匹配项的句子
sentences[str_count(sentences, colour_match) > 1]
str_view_all(more, colour_match)
```

如果您使用simpleify = TRUE，则str_extract_all（）将返回一个矩阵，其中短匹配扩展为与最长匹配的长度相同：

```
x <- c("a", "a b", "a b c")
str_extract_all(x, "[a-z]", simplify = TRUE)
#>      [,1] [,2] [,3]
#> [1,] "a"  ""   ""  
#> [2,] "a"  "b"  ""  
#> [3,] "a"  "b"  "c"
```

### Replacing matches

`str_replace()` and `str_replace_all()` allow you to replace matches with new strings. The simplest use is to replace a pattern with a fixed string:允许您将匹配项替换为新字符串

```
x <- c("apple", "pear", "banana")
str_replace(x, "[aeiou]", "-")
#> [1] "-pple"  "p-ar"   "b-nana"
str_replace_all(x, "[aeiou]", "-")
#> [1] "-ppl-"  "p--r"   "b-n-n-"

x <- c("1 house", "2 cars", "3 people")
str_replace_all(x, c("1" = "one", "2" = "two", "3" = "three"))
#> [1] "one house"    "two cars"     "three people"


```

### Splitting

Use `str_split()` to split a string up into pieces. For example, we could split sentences into words:

```
sentences %>%
  head(5) %>% 
  str_split(" ")
#> [[1]]
#> [1] "The"     "birch"   "canoe"   "slid"    "on"      "the"     "smooth" 
#> [8] "planks."
---

"a|b|c|d" %>% 
  str_split("\\|") %>% 
  .[[1]]
#> [1] "a" "b" "c" "d"
```

 use `simplify = TRUE` to return a matrix

```
sentences %>%
  head(5) %>% 
  str_split(" ", simplify = TRUE)
     [,1]    [,2]    [,3]    [,4]      [,5]  [,6]    [,7]     [,8]          [,9]   
[1,] "The"   "birch" "canoe" "slid"    "on"  "the"   "smooth" "planks."     ""     
[2,] "Glue"  "the"   "sheet" "to"      "the" "dark"  "blue"   "background." ""     
[3,] "It's"  "easy"  "to"    "tell"    "the" "depth" "of"     "a"           "well."
[4,] "These" "days"  "a"     "chicken" "leg" "is"    "a"      "rare"        "dish."
[5,] "Rice"  "is"    "often" "served"  "in"  "round" "bowls." ""            ""     
```

