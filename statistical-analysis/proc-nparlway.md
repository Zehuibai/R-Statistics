# Nonparametric Regression

## Loess Regression

对于某些数据集，不可能通过先验已知形式（例如多项式或指数）来描述响应变量y和预测变量x1，...，xk之间的关系。 在这种情况下，有用的工具是非参数回归

$$
y=f\left(x_{1}, \ldots, x_{k}\right)+\varepsilon
$$

where $$f$$ is a nonparametric response function with no explicit functional form $$f$$没有明确函数形式的非参数响应函数，而$$ε$$是独立的均匀分布的随机变量 均值为零且方差为$$σ2$$恒定的误差。 没有对$$ε$$的概率分布的形式做任何假设。

非参数回归方法估计响应函数f并在散点图上拟合曲线（或三维或更高维的曲面）

### Definition

The **loess (locally estimated scatterplot smoothing) **method 在每个数据点附近上评估f，并在散点图上绘制连接拟合点的分段多项式曲线。在两个以上的维度中，loess 回归方法通过拟合点构造平坦表面。本地散点平滑估计（Locally Estimated Scatterplot Smoothing，LOESS），事先不用确定参数数量，每次预测的时候，用指定的样本点周围的样本点进行临时训练，确定参数。

通过在每个点的局部邻域中拟合多项式回归函数，可以对数据点处的响应函数进行估计。邻域的半径由这些邻域包含的数据点的预定比例（称为平滑参数）确定。多项式回归（通常是线性或二次方）通过加权最小二乘法进行拟合，而权重越小则指向距中心的距离越远。

首先，将平滑参数的值指定为p / n。这意味着对于每个固定点$$\mathbf{x}^{0}=\left(x_{1}^{0}, \ldots, x_{k}^{0}\right),$$，线性函数通过局部邻域$$\mathscr{N}_{p}\left(\mathbf{x}^{0}\right)$$中的$$p$$个点拟合。该线性函数定义为

$$
l(\mathbf{x})=l\left(x_{1}, \ldots, x_{k}\right)=\beta_{0}+\beta_{1}\left(x_{1}-x_{1}^{0}\right)+\cdots+\beta_{k}\left(x_{k}-x_{k}^{0}\right)
$$

for every $$\mathbf{x}=\left(x_{1}, \ldots, x_{k}\right)$$ in $$\mathscr{N}_{p}\left(\mathbf{x}^{0}\right)$$

**The weighted least-squares **predicted line $$\hat{l}(\mathbf{x})=\hat{\beta}_{0}+\hat{\beta}_{1}\left(x_{1}-x_{1}^{0}\right)+\cdots+\hat{\beta}_{k}\left(x_{k}-x_{k}^{0}\right)$$ minimizes

$$
\sum_{\mathbf{x}_{i} \in \mathscr{N}_{p}\left(\mathbf{x}^{0}\right)}\left[y_{i}-l\left(\mathbf{x}_{i}\right)\right]^{2} w\left(\frac{\left\|\mathbf{x}_{i}-\mathbf{x}^{0}\right\|}{r\left(\mathbf{x}^{0}\right)}\right)
$$

where $$\mathbf{x}_{i}=\left(x_{1 i}, \ldots, x_{k i}\right), i=1, \ldots, p .$$ Here $$\left\|\mathbf{x}_{i}-\mathbf{x}^{0}\right\|$$ denotes the Euclidean distance, that is,

$$
\left\|\mathbf{x}_{i}-\mathbf{x}^{0}\right\|=\sqrt{\left(x_{1 i}-x_{1}^{0}\right)^{2}+\cdots+\left(x_{k i}-x_{k}^{0}\right)^{2}}
$$

$$r\left(\mathbf{x}^{0}\right)=\max _{\mathbf{x}_{i} \in \mathscr{N}_{p}\left(\mathbf{x}^{0}\right)}\left\|\mathbf{x}_{i}-\mathbf{x}^{0}\right\|$$ is the radius of the neighborhood, and $$w(\cdot)$$ is the weight function.

最后，通过预测行$$\hat{l}$$上的相应值，即$$\hat{f}\left(\mathbf{x}^{0}\right)=\hat{l}\left(\mathbf{x}^{0}\right)=\hat{\beta}_{0} .$$$，在点$$\mathbf{x}^{0}$$处估计响应函数$$f$$。请注意 只有匹配截距的值才起作用。

一旦完成了每个数据点的响应函数的loess估计，就可以通过将拟合点与直线连接起来，在散点图上绘制loess曲线。 如果在散点图上显示了显着的曲线关系，则二次函数

$$
q\left(x_{1}, \ldots, x_{k}\right)=\beta_{0}+\beta_{1,1}\left(x_{1}-x_{1}^{0}\right)+\beta_{1,2}\left(x_{1}-x_{1}^{0}\right)^{2}+\cdots+\beta_{k, 1}\left(x_{k}-x_{k}^{0}\right)+\beta_{k, 2}\left(x_{k}-x_{k}^{0}\right)^{2}
$$

代替线性函数$$l$$。 黄土回归法提出了不同的权函数。 最常用的是

*   the bisquare function

    $$
    w(\mathbf{x})=\left\{\begin{array}{ll}
    \left(1-\|\mathbf{x}\|^{2}\right)^{2}, & \text { if }\|\mathbf{x}\| \leq 1 \\
    0, & \text { otherwise }
    \end{array}\right.
    $$

    and
*   the tricube function

    $$
    w(\mathbf{x})=\left\{\begin{array}{ll}
    \left(1-\|\mathbf{x}\|^{3}\right)^{3}, & \text { if }\|\mathbf{x}\| \leq 1 \\
    0, & \text { otherwise }
    \end{array}\right.
    $$

    In both cases $$\|\mathbf{x}\|$$ denotes the Euclidean norm of $$\mathbf{x}=\left(x_{1}, \ldots, x_{k}\right),\|\mathbf{x}\|=\sqrt{x_{1}^{2}+\cdots+x_{k}^{2}}$$

### Comparison

采用线性回归会出现欠拟合(underfitting),这种情况下，Loess可以很好地解决这个问题。与线性回归对比：

* 线性回归的目标是最小化 $$\sum\left(y^{(i)}-\theta^{T} x^{(i)}\right)^{2}$$
* Loess的目标是最小化 $$\left(y^{(i)}-\theta^{T} x^{(i)}\right)^{2},$$ 其中 $$\quad \omega^{(i)}=\exp \left(-\frac{\left(x^{(i)}-x\right)^{2}}{2 \iota^{2}}\right)$$

$$\omega$$的作用是使预测点的临近点在最小化目标函数中壳献大：

* $$\left|x^{(i)}-x\right|$$ small $$\Rightarrow \omega^{(i)} \approx 1$$
* $$\left|x^{(i)}-x\right|$$ large $$\Rightarrow \omega^{(i)} \approx 0$$

缺点：并不能解决欠拟合和过拟合问题；当数据量很大时，要对每个带预测数据拟合一次，代价大

### Smoothing Parameter Selection Criterion

已经提出了许多标准来选择一组数据的最佳平滑参数。 在这里，最常用的，经偏差校正的赤池信息准则（AICC）_**the bias corrected Akaike information criterion**_。 该准则是选择使AICC最小化的平滑参数。

where $$\hat{\sigma}^{2}$$ is an estimate of the variance of random error $$\varepsilon$$. The matrix $$\mathbb{L}$$ is an $$n \times n$$ smoothing matrix that satisfies $$\mathbf{y}=\mathbb{L} \hat{\mathbf{y}}$$ where $$\mathbf{y}$$ is the vector of observed $$y$$ -values, and $$\hat{\mathbf{y}}$$ is the vector of the corresponding predicted values. The trace of a matrix 矩阵的轨迹定义为主对角线上元素的总和。

### SAS: Fitting Loess Regression

```
PROC LOESS DATA=data name;
MODEL response=list of predictors/
DEGREE=1 or 2 CLM ALPHA=value SMOOTH=values;
ODS OUTPUT OutputStatistics=output stats data name
ScoreResults=score results data name;
SCORE DATA=points4prediction data name;
RUN;
```

* The option DEGREE= specifies the degree of local polynomials (1 for linear, 2 for quadratic). The default value is 1.
* The CLM option (stands for “confidence limits for the mean”) produces a confidence interval for every observed response value. By default, 95% limits are computed, but the level can be changed by using the ALPHA= option
* As a default, SAS uses the bias corrected Akaike Information Criterion (AICC) to select an optimal smoothing parameter. If the SMOOTH= option is included in the code, a separate loess regression is fitted for every listed value of the smoothing parameter.
* SCORE语句指定一个数据集，该数据集包含需要获得预测值的点。 这些值放在ScoreResults语句中给定的数据集中，可以通过打印此文件来查看。 数据中包含评分结果的最后一列称为P响应，是预测值的列
*   对于局部拟合，SAS使用定义为

    $$
    w(\mathbf{x})=\left\{\begin{array}{ll}
    \frac{32}{5}\left(1-\|\mathbf{x}\|^{3}\right)^{3}, & \text { if }\|\mathbf{x}\| \leq 1 \\
    0, & \text { otherwise }
    \end{array}\right.
    $$

### SAS: Plotting Fitted Loess Curve

#### A two-dimensional scatterplot

```
SYMBOL1 COLOR=black VALUE=dot; /*black dots on scatterplot*/
SYMBOL2 COLOR=black VALUE=none INTERPOL=join LINE=1;
/*curve displayed as solid */
SYMBOL3 COLOR=black VALUE=none INTERPOL=join LINE=2;
/*LowerCL band is displayed as dashed*/
SYMBOL4 COLOR=black VALUE=none INTERPOL=join LINE=2;
/*UpperCL band is displayed as dashed*/

PROC GPLOT data=output data name;
BY SmoothingParameter;
PLOT (DepVar Pred LowerCL UpperCL)*predictor / OVERLAY
NAME=’graph name’;
RUN;

PROC GREPLAY IGOUT=WORK.GSEG TC=SASHELP.TEMPLT
TEMPLATE=template name NOFS;
TREPLAY 1:graph name 2:graph name1 ... ;
RUN;
```

#### The 3D Scatterplot

```
PROC G3D data=data name;
SCATTER predictor1*predictor2=response/ COLOR="color";
RUN;
```

#### Plotting Fitted Loess Surface

除非有足够的3D网格点可以绘制平面，否则SAS将不会显示合适的Loess表面。 作为补救措施，应创建一个带有网格点的数据集，并将其包括在LOESS过程的SCORE选项中。 可以使用以下语法：

```
DATA grid points data name;
DO predictor1 = min TO max BY increment;
DO predictor2 = min TO max BY increment;
OUTPUT;
END;
END;
RUN;

PROC G3D DATA=score results data name;
PLOT predictor1*predictor2 =P response/ CTOP=color CBOTTOM=color;
RUN;
```

### Examples

```
data unempl_rates;
input rate @@;
month= N ;
datalines;
6.0 6.7 4.9 4.4 5.8 4.8 5.5 6.7 4.7 5.6 6.5 6.0
4.7 5.1 7.2 6.1 7.7 5.7 7.1 4.2 5.8 5.1 6.3 5.1
3.9 4.7 4.4 5.9 4.1 5.8 4.9 5.4 3.9 6.0 4.1 4.6
5.7 5.0 4.5 6.9 5.6 4.6 4.4 4.1 3.2 6.3 4.2 4.7
4.3 4.3 4.5 6.7 3.9 4.6 5.8 3.8 5.5 4.7 5.0 4.2
5.0 4.5 3.7 5.5 5.4 2.6 5.0 4.9 5.7 4.3 5.3 7.1
7.5 4.1 5.1 5.7 4.8 6.1 6.3 4.1 5.7 7.2 6.0 7.2
8.0 8.7 8.5 9.1 7.5 10.5 8.5 7.4 10.5 8.9 8.5 9.9
8.3 9.9 7.2 9.5 10.5 11.9 11.4 8.0 10.5 11.2 9.2 9.5
10.0 10.3 9.1 8.1 7.9 9.5 10.7 8.5 9.1 8.7 9.0 8.6
;
proc loess data=unempl_rates;
model rate=month/clm;
ods output OutputStatistics=results;
run;
proc print data=results;
run;
symbol1 color=black value=dot;
symbol2 color=black value=none interpol=join line=1;
symbol3 color=black value=none interpol=join line=2;
symbol4 color=black value=none interpol=join line=2;
proc gplot data=results;
by SmoothingParameter;
plot (DepVar Pred LowerCL UpperCL)*month/ overlay;
run;
```

![](<../.gitbook/assets/image (72).png>)

```
proc loess data=unempl_rates;
model rate=month/ clm smooth=0.05 0.08 0.1 0.12;
ods output OutputStatistics=results;
run;
proc gplot data=results;
by SmoothingParameter;
plot (DepVar Pred LowerCL UpperCL)*month/ overlay name=’graph’;
run;
proc greplay igout=work.gseg tc=sashelp.templt template=l2r2 nofs;
treplay 1:graph 2:graph2 3:graph1 4:graph3;
run;
```

![](<../.gitbook/assets/image (73).png>)

## Thin-Plate Smoothing Spline Method

### Definition

该方法基于惩罚最小二乘估计技术，被称为薄板平滑样条方法。假设正在通过所有可能函数类别中的标准最小二乘法估算响应函数f。这意味着选择拟合函数是为了使残差平方和

$$
\sum_{i=1}^{n} \varepsilon_{i}^{2}=\sum_{i=1}^{n}\left(y_{i}-f\left(x_{1 i}, x_{2 i}, \cdots, x_{k i}\right)\right)^{2}
$$

最小，并且没有限制以拟合函数的形式进行。在这种设置下，线性连接数据点的函数是一种可能的解决方案，因为它将残留的平方和减小为零。从统计学上讲，它不是一个合适的函数，因为它不允许随机误差。为了消除这种解决方案，必须引入对响应函数的功能形式的限制。这种限制称为平滑罚分_**smoothing penalty**_，要求f必须是数据范围内的m倍可微函数，其中m称为函数f的平滑度。在此限制下，f的惩罚最小二乘估计量最小

$$
\frac{1}{n} \sum_{i=1}^{n}\left(y_{i}-f\left(x_{1 i}, x_{2 i}, \cdots, x_{k i}\right)\right)^{2}+\lambda J_{m}(f)
$$

where $$\lambda$$ is called the smoothing parameter, and

$$
J_{m}(f)=\int_{-\infty}^{\infty} \cdots \int_{-\infty}^{\infty} \sum \frac{m !}{\alpha_{1} ! \alpha_{2} ! \cdots \alpha_{k} !}\left(\frac{\partial^{2} f}{\partial x_{1}^{\alpha_{1}} \partial x_{2}^{\alpha_{2}} \cdots \partial x_{k}^{\alpha_{k}}}\right)^{2} d x_{1} \cdots d x_{k}
$$

with $$\alpha_{1}+\alpha_{2}+\cdots+\alpha_{k}=m$$. 例如，对于$$m=2$$，$$J_{2}(f)$$中的被积是二阶偏导数的平方和(**second-order partial derivative**)加上两倍混合二阶偏导数的平方和，即

$$
\begin{array}{c}
J_{2}(f)=\int_{-\infty}^{\infty} \cdots \int_{-\infty}^{\infty}\left[\left(\frac{\partial^{2} f}{\partial x_{1}^{2}}\right)^{2}+\cdots+\left(\frac{\partial^{2} f}{\partial x_{k}^{2}}\right)^{2}+2\left(\frac{\partial^{2} f}{\partial x_{1} \partial x_{2}}\right)^{2}+\cdots\right. \\
\left.+2\left(\frac{\partial^{2} f}{\partial x_{k-1} \partial x_{k}}\right)^{2}\right] d x_{1} \cdots d x_{k}
\end{array}
$$

For instance, in the case of a single predictor $$x_{1}=x$$,

$$
J_{2}(f)=\int_{-\infty}^{\infty}\left(f^{\prime \prime}(x)\right)^{2} d x
$$

and in the case of two predictors $$x_{1}$$ and $$x_{2}$$,

$$
J_{2}(f)=\int_{-\infty}^{\infty} \int_{-\infty}^{\infty}\left[\left(\frac{\partial^{2} f}{\partial x_{1}^{2}}\right)^{2}+2\left(\frac{\partial^{2} f}{\partial x_{1} \partial x_{2}}\right)^{2}+\left(\frac{\partial^{2} f}{\partial x_{2}^{2}}\right)^{2}\right] d x_{1} d x_{2}
$$

### SAS: Fitting Spline

```
PROC TPSPLINE DATA=data name;
MODEL response=(predictor1 predictor2 ... predictor k)/
M=value ALPHA=value;
OUTPUT OUT=output data name PRED LCLM UCLM;
SCORE DATA=point4prediction data name OUT=predicted data name;
RUN;
```

### SAS: Plotting Fitted Spline Curve

```
SYMBOL1 color=black value=dot color=black;
SYMBOL2 color=black value=none interpol=join line=1;
SYMBOL3 color=black value=none interpol=join line=2;
SYMBOL4 color=black value=none interpol=join line=2;
PROC GPLOT data=output data name;
PLOT (response P response LCLM response
UCLM response)*predictor / OVERLAY;
RUN;
```

Plotting Fitted Spline Surface

```
DATA grid points data name;
DO predictor1=min TO max BY increment;
DO predictor2=min TO max BY increment;
OUTPUT;
END;
END;
RUN;

## Next, the procedure TPSPLINE is invoked with the SCORE statement included
PROC G3D data=predicted data name;
PLOT predictor1*predictor2=P response;
RUN;
```

### Example

```
data unempl_rates;
input rate @@;
month= N ;
datalines;
6.0 6.7 4.9 4.4 5.8 4.8 5.5 6.7 4.7 5.6 6.5 6.0
4.7 5.1 7.2 6.1 7.7 5.7 7.1 4.2 5.8 5.1 6.3 5.1
3.9 4.7 4.4 5.9 4.1 5.8 4.9 5.4 3.9 6.0 4.1 4.6
5.7 5.0 4.5 6.9 5.6 4.6 4.4 4.1 3.2 6.3 4.2 4.7
4.3 4.3 4.5 6.7 3.9 4.6 5.8 3.8 5.5 4.7 5.0 4.2
5.0 4.5 3.7 5.5 5.4 2.6 5.0 4.9 5.7 4.3 5.3 7.1
7.5 4.1 5.1 5.7 4.8 6.1 6.3 4.1 5.7 7.2 6.0 7.2
8.0 8.7 8.5 9.1 7.5 10.5 8.5 7.4 10.5 8.9 8.5 9.9
8.3 9.9 7.2 9.5 10.5 11.9 11.4 8.0 10.5 11.2 9.2 9.5
10.0 10.3 9.1 8.1 7.9 9.5 10.7 8.5 9.1 8.7 9.0 8.6
;

## The degree of smoothness m = 2 is the default value in SAS.
proc tpspline data=unempl rates;
model rate=(month)/m=1;
output out=result pred lclm uclm;
run;

title ’m=1’;
symbol1 color=black value=dot;
symbol2 color=black value=none interpol=join line=1;
symbol3 color=black value=none interpol=join line=2;
symbol4 color=black value=none interpol=join line=2;
proc gplot data=result;
plot (rate p rate lclm rate uclm rate)*month /overlay;
run;
```

![](<../.gitbook/assets/image (74).png>)

## Nonparametric Generalized Additive Regression

广义加性模型是非参数线性回归模型的扩展。 假设我们获得了k个预测变量$$x_{1}, \ldots, x_{k}$$和响应y的n个独立观测值的样本。 广义加性回归模型用于通过链接函数g（·）连接mean response$$\mu=\mathbb{E}\left(y \mid x_{1}, \ldots, x_{k}\right)$$和形式为$$s{0}+s{1}\left(x{1}\right)+\cdots+s{k}\left(x_{k}\right)$$的预测变量的加法函数, 其中s0是截距，$$s_{1}(\cdot), \ldots, s_{k}(\cdot)$$是Loess或单变量样条平滑器。广义加性模型的方程为

$$
g(\mu)=s_{0}+s_{1}\left(x_{1}\right)+\cdots+s_{k}\left(x_{k}\right)
$$

使用参数化广义线性模型方法时，如果y为二进制，则应该拟合普通的logistic回归模型。 如果y是一个计数变量，则使用Poisson回归模型是合适的。 如果回归项的线性可能不成立，则非参数广义加性模型是一种解决方案。

### Nonparametric Binary Logistic Model

The nonparametric binary logistic model has the form

$$
g(\pi)=\operatorname{logit}(\pi)=\ln \left(\frac{\pi}{1-\pi}\right)=s_{0}+s_{1}\left(x_{1}\right)+\cdots+s_{k}\left(x_{k}\right)
$$

 其中s0是截距，$$s_{1}(\cdot), \ldots, s_{k}(\cdot)$$是Loess或单变量样条平滑器。

该模型可以改写为

$$
\pi=\frac{\exp \left\{s_{0}+s_{1}\left(x_{1}\right)+\cdots+s_{k}\left(x_{k}\right)\right\}}{1+\exp \left\{s_{0}+s_{1}\left(x_{1}\right)+\cdots+s_{k}\left(x_{k}\right)\right\}}
$$

并且可以用于针对预测变量的某些特定值来估计事件y = 1的概率。 

作为纯非参数逻辑模型的替代，可以考虑半参数模型。 在此模型中，某些预测变量具有线性影响，因此仅需为这些变量估算斜率。 半参数模型的形式为

$$
\pi=\frac{\exp \left\{s_{0}+s_{1} x_{1}+\cdots+s_{i} x_{i}+s_{i+1}\left(x_{i+1}\right)+\cdots+s_{k}\left(x_{k}\right)\right\}}{1+\exp \left\{s_{0}+s_{1} x_{1}+\cdots+s_{i} x_{i}+s_{i+1}\left(x_{i+1}\right)+\cdots+s_{k}\left(x_{k}\right)\right\}}
$$

where $$s_{0}, \ldots, s_{i}$$ are constants and $$s_{i+1}(\cdot), \ldots, s_{k}(\cdot)$$ are smoothing functions. The predictor variables $$x_{1}, \ldots, x_{i}$$ are called regression variables, whereas $$x_{i+1}, \ldots, x_{k}$$ are termed smoothing variables.

#### SAS Implementation

Three smoothing techniques may be specified: LOESS(·) for loess regression, SPLINE(·) for a cubic spline (a univariate thin-plate spline with the degree of smoothness m = 2), or SPLINE2(·,·) for a bivariate thin-plate spline. The syntax for fitting a semiparametric model with the loess regression smoothers, for instance, is

```
PROC GAM DATA=data name;
CLASS list of categorical predictors;
MODEL response(EVENT=’level name’) = PARAM(list of regression predictors)
LOESS(smoothing predictor 1) ... LOESS (smoothing predictor k)
/LINK=LOGIST DIST=BINOMIAL;
OUTPUT PUT=output data name PRED;
SCORE DATA=points4prediction data name out=predicted points data name;
RUN;


data artery_disease;
input age BMI PAD @@;
datalines;
65 27.3 0 26 20.3 0 46 15.7 0 22 28.5 0 71 19.4 1 33 16.2 1
28 14.3 0 64 20.5 0 73 18.8 1 85 22.4 1 43 26.4 1 51 26.4 0
60 24.3 1 83 18.9 1 26 19.7 0 71 18.7 1 24 21.3 0 71 18.7 1
48 16.4 1 33 18.8 0 52 21.8 1 70 19.8 0 40 23.4 0 20 22.1 0
85 18.1 1 35 20.6 1 37 18.4 0 54 22.1 0 55 15.5 1 26 19.5 0
33 17.5 0 65 26.2 1 21 21.2 1 76 21.1 1 83 25.5 1 68 20.9 1
31 20.9 0 70 24.8 0 67 21.3 0 34 20.6 1 30 24.4 0 76 19.8 0
26 18.8 0 71 23.2 1 41 25.0 1 73 19.9 1 29 18.7 1 48 23.6 0
34 23.4 0 23 17.5 1 82 16.5 1 23 24.9 0 61 19.9 1 42 18.7 0
47 26.6 1 54 20.2 1 75 14.4 0 52 27.5 1 26 21.3 1 61 21.3 1
51 28.9 0 85 26.7 1 45 26.5 1 77 20.6 0 59 26.9 1 73 22.8 1
20 17.9 0
;

/***********************************************/
/* LOESS(age) LOESS(BMI) */
data point4pred;
input age BMI;
cards;
50 21.4
;
data grid_points;
do age=20 to 85 by 1;
do BMI=14 to 29 by 1;
output;
end;
end;
run;

proc gam data=artery_disease;
model PAD(event=’1’)=loess(age) loess(BMI)
/link=logist dist=binomial;
output out=result pred;
score data=point4pred out=predicted;
score data=grid_points out=score_results;
run;
title ’LOESS(age) LOESS(BMI)’;
proc g3d data=score_results;
plot age*BMI=P_PAD/ctop=black cbottom=gray name=’graph1’;
run;
proc print data=predicted;
run;

/***********************************************/
/* SPLINE(age) SPLINE(BMI) */
proc gam data=artery_disease;
model PAD(event=’1’)=spline(age) spline(BMI)
/link=logist dist=binomial;
output out=result pred;
score data=point4pred out=predicted;
score data=grid_points out=score_results;
run;
title ’SPLINE(age) SPLINE(BMI)’;
proc g3d data=score_results ;
plot age*BMI=P_PAD/ctop=black cbottom=gray name=’graph2’;
run;
proc print data=predicted;
run;

/*************************************************/
/* SPLINE2(age,BMI) */
proc gam data=artery_disease;
model PAD(event=’1’)=spline2(age, BMI)
/link=logist dist=binomial;
output out=result pred;
score data=point4pred out=predicted;
score data=grid_points out=score_results;
run;
title ’SPLINE2(age, BMI)’;
proc g3d data=score_results;
plot age*BMI=P_PAD/ctop=black cbottom=gray name=’graph3’;
run;
proc print data=predicted;
run;


proc greplay igout=work.gseg tc=sashelp.templt template=l2r2
nofs;
treplay 1:graph1 2:graph3 3:graph2;
run;

```

![](<../.gitbook/assets/image (75).png>)

### Nonparametric Poisson Model

假设响应变量Y假定为整数值0、1、2等。这种观察类型称为计数数据。 如果存在以下特征，则计数频率遵循泊松分布： 

* 样本均值大约等于样本方差。 
* 较小的值是最常见的观察结果。 
* 大观察很少发生。 
* 观察不到太多零。



The Poisson regression model specifies that the response variable $$Y,$$ given predictors $$X_{1}, \ldots, X_{k},$$ follows a Poisson distribution with the probability mass function

$$
\mathbb{P}\left(Y=y \mid X_{1}=x_{1}, \ldots, x_{k}=x_{k}\right)=\frac{\lambda^{y}}{y !} e^{-\lambda}, \quad y=0,1,2, \ldots
$$

where, in the case of

*   parametric regression, the rate $$\lambda$$ satisfies

    $$
    \ln \lambda=\ln \mathbb{E}\left(Y \mid x_{1}, \ldots, x_{k}\right)=\beta_{0}+\beta_{1} x_{1}+\cdots+\beta_{k} x_{k}
    $$
*   nonparametric additive regression,

    $$
    \ln \lambda=s_{0}+s_{1}\left(x_{1}\right)+\cdots+s_{k}\left(x_{k}\right)
    $$

    where $$s_{0}$$ is a constant and $$s_{1}(\cdot), \ldots, s_{k}(\cdot)$$ are loess regression or cubic spline smoothers.
*   semiparametric additive regression,

    $$
    \ln \lambda=s_{0}+s_{1} x_{1}+\cdots+s_{i} x_{i}+s_{i+1}\left(x_{i+1}\right)+\cdots+s_{k}\left(x_{k}\right)
    $$

    where $$s_{0}, \ldots, s_{i}$$ are constants, and $$s_{i+1}(\cdot), \ldots, s_{k}(\cdot)$$ are smoothers.

#### SAS Implementation

```
PROC GAM DATA=data name;
CLASS list of categorical predictors;
MODEL response = PARAM(list of regression predictors)
LOESS(smoothing predictor 1) ... LOESS (smoothing predictor k)
/LINK=LOG DIST=POISSON;
OUTPUT PUT=output data name PRED;
SCORE DATA=points4prediction data name out=predicted points data name;
RUN;
```

#### Example to fit

$$
\ln \lambda=s_{0}+s_{1} \text { poor }+s_{2} \text { fair }+s_{3} \text { good }+s_{4}(\text { age })
$$

```
data docvisits;
input health $ age n_visits @@;
datalines;

exclnt 18 0 exclnt 53 1 good 39 3 good 59 1
good 24 0 exclnt 39 1 good 51 3 good 57 1
exclnt 23 0 good 42 1 fair 56 4 fair 56 1
exclnt 48 0 exclnt 43 1 good 66 4 exclnt 38 1
exclnt 23 0 good 52 1 good 59 4 good 38 1
exclnt 28 0 fair 37 3 fair 67 4 good 41 1
good 54 0 exclnt 54 3 fair 68 4 fair 40 1
good 29 0 exclnt 40 3 fair 69 4 exclnt 36 1
good 35 0 good 56 3 fair 58 4 good 42 1
good 44 0 good 67 3 fair 57 4 fair 52 1
fair 56 0 fair 47 3 fair 54 2 good 41 1
good 33 1 good 41 3 exclnt 44 2 good 27 1
good 52 1 fair 41 3 good 57 2 fair 25 2
good 50 1 good 39 3 good 57 2 good 44 2
fair 49 1 good 52 3 fair 59 2 fair 50 2
fair 48 1 fair 47 3 good 53 2 good 32 2
exclnt 22 1 fair 70 3 good 52 2 good 53 2
exclnt 39 1 good 62 3 exclnt 43 2 exclnt 56 2
exclnt 54 1 good 50 3 good 28 2 fair 45 2
good 40 1 good 52 3 good 52 2 good 50 2
good 47 1 good 68 3 fair 60 2 fair 49 2
exclnt 44 1 good 57 3 exclnt 19 2 good 21 2
fair 55 1 fair 58 3 good 60 2 good 44 2
good 43 1 good 49 3 fair 33 2 good 55 2
fair 50 1 fair 68 3 good 43 3 exclnt 36 2
good 72 4 fair 54 3 good 48 2 poor 77 6
good 67 4 good 66 3 good 48 4 poor 74 6
fair 78 5 good 33 3 good 60 4 fair 58 6
fair 70 5 exclnt 30 3 good 66 4 good 68 7
poor 72 5 good 62 3 good 66 4 poor 68 7
poor 67 5 good 60 3 fair 66 4 poor 71 8
fair 74 5 fair 64 3 good 65 4 fair 55 2
fair 54 5 good 38 3 fair 69 4 good 58 2
poor 62 5 exclnt 66 3 good 58 4 good 49 2
good 73 5 exclnt 28 3 good 69 4 exclnt 24 2
good 55 2 poor 69 5 fair 64 4 good 44 3
exclnt 49 2 fair 69 6 fair 45 2 poor 77 6
good 46 2 poor 75 6
;
proc sort data=docvisits;
by descending health; /* To make "excellent" a reference */
run;
proc gam data=docvisits;
class health (order=data);
model n_visits=param(health) spline(age)/link=log
dist=poisson;
output out=result pred;
run;
proc print data=result;
run;


## Output
A partial output is
Parameter Estimate Pr > |t|
Intercept -0.72057 0.0051
health poor 0.55264 0.0294
health good 0.19599 0.3007
health fair 0.27289 0.1820
Linear(age) 0.02663 <.0001


```

The estimated rate $$\widehat{\lambda}$$ has the form

$$
\begin{aligned}
\ln \widehat{\lambda}=&-0.72057+0.55264 \mathrm{poor}+0.27289 \text { fair }+0.19599 \text { good } \\
&+0.02663 \text { age }+\mathrm{P}_{-} \text {age } .
\end{aligned}
$$

