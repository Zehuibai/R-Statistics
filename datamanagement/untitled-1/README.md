---
description: >-
  R语言教程:
  https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/index.html
---

# Data Management

## Basic R Elements

### Set up

|                     |                  |
| ------------------- | ---------------- |
| ls()                | 查看当前工作空间里面的R对象   |
| data()              | 查看当前能访问的数据集列表    |
| getwd()             | 查看设置的工作路径        |
| setwd("\~/Desktop") | 设置工作路径／目录        |
| source()            | 执行r脚本文件          |
| ai$delete <- NULL   | 删除变量赋值为NULL而不是NA |

### Vector

|                                          | 函数                                                                                                                       |
| ---------------------------------------- | ------------------------------------------------------------------------------------------------------------------------ |
|                                          | **规则序列函数**                                                                                                               |
| seq(11, 15, by=2)                        | 等间隔序列                                                                                                                    |
| rep()                                    | 重复数值                                                                                                                     |
| seq_len(length(my_vector))               | Use the length of the vector to create an index                                                                          |
| lead(1:10)                               | 计算滞后值 dplyr                                                                                                              |
| lag(1:10)                                | 计算领先值 dplyr                                                                                                              |
|                                          |                                                                                                                          |
|                                          | **舍入函数**                                                                                                                 |
| ceiling                                  | 不小于x的最小整数                                                                                                                |
| floor                                    | 不大于x的最大整数                                                                                                                |
| round                                    | 将x舍入为指定位的小数                                                                                                              |
| signif                                   | 将x舍入为指定的有效数字位数                                                                                                           |
| trunc                                    | 向 0 的方向截取的x中的整数部分                                                                                                        |
|                                          |                                                                                                                          |
| format( )                                | <p>digits：数字x和复数x使用多少个有效数字。默认值为NULL</p><p>nsmall：以非科学格式格式化实数/复数时，小数点右边的最小位数。</p><p>justify：字符向量应左对齐（默认），右对齐，居中还是左对齐。</p> |
|                                          |                                                                                                                          |
|                                          | **计算函数**                                                                                                                 |
| %%                                       | 求余                                                                                                                       |
| %/%                                      | 整除 5 %/% 3                                                                                                               |
| complex()                                | <p>复数向量, 指定实部和虚部。 <br>complex(real = c(1,0,-1,0), imaginary = c(0,1,0,-1))<br>相当于c(1+0i, 1i, -1+0i, -1i)。</p>            |
| Re(z)                                    | 求z的实部                                                                                                                    |
| Im(z)                                    | 求z的虚部                                                                                                                    |
| Mod(z) 或 abs(z)                          | 求z的模                                                                                                                     |
| Arg(z)                                   | 求z的辐角                                                                                                                    |
| Conj(z)                                  | 求z的共轭                                                                                                                    |
|                                          |                                                                                                                          |
| sqrt                                     | 平方根函数                                                                                                                    |
| <p>log10,<br>log(x,base=n)</p>           | 对数函数                                                                                                                     |
| sign                                     | 符号函数                                                                                                                     |
| asin, acos, atan, atan2                  | 反三角函数                                                                                                                    |
| sinh, cosh, tanh                         | 双曲函数                                                                                                                     |
| polyroot, uniroot                        | 求根相对频率相对频率                                                                                                               |
|                                          |                                                                                                                          |
|                                          | **排序函数**                                                                                                                 |
| sort(x)                                  | sort(x)返回排序结果                                                                                                            |
| rev(x)                                   | rev(x)返回把各元素排列次序反转后的结果                                                                                                   |
| order(x)                                 | order(x)返回排序用的下标                                                                                                         |
| <p>min_rank(y) <br>min_rank(desc(y))</p> | ranking functions dplyr                                                                                                  |
|                                          |                                                                                                                          |
|                                          | **统计函数**                                                                                                                 |
| range                                    | <p>求最小值和最大值</p><p>sum(求和), mean(求平均值), var(求样本方差),<br>sd(求样本标准差), min(求最小值), max(求最大值)</p>                               |
| prod                                     | 所有元素的乘积                                                                                                                  |
| cumsum/cumprod                           | 计算累加和累乘积                                                                                                                 |
| prop.table                               | 相对频率                                                                                                                     |
| pmax                                     | 两者最大值                                                                                                                    |
| fivenum                                  | Min, Q1, Median, Q3, Max                                                                                                 |
|                                          |                                                                                                                          |
| all(cond)                                | 测试cond的所有元素为真                                                                                                            |
| any(cond)                                | <p>测试cond至少一个元素为真</p><p>函数which()返回真值对应的所有下标</p>                                                                         |
| identical(x,y)                           | <p>比较两个R对象x与y的内容是否完全相同<br> 结果只会取标量TRUE与FALSE两种</p>                                                                       |
| all.equal()                              | <p>与identical()类似， 但是在比较数值型时不区分整数型与实数型，<br>而且相同时返回标量TRUE，<br>不同时会返回一个说明有何不同的字符串</p>                                      |

###

| <p><br></p>                                              | 0 | Inf | NA | NaN |
| -------------------------------------------------------- | - | --- | -- | --- |
| [`is.finite()`](https://rdrr.io/r/base/is.finite.html)   | x |     |    |     |
| [`is.infinite()`](https://rdrr.io/r/base/is.finite.html) |   | x   |    |     |
| [`is.na()`](https://rdrr.io/r/base/NA.html)              |   |     | x  | x   |
| [`is.nan()`](https://rdrr.io/r/base/is.finite.html)      |   |     |    | x   |

### Matrix

#### Building Matrix

```
X <- matrix(c(1,2,3, 10,20,30),
            nrow = 3, ncol=2, byrow=FALSE,
            dimnames = list(c("row1", "row2", "row3"),
                            c("col1", "col2")))
## byrow表示数据给出的值是要按列填充(缺省值)还是按行填充(如果为TRUE)。
## 可以通过选项dimnames给行列命名

colnames(X) <- c("col1", "col2")
rownames(X) <- c("row1", "row2", "row3")
dimnames(X) <- list(c("row1", "row2", "row3"), c("col1", "col2"))
```

#### Calculation

|                      | matrix<-matrix(1:16,nrow = 4)                                                                              |
| -------------------- | ---------------------------------------------------------------------------------------------------------- |
| 取矩阵的上、下三角部分          | <p>lower.tri(matrix)</p><p></p><p>matrix[lower.tri(matrix)]=0</p>                                          |
| 单位矩阵 Identity matrix | diag(3)                                                                                                    |
| 取对角元素                | matrix(c(1:16),4,4); diag(M_diag)                                                                          |
| 转置                   | t(matrix)                                                                                                  |
| 行列式 Determinant      | det(matrix)                                                                                                |
| 乘法 Multiplication    | A %\*% B                                                                                                   |
| 对应位置元素做乘法            | A \* B                                                                                                     |
| Product              | 2\*B                                                                                                       |
| X^{-1}求逆             | solve(B)                                                                                                   |
| t(A) %\*% A          | crossprod(A, A)                                                                                            |
| 求解线性方程               | <p>A1 &#x3C;- array(c(1:8,10)<br>dim = c(3,3)) <br>A2 &#x3C;- t(A1) b &#x3C;- c(1,1,1) <br>solve(A2,b)</p> |



###



### Characters

| Functions                            |                                                                                                                                                                              |
| ------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| nchar()                              | <p>获取字符串长度</p><p>也支持字符串向量操作, 和length()的结果有区别</p>                                                                                                                             |
| strsplit()                           | <p>字符串分割<br></p><p>fruit &#x3C;- 'apple orange grape banana'</p><p>strsplit(fruit,split=' ')  ## list结构</p><p>fruitvec &#x3C;- unlist(strsplit(fruit,split=' ')) ##转化为向量</p> |
| paste()                              | <p>字符串拼接</p><p></p><p>paste(fruitvec,collapse=',') #逗号作为分隔符</p>                                                                                                              |
| substr()                             | <p>字符串截取</p><p>对给定的字符串对象取出子集，其参数是子集所处的起始和终止位置</p>                                                                                                                            |
| <p>gsub() <br>chartr() <br>sub()</p> | <p>字符串替代</p><p>chartr是字母替换，不是字符串替换。 <br>gsub()负责搜索字符串的特定表达式，并用新的内容加以替代。 <br>sub()函数类似gsub()，但只替代第一个。</p>                                                                     |
| grep()                               | <p>字符串匹配</p><p>grep('grape',fruitvec) #返回grape在fruitvec中的位置</p>                                                                                                              |
| <p>toupper() <br>tolower()</p>       | 大小写替换                                                                                                                                                                        |
| abbreviate()                         | 缩写                                                                                                                                                                           |

### List

|            |                                                                              |
| ---------- | ---------------------------------------------------------------------------- |
| as.list()  | 把一个其它类型的对象转换成列表                                                              |
| unlist()   | 把列表转换成基本向量                                                                   |
| strsplit() | <p>输入一个字符型向量并指定一个分隔符<br>返回一个项数与字符型向量元素个数相同的列表<br> 列表每项对应于字符型向量中一个元素的拆分结果</p> |
| sapply()   | <p>为了把拆分结果进一步转换成一个数值型矩阵</p><p>t(sapply(res, as.numeric))</p>                 |



The `...` syntax of base R allows you to:

* **Forward** arguments from function to function, matching them along the way to function parameters.
* **Collect** arguments inside data structures, e.g. with [`c()`](http://search.r-project.org/library/base/html/c.html) or [`list()`](http://search.r-project.org/library/base/html/list.html).



