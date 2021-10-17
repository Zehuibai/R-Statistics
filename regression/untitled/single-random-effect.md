# Randomized Block Design

## Single Random: Randomized Block Design

通常，主要目标是估计和比较治疗手段（即广义上定义的治疗）。在大多数情况下，治疗效果被认为是固定的，因为研究中的治疗方法是唯一可以推断的方法。也就是说，对于未在实验中使用的治疗方法，不会得出任何结论。块效应通常被认为是随机的，因为研究中的块只是构成较大块的一小部分子集，通过这些子集可以推断出治疗手段。换句话说，研究者想估计和比较处理手段的准确性（置信区间）和统计显着性水平的陈述（来自假设检验），这些陈述**对整个块体有效，而不仅仅是那些实验性块体实验中的单位。为此，需要在模型方程中适当指定随机效应**。

将实验单元分组的设计称为随机模块设计。它们有两种形式：完全块和不完全块。

* **Randomized complete block designs**完整模块设计通常称为随机完整模块设计，或简称为RCBD。在RCBD中，将每种处理方法应用于每个模块中的实验单元。each treatment is applied to an experimental unit in each block.
* **Balanced incomplete block and partially balanced incomplete block**: 在不完整的模块设计中，在任何给定的模块中，仅将处理的子集分配给实验单元。平衡的不完全块和部分平衡的不完全块（分别为BIB和PBIB）是这种设计的两个常见示例。

### Mixed Model

无论是完整的还是不完整的，所有随机块设计都共享一个通用模型。假设有$$t$$种处理和$$r$$种Block，并且每个实验单位只有一个观测值。一旦选择了要在给定区块中显示的治疗方法，每个选定的治疗方法就会随机分配给该区块中的一个实验单位。通常，总共有N个实验单位。

* 对于完整的块设计，由于将$$t$$个处理中的每一个分配给$$r$$个块中的每个实验单元，因此总共有$$N = tr$$个实验单元。
* 对于每个块中具有相同数量实验单元的不完整块设计，有$$N = rk$$个实验单元，其中$$k$$表示每个块的实验单元数。

随机块模型的常规假设如下：$$y_{ij}$$表示来自在块$$j$$中接受处理$$i$$的实验单位的响应，该模型的方程式如下：

$$
y_{i j}=\mu+\tau_{i}+b_{j}+e_{i j}
$$

* $$\mu$$ and $$\tau_{i}$$ are fixed parameters such that the mean for the $$i^{\text {th }}$$ treatment is $$\mu_{i}=\mu+\tau_{i}$$
* $$b_{j}$$ is the random effect associated with the $$j^{\text {th }}$$ block
* $$e_{i j}$$ is the random error associated with the experimental unit in block $$j$$ that received treatment $$i$$

#### Assumptions for random effects are as follows:

* Block effects are distributed normally and independently with mean 0 and variance $$\sigma_{b}^{2} ;$$ that is, the $$b_{j}(j=1,2, \ldots, r)$$ are distributed iid $$\mathrm{N}\left(0, \sigma_{b}^{2}\right)$$
* Errors $$e_{i j}$$ are distributed normally and independently with mean 0 and variance $$\sigma^{2} ;$$ that is, the $$e_{i j}(i=1,2, \ldots, t ; j=$$ $$1,2, \ldots, r)$$ are distributed iid \$$\mathrm{N}\left(0, \sigma^{2}\right).
* The$$e_{i j}$$are also distributed independently of the$$b{j}$$.

For the RCBD, treatment mean $$\bar{y}_{1} .,$$ can be written as follows:

$$
\bar{y}_{1} .=\mu_{1}+\bar{b} .+\bar{e}_{1}
$$

The difference between two means, such as $$\bar{y}_{1}-\bar{y}_{2},$$

$$
\bar{y}_{1} .-\bar{y}_{2} .=\mu_{1}-\mu_{2}+\bar{e}_{1} .-\bar{e}_{2}
$$

The variances of $$\bar{y}_{1}$$. and $$\bar{y}_{1} \cdot-\bar{y}_{2}$$. are

$$
\operatorname{Var}\left[\bar{y}_{1 \cdot}\right]=\left(\sigma^{2}+\sigma_{b}^{2}\right) / r
$$

RCBD控制块变化的体现； 估计处理之间差异的方差没有块变化。

$$
\operatorname{Var}\left[\bar{y}_{1} .-\bar{y}_{2} .\right]=2 \sigma^{2} / r
$$

### Traditional Method: ANOVA

Analysis of variance, such as degrees of freedom, sums of squares (SS), mean squares (MS), and expected mean squares (E\[MS]).

$$
\begin{array}{llll}
\hline \text { Source of Variation } & d f & \text { MS } & \text { E[MS] } \\
\hline \text { Blocks } & \mathrm{r}-1 & \mathrm{MS}(\mathrm{Blk}) & \sigma^{2}+t \sigma_{b}^{2} \\
\text { Treatments } & \mathrm{t}-1 & \mathrm{MS}(\mathrm{Trt}) & \sigma^{2}+r \phi^{2} \\
\text { Error } & (\mathrm{r}-1)(\mathrm{t}-1) & \mathrm{MS}(\text { Error }) & \sigma^{2}
\end{array}
$$

The null hypothesis is $$\mathrm{H}_{0}: \mu_{1}=\mu_{2}=\ldots=\mu_{t} .$$ The expression $$\phi^{2}$$ in $$\mathrm{E}[\mathrm{MS}(\mathrm{Trt})]$$ is

$$
\phi^{2}=(t-1)^{-1} \sum_{i=1}^{t}\left(\mu_{i}-\bar{\mu}_{\cdot}\right)^{2}
$$

$$
\hat{\sigma}_{b}^{2}=\frac{1}{t}[\mathrm{MS}(\mathrm{Blk})-\mathrm{MS}(\text { Error })]
$$

For variance

$$
\begin{aligned}
\operatorname{Vâr}\left[\bar{y}_{1 \cdot}\right] &=\left(\hat{\sigma}^{2}+\hat{\sigma}_{b}^{2}\right) / r \\
&=\frac{1}{r t} \operatorname{MS}(\mathrm{Blk})+\frac{t-1}{r t} \mathrm{MS}(\text { Error })
\end{aligned}
$$

and

$$
\operatorname{Vâr}\left[\bar{y}_{1 \cdot}-\bar{y}_{2 \cdot}\right]=\frac{2}{r} \mathrm{MS}(\text { Error })
$$

### SAS MIXED and GLIMMIX Procedures

PROC MIXED可用于线性混合模型（LMM），即当您可以假定响应变量具有高斯分布时。 PROC MIXED使您可以使用平方和和预期均方值来估计方差分量，或使用似然法。 PROC GLIMMIX可用于LMM和广义线性混合模型（GLMM;即，既用于高斯响应变量又用于非高斯响应变量）。 PROC GLIMMIX仅使用基于可能性的方法。使用PROC MIXED或PROC GLIMMIX获得的分析基本上是相同的。

```
proc mixed data=bond cl method=type3;
 class ingot metal;
 model pres = metal;
 random ingot;
 lsmeans metal;
run;

## Covariance Parameter Estimates
## The estimate of σ_b^2 , the block variance component, is 11.4478 (labeled “ingot”), 
## The estimate of σ2, the error variance component, is 10.3716 (labeled “Residual”). 
## The confidence intervals for the variance components are Wald confidence intervals.
```

* The METHOD=TYPE3 option requests that the Type 3 sums of squares method be used in estimating the variance components

在做多因素方差分析时，有三种方法计算平方和（以模型Y \~ A + B + A:B为例，即先输入A，再输入B，最后输入交互项A:B）：

* Type Ⅰ Sums of Squares（Type1, sequential）\
  序贯型，后输入的因素根据之前输入的因素做调整，**与输入顺序有关**（A不做调整，B根据A做调整，A:B根据A和B做调整，因此使用Type1要注意模型中各因素的输入顺序）。\

* Type Ⅱ Sums of Squares（Type2, hierarchical）\
  分层型，根据同阶水平和低阶水平的因素做调整，**与输入顺序无关**（A和B同是一阶的，A根据B做调整，B根据A做调整；A:B是二阶的，因而A:B根据A和B做调整）。\

* Type Ⅲ Sums of Squares（Type3, marginal）\
  边界型，根据其他所有因素做调整，**与输入顺序无关**（A根据B和A:B做调整，B根据A和A:B做调整，A:B根据A和B做调整）。

### CI for Covariance Parameter Estimates

#### Estimation of $$\sigma^{2}$$

A $$(1-\alpha) \times 100 \%$$ confidence interval about $$\sigma^{2}$$ can be constructed by using the chi-square distribution, as

$$
\frac{(b-1)(t-1) \hat{\sigma}^{2}}{\chi_{(1-\alpha / 2)(b-1)(t-1)}^{2}} \leq \sigma^{2} \leq \frac{(b-1)(t-1) \hat{\sigma}^{2}}{\chi_{(\alpha / 2)(b-1)(t-1)}^{2}}
$$

where $$\chi_{(1-\alpha / 2),(b-1)(t-1)}^{2}$$ and$$\chi_{\alpha / 2,(b-1)(t-1)}^{2}$$are the lower and upper $$\alpha / 2$$ percentage points of a central chi-square distribution with $$(b-1) \times(t-1)$$ degrees of freedom, respectively. 

#### Estimation of $$\sigma_{b}^{2}$$

When the estimate of $$\sigma_{b}^{2}$$ is positive, an approximate $$(1-\alpha) \times 100 \%$$ confidence interval about $$\sigma_{b}^{2}$$ can be constructed using a Satterthwaite (1946) approximation. The estimate of $$\sigma_{b}^{2}$$ is a linear combination of mean squares, which in general can be expressed as

$$
\hat{\sigma}_{b}^{2}=\sum_{i=1}^{s} q_{i} M S_{i}
$$

For the randomized complete block, the expression is the following:

$$
\hat{\sigma}_{b}^{2}=\frac{1}{t}(\operatorname{MS}(\mathrm{Blk})-\mathrm{MS}(\text { Error }))
$$

The approximate number of degrees of freedom is as follows:

$$
df=v=\frac{\left(\hat{\sigma}_{b}^{2}\right)^{2}}{\frac{\left(t^{-1} \mathrm{MS}(\mathrm{Blk})\right)^{2}}{b-1}+\frac{\left(t^{-1} \mathrm{MS}(\text { Error })\right)^{2}}{(b-1)(t-1)}}
$$

A $$(1-\alpha) \times 100 \%$$ confidence interval about $$\sigma_{b}^{2}$$ can be constructed using the chi-square distribution, as the following, where $$\chi_{(1-\alpha / 2), v}^{2}$$ and $$\chi_{\alpha / 2, v}^{2}$$ are the lower and upper $$\alpha / 2$$ percentage points with $$v$$ degrees of freedom, respectively

```
## Program to Obtain Satterthwaite Approximation Confidence Intervals
## COVTEST statement with the CL option

proc glimmix data=bond;
 class ingot metal;
 model pres=metal;
 random ingot;
 covtest / cl;
run;
```

对于非常大的数据集，Satterthwaite置信区间比Wald置信区间更准确，因此建议使用。 您可以使用PROC GLIMMIX或PROC MIXED获得Satterthwaite边界。 仅在PROC GLIMMIX中可用的替代过程使用似然比。

可以通过两种方式获得方差分量的轮廓似然置信区间 (profile likelihood )。轮廓似然比（PLR）为确定置信区间的参数的每个新值重新估计所有其他协方差参数。empirical likelihood ratio 经验似然比（ELR）使用2σ的REML估计来计算所有2σb值的似然比。后者在计算上更简单，并且适合于blocked 设计。

$$
2\left\{\log L(\hat{\boldsymbol{\sigma}})-\log L\left(\hat{\sigma}^{2} \mid \tilde{\sigma}_{b}^{2}\right)\right\}<\chi^{2}
$$

```
covtest / cl(type=elr);
```

## Incomplete Block Design

### Introduction

在某些Block应用中，每个Block中没有足够的实验单元来容纳所有治疗方法。不完整的块设计是在每个块中仅应用部分处理的设计。应该选择进入每个模块的处理方式，以便提供与实验目标有关的最多信息。三种类型的不完整块设计是：

* 平衡不完整块设计（BIBD）
  * balanced incomplete block designs 
* 部分平衡不完整块设计（PBIBD）
  * partially balanced incomplete block design
* 不平衡不完整块设计
  * unbalanced incomplete block design

在设计理论中，BIB和PBIB设计的“平衡”含义导致所有处理均值估计具有相同的方差（因此具有相同的标准误差）。同样，对于所有使用BIBD的治疗对和使用PBIBD的治疗组，估计治疗均值的方差都相同。

不完整块设计的两种最常用的分析方法

* **intra-block analysis**  块内分析的方法中，假定块效应是固定的。在统计软件的预混合模型时代，块内分析是唯一可用的方法。\
  \
  _the intra-block analysis is useful for introducing the distinction between Least Squares treatment means, also called adjusted means, and unadjusted arithmetic means, and the associated distinction between Type I tests of hypotheses and Type III tests. You can use PROC GLM, PROC GLIMMIX or PROC MIXED to implement intra-block (fixed block effect) analysis._\
  \
  块内分析可用于引入最小二乘处理方法（也称为调整后的方法）和未调整的算术方法之间的区别，以及假设的I型检验和III型检验之间的相关区别。您可以使用PROC GLM，PROC GLIMMIX或PROC MIXED来实现块内（固定块效果）分析。\

* **combined inter- and intra-block analysis **在另一种方法（称为块间和块内组合分析）中，块效应被认为是随机的。在大多数情况下，使用块差异提供的信息（称为块间信息恢复）可以提高所得分析的准确性和准确性。但是，块内分析对于引入最小二乘处理方法（也称为调整后的方法）和未调整的算术方法之间的区别以及假设的I型检验和III型检验之间的相关区别非常有用。\
  \
  To do combined inter- and intra-block (random block effect) analysis, you must use either PROC GLIMMIX or PROC MIXED.

### Unbalanced Two-Way Mixed Model

Models for an incomplete block design are the same as for an RCBD. 

$$
y_{i j}=\mu+\tau_{i}+b_{j}+e_{i j}
$$

$$
\begin{array}{lll}
\hline \text { Source of Variation } & d f & F \\
\hline \text { Blocks } & \mathrm{r}-1 & \\
\text { (adjusted for treatments) } & & \\
\text { Treatments } & \mathrm{t}-1 & \text { MS(Trts adj.) / MS(Residual) } \\
\text { (adjusted for blocks) } & & \\
\text { Residual } & \mathrm{N}-\mathrm{r}-\mathrm{t}+1 & \\
\hline
\end{array}
$$

### Intra-block Analysis

具有15个块，15个处理和每个块4个处理。数据为每块地的籽棉磅数。块大小是每个块的处理次数。该PBIBD的块大小为四。每种处理以四个块出现。某些对处理在一个块中同时出现（例如，处理1和2），而其他处理则不在同一块中同时出现（例如，处理1和6）按照SAS混合模型过程的要求，以单变量形式排列数据（每个观测值具有一条数据线）

```
data pbib;
 input blk @@;
 do eu=1 to 4;
 input treat y @@;
 output;
 end;
datalines;

proc glimmix data=pbib;
class treat blk;
 model y=treat blk/htype=1,3;
 lsmeans treat / e;                             ## adjusted sample means
 lsmeans treat / bylevel e;                     ## unadjusted sample means
run;
```

The E option enables you to see which linear combination of model parameters is being used to calculate these means. The BYLEVEL option in the second LSMEANS statement causes PROC GLIMMIX to compute unadjusted sample means, and the associated E option enables you to see how these means are calculated. The HTYPE=1,3 statement obtains TYPE I and TYPE III tests of treatment effects

### Combined Intra- and Inter-block

```
主要区别在于，BLK出现在RANDOM语句中，而不是MODEL语句中。

proc glimmix data=pbib;
 class blk treat;
 model response=treat;
 random blk;
 lsmeans treat/diff;
run;
```

## Mean Comparisons for Fixed Effects

### Analysis Using PROC GLIMMIX

了使用PROC GLIMMIX将这些数据作为随机块设计进行分析，必须将这些数据与单独的数据线堆叠在一起，以用于PRE和POST响应。 为此，还需要创建一个变量来标识观察值的“处理方式”，即PRE或POST。

```
data pulsestack; set pulse;
 y=pre; trt="pre "; output;
 y=post; trt="post"; output;
run;

proc glimmix data=pulsestack;
 class trt animal;
 model y = trt;
 random animal;
 lsmeans trt/diff;
run;

## Differences of trt Least Squares Means
## 获得处理均值估计值（LSMEANS），处理均值差（DIFF选项）
```

### Comparison of Several Means

```
proc glimmix data=bond plots=(meanplot diffplot);
 class ingot metal;
 model pres = metal / solution;
 random ingot;
 lsmeans metal / diff cl;
 estimate 'nickel mean' intercept 1 metal 0 0 1;
 estimate 'copper vs iron' metal 1 -1 0;
 contrast 'copper vs iron' metal 1 -1 0;
run;
```

* ESTIMATE和CONTRAST语句用于估计和检验关于混合模型中各项的线性组合的假设，包括随机效应。 
  * GLIMMIX过程中的ESTIMATE语句允许进行多行估计，并且可以针对多重性调整相应的t检验。 
  * CONTRAST语句可以由几个线性组合组成，其中所得的F统计量可以同时检验参数的线性组合集合等于零的假设。 
* LSMEANS陈述产生对治疗均值和均值差的估计和检验。
* DIFF选项获得均值差。 
* PLOT选项提供了不同的可视化处理方式和差异的方式。 
  * MEANPLOT显示了处理手段
  * CL选项将95％的置信范围置于每个均值估计值的上方和下方。 
  * DIFFPLOT显示了所有成对差异的95％置信限，并对它们进行了绘制，以便可以很容易地看到统计上显着和不显着的差异

#### Standard Errors for Mean Comparisons

Variance of a least square mean: For a paired sample with no missing data, the least square mean for treatment $$i$$ is equal to the sample mean, i.e.

$$
\hat{\mu}+\hat{\tau}_{i}=(1 / b) \sum_{j} y_{i j}
$$

express this in model terms as $$(1 / b) \sum_{j}\left(\mu+\tau_{i}+b_{j}+e_{i j}\right)$$ The variance is thus

$$
\begin{aligned}
\operatorname{Var}\left((1 / b) \sum_{j}\left(\mu+\tau_{i}+b_{j}+e_{i j}\right)\right) &=(1 / b)^{2}\left(\sum_{j} \operatorname{Var}\left(\mu+\tau_{i}+b_{j}+e_{i j}\right)\right) \\
&=(1 / b)^{2}\left(b \operatorname{Var}\left(b_{j}\right)+b \operatorname{Var}\left(e_{i j}\right)\right) \\
&=\left(\sigma_{b}^{2}+\sigma^{2}\right) / b
\end{aligned}
$$

A similar derivation gives the standard error of the difference. Begin with the difference between the two-treatment means, express the difference in terms of the model, and compute the variance:

$$
\begin{aligned}
(1 / b) \sum_{j} y_{1 j}-(1 / b) \sum_{j} y_{2 j} &=(1 / b) \sum_{j}\left[\left(\mu+\tau_{1}+b_{j}+e_{1 j}\right)-\left(\mu+\tau_{2}+b_{j}+e_{2 j}\right)\right] \\
&=(1 / b) \sum_{j}\left(\tau_{1}-\tau_{2}+e_{1 j}-e_{2 j}\right)
\end{aligned}
$$

Computing the variance gives $$(1 / b)^{2} \sum_{j} \operatorname{Var}\left(e_{1 j}-e_{2 j}\right)=2 \sigma^{2} / b$$

### Comparison of Quantitative Factors: Polynomial Regression

```
## create a mean plot and overall effect test
## If you want to test the bench variance, 
    use the COVTEST statement to obtain a likelihood ratio test
proc glimmix data=plants;
 class bench n;
 model plant_wt=n;
 random intercept / subject=bench;
 lsmeans n / plot=meanplot(join);
run;

## The full polynomial fit
proc glimmix data=plants;
 class bench;
 model plant_wt = n n*n n*n*n n*n*n*n n*n*n*n*n;
 random intercept / subject=bench;
run;

## The HTYPE=1 option gives you Type I tests, overriding the TYPE III test default. 
proc glimmix data=plants;
 class bench;
 model plant_wt = n|n|n|n|n / htype=1;
 random intercept / subject=bench;
run;

## 消除高阶术语
proc glimmix data=plants;
 class bench;
 model plant_wt = n|n|n / htype=1 solution;
 random intercept / subject=bench;
run;
```

## Mixed Model Power and Precision

### Introduction

由于混合模型的协方差结构更复杂，因此涉及到的功率和精度计算更加复杂

* 治疗效果或对比度，定义为线性组合$$\mathbf{K}^{\prime} \boldsymbol{\beta}$$. 
* The design matrix, $$\mathbf{X}$$. 行数与样本大小相对应，列由设计（通常是处理设计）确定。
* Variance-covariance matrix, $$\mathbf{V}$$. 该矩阵指定了随机变化的来源及其大小。在混合模型中，\$$\mathbf{V}=\mathbf{Z} \mathbf{G} \mathbf{Z}^{\prime}+\mathbf{R}$. $\mathbf{G}\$$ 给出随机模型效应的协方差，通常与实验设计的元素相对应。
* $$\mathbf{Z}$$列指定随机模型效果的样本大小和结构
* $$\mathbf{R}$$给出实验单位之间的方差，并指定实验单位或实验单位子集之间的相关性（如果有）。

线性混合模型理论能够进行功效和精度分析，其主要结果是可估计函数的估计值$$\mathbf{k} \hat{\boldsymbol{\beta}}$$, is distributed approximately $$\mathrm{N}\left(\mathbf{k}^{\prime} \boldsymbol{\beta}, \mathbf{k}^{\prime}\left[\mathbf{X}^{\prime} \mathbf{V}^{-1} \mathbf{X}\right] \mathbf{k}\right)$$

* $$\mathbf{k}^{\prime} \boldsymbol{\beta}$$定义的单个自由度对比度, $$\mathbf{k}$$ is a vector, 对比度估计的标准误差为 $$\operatorname{SE}\left(\mathbf{k}^{\prime} \hat{\boldsymbol{\beta}}\right)=\sqrt{\mathbf{k}^{\prime}\left(\mathbf{X}^{\prime} \mathbf{V}^{-1} \mathbf{X}\right)^{-} \mathbf{k}}$$
* $$\mathbf{k}^{\prime} \boldsymbol{\beta}$$置信区间为$$\mathbf{k}^{\prime} \hat{\boldsymbol{\beta}} \pm t_{(\alpha, v)} \sqrt{\mathbf{k}^{\prime}\left(\mathbf{X}^{\prime} \mathbf{V}^{-1} \mathbf{X}\right)^{-} \mathbf{k}},$$ where $$t$$ is the value from the $t$ 分布中与标准误差的估计相关的$$(1-\alpha) 100 \%$$置信度和$$v$$ 自由度
* 对于测试统计量$$\mathrm{H}_{0}: \mathbf{K}^{\prime} \boldsymbol{\beta}=\mathbf{0}$$ is $$F=\left(\mathbf{K}^{\prime} \hat{\boldsymbol{\beta}}\right)^{\prime}\left[\mathbf{K}^{\prime}\left(\mathbf{X}^{\prime} \mathbf{V}^{-1} \mathbf{X}\right) \mathbf{K}\right]\left(\mathbf{K}^{\prime} \hat{\boldsymbol{\beta}}\right) / \operatorname{rank}(\mathbf{K})$$.

### Computing Precision and Power

```
data crd;
n=4;
input trt$ mu;
do eu=1 to n;
 output;
end;
datalines;
a 60
b 55
c 50
;

proc glimmix data=crd;
 class trt;
 model mu=trt;
 parms (25)/hold=1;
 lsmeans trt / diff cl;
 contrast '5 unit diff' trt 0 1 -1;
 contrast '10 unit diff' trt 1 0 -1;
 ods output tests3=F_overall contrasts=F_contrast;
run;

data power_calc;
 set F_overall F_contrast;
 alpha=0.05;
 nc_parameter=numdf*Fvalue;
 F_critical=Quantile('F',1-alpha,numdf,dendf,0);
 Power=1-cdf('F',F_critical,numdf,dendf,nc_parameter);
run;
```

###
