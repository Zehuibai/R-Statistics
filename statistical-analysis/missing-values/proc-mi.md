---
description: https://stats.idre.ucla.edu/sas/seminars/multiple-imputation-in-sas/mi_new_1/
---

# Proc MI



## Introduction

### MI

多重插补本质上是随机插补的一种迭代形式。但是，不是填写单个值，而是使用观测数据的分布来估计反映真实值周围不确定性的多个值。然后将这些值用于感兴趣的分析（例如OLS模型）中，并将结果组合在一起。每个估算值都包含一个随机分量，其大小反映了估算模型中其他变量无法预测其真实值的程度（Johnson和Young，2011； White等人，2010）。因此，将估算值的“真实性”周围的不确定性水平构建到估算值中。

对缺失数据方法的普遍误解是假设推定值应代表“真实”值。处理丢失的数据的目的是正确地重现我们的观察到的方差/协方差矩阵，如果我们的数据没有任何丢失的信息。选择的插补方法取决于缺少信息的模式以及缺少信息的变量的类型。

MI具有三个基本阶段：

1. Imputation or Fill-in Phase: The missing data are filled in with estimated values and a complete data set is created. This process of fill-in is repeated m times.
2. Analysis Phase: Each of the m complete data sets is then analyzed using a statistical method of interest (e.g. linear regression).
3. Pooling Phase: The parameter estimates (e.g. coefficients and standard errors) obtained from each analyzed data set are then combined for inference.

### Compatibility 

在开发归因模型时，重要的是评估归因模型是“同类的”还是与分析模型一致的(congenial)。一致性意味着您的估算模型包括（至少）包括您的分析或估计模型中相同的变量。这包括评估您的假设所需的对变量的任何转换。这可以包括对数转换，交互作用项或将连续变量重新编码为分类形式（如果在以后的分析中将以此方式使用的话）。其原因与先前关于多重插补目的的评论有关。由于我们正在尝试再现适当的方差/协方差矩阵以进行估算，因此应该同时表示和估算我们的分析变量之间的所有关系。否则，您将假设值与未包含在插补模型中的变量的相关系数为零，从而进行插补。这将导致低估分析中感兴趣的参数与失去检测可能感兴趣的数据属性（例如非线性和统计交互作用）的能力之间的关联。

###  Examine Missing Data Patterns

指定选项nimpute = 0（指定要创建的零估算数据集），可以在不实际执行完整估算的情况下请求“缺少数据模式”表。

```
*** Datasets;
*** https://stats.idre.ucla.edu/sas/seminars/multiple-imputation-in-sas/mi_new_1/;

data new;
    set ats.hsb_mar;
    if prog ^=. then do;
    if prog =1 then progcat1=1;
    else progcat1=0;
    if prog =2 then progcat2=1;
    else progcat2=0;
    end;
run;

data hsb_flag;
    set new;
    if female =.  then female_flag =1; else female_flag =0;
    if write  = . then write_flag  =1; else write_flag  =0;
    if read   = . then read_flag   =1; else read_flag   =0;
    if math   = . then math_flag   =1; else math_flag   =0;
    if prog   = . then prog_flag   =1; else prog_flag   =0;
run;

proc mi data=HSB_flag nimpute=0 ;
    var socst write read female math prog;
    ods select misspattern;
run;
```

### identify potential auxiliary variables

辅助变量是数据集中的变量，它们要么与缺失变量相关（建议r> 0.4），要么被认为与缺失相关。这些因素在您的分析模型中并不是特别重要，但是将它们添加到插补模型中以增加功效和/或帮助使MAR的假设更加合理。已经发现这些变量改善了由多重插补产生的插补值的质量。此外，研究表明，在估算因变量时和/或在变量信息丢失比例很高的情况下，它们特别重要 (Johnson and Young, 2011; Young and Johnson, 2010; Enders , 2010).

可能先验地了解了一些变量，您认为这些变量会根据您对数据和主题的了解而成为良好的辅助变量。 此外，对文献进行良好的回顾通常也可以帮助识别它们。 但是，如果不确定数据中的哪些变量是潜在的候选者（进行分析辅助数据分析时通常是这种情况），则可以使用一些简单的方法来帮助识别潜在的候选者。 识别这些变量的一种方法是检查变量之间的关联。

好的辅助变量也可以是缺失的相关因素或预测因素。t检验可以用于测试那些缺少信息的人和没有信息的人（辅助变量）的分数是否存在显着差异。从而发现缺失的潜在关联

```
proc corr data = new cov outp=test;
var write read female math progcat1 progcat2 ;
run;

proc ttest data=hsb_flag;
var socst science;
class read_flag;
run;
```

##  **MI using multivariate normal distribution (MVN)**

选择插补一个或多个变量时，您要做出的第一个决定是要插补变量的分布类型。 SAS中可用的一种方法是使用马尔可夫链蒙特卡洛（Markov Chain Monte Carl，MCMC），它假定插补模型中的所有变量都具有联合多元正态分布。这可能是多重插补最常用的参数方法。所使用的特定算法称为数据增强（data augmentatio** **DA）算法，它属于MCMC程序系列。该算法通过从条件分布（在这种情况下为多元正态）中提取给定观察数据的情况来填充缺失数据。在大多数情况下，仿真研究表明，即使在给定足够大样本量的情况下违反正态性假设的情况下，假设MVN分布也会导致可靠的估计（Demirtas等，2008； KJ Lee，2010）。但是，当样本量相对较小且信息丢失的比例较高时，会观察到偏差估计。（我们将需要为名义分类变量创建虚拟变量，以便可以解释每个级别的参数估计）

### Conduct MI

数据集“ a_mvn”，其中包含每个插补的参数估计值和关联的协方差矩阵。需要方差/协方差矩阵来估计标准误差。此步骤将参数估计值合并为一组统计数据，这些统计数据适当地反映了与估算值相关的不确定性。系数只是对10个回归模型中的每一个估计的各个系数的算术平均值。对参数估计值求平均可抑制变化，从而提高效率并减少采样变化。对每个变量的标准误差的估计使用Rubin规则

```
## 指定要使用的插补模型和要创建的插补数据集的数量
proc mi data= new nimpute=10 out=mi_mvn seed=54321;
    var socst science write read female math progcat1 progcat2;
run;

## Analysis Phase
## 分别估算每个估算数据集的线性回归模型
TITLE " MULTIPLE IMPUTATION REGRESSION - MVN";
proc glm data = mi_mvn ;
    model read = write female math progcat1 progcat2 ;
    by _imputation_;
    ods output ParameterEstimates=a_mvn;
run;
quit;


## Pooling Phase
proc mianalyze parms=a_mvn;
    modeleffects intercept write female math progcat1 progcat2;
run;
```

### Diagnostics

#### “Variance Information”

![](broken-reference)

SAS输出中的“参数估计”表上方，您将看到一个名为“方差信息”的表。 检查proc mianalyze的输出非常重要，因为可以使用几条信息来评估插补的效果。

* **Variance Between (VB)**: 从10个估算数据集获得的参数估计值（系数）变化的度量。这种变异性会估算由于缺少数据而导致的其他变异（不确定性）''This variability estimates the additional variation (uncertainty) that results from missing data.''

$$
V_B\sqrt{\frac{\sum_{i=1}^m (\theta_i - \overline{\theta})^2}{m-1} }
$$

* **Variance Within (VW)**: 10个估算数据集中每个数据集的采样方差（SE）的算术平均值。可以估算出在没有缺失数据的情况下我们可以预期的采样变异性。''This estimates the sampling variability that we would have expected had there been no missing data.''

$$
V_W = \frac{1}{m}\left (\sum_{i=1}^m{SE_i^2}\right )
$$

* **Variance Total (VT)**: 总方差是多个方差源的总和。Rubin将方差划分为“插补内”以获取预期的不确定性capturing the expected uncertainty，而“插补之间”则可获取由于信息缺失而导致的估计变异性capturing the estimation variability due to missing information

$$
V_{Total} = V_W + V_B + \frac{V_B}{m}
$$

* Relative Increases in Variance  ($$[V_B + V_B/m]/V_W$$)
* Fraction of Missing Information ($$\text{FMI} = [V_B+ V_B/m ]/V_T$$)
* Relative Efficiency: 估算的相对效率（RE）（估计真实总体参数的程度）与缺失信息的数量以及所执行的估算的数量（m）有关。当丢失的信息量非常低时，仅执行少量估算即可获得效率（大多数文献中给出的最小数量为5）。但是，当丢失大量信息时，通常需要更多的估算才能获得足够的参数估计效率。即使m很小，也可以获得相对较好的效率。但是，这并不意味着可以很好地估计标准误差。 对于正确的标准误差估计，通常需要更多的估算，因为估算数据集之间的变异性会在估算值周围合并必要的不确定性。 RE，m和FMI之间的直接关系为： $$\text{RE}=1 /（1 + \text{FMI} / m）$$ 

After performing an imputation it is also useful to look at means, frequencies and box plots comparing observed and imputed values to assess if the range appears reasonable. You may also want to examine plots of residuals and outliers for each imputed dataset individually. If anomalies are evident in only a small number of imputations then this indicates a problem with the imputation model (White et al, 2010).

> 执行插补后，查看均值，频率和箱形图以比较观察值和插补值以评估范围是否合理也很有用。您可能还需要分别检查每个估算数据集的残差图和离群值图。如果仅在少数插补中发现异常，则表明插补模型存在问题

should also assess convergence of your imputation model. This should be done for different imputed variables, but specifically for those variables with a high proportion of missing (e.g. high FMI). Convergence of the proc miprocedure means that DA algorithm has reached an appropriate stationary posterior distribution. Convergence for each imputed variable can be assessed using trace plots. These plots can be requested on the mcmc statement line in the proc mi procedure. Long-term trends in trace plots and high serial dependence are indicative of a slow convergence to stationarity. A stationary process has a mean and variance that do not change over time.

> 还应该评估插补模型的收敛性。应该针对不同的估算变量执行此操作，但应特别针对丢失比例较高（例如，较高的FMI）的那些变量。 proc miprocedure 的收敛意味着DA算法已达到适当的平稳后验分布。可以使用迹线图评估每个估算变量的收敛性。可以在proc mi过程中的mcmc语句行上请求这些图。轨迹图的长期趋势和高度的序列依赖性表明平稳性收敛缓慢。平稳过程的均值和方差不会随时间变化。

另一个对于评估收敛性非常有用的图是自动相关图，该自相关图也在mcmc语句中使用plots = acf指定。这有助于我们评估迭代之间参数值的可能自动相关性。相关性经过几次迭代后迅速接近零，这表明迭代之间几乎没有相关. 相关性高则需要使用niter选项增加插补数据集之间的迭代数。

```
proc mi data= ats.hsb_mar nimpute=10 out=mi_mvn;
    mcmc plots=trace  plots=acf ;
    var socst write read female math;
run;
```

##  **MI using Fully Conditional Specification**

SAS中可用的第二种方法使用完全条件方法（FCS）来估算缺少的变量，该方法不采用联合分布，而是为每个估算变量使用单独的条件分布。如果您要估算的变量只能采用特定值，例如逻辑模型的二进制结果或泊松模型的计数变量，则可能需要此规范。

可用的FCS方法是：

* discriminant function and logistic regression for binary/categorical variables (判别函数方法允许用户指定组成员资格的先验概率。在判别函数中，默认情况下只有连续变量可以是协变量。)
* linear regression and predictive mean matching for continuous variables. 
* by default the discriminant function and regression are used.

### Conduct MI

```
## Imputation Phase

## 如果要使用不同的方法估算这些变量，则可以使用默认方法指定要估算的变量，FCS语句中使用哪种方法估算
   使用判别函数方法来估算二进制变量female和类别变量prog。
## classeffects = include选项，以便在插补female和prog时将所有连续和分类变量用作预测变量。
## var语句中指定的顺序插补缺少值的变量。使用后续变量的观察值和推算值推算出后续变量。
proc mi data= ats.hsb_mar nimpute=20 out=mi_fcs ;
    class female prog;
    fcs plots=trace(mean std); 
    var socst write read female math science prog;
    fcs discrim(female prog /classeffects=include) nbiter =100 ; 
run;



## FCS语句还允许用户指定要用作预测变量的变量，如果估算变量未提供协变量，则SAS假定var语句上
   的所有变量都将用于预测所有其他变量。可以在同一FCS语句中指定多个条件分布。
## 此规范根据适用于非有序分类变量的广义logit分布而不是适用于有序变量
proc mi data= ats.hsb_mar nimpute=20 out=mi_fcs ;
   class female prog; 
   var socst write read female math science prog;
   fcs logistic(female prog /link=glogit); 
run;


## 广义logit分布下估算female prog，并使用预测均值匹配来估算math read write，而不是默认的回归方法
proc mi data= ats.hsb_mar nimpute=20 out=mi_fcs ;
   class female prog; 
   var socst write read female math science prog;
   fcs logistic(female prog /link=glogit) regpmm(math read write); 
run;


## 使用不同的预测变量集来估算female prog
proc mi data= ats.hsb_mar nimpute=20 out=mi_new1;
   class female prog; 
   var socst write read female math science prog;
   fcs logistic(female= math science/link=glogit) ;
   fcs logistic(prog =math socst /link=glogit) regpmm(math read write); 
run;
```

```
## Analysis and Pooling Phase
## 使用proc genmod进行线性回归

proc genmod data=mi_fcs;
    class female prog;
    model read= write female math prog /dist=normal ;
    by _imputation_;
    ods output ParameterEstimates=gm_fcs;
run;

## Pooling Phase
TITLE " MULTIPLE IMPUTATION Linear REGRESSION - FCS";
PROC MIANALYZE parms(classvar=level)=gm_fcs;
    class female prog;
    MODELEFFECTS INTERCEPT write female math prog;
RUN;

## 仅当模型效果包含分类变量时，此选项才适用。 
   由于proc genmod将分类指示器的列指示符命名为“ Level1”，因此我们需要指定classvar = level。
```
