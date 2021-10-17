---
description: Summary of Regression Models HTML Package sjPlot
---

# Models Summary sjPlot

## Summary table in SAS

```
proc format;           
	picture stderrf (round)       
     	low-high=' 9.99)' (prefix='(')                                
            .=' ';                                                  
run;

ods output ParameterEstimates (persist) = t;                        
                                                                    
proc reg data = "d:datahsb2" ;                                              
  model write = female;                                             
  model write = female read;                                        
  model write = female read math;                                   
  model write = female read math science;                           
  model write = female read math science socst;                     
run;
ods output close;     

proc tabulate data=t noseps;      
  class model variable;                      
  var estimate stderr;     
  table variable=''*(estimate =' '*sum=' '                          
                     stderr=' '*sum=' '*F=stderrf.),                
         model=' '                                                  
          / box=[label="Parameter"] rts=15 row=float misstext=' '; 
run;         
```

$$
\begin{array}{|l|r|r|r|r|r|}
\hline \text { Parameter } & \text { MODEL1 } & \text { MODEL2 } & \text { MODEL3 } & \text { MODEL4 } & \text { MODEL5 } \\
\hline \text { FEMALE } & 4.87 & 5.49 & 5.44 & 5.94 & 5.49 \\
& (1.30) & (1.01) & (0.93) & (0.91) & (0.88) \\
\text { Intercept } & 50.12 & 20.23 & 11.90 & 8.58 & 6.14 \\
\text { MATH } & (0.96) & (2.71) & (2.86) & (2.87) & (2.81) \\
& & & 0.40 & 0.29 & 0.24\\
\hline 
\end{array}
$$

## Summary Models as HTML Table

Source: [Summary of Regression Models as HTML Table](https://cran.r-project.org/web/packages/sjPlot/vignettes/tab_model_estimates.html)

tab_model() is the pendant to plot_model(), however, instead of creating plots, tab_model() creates HTML-tables that will be displayed either in your IDE’s viewer-pane, in a web browser or in a knitr-markdown-document (like this vignette).

> tab_model（）是plot_model（）的附属项，但是，tab_model（）不会创建图，而是创建HTML表

### One Model

```
library(sjPlot)
data("efc")
efc <- as_factor(efc, c161sex, c172code)
m1 <- lm(barthtot ~ c160age + c12hour + c161sex + c172code, data = efc)
m2 <- lm(neg_c_7 ~ c160age + c12hour + c161sex + e17age, data = efc)

tab_model(
  m1, show.se = TRUE, show.std = TRUE, show.stat = TRUE,
  col.order = c("p", "stat", "est", "std.se", "se", "std.est")
)     
## tab_model(m1,options)
```

![](<../.gitbook/assets/image (17).png>)

### Options

|                          |                                                         |
| ------------------------ | ------------------------------------------------------- |
| auto.label = FALSE       | Turn off automatic labelling                            |
| transform                | <p>默认情况下使用“ exp”作为适用模型类别的转换</p><p>未转换的估计，请使用NULL</p>    |
| CSS = css_theme("cells") | 定制表格样式                                                  |
| show.intercept           | 截距                                                      |
| show.est                 | 估算值                                                     |
| show.ci                  | 置信区间                                                    |
| show.se                  | 标准错误                                                    |
| show.std                 | 标准化的β系数                                                 |
| show.p                   | p值                                                      |
| show.stat                | 系数的检验统计量                                                |
| show.df                  | <p>p.val =“ kr”，<br>线性混合模型的p值基于df和Kenward-Rogers近似值</p> |
| show.r2                  | r平方值                                                    |
| show.icc                 | 类内相关系数                                                  |
| show.re.var              | 混合模型的随机效果方差                                             |
| show.fstat               | 每个模型的F统计信息                                              |
| show.aic                 | 每个模型的AIC值                                               |
| show.dev                 | 模型的偏差                                                   |
| show.loglik              | 模型的对数似然                                                 |
| show.obs                 | 每个模型的观测数量                                               |
| title                    | 表格标题                                                    |
| collapse.ci              | 置信区间和标准误差的列与估计值合为一栏                                     |

### More Models

```
tab_model(m1,m2, auto.label = FALSE)   ## More than one model
```

![](<../.gitbook/assets/image (16).png>)

### Customize table output

```
tab_model(m1, collapse.ci = TRUE)   
## With collapse.ci and collapse.se, the columns for confidence intervals and standard errors can be collapsed into one column together with the estimates. Sometimes this table layout is required.
```

![](<../.gitbook/assets/image (18).png>)

```
# Defining own labels
tab_model(
  m1, m2, 
  pred.labels = c("Intercept", "Age (Carer)", "Hours per Week", "Gender (Carer)",
                  "Education: middle (Carer)", "Education: high (Carer)", 
                  "Age (Older Person)"),
  dv.labels = c("First Model", "M2"),
  string.pred = "Coeffcient",
  string.ci = "Conf. Int (95%)",
  string.p = "P-Value"
)
```

![](<../.gitbook/assets/image (19).png>)



### Mixed Models

```
library(lme4)
data("sleepstudy")
data("efc")
efc$cluster <- as.factor(efc$e15relat)

## Unlike tables for non-mixed models, tab_models() adds additional 
## information on the random effects to the table output for mixed models. 
m1 <- lmer(neg_c_7 ~ c160age + c161sex + e42dep + (1 | cluster), data = efc)
efc$neg_c_7d <- ifelse(efc$neg_c_7 < median(efc$neg_c_7, na.rm = TRUE), 0, 1)
efc$cluster <- as.factor(efc$e15relat)
m3 <- glmer(
  neg_c_7d ~ c160age + c161sex + e42dep + (1 | cluster),
  data = efc, 
  family = binomial(link = "logit")
)

tab_model(m1, m3)
```

![](<../.gitbook/assets/image (21).png>)

* 边际R平方仅考虑固定效应的方差，而条件R平方则同时考虑固定效应和随机效应。
* p值是基于t统计量并使用正态分布函数的简单近似值。 可以使用p.val =“ kr”计算出更精确的p值。 在这种情况下（仅适用于线性混合模型），p值的计算基于条件F检验，其中自由度采用Kenward-Roger近似\
  tab_model(m1, p.val = "kr", show.df = TRUE)

## Plot regression models

plot_model() creates plots from regression models, either estimates (as so-called forest or dot whisker plots) or marginal effects.

> plot_model（）通过回归模型（估计值（所谓的森林图或点须图）或边际效应）创建图。

plot_model（）允许创建各种绘图类型，可以通过类型参数进行定义。默认值为type =“ fe”，这表示将绘制固定效果（模型系数）。要绘制边际效应，请使用以下命令调用plot_model（）：

* type =“ pred”以绘制特定模型项的预测值（边际效应）。
* type =“ eff”，类似于type =“pred”， 但是，离散预测变量在其比例（而不是参考水平）上保持恒定。它通过内部调用。
* type =“ emm”，类似于type =“ eff”。它通过内部调用。
* type =“ int”绘制交互作用项的边际效应。

### Marginal Effect

```
library(ggplot2)
library(sjPlot)
theme_set(theme_bw())

## data preparation
data(efc)
# create binrary response
y <- ifelse(efc$neg_c_7 < median(na.omit(efc$neg_c_7)), 0, 1)

 
## 绘制特定模型项的预测值（边际效应）
theme_set(theme_sjplot())
fit <- lm(barthtot ~ c12hour + neg_c_7 + c161sex + c172code, data = efc)
plot_model(fit, type = "pred", terms = "c12hour")
```

![](<../.gitbook/assets/plot_zoom_png (25).png>)

```
## Marginal effects for different groups
plot_model(fit, type = "pred", terms = c("c12hour", "c172code"))
```

![](<../.gitbook/assets/plot_zoom_png (26).png>)

```
# x-values and predictions based on exponentiated hp-values
data(mtcars)
mpg_model <- lm(mpg ~ log(hp), data = mtcars)
plot_model(mpg_model, type = "pred", terms = "hp [exp]")
```

### Coefficients

```
## plot coefficients
plot_model(fit, colors = "black")
```

![](<../.gitbook/assets/plot_zoom_png (27).png>)

### 2-ways-interaction

```
data(efc)
theme_set(theme_sjplot())
efc$c161sex <- factor(efc$c161sex)

# fit model with interaction
fit <- lm(neg_c_7 ~ c12hour + barthtot * c161sex, data = efc)
plot_model(fit, type = "pred", terms = c("barthtot", "c161sex"))
## alternatively
plot_model(fit, type = "int")
```

![](<../.gitbook/assets/plot_zoom_png (28).png>)

### 3-way-interaction

```
# fit model with 3-way-interaction
fit <- lm(neg_c_7 ~ c12hour * barthtot * c161sex, data = efc)
# select only levels 30, 50 and 70 from continuous variable Barthel-Index
plot_model(fit, type = "pred", terms = c("c12hour", "barthtot [30,50,70]", "c161sex"))
```

![](<../.gitbook/assets/plot_zoom_png (29).png>)

### Estimates (Fixed Effects)

```
data(efc)
theme_set(theme_sjplot())
## Fitting a logistic regression model
# create binary response
y <- ifelse(efc$neg_c_7 < median(na.omit(efc$neg_c_7)), 0, 1)

# create data frame for fitting model
df <- data.frame(
  y = factor(y),
  sex = factor(efc$c161sex),
  dep = factor(efc$e42dep),
  barthel = efc$barthtot,
  education = factor(efc$c172code)
)
 
# fit model
m1 <- glm(y ~., data = df, family = binomial(link = "logit"))

# Plotting estimates of generalized linear models
plot_model(m1, vline.color = "red")

# Sorting estimates
plot_model(m1, sort.est = TRUE)

```

![](<../.gitbook/assets/image (15).png>)
