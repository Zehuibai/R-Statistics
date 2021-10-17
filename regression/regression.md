# Genaralized linear model

## Genaralised linear model

### Probability distribution:

$$
{\displaystyle f_{Y}(\mathbf {y} \mid {\boldsymbol {\theta }},\tau )=h(\mathbf {y} ,\tau )\exp \left({\frac {\mathbf {b} ({\boldsymbol {\theta }})^{\rm {T}}\mathbf {T} (\mathbf {y} )-A({\boldsymbol {\theta }})}{d(\tau )}}\right).\,\!}
$$

#### Expectation and variance

$$
\begin{aligned}
E(Y) &=\mu=b^{\prime}(\theta) \\
\operatorname{Var}(Y) &=V(\mu) a(\psi)=b^{\prime \prime}(\theta) a(\psi) .
\end{aligned}
$$

当假设为某具体指数族分布时, $$b(\theta)$$ 已知，继而得到 $$b^{\prime}(\theta),$$ 然后由 $$\mu=G(\eta)=b^{\prime}(\theta)$$ 解出 $$\theta$$ 用 $$\theta(\mu)$$ 表示

### Assumptions

**Verteilungsannahme** 

$$
\qquad f\left(y{i} \mid \theta{i}\right)=\exp \left(\frac{y{i} \theta{i}-b\left(\theta{i}\right)}{\phi} \omega{i}-c\left(y{i}, \phi, \omega{i}\right)\right)
$$

abhängiger Dispersionsparameter. Für $$\mathrm{E}\left(y{i} \mid \boldsymbol{x}{i}\right)=\mu{i}$$ _und _$$\operatorname{Var}\left(y{i} \mid \boldsymbol{x}_{i}\right)$$ gilt somit

$$
\mathrm{E}\left(y_{i} \mid \boldsymbol{x}_{i}\right)=\mu_{i}=b^{\prime}\left(\theta_{i}\right), \quad \operatorname{Var}\left(y_{i} \mid \boldsymbol{x}_{i}\right)=\sigma_{i}^{2}=\phi b^{\prime \prime}\left(\theta_{i}\right) / \omega_{i}
$$

**Strukturannahme**

Der (bedingte) Erwartungswert** **$$\mu{i}$$ ist mit dem linearen Prädiktor $$\eta{i}=\boldsymbol{x}{i}^{\prime} \boldsymbol{\beta}=\beta_{0}+\beta_{1} x_{i 1}+\ldots+\beta_{k} x_{i k}$$ durch

$$
\mu_{i}=h\left(\eta_{i}\right)=h\left(\boldsymbol{x}_{i}^{\prime} \boldsymbol{\beta}\right) \quad b z w . \quad \eta_{i}=g\left(\mu_{i}\right)
$$

verknüpft, wobei 

* h eine (eineindeutige und zweimal differenzierbare) Responsefunktion und 
* g die Linkfunktion, d.h. die Umkehrfunktion $$g=h^{-1}$$ von $$h$$ ist.

![Einfache Exponentialfamilien](<../.gitbook/assets/image (12).png>)

### Testen linearer Hypothesen

Für das Testen linearer Hypothesen:

$$
H_{0}: \boldsymbol{C} \boldsymbol{\beta}=\boldsymbol{d} \quad \text { gegen } \quad H_{1}: \boldsymbol{C} \boldsymbol{\beta} \neq \boldsymbol{d}
$$

Teststatistiken 

1. Likelihood-Quotienten-Statistik: $$l q=-2{l(\tilde{\boldsymbol{\beta}})-l(\hat{\boldsymbol{\beta}})}$$ . 
2. Wald-Statistik: $$W=(\boldsymbol{C} \hat{\boldsymbol{\beta}}-\boldsymbol{d})^{\prime}\left[\boldsymbol{C} \boldsymbol{F}^{-1}(\hat{\boldsymbol{\beta}}) \boldsymbol{C}^{\prime}\right]^{-1}(\boldsymbol{C} \hat{\boldsymbol{\beta}}-\boldsymbol{d})$$ 
3. Score-Statistik: $$u=\boldsymbol{s}^{\prime}(\tilde{\boldsymbol{\beta}}) \boldsymbol{F}^{-1}(\tilde{\boldsymbol{\beta}}) \boldsymbol{s}(\tilde{\boldsymbol{\beta}})$$ . 

Dabei ist $$\tilde{\boldsymbol{\beta}}$$ der ML-Schätzer unter der Restriktion $$H_{0}$$ . Testentscheidungen Für großes n gilt unter $$H_0$$ ​ approximativ:

$$
l q, w, u \stackrel{a}{\sim} \chi_{r}^{2}
$$

Dabei ist r der Rang von $$\boldsymbol{C} . H_{0}$$ wird abgelehnt, falls

$$
l q, w, u>\chi_{r}^{2}(1-\alpha)
$$

### Maximum-Likelihood-Schatzung in GLM

Der ML-Schätzer $$\hat\beta$$ maximiert die (Log-) Likelihood und wird als Nullstelle

$$
\boldsymbol{s}(\hat{\boldsymbol{\beta}})=\mathbf{0}
$$

der Score-Funktion

$$
\boldsymbol{s}(\boldsymbol{\beta})=\sum \boldsymbol{x}_{i} \frac{d_{i}}{\sigma_{i}^{2}}\left(y_{i}-\mu_{i}\right)=\boldsymbol{X}^{\prime} \boldsymbol{D} \boldsymbol{\Sigma}^{-1}(\boldsymbol{y}-\boldsymbol{\mu})
$$

bezeichnet. Die Fisher-Matrix ist

$$
\boldsymbol{F}(\boldsymbol{\beta})=\sum \boldsymbol{x}_{i} \boldsymbol{x}_{i}^{\prime} w_{i}=\boldsymbol{X}^{\prime} \boldsymbol{W} \boldsymbol{X}
$$

mit $w_{i}=d_{i}^{2} / \sigma_{i}^{2}$ und der Gewichtsmatrix $\boldsymbol{W}=\operatorname{diag}\left(w_{1}, \ldots, w\_{n}\right)$ Numerische Berechnung Der ML-Schätzer $\hat{\boldsymbol{\beta}}$ wird iterativ berechnet durch Fisher-Scoring in Form einer iterativ gewichteten KQ-Schätzung

$$
\hat{\boldsymbol{\beta}}^{(k+1)}=\left(\boldsymbol{X}^{\prime} \boldsymbol{W}^{(k)} \boldsymbol{X}\right)^{-1} \boldsymbol{X}^{\prime} \boldsymbol{W}^{(k)} \tilde{\boldsymbol{y}}^{(k)}, \quad k=0,1,2, \ldots
$$

mit dem Vektor $\tilde{\boldsymbol{y}}^{(k)}=\left(\ldots, \tilde{y}_{i}\left(\hat{\boldsymbol{\beta}}^{(k)}\right), \ldots\right)^{\prime}$ von Arbeitsbeobachtungen \$$\tilde{y}_{i}\left(\hat{\boldsymbol{\beta}}^{(k)}\right)=\boldsymbol{x}_{i}^{\prime} \hat{\boldsymbol{\beta}}^{(k)}+d_{i}^{-1}\left(\hat{\boldsymbol{\beta}}^{(k)}\right)\left(y_{i}-\hat{\mu}_{i}\left(\hat{\boldsymbol{\beta}}^{(k)}\right)\right)\$$ $$\text{und} \ \ \ \boldsymbol{W}^{(k)}=\boldsymbol{W}\left(\hat{\boldsymbol{\beta}}^{(k)}\right)$$



### glm()函数

| 分 布 族            | 默认的连接函数                                    |
| ---------------- | ------------------------------------------ |
| binomial         | (link = “logit”)                           |
| gaussian         | (link = “identity”)                        |
| gamma            | (link = “inverse”)                         |
| inverse.gaussian | (link = “1/mu^2”)                          |
| poisson          | (link = “log”)                             |
| quasi            | (link = “identity”, variance = “constant”) |
| quasibinomial    | (link = “logit”)                           |
| quasipoisson     | (link = “log”)                             |

当评价模型的适用性时，你可以绘制初始响应变量的预测值与残差的图形。 R将列出帽子值(hat value)、学生化残差值和Cook距离统计量的近似值。不过，对于识别异 常点的阈值，现在并没统一答案，它们都是通过相互比较来进行判断。其中一个方法就是绘制各 统计量的参考图，然后找出异常大的值。

```
plot(predict(model,type="response"),residuals(model,type="deviance"))
plot(hatvalues(model))
plot(rstudent(model))
plot(cooks.distance(model))

## oder
library("car")
influenceplot(model)
```







## IRLS算法估计广义线性模型（GLM）

Iterative reweighted least squares 迭代加权最小二乘

IRLS用于查找广义线性模型的最大似然估计，并在robust回归中用于查找估计量，以减轻异常值在正常分布的数据集中的影响。 例如，通过最小化最小绝对误差而不是最小平方误差。随着绝对残差的减少，权重也随之增加，换句话说，具有较大残差的情况往往会被减少加权。

```
require(foreign)
require(MASS)
cdata <- read.dta("~/Library/Mobile Documents/com~apple~CloudDocs/02. Programm/01 R_lernen/00 Data/crime1.dta")

## iterated re-weighted least squares (IRLS).
summary(rr.huber <- rlm(crime ~ poverty + single, data = cdata))

## 
## Call: rlm(formula = crime ~ poverty + single, data = cdata)
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -846.09 -125.80  -16.49  119.15  679.94 
## 
## Coefficients:
##             Value      Std. Error t value   
## (Intercept) -1423.0373   167.5899    -8.4912
## poverty         8.8677     8.0467     1.1020
## single        168.9858    17.3878     9.7186
## 
## Residual standard error: 181.8 on 48 degrees of freedom
```

##





##
