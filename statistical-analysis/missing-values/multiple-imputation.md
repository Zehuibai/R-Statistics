# MICE R

## Missing Value

### Basic

#### is.na 判断,返回值为逻辑值，TRUE代表缺失，否则为FALSE

```
## 赋予缺失值
is.na(x1) <- which(x1 == 7)

## 替换缺失值，替换为0 再替换为100
x1[is.na(x1)]<-100   

## 计算数据集的缺失值总数
sum(is.na(x2))  
```

#### complete.cases() delete missing values

```
## 删除缺失值[以第一列为标准]
x2[complete.cases(x2)]
```

#### Missing values options in table

```
## show NA in the output only if there is some missing data in the object.
## 仅当对象中缺少某些数据时，第一个才会在输出中显示
table(x2, useNA = "ifany")

## include NA in the output regardless.
## 第二个都会在输出中包含NA
table(x2, useNA = "always")
```

#### Search the missing values

```
## search the missing values from row,col
data <- data.frame(col1=c(1,2,4,NA),col2=c(13,4,NA,9),col3=c(8,5,3,3))
Na_Werte <- function(x){
    a <-ifelse(rowSums(is.na(x)) > 0,"Ja","Nein")
    return(list(Ob_es_fehlenden_Wert=a))
}
Na_Werte(data)
```

```
Welche_Na_Zeilen <- function(x){
  list <-which(colSums(is.na(x)) > 0) 
  Splten_NA <- x[list,]
  return(Splten_NA)
}
Welche_Na_Zeilen(data)
```

### Compare and test group comparisons

```
library(haven)
dt <- read_sav("~/Library/Mobile Documents/com~apple~CloudDocs/02. Programm/01 R_lernen/00 Data/Backpain_test.sav")
df_select <- data.frame(dt[,c("Pain", "Tampascale","Disability", "Age")])

# Copy variable with missing data and rename 
df_select$Tampa_MAR <- df_select$Tampascale
df_select$Disability_MAR <- df_select$Disability

library(finalfit)

explanatory = c("Pain", "Disability", "Age")
dependent = "Tampa_MAR"
df_select %>% 
  missing_compare(dependent, explanatory) %>% 
    knitr::kable(row.names=FALSE, align = c("l", "l", "r", "r", "r"), 
        caption = "Mean comparisons between values of responders (Not missing) and 
        non-responders (Missing) on the Tampa scale variable.") 
```

| Mean comparisons between values of responders (Not missing) and non-responders (Missing) on the Tampa scale variable. |
| --------------------------------------------------------------------------------------------------------------------- |

|                                   |           |             |            |       |
| --------------------------------- | --------- | ----------- | ---------- | ----- |
| Missing data analysis: Tampa\_MAR |           | Not missing | Missing    | p     |
| Pain                              | Mean (SD) | 5.0 (2.1)   | 6.3 (2.3)  | 0.017 |
| Disability                        | Mean (SD) | 12.1 (4.3)  | 9.8 (4.0)  | 0.040 |
| Age                               | Mean (SD) | 40.1 (8.9)  | 41.3 (7.1) | 0.594 |

### Generate missing values

R函数截肢术是多变量缺失数据生成过程的易于使用的实现。使用ampute，可以直接在多个变量中生成缺失值，具有不同的缺失数据比例和不同的潜在缺失机制。

```
## In simulation settings, multivariate data can be generated 
   by using function mvrnorm from R-package MASS. 
## Be aware that the covariance matrix should be semi definite.
set.seed(2016)
testdata <- MASS::mvrnorm(n = 10000, mu = c(10, 5, 0), Sigma = matrix(data = c(1.0, 0.2, 0.2, 0.2, 1.0, 0.2, 0.2, 0.2, 1.0), nrow = 3, byrow = T))
testdata <- as.data.frame(testdata)
summary(testdata)
result <- ampute(testdata)
result


## Multivariate Amputed Data Set
## Call: ampute(data = testdata)
## Class: mads
## Proportion of Missingness:  0.5
## Frequency of Patterns:  0.3333333 0.3333333 0.3333333
## Pattern Matrix:
##   V1 V2 V3
## 1  0  1  1
## 2  1  0  1
## 3  1  1  0
## Mechanism:[1] "MAR"
## Weight Matrix:
##   V1 V2 V3
## 1  0  1  1
## 2  1  0  1
## 3  1  1  0



## see whether the amputation has gone according plan, 
   a quick investigation can be done by using function md.pattern.
## 第一行总是指完整的案例。最后一列包含该特定模式中缺少值的变量数

md.pattern(result$amp)



## 可以通过更改值或添加行来操纵矩阵
mypatterns <- result$patterns
mypatterns[2, 1] <- 0
mypatterns <- rbind(mypatterns, c(0, 1, 0))
mypatterns

V1   V2   V3
0 	 1	   1
0	   0	   1
1	   1	   0
0	   1	   0



## 添加了第四个缺失数据模式以在V1和V3上创建缺失组合, 使用所需的模式矩阵作为第三个参数再次执行 
result <- ampute(testdata, patterns = mypatterns)
md.pattern(result$amp)



## 通过将完整的数据集分为多个子集而起作用
## 参数freq确定子集的大小，从而确定丢失数据模式的相对发生率

myfreq <- c(0.7, 0.1, 0.1, 0.1)
result <- ampute(testdata, freq = myfreq, patterns = mypatterns)
md.pattern(result$amp)





## MAR+MNAR
## ampute包含函数bwplot和xyplot来调查缺失的行为
   ampute 会产生MCAR，MAR和MNAR缺失。 可以通过更改权重矩阵来生成MAR和MNAR缺失的组合
## 不完整的数据集将同时包含两种类型的缺失机制
 
result <- ampute(testdata, freq = c(0.7, 0.3), patterns = c(0, 0, 1, 0, 1, 0), weights = c(0, 0, 1, 1, 0, 1))
 
## 不能直接产生MCAR和MAR the generation of both MCAR and MAR
## 但是，通过两次运行该函数，仍然可以获得所需的机制组合
ampdata1 <- ampute(testdata, patterns = c(0, 1, 1), prop = 0.2, mech = "MAR")$amp
ampdata2 <- ampute(testdata, patterns = c(1, 1, 0), prop = 0.8, mech = "MCAR")$amp
indices <- sample(x = c(1, 2), size = nrow(testdata), replace = TRUE, 
                  prob = c(1/2, 1/2))
ampdata <- matrix(NA, nrow = nrow(testdata), ncol = ncol(testdata))
ampdata[indices == 1, ] <- as.matrix(ampdata1[indices == 1, ])
ampdata[indices == 2, ] <- as.matrix(ampdata2[indices == 2, ])
md.pattern(ampdata)
```

#### Diagnose

两种方式检查结果：箱线图和散点图。 boxplots and scatterplots.

```
## boxplots 
bwplot(result, which.pat = c(1, 3), descriptives = TRUE)

## Scatterplots
xyplot(result, which.pat = 1)
```

### Simulate MAR missing data

```
# Number of data values
n <- 1000

# Simulate 1000 people with high pre-treatment (mean 28, sd 3) and normal (mean 18, sd 3) post-treatment. Correlation between paired data = 0.7.
set.seed(80122)
data <- rmvnorm(n, mean=c(28,18),
                sigma=matrix(c(9,0.7*sqrt(81),0.7*sqrt(81),9),2,2)) # Covariance matrix

# Convert to data frame:
data = as.data.frame(data)
names(data) = c("pre", "post")


# Simulate missing completely at random (MCAR) data:
data$post_mcar <- data$post
set.seed(2)
data$post_mcar[sample(1:nrow(data), 0.2*nrow(data))] = NA
```

创建分组变量frac，其值是我们要设置为缺失的组的分数。我们将使用cut函数创建这些组并设置标签值，然后将标签转换为数字以供以后使用：按frac分组，然后将随机选择的值的一部分设置为NA，使用frac确定设置为NA的值的一部分。

* First, we’ll create a grouping variable, frac, whose value is the fraction of the group that we want to set to missing. We’ll use the cut function to create these groups and set the label values, then we’ll convert the labels to numeric for later use:
* group by frac and set a randomly selected fraction of the values to NA, using frac to determine the fraction of values set to NA.

```
# Simulate missing at random (MAR) data
data = data %>% 
  mutate(post_mar = post,
         frac = as.numeric(as.character(cut(pre, breaks=c(-Inf, 21, 25, Inf),
                                            labels=c(0.2,0.3,0.4)))))
### 小于21，feac=0.2; 21< <25, frac=0.3
head(data)
```

```
set.seed(3)
data = data %>% 
  group_by(frac) %>% 
  mutate(post_mar=replace(post_mar, row_number(post_mar) %in% sample(1:n(), round(unique(frac)*n())), NA)) %>% 
  ungroup

head(data)
```

| pre\<dbl> | post\<dbl> | post\_mcar\<dbl> | post\_mar\<dbl> | frac\<dbl> |
| --------- | ---------- | ---------------- | --------------- | ---------- |
| 29.29612  | 21.68335   | 21.68335         | 21.68335        | 0.4        |
| 29.11421  | 17.98265   | 17.98265         | 17.98265        | 0.4        |
| 30.15447  | 19.81169   | 19.81169         | NA              | 0.4        |
| 30.29253  | 17.42929   | 17.42929         | 17.42929        | 0.4        |
| 29.59090  | 14.03217   | 14.03217         | NA              | 0.4        |
| 25.95561  | 13.94315   | 13.94315         | 13.94315        | 0.4        |

```
## check on the fraction of values missing in each group.
data %>% group_by(frac) %>% 
  summarise(N=n(), 
            N_missing=sum(is.na(post_mar)), 
            Frac_missing=N_missing/N)
```

| frac\<dbl> | N\<int> | N\_missing\<int> | Frac\_missing\<dbl> |
| ---------- | ------- | ---------------- | ------------------- |
| 0.2        | 8       | 2                | 0.2500000           |
| 0.3        | 138     | 41               | 0.2971014           |
| 0.4        | 854     | 342              | 0.4004684           |

## Multiple Imputation

### Expectation-Maximization (EM) algorithm

There are two primary schools about how to deal with the missing data problem.

On one side, there are model-based methods mainly built around the formulation of the **Expectation-Maximization (EM) algorithm** made popular by Dempster, Laird, and Rubin (1977). This technique makes the **computation of the Maximum Likelihood (ML)estimator feasible in problems affected by missing data.** In short, the EM algorithm is an i**terative procedure that produces maximum likelihood estimates. The idea is to treat the missing data as random variables to be removed by integration from the log-likelihood function as if they were not sampled.** The EM algorithm allows dealing with the missing data and parameter estimation in the same step.&#x20;

The major drawback of this model-based method is the **requirement of the explicit modeling of joint multivariate distributions** and, thus, tend to be limited to variables deemed to be of substantive relevance (Graham, Cumsille, and Elek-Fisk, 2003). Furthermore, this approach requires the correct specification of usually high-dimensional distributions, even of aspects which have never been the focus of empirical research and for which justification is hardly available. According to Graham (2009), the parameter estimators (means, variances, and covariances) from the EM algorithm are preferable over a wide range of possible estimators, based on the fact that they enjoy the properties of maximum likelihood estimation. The second approach deals with model-based missing data procedures and was introduced by Rubin (1987) with his concept of Multiple Imputation (MI). Instead of removing the missing values by integration as EM does, MI simulates a sample of m values from the posterior predictive distribution of the missing values given the observed. Each missing value is replaced by this approach with m>1 possible values, accounting for uncertainty in the values predicting the true but unobserved values. The substituted values are called “imputed” values, hence the term “Multiple Imputation.”

> 一方面，存在基于模型的方法，该方法主要建立在Dempster，Laird和Rubin（1977）流行的期望最大化（EM）算法的公式化周围。该技术使**最大似然（ML）估计器的计算在丢失数据影响的问题中可行。**简而言之，**EM算法是产生最大似然估计的迭代过程。想法是将丢失的数据视为随机变量，通过积分从对数似然函数中删除，就好像它们没有被采样一样。 EM算法允许在同一步骤中处理丢失的数据和参数估计**。
>
> 这种基于模型的方法的主要缺点是需要对联合多元分布进行显式建模，因此往往仅限于被认为具有实质相关性的变量（Graham，Cumsille和Elek-Fisk，2003年）。此外，这种方法需要对通常是高维分布的正确规范，即使是从来没有成为经验研究重点且很难找到合理依据的方面。根据Graham（2009）的观点，基于EM算法的参数估计器（均值，方差和协方差）具有最大似然估计的特性，因此它们在各种可能的估计器中都比较可取。

The second approach deals with model-based missing data procedures and was introduced by Rubin (1987) with his concept of Multiple Imputation (MI). Instead of removing the missing values by integration as EM does, MI simulates a sample of m values from the posterior predictive distribution of the missing values given the observed. Each missing value is replaced by this approach with m>1 possible values, accounting for uncertainty in the values predicting the true but unobserved values. The substituted values are called “imputed” values, hence the term “Multiple Imputation.

> 第二种方法处理基于模型的缺失数据过程，并由Rubin（1987）引入他的多重插补（MI）概念。 MI并没有像EM那样通过积分去除缺失值，而是从给定观察值的缺失值的后验预测分布中模拟了m个值的样本。用这种方法用m> 1个可能的值替换每个丢失的值，这说明了预测真实但未观察到的值的不确定性。替换值称为“插补”值，因此称为“多次插补”。

### Advantages of Multiple Imputation

Multiple imputation shares with single imputation the two basic advantages already mentioned, namely, the ability to use complete-data methods of analysis and the ability to incorporate the data collector’s knowledge. In fact, the second basic advantage is not only retained but enchanced because multiple imputations allow data collectors to use their knowledge to reflect uncertainty about whch values to impute. This uncertainty is of two types: sampling variability assuming the reasons for nonresponse are known and variability due to uncertainty about the reasons for nonresponse. Under each posited model for nonresponse, two or more imputations are created to reflect sampling variability under that model; imputations under more than one model for nonresponse reflect uncertainty about the reasons for nonresponse.

There exist three extremely important advantages to multiple imputation over single imputation.&#x20;

* First, when imputations are randomly drawn in an attempt to represent the distribution of the data, multiple imputation increases the efficiency of estimation.
* The second distinct advantage of multiple imputation over single impu- tation is that when the multiple imputations represent repeated random draws under a model for nonresponse, valid inferences- that is, ones that reflect the additional variability due to the missing values under that model -are obtained simply by combining complete-data inferences in a straightforward manner.
* The third distinct advantage of multiple imputation is that by generating repeated randomly drawn imputations under more than one model, it allows the straightforward study of the sensitivity of inferences to various models for nonresponse simply using complete-data methods repeatedly.

> 多重归因与单一归因共有两个已经提到的基本优点，即使用完整数据分析方法的能力和合并数据收集者知识的能力。实际上，第二个基本优点不仅保留，而且具有吸引力，因为多重估算使数据收集者可以利用其知识来反映有关估算值的不确定性。这种不确定性有两种类型：假定已知无响应原因的采样变异性和由于无响应原因的不确定性造成的变异性。在每个无响应的假定模型下，都会创建两个或多个插补，以反映该模型下的抽样变异性。在不答复的一种以上模型下的估算反映了对不答复原因的不确定性。
>
> &#x20;相对于单一插补，多重插补存在三个极其重要的优势。
>
> * 首先，多次插补会提高估算效率。
> * 多重插补相对于单一插补的第二个独特优势是，当多个插补代表无响应模型下的重复随机抽取时，有效推论-即由于该模型下缺少值而反映出额外可变性的推论-通过以简单的方式组合完整数据推断即可轻松获得。&#x20;
> * 多重插补的第三个独特优势是，通过在多个模型下生成重复随机绘制的插补，可以简单地反复使用完整数据方法，直接研究对无响应的各种模型的推论的敏感性。

There are three obvious disadvantages of multiple imputation relative to single imputation.&#x20;

* First, more work is needed to produce multiple imputations than single imputations.&#x20;
* Second, more space is needed to store a multiply-imputed data set. Thud, more work is needed to analyze a multiply-imputed data set than a singly-imputed data set. These disadvantages are not serious when m is modest.&#x20;
* Modest m is adequate when fractions of missing information are modest. When fractions of missing information are large, modest-m multiple imputation is not fully satisfactory, but then single imputation can be disastrous.

> 首先，要产生多个插补而不是单个插补需要更多的工作。 \
> 其次，需要更多空间来存储多次插值数据集。 因此，与单插值数据集相比，分析多插值数据集需要更多的工作。 当m适中时，这些缺点并不严重。 当丢失信息的分数适中时，适度的m就足够了。 \
> 当丢失信息的分数很大时，适度的多重插补不能完全令人满意，但是单次插补可能会造成灾难性的后果。

### JM and FCS

There are two ways of specifying imputation models: Joint modeling (JM) and fully conditional specification (FCS). Joint modeling involves specifying a multivari-ate distribution for the variables whose values have not been observed conditional on the observed data and then drawing imputations from this conditional distribu-tion by Markov chain Monte Carlo (MCMC) techniques (Schafer, 1997). On the other hand, with the fully conditional specification, also known as multivariate imputation by chained equations (van Buuren and Groothuis-Oudshoorn, 2011), a univariate imputation model is specified for each variable with missings conditional on other vari-ables of the data set. Initial missing values are imputed with a bootstrap sample, and then subsequent imputations are drawn by iterating over conditional densities (vanBuuren, 2007; van Buuren and Groothuis-Oudshoorn, 2011).

> 指定插补模型的方式有两种：联合建模（JM）和完全条件规范（FCS）。 联合建模涉及为未在观测数据条件下观测到其值的变量指定多变量分布，然后通过马尔可夫链蒙特卡洛（MCMC）技术从该条件分布中得出推论（Schafer，1997）。 另一方面，对于完全条件规范（也称为链式方程式的多元插补）（van Buuren和Groothuis-Oudshoorn，2011），为每个变量指定了单变量插补模型，其缺失取决于数据集的其他变量。 初始缺失值通过引导样本进行插补，然后通过对条件密度进行迭代来得出后续插补（vanBuuren，2007； van Buuren和Groothuis-Oudshoorn，2011）

### Compatible

To discuss the validity of the FCS approach it is necessary to define the term “compatibility” first. A set of density functions,{f1, . . . ,fj}, is said to be compatible if there is a joint distribution that generates such set. The same flexibility of MICE that allows for very special conditional distributions and imputation models has as a drawback the fact that the joint distribution is not explicitly known, and there is the possibility that it doesn’t even exists. This is the case if the conditional distributions specified are incompatible. Incompatibility in MICE can be the result of imputing deterministic functions of variables in the data along with those same original variables. For example, there could be interaction terms or nonlinear functions of the data in the imputation models, introducing feedback loops and impossible combination in the algorithm which would lead to invalid imputations (van Buuren and Groothuis-Oudshoorn, 2011). For that reason, the discussion about the congeniality of the imputation and substantive models is replaced by an analysis of their compatibility.

Although FCS is only justified to work if the conditional models are compatible, Buuren et al. (2006) reports a simulation study with models with strong incompatibilities where the estimates after performing multiple imputation were still acceptable.

> 为了讨论FCS方法的有效性，有必要首先定义术语“兼容性”。一组密度函数，{f1，...。 。 。 ，fj}，如果存在产生这种集合的联合分布，则被认为是兼容的。 MICE具有相同的灵活性，可用于非常特殊的条件分布和归因模型，但缺点是联合分布未知，甚至有可能根本不存在。如果指定的条件分布不兼容，就是这种情况。 MICE中的不兼容性可能是由于将数据中的变量以及那些相同的原始变量插入了确定性函数而导致的。例如，归因模型中可能存在交互项或数据的非线性函数，在算法中引入了反馈循环和不可能的组合，这将导致无效的归因（van Buuren和Groothuis-Oudshoorn，2011）。因此，关于插补模型和实体模型的合意性的讨论被对它们的兼容性的分析所代替。
>
> 尽管只有在条件模型兼容的情况下，才有理由证明FCS可以工作，但Buuren等人。 （2006年）报道了一个模拟研究，该模型使用不兼容很强的模型进行了多次插补后的估计仍然可以接受。

### Congenial

Strengths and weaknesses of multiple imputationproceduresAn important notion concerning the success of the method of multiple imputation is the hypothesis of “proper” multiple imputation. The concept of proper imputations is based on a set of conditions imposed on the imputation procedure. An imputation method tends to be proper if the imputations are independent draws from an appropriate posterior predictive distribution of the variables with missing values given all other variables (Rubin, 1987). This implies, that both, the average of them point estimators is a consistent, asymptotically normal estimator of the parameter of scientific interest and that an estimator of its asymptotic variance is given by a combination of the within and between variance of the point estimators. Meng (1994) showed the consistency of the multiple imputation variance estimator as the number of imputations tends to infinity but restricted his analysis to “congenial” situations, in which imputation and analysis models match each other in a certain sense.

A simpler explanation is that there must be some connection between the analysis and imputation models. They can be fitted separately and to some extent, considered independently from each other, but they are not. The concept of “congeniality”, introduced by Meng (1994), establishes the required relationship between analysis procedure and imputation method.

> 多重插补程序的优点和缺点关于多重插补方法成功的一个重要概念是“适当”多重插补的假设。适当插补的概念基于对插补程序施加的一组条件。如果插补是从变量的适当后验预测分布中独立得出的，则插补方法往往是适当的，这些变量在给定所有其他变量的情况下具有缺失值（Rubin，1987）。这意味着，两者的均值估计量是科学兴趣参数的一致，渐近正态估计量，并且其渐近方差的估计量是由点估计量的方差之内和之间的组合给出的。 Meng（1994）证明了多重插补方差估计量的一致性，因为插补的数量趋于无穷大，但他的分析仅限于“同类”情况，在这种情况下，插补和分析模型在某种意义上是相互匹配的。
>
> 一个更简单的解释是，分析模型和插补模型之间必须存在某种联系。它们可以分开安装，并且在某种程度上可以相互独立考虑，但事实并非如此。 Meng（1994）提出的“同质性”概念建立了分析程序和插补方法之间的必要关系。

### Imputation under the normal linear normal

#### Predict method (norm.predict)

$$\dot y=\hat\beta_0+X_\mathrm{mis}\hat\beta_1$$ 第一种可能性是计算回归线，并从回归线中进行估算。预测值不能反映这种不确定性，因此不能用作多重估算。

norm.predict存在严重偏差，因此方差校正是没有用的。归因于插补模型不确定性的方法norm和norm.boot提供了统计上正确的推论

#### Predict + noise (norm.nob)

$$\dot y=\hat\beta_0+X_\mathrm{mis}\hat\beta_1+\dot\epsilon$$, where $$\dot\epsilon$$ is randomly drawn from the normal distribution as $$\dot\epsilon \sim N(0,\hat\sigma^2)$$ 我们可以通过在预测值上添加适当数量的随机噪声来改进预测方法。

#### Bayesian multiple imputation (norm)

参数不确定性也需要包括在估算中。这样做有两种主要方法。贝叶斯方法直接从其后验分布中提取参数，而自举方法则对观察到的数据进行重新采样并从重新采样的数据中重新估计参数

最好通过贝叶斯方法或引导方法包括参数不确定性。这样做的效果会随着样本数量的增加而减弱（练习3.2），因此对于基于大样本的估计，如果计算速度非常快，则可以选择更简单的norm.nob方法。请注意，在子组分析中，大样本要求适用于子组大小，而不适用于总样本大小。

$$\dot y =\dot\beta_0 + X_\mathrm{mis}\dot\beta_1+\dot\epsilon$$, where $$\dot\epsilon \sim N(0,\dot\sigma^2)$$ and $$\dot\beta_0,\dot\beta_1,\dot\sigma$$ are random draws from their posterior distribution, given the data.

**Algorithm: Bayesian imputation under the normal linear model**

* $$y_{obs}$$ the n1 × 1 vector of observed data in the incomplete (or target) variable y
* $$X_{obs}$$ the n1 × q matrix of predictors of rows with observed data in y
* $$X_{mis}$$ the n0 × q matrix of predictors of rows with missing data in y

The algorithm uses a ridge parameter κ to evade problems with singular matrices. This number should be set to a positive number close to zero, e.g., 0.0001&#x20;

1. Calculate the cross-product matrix $$S=X\mathrm{obs}^\prime X\mathrm{obs}$$.
2. Calculate $$V = (S+\mathrm{diag}(S)\kappa)^{-1}$$ with some small κ.
3. Calculate regression weights $$\hat\beta = VX\mathrm{obs}^\prime y\mathrm{obs}$$
4. Draw a random variable $$\dot g \sim \chi^2_\nu$ with $\nu=n_1 - q$$
5. Calculate $$\dot\sigma^2 = (y\mathrm{obs} - X\mathrm{obs}\hat\beta)^\prime(y\mathrm{obs} - X\mathrm{obs}\hat\beta) / \dot g$$
6. Draw $q$ independent $$N(0,1)$$ variates in vector $$\dot z_1$$
7. Calculate $$V^{1/2}$$ by Cholesky decomposition.
8. Calculate $$\dot\beta = \hat\beta + \dot\sigma\dot z_1 V^{1/2}$$
9. Draw $n\_0$ dependent $$N(0,1)$$ variates in vector $$\dot z_1$$
10. Calculate the $$n0$$_values_ $$y\mathrm{imp} = X_\mathrm{mis}\dot\beta + \dot z_2\dot\sigma$$

#### Bootstrap multiple imputation (norm.boot)

$$\dot y =\dot\beta_0 + X_\mathrm{mis}\dot\beta_1+\dot\epsilon$$, where $$\dot\epsilon \sim N(0,\dot\sigma^2)$$ and $$\dot\beta_0,\dot\beta_1,\dot\sigma$$ are the least squares estimates calculated from a bootstrap sample taken from the observed data.

**Algorithm: Imputation under the normal linear model with bootstrap**

1. Draw a bootstrap sample $$(\dot y\mathrm{obs},\dot X\mathrm{obs})$$ of size $$n1$$ from $$(y\mathrm{obs},X_\mathrm{obs})$$
2. Calculate the cross-product matrix $$\dot S=\dot X\mathrm{obs}^\prime\dot X\mathrm{obs}$$
3. Calculate $$\dot V = (\dot S+\mathrm{diag}(\dot S)\kappa)^{-1}$$, with some smal
4. Calculate regression weights $$\dot\beta = \dot V\dot X\mathrm{obs}^\prime\dot y\mathrm{obs}$$
5. Calculate $$\dot\sigma^2 = (\dot y\mathrm{obs} - \dot X\mathrm{obs}\dot\beta)\prime(\dot y\mathrm{obs} - \dot X\mathrm{obs} \dot\beta)/(n_1-q-1)$$
6. Draw $$n_0$$ independent $$N(0,1)$$ variates in vector $$\dot z_2$$
7. Calculate the $$n0$$ values $$y\mathrm{imp} = X_\mathrm{mis}\dot\beta + \dot z_2\dot\sigma$$

### Multivariate imputation by chained equations (MICE)

Multivariate imputation by chained equations (MICE) (Van Buuren 2018) is also known as Sequential Regression Imputation, Fully Conditional Specification or Gibbs sampling. In the MICE algorithm, a chain of regression equations is used to obtain imputations, which means that variables with missing data are imputed one by one. The regression models use information from all other variables in the model, i.e. (conditional) imputation models. In order to add sampling variability to the imputations, residual error is added to create the imputed values. This residual error can either be added to the prediced values directly, which is esentially similar to repeating stochastic regression imputation over several imputation runs. Residual variance can also be added to the parameter estimates of the regression model, i.e. the regression coefficients, which makes it a Bayesian method. The Bayesian method is the default in the mice package in R.

> 通过链式方程进行的多元插补（MICE）（Van Buuren 2018）也被称为顺序回归插补，全条件规范或吉布斯采样。 在MICE算法中，使用了一系列回归方程来获得估算值，这意味着具有缺失数据的变量将被逐一估算。 回归模型使用模型中所有其他变量的信息，即（条件）插补模型。 为了将采样的可变性添加到估算中，添加了残留误差以创建估算值。 该残留误差可以直接添加到预测值，本质上类似于在多个插补运行中重复随机回归插补。 残差也可以添加到回归模型的参数估计中，即回归系数，这使其成为贝叶斯方法。 贝叶斯方法是R中mouses包中的默认方法。

This regression model is defined as:

$$
Disability_{mis} = \beta_0 + \beta_1Pain + \beta_2Tampa + \beta_3Radiation
$$

### Guidelines for the Imputation model

* Include all variables that are part of the analysis model, including the dependent (outcome) variable. 包括属于分析模型的所有变量，包括因变量（结果）。
* Include the variables at the same way in the imputation model as they appear in the analysis model (i.e. if interaction terms are in the analysis model also include them in the imputation model). \
  以与分析模型中出现的变量相同的方式将其包括在插补模型中（即，如果交互项在分析模型中，则也将其包括在插补模型中）
* Include additional (auxiliary) variables that are related to missingness or to variables with missing values. \
  包括与缺失或具有缺失值的变量相关的其他（辅助）变量
* Be aware of including variables with a high correlation (> 0.90). \
  注意具有高相关性（> 0.90）的变量
* Be aware of variables with high percentages of missing data (> 50%). \
  注意丢失数据所占百分比较高（大于50％）的变量

## MICE

1. mice()首先从一个包含缺失数据的数据框开始，然后返回一个包含多个（默认为5个）完整数据集的对象。每个完整数据集都是通过对原始数据框中的缺失数据进行插补而生成的。meth为默认插补方式，PMM为默认方式预测均值匹配。 help(mice)获取信息
2. with()函数可依次对每个完整数据集应用统计模型,选择插补模型：方法包括做线回归模型的lm（）函数、做广义线性模型的glm（）函数、做广义可加模型的gam（）、及做负二项模型的nbrm（）函数。
3. pool()函数将这些单独的分析结果整合为一组结果。最终模型的标准误和p值都将准确地反映出由于缺失值和多重插补而产生的不确定性

### 缺失值可视化

* md.pattern
* package: VIM
  * aggr
  * marginplot

```
data(iris)
str(iris)
md.pattern(iris)

##  /\     /\
## {  `---'  }
## {  O   O  }
## ==>  V <==  No need for mice. This data set is completely observed.
##  \  \|/  /
##   `-----'

## Generate 10% missing values at Random 
library(missForest)
iris.mis <- prodNA(iris, noNA = 0.1)
summary(iris.mis)      # Check missing values introduced in the data

## 计算数据的缺失率
miss <- function(x){sum(is.na(x))/length(x)*100}
apply(iris.mis,2,miss)
md.pattern(iris.mis)


## 用VIM包获得缺失值的可视化表示
library(VIM)
aggr(iris.mis, col=c('navyblue','red'), numbers=TRUE, labels=names(iris.mis), 
     cex.axis=.7, gap=3, ylab=c("Histogram  of missing data","Pattern"))
```

![](https://firebasestorage.googleapis.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-MRt1rIudcHYDxTcux\_Q%2Fuploads%2Fv16J62LD5yhnnnjXECzV%2Ffile.png?alt=media)

```
# marginplot(data[c(1,2)])一次只表示2个变量的缺失情况
# 红色箱线图表示有缺失的样本的变量的分布，蓝色的箱线图表示的是剩下的数据点的分布
marginplot(iris.mis[c(2,3)])
```

![](https://firebasestorage.googleapis.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-MRt1rIudcHYDxTcux\_Q%2Fuploads%2FkgxiTGjRK9uxSstbchnP%2Ffile.png?alt=media)

### Imputation methods

```
# maxit，迭代次数，默认50次
iris.imp<-mice(iris.mis,seed=123,m=5,maxit=50,meth='pmm')
summary(iris.imp)
iris.imp$imp$Species          # 查看填充值

## 生成完全数据集,iris.imp$imp中的第一个矩阵来填充

completedData <- complete(iris.imp,1)
summary(completedData)

## stochastic regression imputation.
method = "norm.nob"

## regression imputation.
method = "norm.predict"

## mean imputation.
method = "mean"

## An up-to-date overview of the methods in mice can be found by
methods(mice)

##  [1] mice.impute.2l.bin       mice.impute.2l.lmer      mice.impute.2l.norm     
##  [4] mice.impute.2l.pan       mice.impute.2lonly.mean  mice.impute.2lonly.norm 
##  [7] mice.impute.2lonly.pmm   mice.impute.cart         mice.impute.jomoImpute  
## [10] mice.impute.lda          mice.impute.logreg       mice.impute.logreg.boot 
## [13] mice.impute.mean         mice.impute.midastouch   mice.impute.mnar.logreg 
## [16] mice.impute.mnar.norm    mice.impute.norm         mice.impute.norm.boot   
## [19] mice.impute.norm.nob     mice.impute.norm.predict mice.impute.panImpute   
## [22] mice.impute.passive      mice.impute.pmm          mice.impute.polr        
## [25] mice.impute.polyreg      mice.impute.quadratic    mice.impute.rf          
## [28] mice.impute.ri           mice.impute.sample       mice.mids               
## [31] mice.theme   
```

### Predictive matrix

```
set.seed(123)
imp <- mice(nhanes, m = 3, print=F)

## Change the predictor matrix
## 初始预测变量矩阵
ini <- mice(nhanes, maxit=0, print=F)
pred <- ini$pred
pred


## 修改矩阵：删除了变量hyp，但仍然让其他变量对其进行预测。
pred[ ,"hyp"] <- 0
imp <- mice(nhanes, pred=pred, print=F)

## quickpred（）的特殊函数，用于快速选择预测变量，对于包含许多变量的数据集可能很方便
## Selecting predictors according to data relations with a minimum 
   correlation of ρ=.30
ini <- mice(nhanes, pred=quickpred(nhanes, mincor=.3), print=F)
ini$pred

##     age bmi hyp chl
## age   0   0   0   0
## bmi   1   0   0   1
## hyp   1   0   0   1
## chl   1   1   1   0
```

### Algorithmic convergence

```
## Inspect the convergence of the algorithm
## The mice() function implements an iterative Markov Chain Monte Carlo type 
   of algorithm. the trace lines generated by the algorithm to study convergence:
## In general, we would like the streams to intermingle and be free of any trends 
   at the later iterations. 
   通常，我们希望这些流混合在一起，并且在以后的迭代中没有任何趋势。
imp <- mice(nhanes, print=F)
plot(imp)

## extend the number of iterations
## 需要扩展MICE算法的迭代次数，以确认没有趋势并且迹线混合良好。
   通过使用mice.mids（）函数运行35个额外的迭代，我们可以将迭代次数增加到40。
imp40 <- mice.mids(imp, maxit=35, print=F)
plot(imp40)
```

### Diagnostic checking

```
## plausibility: 合理性的想法，可以检查估算并将其与观察值进行比较
## 在MCAR下，观测数据和估算数据的单变量分布预计是相同的。
   在MAR下，它们在位置和传播方面都可以不同，但是假定它们的多元分布是相同的。
stripplot(imp, chl~.imp, pch=20, cex=2)
stripplot(imp) 


## Several plotting methods based on the package lattice for Trellis graphics 
   are implemented in mice for imputed data.
## obtain all densities of the different imputed datasets use
## All parameters
densityplot(imp1)
## One parameter
densityplot(imp1, ~ popular)
## Separate
densityplot(imp1, ~ popular | .imp)
```

### Test

#### Pooling Independent T-tests

```
imp <- mice(dataset, m=3, maxit=50, seed=2375, printFlag=F)
 
# Conduct an independent t-test via lm in each imputed dataset
fit.t.test <- with(data=imp, exp=lm(Tampascale ~ Radiation))
t.test.estimates <- pool(fit.t.test)
summary(t.test.estimates)

# Extract the imputed datasets and define the Radiation variable as factor variable
dataset1 <- complete(imp,1)
dataset1$Radiation <- factor(dataset1$Radiation)
dataset2 <- complete(imp,2)
dataset2$Radiation <- factor(dataset2$Radiation)
dataset3 <- complete(imp,3)
dataset3$Radiation <- factor(dataset3$Radiation)
 
# Assign the imputed datasets to the list object dataset.imp
dataset.imp <- list(dataset1, dataset2, dataset3)
 
# Start the MKmisc library and run the mi.t.test function to get pooled 
# results  of the t-test
library(MKmisc)

# Result of the pooled t-test
mi.t.test(dataset.imp, x = "Tampascale", y = "Radiation", var.equal = TRUE)
```

#### Pooling Chi-square tests

```
library(miceadds)
micombine.chisquare(c(1.829, 1.311, 2.861, 1.771, 3.690), 2, display = TRUE, version=1)
```

#### Analysis of Variance (ANOVA)

```
# Generate 5 impued datasets 
# and set printFlag = F for a silent imputation
imp.Tampa.cat <- mice(dataset, m=5, maxit=50, seed=2345, printFlag = FALSE)

# Apply the mi.anova function
library(miceadds)
mi.anova(mi.res=imp.Tampa.cat, formula="Function ~ Tampa_Cat")
```

#### Pooling Linear regression models

```
imp <- mice(dataset, m=3, maxit=50, seed=3715, printFlag=FALSE)
fit <- with(data=imp,exp=lm(Function ~ Tampascale))
lin.pool <- pool(fit)
summary(lin.pool)
```

#### Pooling Logistic Regression models

```
library(foreign)
dataset <- read.spss(file="~/Library/Mobile Documents/com~apple~CloudDocs/02. Programm/01 R_lernen/00 Data/Backpain 150 Missing.sav", to.data.frame=T)
imp.LR <- mice(dataset, m=3, maxit=50, seed=2268, printFlag = FALSE)
fit <- with(data=imp.LR, exp=glm(Radiation ~ Function, family = binomial))
summary.fit <- summary(pool(fit))
pool.OR <- exp(cbind(summary.fit[,2], (summary.fit[,2]-1.96*(summary.fit[,3])), 
           (summary.fit[,2]+1.96*(summary.fit[,3]))))
colnames(pool.OR) <- (c("OR", "95% LO", "95% UP"))
pool.OR

##             OR    95% LO   95% UP
## [1,] 1.2840917 0.4971941 3.316394
## [2,] 0.9426891 0.8723801 1.018665
```







### Passive imputation

```
meth<- ini$meth
meth["bmi"]<- "~ I(wgt / (hgt / 100)^2)"
imp <- mice(boys, meth=meth, print=FALSE)

xyplot(imp, bmi ~ I(wgt / (hgt / 100)^2), na.groups = miss,
       cex = c(1, 1), pch = c(1, 20),
       ylab = "BMI (kg/m2) Imputed", xlab = "BMI (kg/m2) Calculated")
```

### MICE Imputing multi-level data

```
## load data
con <- url("https://www.gerkovink.com/mimp/popular.RData")
load(con)
ls()

dim(popNCR)
nrow(popNCR)
summary(popNCR)
md.pattern(popNCR)

histogram(~ popteach | is.na(popular), data=popNCR)


## Have a look at the intraclass correlation (ICC) for the incomplete variables 
   类内相关性
icc(aov(popular ~ as.factor(class), data = popNCR))
## [1] 0.328007

icc(aov(popteach ~ as.factor(class), data = popNCR))
## [1] 0.3138658

icc(aov(texp ~ as.factor(class), data = popNCR))
## [1] 1


## 0 maxit imputation
ini <- mice(popNCR, maxit = 0)
meth <- ini$meth
## change the methode
meth[c(3, 5, 6, 7)] <- "norm"


## change the predict matrix
pred <- ini$pred
pred[, "class"] <- 0
pred[, "pupil"] <- 0

imp1 <- mice(popNCR, meth = meth, pred = pred, print = FALSE)
## comparison
summary(complete(imp1))

## Compare the ICC’s of the variables in the first imputed dataset 
   with those in the incomplete dataset
data.frame(vars = names(popNCR[c(6, 7, 5)]), 
           observed = c(icc(aov(popular ~ class, popNCR)), 
                        icc(aov(popteach ~ class, popNCR)), 
                        icc(aov(texp ~ class, popNCR))), 
           norm     = c(icc(aov(popular ~ class, complete(imp1))), 
                        icc(aov(popteach ~ class, complete(imp1))), 
                        icc(aov(texp ~ class, complete(imp1)))))
                        
## obtain all densities of the different imputed datasets use
densityplot(imp1)
densityplot(imp1, ~ popular)
```

### Variable selection

#### Variable Selection with Logistic Regression models

“Package psfmi” contains functions to pool and select variables in multiple imputed datasets.

```
library(foreign)
library(psfmi)

pool_lr <- psfmi_lr(data=lbpmilr, nimp=10, impvar="Impnr", Outcome="Chronic",
  predictors=c("Gender", "Smoking", "Function", "JobControl", "JobDemands",
  "SocialSupport"), cat.predictors = c("Carrying", "Satisfaction"), 
  p.crit = 0.05, method="D1", print.method = FALSE)
pool_lr$RR_Model
```

```
## $`Step 1`
##                           est std.err signif   lower  upper     OR   L.OR
## (Intercept)           -2.2989  2.7095 0.3962 -7.6115 3.0138 0.1004 0.0005
## Gender                -0.2877  0.4580 0.5299 -1.1855 0.6101 0.7500 0.3056
## Smoking               -0.0245  0.3761 0.9481 -0.7617 0.7128 0.9758 0.4669
## Function              -0.0811  0.0505 0.1088 -0.1802 0.0180 0.9221 0.8351
## JobControl             0.0027  0.0215 0.8996 -0.0394 0.0448 1.0027 0.9614
---
---
## $`Step 8`
##                       est std.err signif   lower   upper     OR   L.OR    U.OR
## (Intercept)       -1.6359  0.3985  0.000 -2.4173 -0.8544 0.1948 0.0892  0.4255
## factor(Carrying)2  1.4768  0.4956  0.003  0.5042  2.4495 4.3790 1.6556 11.5821
## factor(Carrying)3  2.2751  0.4886  0.000  1.3174  3.2328 9.7289 3.7338 25.3501
```

#### Variable Selection with Cox Regression models

使用“ D1”方法在该模型中应用变量选择，以获取分类变量的合并p值，而二分和连续变量则使用鲁宾规则， 并使用0.05的p值作为选择标准。

```
library(foreign)
library(psfmi)

pool_coxr <- psfmi_coxr(data=lbpmicox, nimp=10, impvar="Impnr", time="Time", status="Status",
  predictors=c("Duration", "Radiation", "Onset", "Function", "Age",
  "Previous", "Tampascale", "JobControl", "JobDemand", "Social"), 
  cat.predictors=c("Expect_cat"), p.crit = 0.05, method="D1", print.method = FALSE)
pool_coxr$RR_Model
```
