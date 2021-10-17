# GEE

## Generalized Estimating Equation

### **Generalized Linear Model**

GLM方法关心的是：当响应变量不服从正态分布，也不连续，应该如何去挖掘隐藏在这样数据中的信息。Nelder & Wedderburn 指出，如果响应变量服从指数族分布时，我们可以通过GLM方法去研究响应变量的均值与自变量之间的协变（Association）。常见的指数分布族包括：正态（连续），0-1（离散），二项（离散），泊松（离散）分布。

#### Verteilungsannahme:

目标变量$$Y_{i}$$ 计数度量的密度来自一个指数族，即对于公共载体$$\mathbb{T}$$所有值$$Y_{i}$$

$$
f\left(y_{i} \mid \theta_{i}, \phi, \omega_{i}\right)=f\left(y_{i} \mid \theta_{i}\right)=\exp \left\{\frac{y_{i} \theta_{i}-b\left(\theta_{i}\right)}{\phi} \omega_{i}-c\left(y_{i}, \phi, \omega_{i}\right)\right\}
$$

#### Strukturannahme:

$$
\mu_{i}=E\left(Y_{i}\right)=b^{\prime}\left(\theta_{i}\right)=h\left(\eta_{i}\right)
$$

$$
\text {for } \eta_{i}=\mathbf{x}_{i} \beta
$$

$$
\mathbf{x}_{i} \beta=\eta_{i}=g\left(\mu_{i}\right)=g\left[b^{\prime}\left(\theta_{i}\right)\right] \text { für } g=h^{-1}
$$

für stochastisch unabhängige Beobachtungen für die Score-Funktion

$$
\mathbf{s}(\beta)=\frac{\partial}{\partial \beta} I(\beta)=\sum_{i=1}^{n} \mathbf{x}_{i}^{T} \frac{h^{\prime}\left(\eta_{i}\right)}{v\left(\mu_{i}\right)}\left\{Y_{i}-\mu_{i}\right\} \omega_{i} / \phi
$$

Der MLE für $$\beta$$ wurde dann als Lösung der Schätzgleichung

$$
\sum_{i=1}^{n} \mathbf{x}_{i}^{T} \frac{h^{\prime}\left(\eta_{i}\right)}{v\left(\mu_{i}\right)}\left\{Y_{i}-\mu_{i}\right\} \omega_{i} / \phi=\mathbf{0}
$$

#### Link function

* Traditional Linear Model: $$g(\mu)=\mu$$ 
* Logistic Regression (logit): $$g(\mu)=\log \left(\frac{\mu}{1-\mu}\right)$$
* Poisson Regression in Log Linear Model: $$g(\mu)=\log (\mu)$$

### GEE

GLM方法需要假设响应变量之间是相互独立的，同时需要服从于指数分布族。然而，在这些领域，每个个体常常是重复测量（repeated measurement）的，或是存在聚类（cluster）关系。例如，在医学领域，病人的体温、心跳、血压等医学指标会每X小时进行一次测量。当该病人出院后，其重复测量的医学指标事实上就是一条时间序列，而多个（独立）病人的医学指标序列便形成了重复测量数据，也常被称作纵向数据（longitudinal data）与面板数据（panel data）。需要指出的是，纵向数据容许非平衡测量（unbalance），而面板数据通常都指平衡测量（balance）。

非平衡的含义是：可以容许病人的测量间隔不同，而测量总次数也不同。\
平衡的含义是：测量总次数相同，测量间隔可以看做是相同的。

* Die Zufallsvariable $$Y_{i j}$$ ist die $$j$$ -te Messung der Zielvariable für das $$i$$ -te Individuum, $$i=1, \ldots, N, j=1, \ldots, n_{i}$$
* $$\mathbf{x}_{i j}$$ ist der zugehörige Kovariablenvektor

#### Verteilungsannahme:

目标变量$$Y_{ij}$$ 计数度量的密度来自一个指数族，即对于公共载体$$\mathbb{T}$$所有值$$Y_{ij}$$

$$
f\left(y_{i j} \mid \theta_{i j}, \phi, \omega_{i j}\right)=f\left(y_{i j} \mid \theta_{i j}\right)=\exp \left\{\frac{y_{i j} \theta_{i j}-b\left(\theta_{i j}\right)}{\phi} \omega_{i j}-c\left(y_{i j}, \phi, \omega_{i j}\right)\right\}
$$

#### Strukturannahme:

$$
\mu_{i j}=E\left(Y_{i j}\right)=b^{\prime}\left(\theta_{i j}\right)=h\left(\eta_{i j}\right)
$$

$$
\eta_{i j}=\mathbf{x}_{i j} \beta
$$

$$
\mathbf{x}_{i j} \beta=\eta_{i j}=g\left(\mu_{i j}\right)=g\left[b^{\prime}\left(\theta_{i j}\right)\right], \ \ g=h^{-1}
$$

### Score Function and MLE

$$
\mathbf{s}(\beta)=\sum_{i=1}^{N} \sum_{j=1}^{n_{i}} \mathbf{x}_{i j}^{T} \frac{h^{\prime}\left(\eta_{i j}\right)}{v\left(\mu_{i j}\right)}\left\{Y_{i j}-\mu_{i j}\right\} \omega_{i j} / \phi
$$

Der MLE für $$\beta$$ kann dann als Lösung der Schätzgleichung bestimmt werden (z.B. numerisch via Fisher-Scoring)

$$
\sum_{i=1}^{N} \sum_{j=1}^{n_{i}} \mathbf{x}_{i j}^{T} \frac{h^{\prime}\left(\eta_{i j}\right)}{v\left(\mu_{i j}\right)}\left\{Y_{i j}-\mu_{i j}\right\} \omega_{i j} / \phi=\mathbf{0}
$$

在各个级别上用矩阵符号表示此估计方程：

$$
\sum_{i=1}^{N} \mathbf{X}_{i}^{T} \mathbf{D}_{i} \mathbf{V}_{i}^{-1}\left(\mathbf{Y}_{i}-\mu_{i}(\beta)\right)=\mathbf{0}
$$

$$
\mathbf{X}_{i}=\left(\begin{array}{c}
\mathbf{x}_{i 1} \\
\vdots \\
\mathbf{x}_{i n_{i}}
\end{array}\right), \quad \mathbf{D}_{i}=\left(\begin{array}{cccc}
h^{\prime}\left(\eta_{i 1}\right) & & 0 \\
& \ddots & \\
0 & & h^{\prime}\left(\eta_{i n_{i}}\right)
\end{array}\right) ,\quad \mathbf{V}_{i}=\phi\left(\begin{array}{ccc}
\frac{v\left(\mu_{i 1}\right)}{\omega_{i 1}} & & 0 \\
& \ddots & \\
0 & & \frac{v\left(\mu_{i n_{i}}\right)}{\omega_{i n_{i}}}
\end{array}\right)
$$

估计方程中的矩阵$$V_i$$本质上描述了第i个个体的数据的协方差结构, 可以表示如下

$$
\mathbf{V}_{i}=\phi \mathbf{A}_{i}^{1 / 2} \mathbf{W}_{i}^{-1 / 2} \mathbf{I}_{n_{i}} \mathbf{W}_{i}^{-1 / 2} \mathbf{A}_{i}^{1 / 2}
$$

$$
\mathbf{A}_{i}=\left(\begin{array}{ccc}
v\left(\mu_{i 1}\right) & & 0 \\
& \ddots & \\
0 & & v\left(\mu_{i n_{i}}\right)
\end{array}\right) \quad \text { und } \quad \mathbf{W}_{i}=\left(\begin{array}{ccc}
\omega_{i 1} & & 0 \\
& \ddots & \\
0 & & \omega_{i n_{i}}
\end{array}\right)
$$

显然，估计方程将个体内的观察视为独立的, 单位矩阵 $$\mathbf{I}_{n_{i}}$$ 是个体内结果的边际分布的相关矩阵. 我们通过将矩阵$$\mathbf {I} _ {n_ {i}}$$替换为更通用的相关矩阵$$\mathbf {R} _ {i}(\alpha)$$来概括估算方程

### Quasi-Likelihood

#### generalized estimating equation – GEE

$$
\sum_{i=1}^{N} \mathbf{X}_{i}^{T} \mathbf{D}_{i} \mathbf{V}_{i}^{-1}\left(\mathbf{Y}_{i}-\mu_{i}(\beta)\right)=\mathbf{0}
$$

$$
\mathbf{V}_{i}=\phi \mathbf{A}_{i}^{1 / 2} \mathbf{W}_{i}^{-1 / 2} \mathbf{R}_{i}(\alpha) \mathbf{W}_{i}^{-1 / 2} \mathbf{A}_{i}^{1 / 2}
$$

不再假设个体内的观察是独立的, estimation equation is not the score equation of a likelihood (quasi-likelihood)

#### Estimation for $$\mathbf {R} _ {i}(\alpha)$$

通常$$\mathbf {R} _ {i}(\alpha)$$是通过Pearson残差参数化的

$$
e_{i j}=\frac{Y_{i j}-\mu_{i j}}{\sqrt{v\left(\mu_{i j}\right) / \omega_{i j}}}
$$

估算器$$\hat{\beta}$$für$$\beta$$可以通过以下方式估算

$$
\hat{e}_{i j}=\frac{Y_{i j}-\hat{\mu}_{i j}}{\sqrt{v\left(\hat{\mu}_{i j}\right) / \omega_{i j}}}
$$

wobei $$\hat{\mu}_{i j}=h\left(\mathbf{x}_{i} \hat{\beta}\right)$$

#### Estimation for Dispersionsparameters

$$
\hat{\phi}=\frac{1}{N-k} \sum_{i=1}^{n} \sum_{j=1}^{n_{i}} \frac{\left(Y_{i j}-\hat{\mu}_{i j}\right)^{2}}{v\left(\hat{\mu}_{i j}\right)} \omega_{i j}=\frac{1}{N-k} \sum_{i=1}^{n} \sum_{j=1}^{n_{i}} \hat{e}_{i j}^{2}
$$

#### Calculation $$\hat \beta$$ 

1. 假设独立性（例如MLE），根据常规GLM计算$$\beta$$的第一个估算器
2. 计算估计的皮尔逊残差，从而计算$$\mathbf{R}_{i}(\alpha)$$和$$\phi$$的估计值
3.  计算协方差的估计量

    $$
    \hat{\mathbf{V}}_{i}=\hat{\phi} \hat{\mathbf{A}}_{i}^{1 / 2} \mathbf{W}_{i}^{-1 / 2} \mathbf{R}_{i}(\hat{\alpha}) \mathbf{W}_{i}^{-1 / 2} \hat{\mathbf{A}}_{i}^{1 / 2}
    $$
4.  更新$$\hat{\beta}$$通过

    $$
    \hat{\beta}_{r+1}=\hat{\beta}_{r}+\left[\sum_{i=1}^{N} \mathbf{X}_{i}^{T} \mathbf{D}_{i} \mathbf{V}_{i}^{-1} \mathbf{D}_{i} \mathbf{X}_{i}\right]^{-1} \sum_{i=1}^{N} \mathbf{X}_{i}^{T} \mathbf{D}_{i} \mathbf{V}_{i}^{-1}\left(\mathbf{Y}_{i}-\mu_{i}(\beta)\right)
    $$
5. 重复2-4。 直到收敛

### Eigenschaft

$$\hat{\beta}$$ 仍然是一致的（并且近似正态分布）

$$
(\hat{\beta}-\beta) \stackrel{a}{\sim} N\left(\mathbf{0},\left[\sum_{i=1}^{N} \mathbf{X}_{i}^{T} \mathbf{D}_{i} \mathbf{V}_{i}^{-1} \mathbf{D}_{i} \mathbf{X}_{i}\right]^{-1}\right)
$$

**如果**$$\mathbf{R}_{i}(\alpha)$$**错误指定**

通常，$$\mathbf{R}_{i}(\alpha)$$不对应于真实的相关结构 $$\mathbf{R}_{i}(\alpha)$$ : working correlation matrix 认为是一个工作相关矩阵，不一定与真相对应. (在协方差的估计中必须考虑方差结构的错误指定)

但是即使$$\mathbf{R}_{i}(\alpha)$$与实际的相关结构不对应，也可以显示：

$$
(\hat{\beta}-\beta) \stackrel{a}{\sim} N\left(\mathbf{0}, \widehat{\operatorname{Cov}_{S}}(\hat{\beta})\right)
$$

$$\widehat{\operatorname{Cov}}_{S}(\hat{\beta})$$ 称为robust 鲁棒方差估计器。 使用所谓的**三明治估计**来估计协方差：sandwich-estimate

$$
\widehat{\operatorname{Cov}}_{S}(\hat{\beta})=\left[\sum_{i=1}^{N} \mathbf{X}_{i}^{T} \mathbf{D}_{i} \mathbf{V}_{i}^{-1} \mathbf{D}_{i} \mathbf{X}_{i}\right]^{-1} \sum_{i=1}^{N} \mathbf{X}_{i}^{T} \mathbf{D}_{i} \mathbf{V}_{i}^{-1} \operatorname{Cov}\left(\mathbf{Y}_{i}\right) \mathbf{V}_{i}^{-1} \mathbf{D}_{i} \mathbf{X}_{i}\left[\sum_{i=1}^{N} \mathbf{X}_{i}^{T} \mathbf{D}_{i} \mathbf{V}_{i}^{-1} \mathbf{D}_{i} \mathbf{X}_{i}\right]^{-1}
$$

$$\operatorname{Cov}\left(\mathbf{Y}_{i}\right)$$ 是未知的，并由其估算器代替：

$$
\widehat{\operatorname{Cov}}\left(\mathbf{Y}_{i}\right)=\left(\mathbf{Y}_{i}-\mu_{i}(\hat{\beta})\right)\left(\mathbf{Y}_{i}-\mu_{i}(\hat{\beta})\right)^{T}
$$

**如果**$$\mathbf{R}_{i}(\alpha)$$**正确指定**

Kovarianz $$\hat{\beta}$$$$\widehat{\operatorname{Cov}}(\hat{\beta})=\left[\sum_{i=1}^{N} \mathbf{X}_{i}^{T} \mathbf{D}_{i} \mathbf{V}_{i}^{-1} \mathbf{D}_{i} \mathbf{X}_{i}\right]^{-1}$$ 

$$
\widehat{\operatorname{Cov}}(\hat{\beta})=\widehat{\operatorname{Cov}}_{S}(\hat{\beta})
$$

### Testing linear hypotheses

$$
H_{0}: \mathbf{C} \beta=\mathbf{d} \text { vs. } H_{1}: \mathbf{C} \beta \neq \mathbf{d}
$$

Aus der Normalverteilungseigenschaft des Schätzers $$\hat{\beta}(\alpha)$$ folgt, dass

$$
W=(\mathbf{C} \hat{\beta}-\mathbf{d})^{T}\left[\mathbf{C} \widehat{\operatorname{Cov}}_{S}(\hat{\beta}) \mathbf{C}^{T}\right]^{-1}(\mathbf{C} \hat{\beta}-\mathbf{d}) \stackrel{a}{\sim} \chi_{r}^{2}
$$

mit $$r=\operatorname{Rang}(\mathbf{C})$$ Wir können $$H_{0}$$ verwerfen, falls $$W \geq Q_{r}^{\chi^{2}}(1-\alpha)$$

#### Score Tests 

#### Without Likelihood-Ratio-Test

#### Gauß-Test

Für einzelne Kontrasthypothesen $$H_{0}: \mathbf{c} \beta=d,$$ mit $$\mathbf{c} \in \mathbb{R}^{k}$$ und $$d \in \mathbb{R}$$ kann ein Gauß-Test benutzt werden. Verwerfe dafür $$H_{0}$$ zum Niveau $$\alpha,$$ falls

$$
Z=\frac{\mathbf{c} \hat{\beta}-d}{\mathbf{c} \operatorname{Cov}_{S}(\hat{\beta}) \mathbf{c}^{T}} \geq \Phi^{-1}(1-\alpha)
$$

* Dieser Test kann auch einseitig durchgeführt werden

### Working Correlation

The Generalized Estimating Equation for estimating $$\beta$$ is an extension of the independence estimating equation to correlated data and is given by

$$
\sum_{i=1}^{K} \frac{\partial \boldsymbol{\mu}_{i}^{\prime}}{\partial \boldsymbol{\beta}} \mathbf{V}_{i}^{-1}\left(\mathbf{Y}_{i}-\boldsymbol{\mu}_{i}(\boldsymbol{\beta})\right)=\mathbf{o}
$$

Let $$\mathbf{R}_{i}(\boldsymbol{\alpha})$$ be an $$n_{i} \times n_{i}$$ "working" correlation matrix that is fully specified by the vector of parameters $$\alpha$$. The covariance matrix of $$\mathrm{Y}_{i}$$ is modeled as

$$
\mathbf{V}_{i}=\phi \mathbf{A}_{i}^{\frac{1}{2}} \mathbf{R}(\boldsymbol{\alpha}) \mathbf{A}_{i}^{1}
$$

where $$\mathrm{A}$$ is an $$n_{i} \times n_{i}$$ diagonal matrix with $$v\left(\mu_{i j}\right)$$ as the th diagonal element. If $$\mathbf{R}_{i}(\boldsymbol{\alpha})$$ is the true correlation matrix of $$\mathrm{Y}_{i}$$, then $$\mathrm{V}_{i}$$ is the true covariance matrix of $$\mathbf{Y}_{i}$$

通常，$$\mathbf{R}_{i}(\alpha)$$不对应于真实的相关结构 $$\mathbf{R}_{i}(\alpha)$$ : working correlation matrix 认为是一个工作相关矩阵，不一定与真相对应. (在协方差的估计中必须考虑方差结构的错误指定)

#### Unabhängigkeit

$$
\mathbf{R}_{i}(\alpha)=\mathbf{I}_{n_{i}}
$$

#### Interchangeable correlation matrix (Austauschbare Korrelationsmatrix)

$$
\mathbf{R}_{i}(\alpha)=\left(\begin{array}{ccccc}
1 & \alpha & \alpha & \ldots & \alpha \\
\alpha & 1 & \alpha & \ldots & \alpha \\
\alpha & \alpha & 1 & \ldots & \alpha \\
\vdots & \vdots & \vdots & \ddots & \vdots \\
\alpha & \alpha & \alpha & \ldots & 1
\end{array}\right)
$$

#### AR(1) - Struktur

$$
\mathbf{R}_{i}(\alpha)=\left(\begin{array}{ccccc}
1 & \alpha & \alpha^{2} & \ldots & \alpha^{n_{i}} \\
\alpha & 1 & \alpha & \ldots & \alpha^{n_{i}-1} \\
\alpha^{2} & \alpha & 1 & \ldots & \alpha^{n_{i}-2} \\
\vdots & \vdots & \vdots & \ddots & \vdots \\
\alpha^{n_{i}} & \alpha^{n_{i}-1} & \alpha^{n_{i}-2} & \ldots & 1
\end{array}\right)
$$

$$
\hat{\alpha}=\frac{1}{\hat{\phi}(N-k)} \sum_{i=1}^{N} \sum_{j \leq n_{i}-1} \hat{e}_{i j} \hat{e}_{i, j+1}
$$

#### m-dependent

$$
\operatorname{Corr}\left(Y_{i j}, Y_{i, j+t}\right)=\left\{\begin{array}{ll}
1 & t=0 \\
\alpha_{t} & 0<t \leq m \\
0 & t>m
\end{array}\right.
$$

#### Unstructured

不想对相关性的结构做任何假设，则可以选择非结构化的工作相关性：在这种情况下，将估算观察值之间的所有成对相关性

$$
\operatorname{Corr}\left(Y_{i u}, Y_{i v}\right)=\left\{\begin{array}{ll}
1 & u=v \\
\alpha_{u v} & u \neq v
\end{array}\right.
$$

但是, 不能保证估计矩阵 $$\mathbf{R}_{i}(\alpha)$$ 是可逆的, 特别是对于不平衡的数据集（对个人的测量数量非常不同），可能会出现数字问题. 

对非结构化相关矩阵的修改是假设在两次测量之间的某个（时间）间隔不再存在任何相关性，否则无需进行任何假设 (非平稳相关矩阵)

#### nonstationary

$$
\operatorname{Corr}\left(Y_{i u}, Y_{i v}\right)=\left\{\begin{array}{ll}
1 & u=v \\
\alpha_{u v} & 0<|u-v| \leq m \\
0 & |u-v|>m
\end{array}\right.
$$

## PROC GENMOD

纵向模型数据分析涉及的步骤：

* 1）通过指定链接函数选择模型，该函数描述了希望使用的模型形式
* 2）选择方差-协方差结构（指定每个主题的工作相关结构） 
* 3）选择因变量的分布
* 4）评估模型的拟合优度和方差协方差结构

### Logistic regression on uncorrelated data

```
## Datasets
input id gender race maritalstat mi anthrax;
14 1 1 1 1 1
14 2 1 1 0 1
15 1 1 0 0 1
16 2 3 1 1 0
17 1 2 1 0 0
18 1 1 0 0 0
19 2 2 0 0 0
20 2 3 1 0 1
…
…
…
45 2 3 1 0 1
```

#### PROC LOGISTIC

$$
\begin{aligned}
\mathrm{P}(\mathrm{Y}=1)=& \frac{1}{1+\exp \left[-\beta_{0}+\left(\sum_{k=1}^{p} \beta_{k} \mathrm{X}_{k}\right)\right]}
\end{aligned}
$$

```
ods html path = 'c:\YourPath' body='Name.html';
proc logistic data=MIdata descending;
 class anthrax (ref = '0') race (ref='1') maritalstat (ref='0') 
       gender (ref='1') / param=reference;
 model mi=anthrax race maritalstat gender / clodds=wald clodds=pl lackfit;
title1 ‘Proc Logistic for Log Odds’;
run;
ods html close;
```

* **Descending**: SAS的默认值是对MI的因变量结果等于0的概率进行建模。降序选项使我们可以对MI等于1的概率进行建模，并将结果的概率与没有结果的概率进行比较。
* **Param = reference**要求使用参考单元编码来计算参数估计值，优势比和置信区间。 将使用效果编码方案来计算默认参数估计值，该效果编码方案估计每个非参考级别的效果与变量其他级别上的平均效果相比的差异。
* **Clodds**= 请求每个解释变量，即95％ **Wald or profile likelihood CI** for the odds ratios（默认Alpha级别，因为未调用ALPHA =选项）
* **Lackfit**请求对该模型进行Hosmer-Lemeshow拟合优度检验。 零假设是该模型与各个风险组的观察数据非常吻合（我们希望不能拒绝零）'**Hosmer and Lemeshow Goodness-of-Fit Test**'

#### PROC GENMOD

```
 proc genmod data=MIdata descending;
  class anthrax race maritalstat gender;
  model mi= anthrax race maritalstat gender / dist=binomial link=logit;
  estimate "Anthrax" anthrax -1 1 / exp;
  estimate "Sex" gender -1 1 / exp;
  estimate "Black" race -1 1 0 / exp;
  estimate "Hispan" race -1 0 1 / exp;
  estimate "maritalstat" maritalstat -1 1 / exp;
 run;
 quit;
```

* Estimate 将产生曝光效果的估计比值比值及其相关的标准误差和置信度限制。尽管CONTRAST语句测试均值的线性组合是否显着不同于0，但ESTIMATE语句的语法与CONTRAST语句的语法完全相同。应该指出的是，在link=logit语句后包括语句“ lrci”和“ waldci”，将生成有关参数估计值的wald和似然比置信区间。
* 输出优势比将反映出与PROC LOGISTIC中相同的比较。反斜杠后的Exp要求计算并输出参数估计值，标准误差和置信度限制。
* 与LOGISTIC过程不同，GENMOD过程不会对零假设的整体检验，即与仅具有截距的模型相比，拟合模型中所有参数合计等于0。

### Logistic regression on correlated data

```
proc genmod data=MIdata descending;
  class id anthrax race maritalstat gender ;
  model mi= anthrax race maritalstat gender / dist=binomial link=logit;
  repeated subject=id / type=cs corrw covb;
  estimate "Anthrax" anthrax -1 1 / exp;
  estimate "Sex" gender -1 1 / exp;
  estimate "Black" race -1 1 0 / exp;
  estimate "Hispan" race -1 0 1 / exp;
  estimate "maritalstat" maritalstat -1 1 / exp;
 run;
 quit;
```

* **Repeated** 调用GEE方法。在此行中，我们指定GEE模型拟合的多元响应的协方差结构，GEE中使用的迭代拟合算法以及可选输出。
* **Subject=id** 指定各个主题由变量id标识。变量id也必须在class语句中列出。效果的每个不同值或级别标识一个不同的主题或群集。假定来自不同主题的响应在统计上是独立的，并且假定主题内的响应是相关的。
* **Type=cs** 指定用于对受试者反应的相关性进行建模的工作相关性矩阵的结构。默认的工作相关类型是独立的。类型=的可能性包括 autoregressive (AR), exchangeable (EXCH or CS), independent (IND), m dependent (MDEP), unstructured (UN or UNSTR), and user specified correlation matrix (USER or FIXED).\
  如果响应是单变量类型，则分配是二项式的，数据是binary，而不是Type =语句，可以通过在repeated 语句中使用**logor**选项来采用交替的最小二乘。 这指定了 log odds ratio 的回归结构，该比对率比值用于对受试者对二进制数据的响应的关联进行建模，而不使用工作相关性。 logor =的可能性包括exchangeable, fully parameterized clusters, and nested, among others.(可交换的，完全参数化的群集和嵌套的群集)。
* **Corrw** 显示估计的工作相关矩阵。 
* **Covb **显示估计的回归参数协方差矩阵。 显示基于模型的协方差和基于经验的协方差。(**model-based and empirical**)

### Poisson regression

Employ a Log-linear model with $$v(\mu)=\mu$$ (the Poisson variance function) and

$$
\begin{aligned}
\log \left(E\left(Y_{i j}\right)\right)=& \beta_{0}+x_{i 1} \beta_{1}+x_{i 2} \beta_{2}+ x_{i 1} x_{i 2} \beta_{3}+\log \left(t_{i j}\right)
\end{aligned}
$$

where

* $$Y_{i j}:$$ 癫痫发作的次数 in interval $$j$$
* $$t_{i j}:$$ length of interval
* $$x_{i 1}=\left\{\begin{array}{l}1: \text { weeks } 8-16 \\ 0: \text { weeks } 0-8\end{array}\right.$$
* $$x_{i 2}=\left\{\begin{array}{l}1: \text { progabide group } \\ 0: \text { placebo group }\end{array}\right.$$
* The correlations between the counts are modeled as $$r_{i j}=\alpha, i \neq j$$ (**exchangeable **correlations).

**Interpretation of Regression Parameters**

$$
\begin{array}{|lr|l|}
\hline \text { Treatment } & \text { Visit } & \log \left(E\left(Y_{i j}\right) / t_{i j}\right) \\
\hline \text { Placebo } & \text { Baseline } & \beta_{0} \\
& 1-4 & \beta_{0}+\beta_{1} \\
\text { Progabide } & \text { Baseline } & \beta_{0}+\beta_{2} \\
& 1-4 & \beta_{0}+\beta_{1}+\beta_{2}+\beta_{3} \\
\hline
\end{array}
$$

```
data thall;
input id y visit trt bline age;
intercpt=1;
cards;
104 5 1 0 11 31
104 3 2 0 11 31
104 3 3 0 11 31
104 3 4 0 11 31
106 3 1 0 11 30
106 5 2 0 11 30
106 3 3 0 11 30
106 3 4 0 11 30
107 2 1 0 6 25
107 4 2 0 6 25
107 0 3 0 6 25
107 5 4 0 6 25
114 4 1 0 8 36
114 4 2 0 8 36
...
run;


## 为基线度量创建一个观察值，
## 创建一个区间变量，并为该观察值是用于基线度量还是访问度量创建一个指标变量。

data new;
set thall;
output;
if visit=1 then do; y=bline; visit=0; output; end;
run;
data new2;
set new;
if id ne 207;
if visit=0 then do; x1=0; ltime=log(8); end;
else do; x1=1; ltime=log(2); end;
x1trt=x1*trt;
run;

## CORRW选项显示工作相关矩阵。 TYPE = option指定相关结构； EXCH值表示可交换结构。 
   现在支持的其他结构包括unstructured, AR(1), independent, and user-specified.
   
proc genmod data=new2;
   model y=x1 | trt / d=poisson offset=ltime itprint;
   class id;
   repeated subject=id / corrw type=exch;
run;
```





****
