# Basic Markdown

## Basic Markdown

### Chuks output option&#x20;

The following table summarises which types of output each option suppresses:

| Option              | Run code | Show code | Output | Plots | Messages | Warnings |
| ------------------- | -------- | --------- | ------ | ----- | -------- | -------- |
| `eval = FALSE`      | -        |           | -      | -     | -        | -        |
| `include = FALSE`   |          | -         | -      | -     | -        | -        |
| `echo = FALSE`      |          | -         |        |       |          |          |
| `results = "hide"`  |          |           | -      |       |          |          |
| `fig.show = "hide"` |          |           |        | -     |          |          |
| `message = FALSE`   |          |           |        |       | -        |          |
| `warning = FALSE`   |          |           |        |       |          | -        |



### Not certain heading number

If you do not want a **certain** heading to be numbered, you can add `{-}` or `{.unnumbered}` after the heading, e.g.,

```
# Preface {-}
```

### Quoting text

```
> "I thoroughly disapprove of duels. If a man should challenge me,
  I would take him kindly and forgivingly by the hand and lead him
  to a quiet place and kill him."
>
> --- Mark Twain
```

> "I thoroughly disapprove of duels. If a man should challenge me, I would take him kindly and forgivingly by the hand and lead him to a quiet place and kill him."
>
> \--- Mark Twain

### Links

```
This site was built using [GitHub Pages](https://pages.github.com/).
```

This site was built using [GitHub Pages](https://pages.github.com/).

### Relative links

You can define relative links and image paths in your rendered files to help readers navigate to other files in your repository. A relative link is a link that is relative to the current file. For example, if you have a README file in root of your repository, and you have another file in docs/CONTRIBUTING.md, the relative link to CONTRIBUTING.md in your README might look like 在渲染文件中定义相对链接和图像路径，以帮助读者导航到存储库中的其他文件。相对链接是相对于当前文件的链接。 例如，如果您的存储库根目录中有一个README文件，而在docs / CONTRIBUTING.md中有另一个文件，则自述文件中指向CONTRIBUTING.md的相对链接可能如下所示

```
[Contribution guidelines for this project](docs/CONTRIBUTING.md)
```

### Task Lists

```
- [x] Finish my changes
- [ ] Push my commits to GitHub
- [ ] Open a pull request
```

* [x] Finish my changes
* [ ] Push my commits to GitHub
* [ ] Open a pull request

```
- [ ] \(Optional) Open a followup issue
```

* [ ] (Optional) Open a followup issue

## Table

[https://www.tablesgenerator.com/markdown\_tables](https://www.tablesgenerator.com/markdown\_tables)

### Content Align

Align left

```
| Time                   | Plan                                                 |
|------------------------|------------------------------------------------------|
| Mid of May             | Plan and carry out IDEFICS analysis                  |
```

Align center

```
|          Time          |                         Plan                         |
|:----------------------:|:----------------------------------------------------:|
|       Mid of May       |          Plan and carry out IDEFICS analysis         |
```

Align right

```
|                   Time |                                                 Plan |
|-----------------------:|-----------------------------------------------------:|
|             Mid of May |                  Plan and carry out IDEFICS analysis |
```

### Caption Alignment

| Align           |                       |
| --------------- | --------------------- |
| >{\raggedright} | Left-aligned column   |
| >{\centering}   | Center-aligned column |
| >{\raggedleft}  | Right-aligned column  |

###

### Package Pander

R扩展包pander提供了更好的表格能力， 也能与knitr包很好的合作输出。 其pander()函数可以将多种R输出格式转换成knitr需要的表格形式。

```
# pander
library(memisc)
library(pander)
lm1_table <- mtable(lm1)
pander(lm1)
```

### Package Stargazer

#### Descriptive analysis of the data set

Source: [https://cran.r-project.org/web/packages/stargazer/vignettes/stargazer.pdf](https://cran.r-project.org/web/packages/stargazer/vignettes/stargazer.pdf)

```
library(stargazer)
data("attitude")
# 展示数据集的描述性分析
stargazer(attitude,
          type = "latex", summary = NULL, header = F)  
          
## 
## \begin{table}[!htbp] \centering 
##   \caption{} 
##   \label{} 
## \begin{tabular}{@{\extracolsep{5pt}}lccccccc} 
## \\[-1.8ex]\hline 
## \hline \\[-1.8ex] 
## Statistic & \multicolumn{1}{c}{N} & \multicolumn{1}{c}{Mean} & \multicolumn{1}{c}{St. Dev.} & \multicolumn{1}{c}{Min} & \multicolumn{1}{c}{Pctl(25)} & \multicolumn{1}{c}{Pctl(75)} & \multicolumn{1}{c}{Max} \\ 
## \hline \\[-1.8ex] 
## rating & 30 & 64.633 & 12.173 & 40 & 58.8 & 71.8 & 85 \\ 
## complaints & 30 & 66.600 & 13.315 & 37 & 58.5 & 77 & 90 \\ 
## privileges & 30 & 53.133 & 12.235 & 30 & 45 & 62.5 & 83 \\ 
## learning & 30 & 56.367 & 11.737 & 34 & 47 & 66.8 & 75 \\ 
## raises & 30 & 64.633 & 10.397 & 43 & 58.2 & 71 & 88 \\ 
## critical & 30 & 74.767 & 9.895 & 49 & 69.2 & 80 & 92 \\ 
## advance & 30 & 42.933 & 10.289 & 25 & 35 & 47.8 & 72 \\ 
## \hline \\[-1.8ex] 
## \end{tabular} 
## \end{table}
```

```
# 展示数据的前四行 summary = FALSE
stargazer(attitude[1:4,],
          type = "latex", summary = FALSE, rownames=F) 
```

#### linear mode

```
# 构建线性和Probit模型
linear.1 <- lm(rating ~ complaints + privileges + learning + raises + critical,data=attitude)
linear.2 <- lm(rating ~ complaints + privileges + learning, data=attitude)
## create binary variable
attitude$high.rating <- (attitude$rating > 70)
probit.model <- glm(high.rating ~ learning + critical + advance, 
                    data=attitude,
                    family = binomial(link = "probit"))

# 展示模型结果
stargazer(probit.model, title="Results",align=TRUE)
```

### Package Xtable

```
AG_1 <- data.frame(Variable <- c("Intercept","Weinkonsum","Rauchen","Weinkonsum × Rauchen"),
                   Schätzer <- c(-3.3,-0.3,0.6,1.2),
                   OddsRatio <- c(NA,0.74,1.82,3.32))
names(AG_1) <- c("Variable","Schätzer","Odds Ratio")
AG_1.table <- xtable(AG_1)
print(AG_1.table)
```

```
## % latex table generated in R 4.0.0 by xtable 1.8-4 package
## % Mon Jun 29 15:17:42 2020
## \begin{table}[ht]
## \centering
## \begin{tabular}{rlrr}
##   \hline
##  & Variable & Schätzer & Odds Ratio \\ 
##   \hline
## 1 & Intercept & -3.30 &  \\ 
##   2 & Weinkonsum & -0.30 & 0.74 \\ 
##   3 & Rauchen & 0.60 & 1.82 \\ 
##   4 & Weinkonsum × Rauchen & 1.20 & 3.32 \\ 
##    \hline
## \end{tabular}
## \end{table}
```

### Package Texreg

```
## html
library(texreg)
htmlreg(lm1, custom.note="%stars. htmlreg")

## pdf/latex
texreg::texreg(list(lm1), custom.note="%stars. texreg")
```

## Figure

#### 1 figure

````
```{r echo=FALSE,fig.align="center", out.width = '65%',fig.cap="Figure: Here is a really important caption."}
knitr::include_graphics("./Fotos/Organigramm")
```
````

#### 2 figures in 2 columns

````
```{r out.width=c('50%', '50%'), fig.show='hold'}
boxplot(1:10)
plot(rnorm(10))
```
````





##
