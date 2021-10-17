# ROC and Cutoff

## ROC Curves and Cutoff

* [https://ncss-wpengine.netdna-ssl.com/wp-content/themes/ncss/pdf/Procedures/NCSS/One_ROC_Curve_and_Cutoff_Analysis.pdf](https://ncss-wpengine.netdna-ssl.com/wp-content/themes/ncss/pdf/Procedures/NCSS/One_ROC_Curve_and_Cutoff_Analysis.pdf)

A receiver operating characteristic (ROC) curve plots the true positive rate (sensitivity) against the false positive rate (1 – specificity) for all possible cutoff values.

> 接收器工作特性（ROC）曲线针对所有可能的临界值绘制了真实的阳性率（灵敏度）相对于假阳性率（1-特异性）

### Empirical ROC Curve 

The empirical ROC curve is a plot of the true positive rate versus the false positive rate for all possible cut-off values.

> 经验ROC曲线是所有可能的临界值的真实阳性率与假阳性率的关系图。

That is, each point on the ROC curve represents a different cutoff value. The points are connected to form the curve. Cutoff values that result in low false-positive rates tend to result low true-positive rates as well. As the true-positive rate increases, the false positive rate increases. The better the diagnostic test, the more quickly the true positive rate nears 1 (or 100%). A near-perfect diagnostic test would have an ROC curve that is almost vertical from (0,0) to (0,1) and then horizontal to (1,1). The diagonal line serves as a reference line since it is the ROC curve of a diagnostic test that randomly classifies the condition.

> 也就是说，ROC曲线上的每个点都代表一个不同的截止值。 这些点被连接以形成曲线。 导致假阳性率低的临界值也往往导致假阳性率低。 随着真阳性率的增加，假阳性率的增加。 诊断测试越好，真实阳性率就越快接近1（或100％）。 接近完美的诊断测试的ROC曲线从（0,0）到（0,1）几乎是垂直的，然后到（1,1）则水平。 对角线用作参考线，因为它是诊断测试的ROC曲线对状况进行随机分类。

### Binormal ROC Curve

The Binormal ROC curve is based on the assumption that the diagnostic test scores corresponding to the positive condition and the scores corresponding to the negative condition can each be represented by a Normal distribution. To estimate the Binormal ROC curve, the sample mean and sample standard deviation are estimated from the known positive group, and again for the known negative group. These sample means and sample standard deviations are used to specify two Normal distributions. The Binormal ROC curve is then generated from the two Normal distributions. When the two Normal distributions closely overlap, the Binormal ROC curve is closer to the 45-degree diagonal line. When the two Normal distributions overlap only in the tails, the Binormal ROC curve has a much greater distance from the 45-degree diagonal line.

> 双正态ROC曲线基于这样的假设：与阳性条件相对应的诊断测试得分和与阴性条件相对应的得分都可以用正态分布表示。 为了估计Binormal ROC曲线，从已知阳性组估计样品均值和样品标准偏差，然后从已知阴性组估计样品均值和样品标准偏差。 这些样本均值和样本标准差用于指定两个正态分布。 然后，根据两个正态分布生成Binormal ROC曲线。 当两个正态分布紧密重叠时，Binormal ROC曲线更靠近45度对角线。 当两个正态分布仅在尾部重叠时，Binormal ROC曲线与45度对角线的距离要大得多。

### Area under the ROC Curve (AUC)

* The AUC has a physical interpretation. The AUC is the probability that the criterion value of an individual drawn at random from the population of those with a positive condition is larger than the criterion value of another individual drawn at random from the population of those where the condition is negative.
* Another interpretation of AUC is the average true positive rate (average sensitivity) across all possible false positive rates.

> ROC曲线下的面积（AUC）是诊断测试准确性的常用度量。通常，较高的AUC值表示较好的测试性能。 AUC的可能值范围从0.5（无诊断能力）到1.0（完美的诊断能力）。
>
>  AUC具有物理解释。 AUC是从具有阳性条件的人群中随机抽取的一个人的标准值大于从一个条件为阴性的人群中随机抽取的另一个人的标准值的概率。 \
> AUC的另一种解释是所有可能的假阳性率的平均真实阳性率（平均敏感性）

```
## Plot ROC curve


library("ROCit")
data("Diabetes")
## first, fit a logistic model
logistic.model <- glm(as.factor(dtest)~chol+age+bmi,
                      data = Diabetes,
                      family = "binomial")

## make the score and class
class <- logistic.model$y
 
# score = log odds
score <- log(logistic.model$fitted.values/(1-logistic.model$fitted.values))

## rocit object
rocit_emp <- rocit(score = score,
                   class = class,
                   method = "emp")
rocit_bin <- rocit(score = score,
                   class = class,
                   method = "bin")
rocit_non <- rocit(score = score,
                   class = class,
                   method = "non")
summary(rocit_emp)
summary(rocit_bin)
summary(rocit_non)

plot(rocit_emp, col = c(1,"gray50"),
     legend = FALSE, YIndex = FALSE)
```

#### AUC of an Empirical ROC Curve

The value of AUC using the empirical method is calculated by summing the area of the trapezoids that are formed below the connected points making up the ROC curve. From DeLong et al. (1988), define the $$T{1}$$ component of the $$\mathrm{i}^{\text {th }}$$ subject, $$V\left(T{1 \mathrm{i}}\right)$$ as

$$
V\left(T_{1 i}\right)=\frac{1}{n_{0}} \sum_{j=1}^{n_{0}} \Psi\left(T_{1 i}, T_{0 j}\right)
$$

and define the $$T{0}$$ component of the $$\mathrm{j}^{\text {th }}$$ subject, $$V\left(T{0}\right)$$ as

$$
V\left(T_{0 j}\right)=\frac{1}{n_{1}} \sum_{i=1}^{n_{1}} \Psi\left(T_{1 i}, T_{0 j}\right)
$$

where

$$
\begin{array}{l}
\Psi(X, Y)=0 \text { if } Y>X \\
\Psi(X, Y)=1 / 2 \text { if } Y=X \\
\Psi(X, Y)=1 \text { if } Y<X
\end{array}
$$

The empirical AUC is estimated as

$$
A_{E m p}=\sum_{i=1}^{n_{1}} V\left(T_{1 i}\right) / n_{1}=\sum_{j=1}^{n_{0}} V\left(T_{0 j}\right) / n_{0}
$$

The variance of the estimated AUC is estimated as

$$
V\left(A_{E m p}\right)=\frac{1}{n_{1}} S_{T_{1}}^{2}+\frac{1}{n_{0}} S_{T_{0}}^{2}
$$

where $$S{T{1}}^{2}$$ and $$S{T{0}}^{2}$$ are the variances

$$
S_{T_{i}}^{2}=\frac{1}{n_{i}-1} \sum_{i=1}^{n_{i}}\left[V\left(T_{1 i}\right)-A_{E m p}\right]^{2}, \quad i=0,1
$$

#### AUC of a Binormal ROC Curve

The binormal model assumes that both $$X$$ and $$Y$$ are normally distributed with different means and variances. That is,

$$
X \sim N\left(\mu_{x}, \sigma_{x}^{2}\right), Y \sim N\left(\mu_{y}, \sigma_{y}^{2}\right)
$$

The ROC curve is traced out by the function

$$
\{F P(c), T P(c)\}=\left\{\Phi\left(\frac{\mu_{x}-c}{\sigma_{x}}\right), \Phi\left(\frac{\mu_{y}-c}{\sigma_{y}}\right)\right\}, \quad-\infty<c<\infty
$$

where $$\Phi(z)$$ is the cumulative normal distribution function. The area under the whole ROC curve is

$$
\begin{aligned}
A &=\int_{-\infty}^{\infty} T P(c) F P^{\prime}(c) \mathrm{d} c \\
&=\int_{-\infty}^{\infty}\left[\Phi\left(\frac{\mu_{y}-c}{\sigma_{y}}\right) \phi\left(\frac{\mu_{x}-c}{\sigma_{x}}\right)\right] \mathrm{d} c \\
&=\Phi\left[\frac{a}{\sqrt{1+b^{2}}}\right]
\end{aligned}
$$

where

$$
a=\frac{\mu_{y}-\mu_{x}}{\sigma_{y}}=\frac{\Delta}{\sigma_{y}}, b=\frac{\sigma_{x}}{\sigma_{y}}, \Delta=\mu_{y}-\mu_{x}
$$

The area under a portion of the AUC curve is given by

$$
\begin{aligned}
A &=\int_{c_{1}}^{c_{2}} T P(c) F P^{\prime}(c) \mathrm{d} c \\
&=\frac{1}{\sigma_{x}} \int_{c_{2}}^{c_{1}}\left[\Phi\left(\frac{\mu_{y}-c}{\sigma_{y}}\right) \phi\left(\frac{\mu_{x}-c}{\sigma_{x}}\right)\right] \mathrm{d} c
\end{aligned}
$$



### Optimal Cutoff Value (Optimal Decision Threshold)

While the ROC curve and corresponding AUC give an overall picture of the behavior of a diagnostic test across all cutoff values, there remains a practical need to determine the specific cutoff value that should be used for individuals requiring diagnosis. If the cost of each diagnostic decision is known, as well as the positive condition prevalence, the optimal cutoff value is the one that minimizes cost, as described under Cost in the Other Diagnostic Accuracy Indices section. However, cost and prevalence values are typically unknown and unattainable. In this case, a recommended approach is to find the cutoff with highest Youden Index, or equivalently, the highest Sensitivity + Specificity.

> 尽管ROC曲线和相应的AUC给出了所有临界值的诊断测试行为的总体情况，但仍然需要确定应该用于需要诊断的个体的特定临界值。 如果每个诊断决策的成本以及阳性条件患病率都是已知的，则最佳截止值就是使成本最小化的最优截止值，如“其他诊断准确性指标”部分的“成本”所述。 但是，成本和患病率值通常是未知且无法达到的。 在这种情况下，建议的方法是找到具有最高Youden指数或等效地具有最高灵敏度+特异性的临界值。

## Package ROCit

### Functions

|                                                                                |                                            |
| ------------------------------------------------------------------------------ | ------------------------------------------ |
| [ciAUC](https://rdrr.io/cran/ROCit/man/ciAUC.html)                             | Confidence Interval of AUC                 |
| [ciAUC.rocit](https://rdrr.io/cran/ROCit/man/ciAUC.rocit.html)                 | Confidence Interval of AUC                 |
| [ciROC](https://rdrr.io/cran/ROCit/man/ciROC.html)                             | Confidence Interval of ROC curve           |
| [ciROCbin](https://rdrr.io/cran/ROCit/man/ciROCbin.html)                       | Confidence Interval of Binormal ROC Curve  |
| [ciROCemp](https://rdrr.io/cran/ROCit/man/ciROCemp.html)                       | Confidence Interval of Empirical ROC Curve |
| [ciROC.rocit](https://rdrr.io/cran/ROCit/man/ciROC.rocit.html)                 | Confidence Interval of ROC curve           |
| [convertclass](https://rdrr.io/cran/ROCit/man/convertclass.html)               | Converts Binary Vector into 1 and 0        |
| [Diabetes](https://rdrr.io/cran/ROCit/man/Diabetes.html)                       | Diabetes Data                              |
| [gainstable](https://rdrr.io/cran/ROCit/man/gainstable.html)                   | Gains Table for Binary Classifier          |
| [gainstable.default](https://rdrr.io/cran/ROCit/man/gainstable.default.html)   | Gains Table for Binary Classifier          |
| [gainstable.rocit](https://rdrr.io/cran/ROCit/man/gainstable.rocit.html)       | Gains Table for Binary Classifier          |
| [getsurvival](https://rdrr.io/cran/ROCit/man/getsurvival.html)                 | Survival Probability                       |
| [gettptnfpfn-defunct](https://rdrr.io/cran/ROCit/man/gettptnfpfn-defunct.html) | Get number of TP, FP, TN and FN            |
| [invlogit-deprecated](https://rdrr.io/cran/ROCit/man/invlogit-deprecated.html) | Logistic Transformation                    |
| [ksplot](https://rdrr.io/cran/ROCit/man/ksplot.html)                           | KS Plot                                    |
| [ksplot.rocit](https://rdrr.io/cran/ROCit/man/ksplot.rocit.html)               | KS Plot                                    |
| [Loan](https://rdrr.io/cran/ROCit/man/Loan.html)                               | Loan Data                                  |
| [logit-deprecated](https://rdrr.io/cran/ROCit/man/logit-deprecated.html)       | Log Odds of Probability                    |
| [measureit](https://rdrr.io/cran/ROCit/man/measureit.html)                     | Performance Metrics of Binary Classifier   |
| [measureit.default](https://rdrr.io/cran/ROCit/man/measureit.default.html)     | Performance Metrics of Binary Classifier   |
| [measureit.rocit](https://rdrr.io/cran/ROCit/man/measureit.rocit.html)         | Performance Metrics of Binary Classifier   |
| [MLestimates](https://rdrr.io/cran/ROCit/man/MLestimates.html)                 | ML Estimate of Normal Parameters           |
| [plot.gainstable](https://rdrr.io/cran/ROCit/man/plot.gainstable.html)         | Plot '"gainstable"' Object                 |
| [plot.rocci](https://rdrr.io/cran/ROCit/man/plot.rocci.html)                   | Plot ROC Curve with confidence limits      |
| [plot.rocit](https://rdrr.io/cran/ROCit/man/plot.rocit.html)                   | Plot ROC Curve                             |
| [print.gainstable](https://rdrr.io/cran/ROCit/man/print.gainstable.html)       | Print "gainstable" Object                  |
| [print.measureit](https://rdrr.io/cran/ROCit/man/print.measureit.html)         | Print "measureit" Object                   |
| [print.rocci](https://rdrr.io/cran/ROCit/man/print.rocci.html)                 | Print 'rocci' Object                       |
| [print.rocit](https://rdrr.io/cran/ROCit/man/print.rocit.html)                 | Print 'rocit' Object                       |
| [print.rocitaucci](https://rdrr.io/cran/ROCit/man/print.rocitaucci.html)       | Print Confidence Interval of AUC           |
| [rankorderdata](https://rdrr.io/cran/ROCit/man/rankorderdata.html)             | Rank order data                            |
| [rocit](https://rdrr.io/cran/ROCit/man/rocit.html)                             | ROC Analysis of Binary Classifier          |
| [ROCit-defunct](https://rdrr.io/cran/ROCit/man/ROCit-defunct.html)             | Removed functions in package 'ROCit'.      |
| [ROCit-deprecated](https://rdrr.io/cran/ROCit/man/ROCit-deprecated.html)       | Deprecated functions in package 'ROCit'.   |
| [summary.rocit](https://rdrr.io/cran/ROCit/man/summary.rocit.html)             | Summary of rocit object                    |
| [trapezoidarea](https://rdrr.io/cran/ROCit/man/trapezoidarea.html)             | Approximate Area with Trapezoid Rule       |

### 1/0 coding of response

```
library(ROCit)
data("Loan")

# check the class variable
summary(Loan$Status)

# Change the refrence as numeric
Simple_Y <- convertclass(x = Loan$Status, reference = "FP") 
table(Simple_Y)

# Calculate prob.table
mean(Simple_Y)
mean(convertclass(x = Loan$Status))
```

### Performance metrics 

Following metrics can be called for via measure argument:

* `ACC:` Overall accuracy of classification = P(Y = \hat{Y}) = (TP + TN) / (TP + FP + TN + FN)
* `MIS:` Misclassification rate = 1 - ACC
* `SENS:` Sensitivity = P(\hat{Y} = 1|Y = 1) = TP / (TP + FN)
* `SPEC:` Specificity = P(\hat{Y} = 0|Y = 0) = TN / (TN + FP)
* `PREC:` Precision = P(Y = 1| \hat{Y} = 1) = TP / (TP + FP)
* `REC:` Recall. Same as sensitivity.
* `PPV:` Positive predictive value. Same as precision
* `NPV:` Positive predictive value = P(Y = 0| \hat{Y} = 0) = TN / (TN + FN)
* `TPR:` True positive rate. Same as sensitivity.
* `FPR:` False positive rate. Same as 1 - specificity.
* `TNR:` True negative rate. Same as specificity.
* `FNR:` False negative rate = P(\hat{Y} = 0|Y = 1) = FN / (FN +TP)
* `pDLR:` Positive diagnostic likelihood ratio = TPR / FPR
* `nDLR:` Negative diagnostic likelihood ratio = FNR / TNR
* `FSCR:` F-score, defined as 2 \* (PPV \* TPR) / (PPV + TPR)

```
data("Diabetes")
logistic.model <- glm(as.factor(dtest)~chol+age+bmi,
                      data = Diabetes,family = "binomial")
class <- logistic.model$y
score <- logistic.model$fitted.values

## Performance Metrics of Binary Classifier
measure <- measureit(score = score, class = class,
                     measure = c("ACC", "SENS", "FSCR"))
names(measure)
plot(measure$ACC~measure$Cutoff, type = "l")
```

An object of class `"measureit"`. By default it contains the followings:

| ``       |                                                                                                                    |
| -------- | ------------------------------------------------------------------------------------------------------------------ |
| `Cutoff` | Cutoff at which metrics are evaluated.                                                                             |
| `Depth`  | What portion of the observations fall on or above the cutoff.                                                      |
| `TP`     | Number of true positives, when the observations having score equal or greater than cutoff are predicted positive.  |
| `FP`     | Number of false positives, when the observations having score equal or greater than cutoff are predicted positive. |
| `TN`     | Number of true negatives, when the observations having score equal or greater than cutoff are predicted positive.  |
| `FN`     | Number of false negatives, when the observations having score equal or greater than cutoff are predicted positive. |

### ROC curve estimation

| ``       |                                                                                                                                                                                                                                                       |
| -------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `score`  | An numeric array of diagnostic score.                                                                                                                                                                                                                 |
| `class`  | An array of equal length of score, containing the class of the observations.                                                                                                                                                                          |
| `negref` | The reference value, same as the `reference` in `convertclass`. Depending on the class of `x`, it can be numeric or character type. If specified, this value is converted to 0 and other is converted to 1. If NULL, reference is set alphabetically. |
| `method` | The method of estimating ROC curve. Currently supports `"empirical"`, `"binormal"` and `"nonparametric"`. Pattern matching allowed thorough `grep`.                                                                                                   |

```
data("Diabetes")
summary(as.factor(Diabetes$dtest))

roc_empirical <- rocit(score = Diabetes$chol, class = Diabetes$dtest,
                       negref = "-") 
summary(roc_empirical)

 Method used: empirical    
 Number of positive(s): 60 
 Number of negative(s): 329
 Area under curve: 0.6494 

message("Number of positive responses used: ", roc_empirical$pos_count)
## Number of positive responses used: 60
message("Number of negative responses used: ", roc_empirical$neg_count)
## Number of negative responses used: 329

methods(class="rocit")
## [1] ciAUC      ciROC      gainstable ksplot     measureit  plot       print      summary   


head(cbind(Cutoff=roc_empirical$Cutoff, 
                 TPR=roc_empirical$TPR, 
                 FPR=roc_empirical$FPR))
#>      Cutoff        TPR         FPR
#> [1,]    Inf 0.00000000 0.000000000
#> [2,]    443 0.01666667 0.000000000
#> [3,]    404 0.03333333 0.000000000
#> [4,]    347 0.03333333 0.003039514
#> [5,]    342 0.05000000 0.003039514
#> [6,]    337 0.05000000 0.006079027

```

```
## Plotting
# Default plot
plot(roc_empirical, values = F)

# Changing color
roc_binormal <- rocit(score = Diabetes$chol, 
                      class = Diabetes$dtest,
                      negref = "-", 
                      method = "bin") 
plot(roc_binormal, YIndex = F, 
     values = F, col = c(2,4), legend = T)
     

## Plot ROC curves
roc_nonparametric <- rocit(score = Diabetes$chol, 
                           class = Diabetes$dtest,
                           negref = "-", 
                           method = "non") 

plot(roc_empirical, col = c(1,"gray50"), 
     legend = FALSE, YIndex = FALSE)
lines(roc_binormal$TPR~roc_binormal$FPR, 
      col = 2, lwd = 2)
lines(roc_nonparametric$TPR~roc_nonparametric$FPR, 
      col = 4, lwd = 2)
legend("bottomright", col = c(1,2,4),
       c("Empirical ROC", "Binormal ROC",
         "Non-parametric ROC"), lwd = 2)
```

![](<../../.gitbook/assets/plot_zoom_png (44).png>)

###  **CI of AUC**

```
ciAUC(roc_empirical, level = 0.9)

   estimated AUC : 0.649417426545086                     
   AUC estimation method : empirical                     
                                                         
   CI of AUC                                             
   confidence level = 90%                                
   lower = 0.58204561692333     upper = 0.716789236166842
   
   
## Other methods
# bootstrap method
set.seed(200)
ciAUC_boot <- ciAUC(rocit_non, 
                level = 0.9, nboot = 200)
print(ciAUC_boot)

# DeLong method
ciAUC(rocit_bin, delong = TRUE)

# logit and inverse logit applied
ciAUC(rocit_bin, delong = TRUE,
      logit = TRUE)
```

###  **CI of ROC curve**

```
## first, fit a logistic model
logistic.model <- glm(as.factor(dtest)~
                        chol+age+bmi,
                        data = Diabetes,
                        family = "binomial")

## make the score and class
class <- logistic.model$y
# score = log odds
score <- qlogis(logistic.model$fitted.values)
score <- logistic.model$fitted.values

rocit_emp <- rocit(score = score, 
                   class = class, 
                   method = "emp")
rocit_bin <- rocit(score = score, 
                   class = class, 
                   method = "bin")
ciROC_emp90 <- ciROC(rocit_emp, 
                     level = 0.9)
set.seed(200)
ciROC_bin90 <- ciROC(rocit_bin, 
                     level = 0.9, nboot = 200)
plot(ciROC_emp90, col = 1, 
     legend = FALSE)
lines(ciROC_bin90$TPR~ciROC_bin90$FPR, 
      col = 2, lwd = 2)
lines(ciROC_bin90$LowerTPR~ciROC_bin90$FPR, 
      col = 2, lty = 2)
lines(ciROC_bin90$UpperTPR~ciROC_bin90$FPR, 
      col = 2, lty = 2)
legend("bottomright", c("Empirical ROC",
                        "Binormal ROC",
                        "90% CI (Empirical)", 
                        "90% CI (Binormal)"),
       lty = c(1,1,2,2), col = 
         c(1,2,1,2), lwd = c(2,2,1,1))
```

![](<../../.gitbook/assets/plot_zoom_png (45).png>)

### Gains table

收益表是直接营销中使用的有用工具。观测值是第一等级的，并使用观测值创建一定数量的存储桶。收益表显示了与存储桶相关的若干统计信息。

* `Obs`: Number of observation in the group.
* `CObs`: Cumulative number of observations up to the group.
* `Depth`: Cumulative population depth up to the group.
* `Resp`: Number of (positive) responses in the group.
* `CResp`: Cumulative number of (positive) responses up to the group.
* `RespRate`: (Positive) response rate in the group.
* `CRespRate`: Cumulative (positive) response rate up to the group
* `CCapRate`: Cumulative overall capture rate of (positive) responses up to the group.
* `Lift`: Lift index in the group. Calculated as GroupResponseRate / OverallResponseRate.
* `CLift`: Cumulative lift index up to the group.

```
data("Loan")
class <- Loan$Status
score <- Loan$Score
# ----------------------------
gtable15 <- gainstable(score = score, 
                       class = class,
                       negref = "FP", 
                       ngroup = 15)
print(gtable15)

   Bucket Obs CObs Depth Resp CResp RespRate CRespRate CCapRate  Lift CLift
1       1  60   60 0.067   20    20    0.333     0.333    0.153 2.290 2.290
2       2  60  120 0.133   11    31    0.183     0.258    0.237 1.260 1.775
3       3  60  180 0.200   12    43    0.200     0.239    0.328 1.374 1.641
4       4  60  240 0.267   14    57    0.233     0.238    0.435 1.603 1.632
5       5  60  300 0.333   11    68    0.183     0.227    0.519 1.260 1.557
6       6  60  360 0.400   13    81    0.217     0.225    0.618 1.489 1.546
7       7  60  420 0.467    9    90    0.150     0.214    0.687 1.031 1.472
8       8  60  480 0.533    7    97    0.117     0.202    0.740 0.802 1.388
9       9  60  540 0.600    5   102    0.083     0.189    0.779 0.573 1.298
10     10  60  600 0.667    9   111    0.150     0.185    0.847 1.031 1.271
11     11  60  660 0.733    4   115    0.067     0.174    0.878 0.458 1.197
12     12  60  720 0.800    7   122    0.117     0.169    0.931 0.802 1.164
13     13  60  780 0.867    3   125    0.050     0.160    0.954 0.344 1.101
14     14  60  840 0.933    6   131    0.100     0.156    1.000 0.687 1.071
15     15  60  900 1.000    0   131    0.000     0.146    1.000 0.000 1.000


## rocit object can be passed
rocit_emp <- rocit(score = score, 
                   class = class, 
                   negref = "FP")
gtable_custom <- gainstable(rocit_emp, 
                    breaks = seq(1,100,15))
```



## Package InformationValue

Performance Analysis and Companion Functions for Binary Classification Models

### Functions

|                                                                                     |                  |
| ----------------------------------------------------------------------------------- | ---------------- |
| [ActualsAndScores](https://rdrr.io/cran/InformationValue/man/ActualsAndScores.html) | ActualsAndScores |
| [AUROC](https://rdrr.io/cran/InformationValue/man/AUROC.html)                       | AUROC            |
| [Concordance](https://rdrr.io/cran/InformationValue/man/Concordance.html)           | Concordance      |
| [confusionMatrix](https://rdrr.io/cran/InformationValue/man/confusionMatrix.html)   | confusionMatrix  |
| [IV](https://rdrr.io/cran/InformationValue/man/IV.html)                             | IV               |
| [kappaCohen](https://rdrr.io/cran/InformationValue/man/kappaCohen.html)             | kappaCohen       |
| [ks_plot](https://rdrr.io/cran/InformationValue/man/ks_plot.html)                   | ks_plot          |
| [ks_stat](https://rdrr.io/cran/InformationValue/man/ks_stat.html)                   | ks_stat          |
| [misClassError](https://rdrr.io/cran/InformationValue/man/misClassError.html)       | misClassError    |
| [npv](https://rdrr.io/cran/InformationValue/man/npv.html)                           | npv              |
| [optimalCutoff](https://rdrr.io/cran/InformationValue/man/optimalCutoff.html)       | optimalCutoff    |
| [plotROC](https://rdrr.io/cran/InformationValue/man/plotROC.html)                   | plotROC          |
| [precision](https://rdrr.io/cran/InformationValue/man/precision.html)               | precision        |
| [sensitivity](https://rdrr.io/cran/InformationValue/man/sensitivity.html)           | sensitivity      |
| [SimData](https://rdrr.io/cran/InformationValue/man/SimData.html)                   | SimData          |
| [somersD](https://rdrr.io/cran/InformationValue/man/somersD.html)                   | somersD          |
| [specificity](https://rdrr.io/cran/InformationValue/man/specificity.html)           | specificity      |
| [WOE](https://rdrr.io/cran/InformationValue/man/WOE.html)                           | WOE              |
| [WOETable](https://rdrr.io/cran/InformationValue/man/WOETable.html)                 | WOETable         |
| [youdensIndex](https://rdrr.io/cran/InformationValue/man/youdensIndex.html)         | youdensIndex     |

```
Evaluation1 <- function(description,probs){
  
  ## Calculate the percentage misclassification error 
  ## for the given actuals and probaility scores
  Misclassification <- misClassError(y_test, probs)
  Accuracy <- 1-Misclassification
  
  ## Calculate the area uder ROC curve statistic for a given logit model. 
  AUC <- round(AUROC(y_test, probs),4)
  
  ## ## Compute the optimal probability cutoff score
  OptimalCutoff <- round(optimalCutoff(y_test, probs),3)
  
  return(data.frame(Model=description,
                    Misclassification=Misclassification,
                    Accuracy=Accuracy,
                    AUC=AUC,
                    OptimalCutoff=OptimalCutoff))
}

Modelname1 <- list("LR.Cor","LR.Bestcor","Lasso","Elastic net","MARS","LDA","LR.PCA")
Testprobs1 <- list(LR.cor.probs, LR.Bestcor.probs, lasso.probs,
                   enet.probs, MARS.probs, lda.probs, LR.PCA.probs)
mapply(Evaluation1, Modelname1, Testprobs1)
```

