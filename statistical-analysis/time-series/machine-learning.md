---
description: Time Series Analysis With Applications in R Second Edition
---

# Basic Time Series

## Introduction

**Studying models that incorporate dependence is the key concept in time series analysis.**

> 研究包含依赖性的模型是时间序列分析中的关键概念。

### Examples of Time Series

* 在业务中，我们观察每周利率，每日收盘价，每月价格指数，年度销售数字等。
* 在气象学中，我们观察到每天的高温和低温，年降水量和干旱指数以及每小时的风速。
  * Regular pattern called **seasonality**. Seasonality for monthly values occurs when observations twelve months apart are related in some manner or another
* 在农业方面，我们记录了农作物和牲畜产量，水土流失和出口销售的年度数据。
* 在生物科学中，我们以毫秒为间隔观察心脏的电活动。
* 在生态学中，我们记录了动物物种的丰富度。

The purpose of time series analysis is generally twofold: to understand or model the stochastic mechanism that gives rise to an observed series and to predict or forecast the future values of a series based on the history of that series and, possibly, other related series or factors.

> 时间序列分析的目的通常是双重的：了解或建模产生观察序列的随机机制，并基于该序列以及其他相关序列或历史的历史预测或预测该序列的未来值因素。

### Means, Variances, and Covariances

For a stochastic process $$\left\{Y_{t}: t=0, \pm 1, \pm 2, \pm 3, \ldots\right\},$$

The **autocovariance function**, $$\gamma_{t, s}$$, is defined as

$$
\gamma_{t, s}=\operatorname{Cov}\left(Y_{t}, Y_{s}\right) \quad \text { for } t, s=0, \pm 1, \pm 2, \ldots
$$

where $$\operatorname{Cov}\left(Y_{t}, Y_{s}\right)=E\left[\left(Y_{t}-\mu_{t}\right)\left(Y_{s}-\mu_{s}\right)\right]=E\left(Y_{t} Y_{s}\right)-\mu_{t} \mu_{s}$$

The **autocorrelation function,** $$\rho_{t, s}$$, is given by

$$
\rho_{t, s}=\operatorname{Corr}\left(Y_{t}, Y_{s}\right) \quad \text { for } t, s=0, \pm 1, \pm 2, \ldots
$$

where

$$
\operatorname{Corr}\left(Y_{t}, Y_{s}\right)=\frac{\operatorname{Cov}\left(Y_{t}, Y_{s}\right)}{\sqrt{\operatorname{Var}\left(Y_{t}\right) \operatorname{Var}\left(Y_{s}\right)}}=\frac{\gamma_{t, s}}{\sqrt{\gamma_{t, t} \gamma_{s, s}}}
$$

The following important properties follow from known results and our definitions:

$$
\left.\begin{array}{ll}
\gamma_{t, t}=\operatorname{Var}\left(Y_{t}\right) & \rho_{t, t}=1 \\
\gamma_{t, s}=\gamma_{s, t} & \rho_{t, s}=\rho_{s, t} \\
\left|\gamma_{t, s}\right| \leq \sqrt{\gamma_{t, t} \gamma_{s, s}} & \left|\rho_{t, s}\right| \leq 1
\end{array}\right\}
$$

Values of $$\rho_{t, s}$$ near $$\pm 1$$ indicate strong (linear) dependence, whereas values near zero indicate weak (linear) dependence. If $$\rho_{t, s}=0$$, we say that $$Y_{t}$$ and $$Y_{s}$$ are uncorrelated.

#### Properties 

To investigate the covariance properties of various time series models, the following result will be used repeatedly: If $$c_{1}, c_{2}, \ldots, c_{m}$$ and $$d_{1}, d_{2}, \ldots, d_{n}$$ are constants and $$t_{1}$$, $$t_{2}, \ldots, t_{m}$$ and $$s_{1}, s_{2}, \ldots, s_{n}$$ are time points, then

$$
\operatorname{Cov}\left[\sum_{i=1}^{m} c_{i} Y_{t_{i}}, \sum_{j=1}^{n} d_{j} Y_{s_{j}}\right]=\sum_{i=1}^{m} \sum_{j=1}^{n} c_{i} d_{j} \operatorname{Cov}\left(Y_{t_{i}}, Y_{s_{j}}\right)
$$

As a special case, we obtain the well-known result

$$
\operatorname{Var}\left[\sum_{i=1}^{n} c_{i} Y_{t_{i}}\right]=\sum_{i=1}^{n} c_{i}^{2} \operatorname{Var}\left(Y_{t_{i}}\right)+2 \sum_{i=2}^{n} \sum_{j=1}^{i-1} c_{i} c_{j} \operatorname{Cov}\left(Y_{t_{i}}, Y_{t_{j}}\right)
$$

### Properties

#### Properties of Expectation

If $$h(x)$$ is a function such that $$\int_{-\infty}^{\infty}|h(x)| f(x) d x<\infty$$, it may be shown that

$$
E[h(X)]=\int_{-\infty}^{\infty} h(x) f(x) d x
$$

Similarly, if $$\int_{-\infty}^{\infty} \int_{-\infty}^{\infty}|h(x, y)| f(x, y) d x d y<\infty$$, it may be shown that

$$
E[h(X, Y)]=\int_{-\infty}^{\infty} \int_{-\infty}^{\infty} h(x, y) f(x, y) d x d y
$$

As a corollary, we easily obtain the important result

$$
E(a X+b Y+c)=a E(X)+b E(Y)+c
$$

We also have

$$
E(X Y)=\int_{-\infty}^{\infty} \int_{-\infty}^{\infty} x y f(x, y) d x d y
$$

The variance of a random variable $X$ is defined as

$$
\operatorname{Var}(X)=E\left\{[X-E(X)]^{2}\right\}
$$

(provided $$E\left(X^{2}\right)$$ exists). The variance of $$X$$ is often denoted by $$\sigma^{2}$$ or $$\sigma_{X}^{2}$$

#### Properties of Variance

$$
\begin{array}{c}
\operatorname{Var}(X) \geq 0 \\
\operatorname{Var}(a+b X)=b^{2} \operatorname{Var}(X)
\end{array}
$$

If $$X$$ and $$Y$$ are independent, then

$$
\operatorname{Var}(X+Y)=\operatorname{Var}(X)+\operatorname{Var}(Y)
$$

In general, it may be shown that

$$
\operatorname{Var}(X)=E\left(X^{2}\right)-[E(X)]^{2}
$$

The positive square root of the variance of $$X$$ is called the standard deviation of $$X$$ and is often denoted by $$\sigma$$ or $$\sigma_{X}$$. The random variable $$\left(X-\mu_{X}\right) / \sigma_{X}$$ is called the standardized version of $$X .$$ The mean and standard deviation of a standardized variable are always zero and one, respectively. The covariance of $$X$$ and $$Y$$ is defined as $$\operatorname{Cov}(X, Y)=E\left[\left(X-\mu_{X}\right)\left(Y-\mu_{Y}\right)\right]$$.

#### Properties of Covariance

$$
\begin{array}{c}
\operatorname{Cov}(a+b X, c+d Y)=b d \operatorname{Cov}(X, Y) \\
\operatorname{Var}(X+Y)=\operatorname{Var}(X)+\operatorname{Var}(Y)+2 \operatorname{Cov}(X, Y) \\
\operatorname{Cov}(X+Y, Z)=\operatorname{Cov}(X, Z)+\operatorname{Cov}(Y, Z) \\
\operatorname{Cov}(X, X)=\operatorname{Var}(X) \\
\operatorname{Cov}(X, Y)=\operatorname{Cov}(Y, X)
\end{array}
$$

If $$X$$ and $$Y$$ are independent,

$$
\operatorname{Cov}(X, Y)=0
$$

The correlation coefficient of $$X$$ and $$Y$$, denoted by $$\operatorname{Cor}(X, Y)$$ or $$\rho$$, is defined as

$$
\rho=\operatorname{Corr}(X, Y)=\frac{\operatorname{Cov}(X, Y)}{\sqrt{\operatorname{Var}(X) \operatorname{Var}(Y)}}
$$

Alternatively, if $$X^{*}$$ is a standardized $$X$$ and $$Y^{*}$$ is a standardized $$Y$$, then $$\rho=E\left(X^{*} Y^{*}\right) .$$

### The Random Walk

Let $$e_{1}, e_{2}, \ldots$$ be a sequence of independent, identically distributed random variables each with zero mean and variance $$\sigma_{e}^{2} .$$ The observed time series, $$\left\{Y_{t}: t=1,2, \ldots\right\}$$, is constructed as follows:

$$
\left.\begin{array}{rl}
Y_{1} & =e_{1} \\
Y_{2} & =e_{1}+e_{2} \\
& \vdots \\
Y_{t} & =e_{1}+e_{2}+\cdots+e_{t}
\end{array}\right\}
$$

Alternatively, we can write

$$
Y_{t}=Y_{t-1}+e_{t}
$$

with "initial condition" $$Y_{1}=e_{1} .$$

#### The mean function is

$$
\begin{aligned}
\mu_{t} &=E\left(Y_{t}\right)=E\left(e_{1}+e_{2}+\cdots+e_{t}\right)=E\left(e_{1}\right)+E\left(e_{2}\right)+\cdots+E\left(e_{t}\right) \\
&=0+0+\cdots+0
\end{aligned}
$$

so that

$$
\mu_{t}=0 \quad \text { for all } t
$$

#### The variance function is

$$
\begin{aligned}
\operatorname{Var}\left(Y_{t}\right) &=\operatorname{Var}\left(e_{1}+e_{2}+\cdots+e_{t}\right)=\operatorname{Var}\left(e_{1}\right)+\operatorname{Var}\left(e_{2}\right)+\cdots+\operatorname{Var}\left(e_{t}\right) \\
&=\sigma_{e}^{2}+\sigma_{e}^{2}+\cdots+\sigma_{e}^{2}
\end{aligned}
$$

so that

$$
\operatorname{Var}\left(Y_{t}\right)=t \sigma_{e}^{2}
$$

#### The covariance function is

Notice that the process variance increases linearly with time. To investigate the covariance function, suppose that $$1 \leq t \leq s .$$ Then we have

$$
\gamma_{t, s}=\operatorname{Cov}\left(Y_{t}, Y_{s}\right)=\operatorname{Cov}\left(e_{1}+e_{2}+\cdots+e_{t}, e_{1}+e_{2}+\cdots+e_{t}+e_{t+1}+\cdots+e_{s}\right)
$$

From Equation above, we have

$$
\gamma_{t, s}=\sum_{i=1}^{s} \sum_{j=1}^{t} \operatorname{Cov}\left(e_{i}, e_{j}\right)
$$

However, these covariances are zero unless $$i=j$$, in which case they equal $$\operatorname{Var}\left(e_{i}\right)=\sigma_{e}^{2}$$. There are exactly $$t$$ of these so that $$\gamma_{t, s}=t \sigma_{e}^{2}$$.

#### The autocorrelation function

Since $$\gamma_{t, s}=\gamma_{s, t}$$, this specifies the autocovariance function for all time points $$t$$ and $$s$$ and we can write

$$
\gamma_{t, s}=t \sigma_{e}^{2}
$$

for $$1 \leq t \leq s$$ The autocorrelation function for the random walk is now easily obtained as

$$
\rho_{t, s}=\frac{\gamma_{t, s}}{\sqrt{\gamma_{t, t} \gamma_{s, s}}}=\sqrt{\frac{t}{s}}
$$

### A Moving Average

As a second example, suppose that $$\left\{Y_{t}\right\}$$ is constructed as

$$
Y_{t}=\frac{e_{t}+e_{t-1}}{2}
$$

where (as always throughout this book) the $$e$$ 's are assumed to be independent and identically distributed with zero mean and variance $$\sigma_{e}^{2} .$$ Here

$$
\begin{aligned}
\mu_{t} &=E\left(Y_{t}\right)=E\left\{\frac{e_{t}+e_{t-1}}{2}\right\}=\frac{E\left(e_{t}\right)+E\left(e_{t-1}\right)}{2} =0
\end{aligned}
$$

$$
\begin{aligned}
\operatorname{Var}\left(Y_{t}\right) &=\operatorname{Var}\left\{\frac{e_{t}+e_{t-1}}{2}\right\}=\frac{\operatorname{Var}\left(e_{t}\right)+\operatorname{Var}\left(e_{t-1}\right)}{4} \\
&=0.5 \sigma_{e}^{2}
\end{aligned}
$$

$$
\begin{aligned}
\operatorname{Cov}\left(Y_{t}, Y_{t-1}\right)=& \operatorname{Cov}\left\{\frac{e_{t}+e_{t-1}}{2}, \frac{e_{t-1}+e_{t-2}}{2}\right\} \\
=& \frac{\operatorname{Cov}\left(e_{t}, e_{t-1}\right)+\operatorname{Cov}\left(e_{t}, e_{t-2}\right)+\operatorname{Cov}\left(e_{t-1}, e_{t-1}\right)}{4} 
+\frac{\operatorname{Cov}\left(e_{t-1}, e_{t-2}\right)}{4} \\
=& \frac{\operatorname{Cov}\left(e_{t-1}, e_{t-1}\right)}{4} \quad \text { (as all the other covariances are zero) } \\
=& 0.25 \sigma_{e}^{2}
\end{aligned}
$$

Or

$$
\gamma_{t, t-1}=0.25 \sigma_{e}^{2}
$$

for all $$t$$. Furthermore,

$$
\begin{aligned}
\operatorname{Cov}\left(Y_{t}, Y_{t-2}\right) &=\operatorname{Cov}\left\{\frac{e_{t}+e_{t-1}}{2}, \frac{e_{t-2}+e_{t-3}}{2}\right\} \\
&=0 \quad \text { since the } e^{\prime} \text { s are independent }
\end{aligned}
$$

Similarly, $$\operatorname{Cov}\left(Y_{t}, Y_{t-k}\right)=0$$ for $$k>1$$, so we may write

$$
\gamma_{t, s}=\left\{\begin{array}{cc}
0.5 \sigma_{e}^{2} & \text { for }|t-s|=0 \\
0.25 \sigma_{e}^{2} & \text { for }|t-s|=1 \\
0 & \text { for }|t-s|>1
\end{array}\right.
$$

### Stationarity

要基于观察到的随机过程的结构对统计进行推断，通常必须对这种结构进行一些简化（假定合理）的假设。最重要的假设是平稳性。平稳性的基本思想是，控制流程行为的概率定律不会随时间变化。从某种意义上说，这个过程处于统计平衡状态。具体来说，如果的联合分布与所有时间点t1，t2，…，tn以及所有时滞k的选择的联合分布相同，则说过程{Yt}是严格平稳的。

因此，当n = 1时，对于所有t和k，Yt的（单变量）分布与Yt-k的分布相同；换句话说，Y的边际分布是相同的。然后得出，对于所有t和k，E（Yt）= E（Yt-k），因此均值函数在所有时间内都是恒定的。另外，对于所有t和k，Var（Yt）= Var（Yt − k），因此方差随时间变化也是恒定的。在平稳性定义中设置n = 2，我们看到Yt和Ys的二元分布必须与Yt-k和Ys-k的二元分布相同，由此得出Cov（Yt，Ys）= Cov（Yt-k，所有t，s和k均为Ys-k）。令k = s，然后k = t，我们得到

$$
\begin{aligned}
\gamma_{t, s} &=\operatorname{Cov}\left(Y_{t-s}, Y_{0}\right) \\
&=\operatorname{Cov}\left(Y_{0}, Y_{s-t}\right) \\
&=\operatorname{Cov}\left(Y_{0}, Y_{|t-s|}\right) \\
&=\gamma_{0,|t-s|}
\end{aligned}
$$

也就是说，Yt和Ys之间的协方差仅取决于时间差| t-s |。 而不是在实际时间t和s上。 因此，对于固定过程，我们可以简化符号并编写

$$
\gamma_{k}=\operatorname{Cov}\left(Y_{t}, Y_{t-k}\right) \quad \text { and } \quad \rho_{k}=\operatorname{Corr}\left(Y_{t}, Y_{t-k}\right)
$$

$$
\rho_{k}=\frac{\gamma_{k}}{\gamma_{0}}
$$

一般属性现在为

$$
\left.\begin{array}{ll}
\gamma_{0}={Var}\left(Y_{t}\right) & \rho_{0}=1 \\
\gamma_{k}=\gamma_{-k} & \rho_{k}=\rho_{-k} \\
\left|\gamma_{k}\right| \leq \gamma_{0} & \left|\rho_{k}\right| \leq 1
\end{array}\right\}
$$

That is, the covariance between Yt and Ys depends on time only through the time difference |t − s| and not otherwise on the actual times t and s. Thus, for a stationary process, we can simplify our notation and write

**A stochastic process **$$\left\{Y_{t}\right\}$$** is said to be weakly (or second-order) stationary if**

1. The mean function is constant over time, and 
2. for all time $$t$$ and lag $$k$$

$$
\gamma_{t, t-k}=\gamma_{0, k}
$$

#### White Noise 

白噪声过程定义为一系列独立的，均匀分布的随机变量. 定义为一系列独立的，均匀分布的随机变量{et}。

$$
\rho_{k}=\left\{\begin{array}{cl}
1 & \text { for } k=0 \\
0.5 & \text { for }|k|=1 \\
0 & \text { for }|k| \geq 2
\end{array}\right.
$$

#### Random Cosine Wave

As a somewhat different example, $$^{\dagger}$$ consider the process defined as follows:

$$
Y_{t}=\cos \left[2 \pi\left(\frac{t}{12}+\Phi\right)\right] \quad \text { for } t=0, \pm 1, \pm 2, \ldots
$$

where $$\Phi$$ is selected (once) from a uniform distribution on the interval from 0 to 1 .

So the process is stationary with autocorrelation function

$$
\rho_{k}=\cos \left(2 \pi \frac{k}{12}\right) \quad \text { for } k=0, \pm 1, \pm 2, \ldots
$$

### Regression Methods

回归分析的经典统计方法可以很容易地用于估计常见的非恒定平均趋势模型的参数。 我们将考虑最有用的方法：linear, quadratic, seasonal means, and cosine trends (余弦趋势).

#### Linear and Quadratic Trends in Time

Consider the deterministic time trend expressed as

$$
\mu_{t}=\beta_{0}+\beta_{1} t
$$

where the slope and intercept, $$\beta_{1}$$ and $$\beta_{0}$$ respectively, are unknown parameters. The classical least squares (or regression) method is to choose as estimates of $$\beta_{1}$$ and $$\beta_{0}$$ values that minimize

$$
Q\left(\beta_{0}, \beta_{1}\right)=\sum_{t=1}^{n}\left[Y_{t}-\left(\beta_{0}+\beta_{1} t\right)\right]^{2}
$$

#### Cyclical or Seasonal Trends

Here we assume that the observed series can be represented as

$$
Y_{t}=\mu_{t}+X_{t}
$$

where $$E\left(X_{t}\right)=0$$ for all $$t$$. The most general assumption for $$\mu_{t}$$ with monthly seasonal data is that there are 12 constants (parameters), $$\beta_{1}, \beta_{2}, \ldots$$, and $$\beta_{12}$$, giving the expected average temperature for each of the 12 months. We may write

$$
\mu_{t}=\left\{\begin{array}{cc}
\beta_{1} & \text { for } t=1,13,25, \ldots \\
\beta_{2} & \text { for } t=2,14,26, \ldots \\
\vdots & \\
\beta_{12} & \text { for } t=12,24,36, \ldots
\end{array}\right.
$$

This is sometimes called a seasonal means model.

```
> data(tempdub)
> month.=season(tempdub)      # period added to improve table display
> model2=lm(tempdub~month.-1) # -1 removes the intercept term
> summary(model2)
```

#### Cosine Trends

月度数据的季节性均值模型由12个独立参数组成，根本没有考虑季节性趋势的形状。例如，三月和四月均值非常相似（与六月和七月均值不同）这一事实并未反映在模型中。在某些情况下，可以使用余弦曲线在经济上模拟季节性趋势，该曲线包含了从一个时间段到下一个时间段的预期平滑变化，同时仍保留了季节性。

$$
\mu_{t}=\beta \cos (2 \pi f t+\Phi)
$$

β（> 0）称为振幅，将f称为频率，Φ曲线的相位。随着t的变化，曲线在最大β和最小-β之间振荡。由于曲线精确地每$$1/f$$时间单位重复一次，因此$$1/f$$称为余弦波的周期。Φ用于设置时间轴上的任意原点。对于时间索引为1、2，...的月度数据，最重要的频率是f = 1/12，因为这样的余弦波将每12个月重复一次。可以使用三角恒等式更方便地重新参数化

$$
\beta \cos (2 \pi f t+\Phi)=\beta_{1} \cos (2 \pi f t)+\beta_{2} \sin (2 \pi f t)
$$

where

$$
\beta=\sqrt{\beta_{1}^{2}+\beta_{2}^{2}}, \quad \Phi=\operatorname{atan}\left(-\beta_{2} / \beta_{1}\right)
$$

and, conversely,

$$
\beta_{1}=\beta \cos (\Phi), \quad \beta_{2}=\beta \sin (\Phi)
$$

To estimate the parameters $$\beta_{1}$$ and $$\beta_{2}$$ with regression techniques, we simply use $$\cos (2 \pi f t)$$ and $$\sin (2 \pi f t)$$ as regressors or predictor variables. The simplest such model for the trend would be expressed as

$$
\mu_{t}=\beta_{0}+\beta_{1} \cos (2 \pi f t)+\beta_{2} \sin (2 \pi f t)
$$

Here the constant term, $$\beta_{0}$$, can be meaningfully thought of as a cosine with frequency zero.

```
library(TSA)
data(tempdub)
har.=harmonic(tempdub,1)

## har.: cos(2*pi*t)   sin(2*pi*t)

model4=lm(tempdub~har.)
summary(model4)

Coefficients:
                Estimate Std. Error t value Pr(>|t|)    
(Intercept)      46.2660     0.3088 149.816  < 2e-16 ***
har.cos(2*pi*t) -26.7079     0.4367 -61.154  < 2e-16 ***
har.sin(2*pi*t)  -2.1697     0.4367  -4.968 1.93e-06 ***


plot(ts(fitted(model4),freq=12,start=c(1964,1)),
     ylab='Temperature',type='l',
ylim=range(c(fitted(model4),tempdub)))
points(tempdub)
```

![](<../../.gitbook/assets/plot_zoom_png (48).png>)

#### Reliability and Efficiency of Regression Estimates

To comparing the least squares estimates with the so-called best linear unbiased estimates (BLUE) or the generalized least squares (GLS) estimates. If the stochastic component {Xt } is not white noise, estimates of the unknown parameters in the trend function may be made; they are linear functions of the data, are unbiased, and have the smallest variances among all such estimates—the so-called BLUE or GLS estimates.

> 将最小二乘估计与所谓的最佳线性无偏估计（BLUE）或广义最小二乘（GLS）估计进行比较。如果随机成分{Xt}不是白噪声，则可以估算趋势函数中的未知参数；它们是数据的线性函数，是无偏的，并且在所有此类估计（即所谓的BLUE或GLS估计）中具有最小的方差。

基于大样本量的结果，这些结果支持对我们考虑的趋势类型使用更简单的最小二乘估计。特别是，假设趋势是时间上的多项式，三角多项式，季节性均值或以下项的线性组合. 对于非常一般的平稳随机成分{Xt}，趋势的最小二乘估计与大样本量的最佳线性无偏估计具有相同的方差。尽管简单的最小二乘估计可能在渐近效率上有效，但是并不能证明所有回归例程打印出的系数的估计标准偏差都是正确的。

#### Interpreting Regression Output

标准回归例程会计算未知回归系数（β）的最小二乘估计值。 因此，在对随机成分{Xt}的最小假设下，估计值是合理的。 但是，回归输出的某些属性在很大程度上取决于{Xt}是白噪声的通常回归假设，而某些则取决于{Xt}近似正态分布的进一步假设。

如果{Xt}过程具有恒定方差，那么我们可以通过剩余标准偏差(**residual standard deviation**)来估计Xt的标准偏差

$$
s=\sqrt{\frac{1}{n-p} \sum_{t=1}^{n}\left(Y_{t}-\hat{\mu}_{t}\right)^{2}}
$$

### Residual Analysis

As we have already noted, the unobserved stochastic component $$\left\{X_{t}\right\}$$ can be estimated, or predicted, by the residual

$$
\hat{X}_{t}=Y_{t}-\hat{\mu}_{t}
$$

如果趋势模型是合理正确的，则残差的行为应大致类似于真实随机成分，并且可以通过查看残差来评估有关随机成分的各种假设。如果随机成分是白噪声，则残差的行为应大致类似于独立的（正常）随机变量，均值为零且标准差为s。(**考虑残差标准化**)

掌握了残差或标准化残差后，下一步就是检查各种残差图。我们首先看一下残差随时间变化的图。如果随机成分是白噪声，并且对趋势进行了充分的建模，则我们期望这样的图将显示出矩形散点，而没有任何明显的趋势。

接下来，我们将标准化残差与相应的趋势估计值或拟合值进行比较，寻找模式。 小残差是否与较小的拟合趋势值相关联，而大残差是否与较大的拟合趋势值相关联？ 与某些大小的拟合趋势值相关的残差变化是否较小，还是与其他拟合趋势值相关的变化较大？

可以通过绘制残差或标准化残差的直方图来评估总体非正态性, 也可以通过绘制所谓的正常得分或分位数（QQ）图来更仔细地检查正态性。 这样的图显示了数据的分位数与正态分布的理论分位数之间的关系。 对于正态分布的数据，QQ图看起来近似于一条直线。

出色的正态性检验称为Shapiro-Wilk检验。†它本质上是计算残差与相应法向分位数之间的相关性。这种相关性越低，我们就越有反常态的证据。将检验应用于这些残差可得出W = 0.9929的检验统计量，其p值为0.6954。我们不能拒绝零假设，即该模型的随机成分是正态分布的。

随机成分的独立性可以通过几种方法进行测试。运行测试按顺序检查残差以寻找模式，这些模式会提供反对独立性的证据。样本自相关函数另一个用于检查依赖性的非常重要的诊断工具是样本自相关函数 (_**Sample Autocorrelation Function**_) `acf(rstudent(model3))`

## Stationary Models

A broad class of parametric time series models—the autoregressive moving average (ARMA) models.

#### Model-Building Strategy

1.  **model specification (or identification)**

    在这一步中，我们查看序列的时间图，从数据中计算出许多不同的统计数据，并应用有关数据出现的主题的任何知识，例如生物学，商业或生态学。应该强调的是，此时选择的模型是暂定的，并且在分析中稍后会进行修订。在选择模型时，我们将尝试坚持简约原则。也就是说，所使用的模型应要求数量最少的参数，这些参数足以代表时间序列。
2. **model fitting**, and\
   模型将不可避免地涉及一个或多个参数，这些参数的值必须从观察到的序列中估算出来。模型拟合包括找到给定模型中那些未知参数的最佳可能估计。我们将考虑诸如最小二乘法和最大似然估计之类的标准。
3. **model diagnostics**\
   模型诊断与评估我们指定和估计的模型的质量有关。模型对数据的拟合程度如何？该模型的假设是否合理满足？如果没有发现不足之处，则可以假定建模是完整的，并且该模型可以用于例如预测未来值。否则，我们将根据发现的不足之处选择另一个模型。也就是说，我们返回模型说明步骤。通过这种方式，我们循环执行这三个步骤，直到找到理想的模型为止。

### General Linear Processes

我们将始终让{Yt}表示观察到的时间序列。 从这里开始，我们还将让{et}代表一个未观察到的白噪声序列，即一系列相同分布的，零均值，独立的随机变量。 在我们的大部分工作中，可以用较弱的假设代替独立性的假设，即{et}是不相关的随机变量，但是我们不会追求这种轻微的普遍性。 一般的线性过程{Yt}可以表示为当前和过去的白噪声项的加权线性组合，例如

$$
Y_{t}=e_{t}+\psi_{1} e_{t-1}+\psi_{2} e_{t-2}+\cdots
$$

ψ形成指数衰减序列的情况 (exponentially decaying sequence)

$$
\psi_{j}=\phi^{j}
$$

where $$\phi$$ is a number strictly between $$-1$$ and $$+1$$. Then

$$
Y_{t}=e_{t}+\phi e_{t-1}+\phi^{2} e_{t-2}+\cdots
$$

For this example,

$$
E\left(Y_{t}\right)=E\left(e_{t}+\phi e_{t-1}+\phi^{2} e_{t-2}+\cdots\right)=0
$$

so that $$\left\{Y_{t}\right\}$$ has a constant mean of zero. Also,

$$
\begin{aligned}
\operatorname{Var}\left(Y_{t}\right) &=\operatorname{Var}\left(e_{t}+\phi e_{t-1}+\phi^{2} e_{t-2}+\cdots\right) \\
&=\operatorname{Var}\left(e_{t}\right)+\phi^{2} \operatorname{Var}\left(e_{t-1}\right)+\phi^{4} \operatorname{Var}\left(e_{t-2}\right)+\cdots \\
&=\sigma_{e}^{2}\left(1+\phi^{2}+\phi^{4}+\cdots\right)\\
&=\frac{\sigma_{e}^{2}}{1-\phi^{2}} \text{(by summing a geometric series)}
\end{aligned}
$$

Furthermore,

$$
\begin{aligned}
\operatorname{Cov}\left(Y_{t}, Y_{t-1}\right) &=\operatorname{Cov}\left(e_{t}+\phi e_{t-1}+\phi^{2} e_{t-2}+\cdots, e_{t-1}+\phi e_{t-2}+\phi^{2} e_{t-3}+\cdots\right) \\
&=\operatorname{Cov}\left(\phi e_{t-1}, e_{t-1}\right)+\operatorname{Cov}\left(\phi^{2} e_{t-2}, \phi e_{t-2}\right)+\cdots \\
&=\phi \sigma_{e}^{2}+\phi^{3} \sigma_{e}^{2}+\phi^{5} \sigma_{e}^{2}+\cdots \\
&=\phi \sigma_{e}^{2}\left(1+\phi^{2}+\phi^{4}+\cdots\right)\\
&=\frac{\phi \sigma_{e}^{2}}{1-\phi^{2}}  \text{(again summing a geometric series)}
\end{aligned}
$$

Thus

$$
\operatorname{Corr}\left(Y_{t}, Y_{t-1}\right)=\left[\frac{\phi \sigma_{e}^{2}}{1-\phi^{2}}\right] /\left[\frac{\sigma_{e}^{2}}{1-\phi^{2}}\right]=\phi
$$

In a similar manner, we can find

$$
\operatorname{Cov}\left(Y_{t}, Y_{t-k}\right)=\frac{\phi^{k} \sigma_{e}^{2}}{1-\phi^{2}}
$$

and thus

$$
\operatorname{Corr}\left(Y_{t}, Y_{t-k}\right)=\phi^{k}
$$

重要的是要注意，以这种方式定义的过程是固定的-自协方差结构仅取决于时滞(Time Lag)，而不取决于绝对时间。 对于一般的线性过程

$$
E\left(Y_{t}\right)=0 \quad \gamma_{k}=\operatorname{Cov}\left(Y_{t}, Y_{t-k}\right)=\sigma_{e}^{2} \sum_{i=0}^{\infty} \psi_{i} \psi_{i+k} \quad k \geq 0
$$

with $$\psi_{0}=1$$.

### Moving Average Processes

q阶的移动平均值，并将其名称缩写为MA（q）。 术语移动平均来自以下事实：Yt是通过将权重1，-θ1，-θ2，...，-θq应用于变量et，et-1，et-2，…，et-q然后得到的 移动权重并将其应用于et + 1，et，et-1，...，et-q + 1以获得Yt + 1，依此类推。

$$
Y_{t}=e_{t}-\theta_{1} e_{t-1}-\theta_{2} e_{t-2}-\cdots-\theta_{q} e_{t-q}
$$

#### The First-Order Moving Average Process

MA(1) series:

$$
Y_{t}=e_{t}-\theta e_{t-1}
$$

$$k ≥ 2$$, The process has no correlation beyond lag 1

$$
\left.\begin{array}{rl}
E\left(Y_{t}\right) & =0 \\
\gamma_{0} & =\operatorname{Var}\left(Y_{t}\right)=\sigma_{e}^{2}\left(1+\theta^{2}\right) \\
\gamma_{1} & =-\theta \sigma_{e}^{2} \\
\rho_{1} & =(-\theta) /\left(1+\theta^{2}\right) \\
\gamma_{k} & =\rho_{k}=0 \quad \text { for } k \geq 2
\end{array}\right\}
$$

#### The Second-Order Moving Average Process

$$
Y_{t}=e_{t}-\theta_{1} e_{t-1}-\theta_{2} e_{t-2}
$$

$$
\begin{aligned}
\gamma_{0} &=\operatorname{Var}\left(Y_{t}\right)=\operatorname{Var}\left(e_{t}-\theta_{1} e_{t-1}-\theta_{2} e_{t-2}\right)=\left(1+\theta_{1}^{2}+\theta_{2}^{2}\right) \sigma_{e}^{2} \\
\gamma_{1} &=\operatorname{Cov}\left(Y_{t}, Y_{t-1}\right)=\operatorname{Cov}\left(e_{t}-\theta_{1} e_{t-1}-\theta_{2} e_{t-2}, e_{t-1}-\theta_{1} e_{t-2}-\theta_{2} e_{t-3}\right) \\
&=\operatorname{Cov}\left(-\theta_{1} e_{t-1}, e_{t-1}\right)+\operatorname{Cov}\left(-\theta_{1} e_{t-2},-\theta_{2} e_{t-2}\right) \\
&=\left[-\theta_{1}+\left(-\theta_{1}\right)\left(-\theta_{2}\right)\right] \sigma_{e}^{2} \\
&=\left(-\theta_{1}+\theta_{1} \theta_{2}\right) \sigma_{e}^{2} \\
\gamma_{2} &=\operatorname{Cov}\left(Y_{t}, Y_{t-2}\right)=\operatorname{Cov}\left(e_{t}-\theta_{1} e_{t-1}-\theta_{2} e_{t-2}, e_{t-2}-\theta_{1} e_{t-3}-\theta_{2} e_{t-4}\right) \\
&=\operatorname{Cov}\left(-\theta_{2} e_{t-2}, e_{t-2}\right) \\
&=-\theta_{2} \sigma_{e}^{2}
\end{aligned}
$$

Thus, for an MA(2) process,

$$
\begin{aligned}
\rho_{1} &=\frac{-\theta_{1}+\theta_{1} \theta_{2}}{1+\theta_{1}^{2}+\theta_{2}^{2}} \\
\rho_{2} &=\frac{-\theta_{2}}{1+\theta_{1}^{2}+\theta_{2}^{2}} \\
\rho_{k} &=0 \text { for } k=3,4, \ldots
\end{aligned}
$$

#### The General MA(q) Process

For the general $$\operatorname{MA}(q)$$ process $$Y_{t}=e_{t}-\theta_{1} e_{t-1}-\theta_{2} e_{t-2}-\cdots-\theta_{q} e_{t-q}$$, similar calcu- lations show that

$$
\gamma_{0}=\left(1+\theta_{1}^{2}+\theta_{2}^{2}+\cdots+\theta_{q}^{2}\right) \sigma_{e}^{2}
$$

$$
\rho_{k}=\left\{\begin{array}{l}
\frac{-\theta_{k}+\theta_{1} \theta_{k+1}+\theta_{2} \theta_{k+2}+\cdots+\theta_{q-k} \theta_{q}}{1+\theta_{1}^{2}+\theta_{2}^{2}+\cdots+\theta_{q}^{2}} \\
0 \quad \text { for } k>q
\end{array}\right.
$$

### Autoregressive Processes

Autoregressive processes are as their name suggests - regressions on themselves. Specifically, a $$p$$ th-order autoregressive process $$\left\{Y_{t}\right\}$$ satisfies the equation

$$
Y_{t}=\phi_{1} Y_{t-1}+\phi_{2} Y_{t-2}+\cdots+\phi_{p} Y_{t-p}+e_{t}
$$

#### The First-Order Autoregressive Process

$$
Y_{t}=\phi Y_{t-1}+e_{t}
$$

![](<../../.gitbook/assets/image (77).png>)

#### The Second-Order Autoregressive Process

$$
Y_{t}=\phi_{1} Y_{t-1}+\phi_{2} Y_{t-2}+e_{t}
$$

### ARMA Model

If we assume that the series is partly autoregressive and partly moving average, we obtain a quite general time series model.

$$
\begin{array}{r}
Y_{t}=\phi_{1} Y_{t-1}+\phi_{2} Y_{t-2}+\cdots+\phi_{p} Y_{t-p}+e_{t}-\theta_{1} e_{t-1}-\theta_{2} e_{t-2} \\
-\cdots-\theta_{q} e_{t-q}
\end{array}
$$

we say that $$\left\{Y_{t}\right\}$$ is a mixed autoregressive moving average process of orders $$p$$ and $$q$$, respectively; we abbreviate the name to ARMA $$(p, q)$$.

#### The ARMA(1,1) Model

$$
Y_{t}=\phi Y_{t-1}+e_{t}-\theta e_{t-1}
$$

To derive Yule-Walker type equations, we first note that

$$
\begin{aligned}
E\left(e_{t} Y_{t}\right) &=E\left[e_{t}\left(\phi Y_{t-1}+e_{t}-\theta e_{t-1}\right)\right] \\
&=\sigma_{e}^{2}
\end{aligned}
$$

