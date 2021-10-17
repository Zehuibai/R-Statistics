---
description: 多元自适应回归样条
---

# Multivariate Adaptive Regression Spline

使用基于概率的线性模型预测定性响应变量，三种方法:

* **Logistic regression**
* **Linear discriminant analysis (线性判别分析)**
* **Multivariate adaptive regression spline (多元自适应回归样条)**

## introduction

### Initialization introduction

多元自适应回归样条(Multivariate Adaptive Regression Splines,MARS)是由美国的统计学家Jerome Friedman于1991年提出的一种数据分析方法。 多元自适应回归样条（MARS）（Friedman 1991）可自动创建分段线性模型，可以直观地逐步进入非线性。对于回归和分类问题，多元自适应回归样条都可以灵活建立线性模型和非线性模型。支持变量的交互项。几乎不需要数据预处理。可以处理所有类型的数据:数值型、因子等可以很好地预测未知数据，也就是说，在偏差方差的权衡方面做得非常好. 可以从两个角度来理解它

* **首先，它可以看成是逐步线性回归的推广**: 线性模型对线性有很强的假设，而这种假设通常很差，可能会影响预测精度。我们可以扩展线性模型以捕获任何非线性关系。通常，这是通过明确包含多项式项 （多项式回归是一种回归形式，但是过多的包含会出现多重共线性）。多项式的替代方法是使用阶跃函数。
* **其次，也可以看成是为了提高 CART 在回归中的效果而进行的改进**

First, MARS naturally handles mixed types of predictors (quantitative and qualitative). MARS considers all possible binary partitions of the categories for a qualitative predictor into two groups. Each group then generates a pair of piecewise indicator functions for the two categories. MARS also requires minimal feature engineering (e.g., feature scaling) and performs automated feature selection. For example, since MARS scans each predictor to identify a split that improves predictive accuracy, non-informative features will not be chosen. Furthermore, highly correlated predictors do not impede predictive accuracy as much as they do with OLS models.

> 首先，MARS自然处理混合类型的预测变量（定量和定性）。 MARS将定性预测变量的类别的所有可能的二元划分考虑为两组。然后，每个组为这两个类别生成一对分段指标函数。 MARS还需要最少的特征工程（例如，特征缩放）并执行自动特征选择。例如，由于MARS会扫描每个预测变量以识别可提高预测准确性的拆分，因此将不会选择非信息特征。此外，高度相关的预测变量不会像使用OLS模型那样严重地妨碍预测准确性。

However, one disadvantage to MARS models is that they’re typically slower to train. Since the algorithm scans each value of each predictor for potential cutpoints, computational performance can suffer as both n and p increase. Also, although correlated predictors do not necessarily impede model performance, they can make model interpretation difficult. When two features are nearly perfectly correlated, the algorithm will essentially select the first one it happens to come across when scanning the features. Then, since it randomly selected one, the correlated feature will likely not be included as it adds no additional explanatory power.

> MARS模型的缺点之一是训练速度通常较慢。由于该算法会扫描每个预测变量的每个值以寻找潜在的切入点，因此随着n和p的增加，计算性能可能会受到影响。同样，尽管相关的预测变量不一定会阻碍模型性能，但它们会使模型解释变得困难。当两个特征几乎完全相关时，该算法实质上将选择扫描特征时碰巧遇到的第一个特征。然后，由于它是随机选择的，因此相关特征可能不包括在内，因为它没有增加任何附加的解释能力。

### Model

MARS 采用形式为 $$(x-t)_{+},(t-x)_{+}$$ 的分段线性基函数的展开. “+"表示正的部分, 所以

$$
(x-t)_{+}=\left\{\begin{array}{ll}
x-t & \text { if } x>t \\
0 & \text { otherwise }
\end{array}\right. \text { and }(t-x)_{+}=\left\{\begin{array}{ll}
t-x & \text { if } x<t \\
0 & \text { otherwise }
\end{array}\right.
$$

 每个函数是分段线性的，在值 t 处有一个 **结点 (knot)**．建立模型的策略类似向前逐步线性回归，使用集合 C 中的基函数及其基函数间的乘积．

z.B.

$$
\begin{equation}
  \text{y} = 
  \begin{cases}
    \beta_0 + \beta_1(1.183606 - \text{x}) & \text{x} < 1.183606, \\
    \beta_0 + \beta_1(\text{x} - 1.183606) & \text{x} > 1.183606 \quad \& \quad \text{x} < 4.898114, \\
    \beta_0 + \beta_1(4.898114 - \text{x}) & \text{x} > 4.898114
  \end{cases}
\end{equation}
$$

### Fitting a basic MARS model

```
## Data Preparation
library(MASS)
data(biopsy)
biopsy$ID = NULL
names(biopsy) = c("thick", "u.size", "u.shape", "adhsn", 
                  "s.size", "nucl", "chrom", "n.nuc", "mit", "class")
biopsy.v2 <- na.omit(biopsy)

## 为了满 足这个要求，可以创建一个变量y，用0表示良性，用1表示恶性
## 使用ifelse()函数为y赋值
y <- ifelse(biopsy.v2$class == "malignant", 1, 0)
set.seed(123) #random number generator
ind <- sample(2, nrow(biopsy.v2), replace = TRUE, prob = c(0.7, 0.3))
train <- biopsy.v2[ind==1, ] #the training data set

## 一定要设定如何精简模型，并且使响应变量服从二元分布
## 可以使用5折交叉验证 来选择模型(pmethod="cv"，nfold = 5)，重复3次(ncross = 3);
## 使用没有交互项的加法 模型(degree = 1)，每个输入特征只使用一个铰链函数(minspan = -1)
## 当前数据中，交互项和多个铰链函数都会导致过拟合。
library(earth)
set.seed(1)
earth.fit <- earth(class ~ ., data = train,
                   pmethod = "cv",
                   nfold = 5,
                   ncross = 3,
                   degree = 1,
                   minspan = -1,
                   glm=list(family=binomial)
                   )
summary(earth.fit)

## MARS模型的先根据数据建立一个标准线性回归模型，它的响应变量在内部编码为0和1
## 对特征(变量) 进行精简之后，生成最终模型，并将其转换为一个GLM模型。可以不用考虑R方的值

## Call: earth(formula=class~., data=train, pmethod="cv",
##             glm=list(family=binomial), degree=1, nfold=5, ncross=3, minspan=-1)
## 
## GLM coefficients
##              malignant
## (Intercept) -6.2980613
## u.size       0.1183107
## adhsn        0.3022766
## s.size       0.2783728
## nucl         0.4133847
## n.nuc        0.2208604
## h(3-thick)  -0.3658432
## h(thick-3)   0.6670414
## h(3-chrom)  -0.5960837
## h(chrom-3)   0.2151845
## 
## GLM (family binomial, link logit):
##  nulldev  df       dev  df   devratio     AIC iters converged
##  620.989 473   80.9185 464       0.87   100.9     8         1
## 
## Earth selected 10 of 10 terms, and 7 of 9 predictors (pmethod="cv")
## Termination condition: RSq changed by less than 0.001 at 10 terms
## Importance: nucl, u.size, thick, n.nuc, chrom, s.size, adhsn, ...
## Number of terms at each degree of interaction: 1 9 (additive model)
## Earth GRSq 0.8327277  RSq 0.8452165  mean.oof.RSq 0.8390962 (sd 0.0403)
## 
## pmethod="backward" would have selected:
##     8 terms 7 preds,  GRSq 0.8354593  RSq 0.8450554  mean.oof.RSq 0.8325594


## 展示了保持其他预测变量不变， 某个预测变量发生变化时，响应变量发生的改变。
## 清楚地看到铰链函数对浓度所起的作用
plotmo(earth.fit)

## plotd()函数，可以生成按类别标签分类的预测概率密度图
plotd(earth.fit)

## 变量之间的相对重要性
## nsubsets是精简过程完成之后包含这个变 量的模型的个数。
## 对于gcv和rss列，其中的值表示这个变量贡献的gcv和rss值的减少量(gcv 和rss的范围都是0~100):
evimp(earth.fit)
##        nsubsets   gcv    rss
## nucl          9 100.0  100.0
## u.size        8  43.9   44.9
## thick         7  23.1   25.2
## n.nuc         6  14.0   16.9
## chrom         5   6.1   10.8
## s.size        4   1.7    8.2
## adhsn         3  -5.2    4.8
```

### Tuning

MARS模型相关的两个重要调整参数是：

* 最大的交互程度 the maximum degree of interactions
* 最终模型中保留的项数 the number of terms retained in the final model

我们需要执行网格搜索来确定这些超参数的最佳组合，以使预测误差最小. 将执行CV网格搜索以识别最佳的超参数组合。 在下面，我们建立了一个网格，该网格评估了交互复杂度（度）和保留在最终模型中的项数（nprune）的30种不同组合。

```
# create a tuning grid
hyper_grid <- expand.grid(
  degree = 1:3, 
  nprune = seq(2, 100, length.out = 10) %>% floor()
)

head(hyper_grid)
##   degree nprune
## 1      1      2
## 2      2      2
## 3      3      2
## 4      1     12
## 5      2     12
## 6      3     12


# Cross-validated model
# 我们可以使用caret使用10折CV执行网格搜索。 
# 提供最佳组合的模型包括二次相互作用效应，并保留56个项。

set.seed(123)  # for reproducibility
cv_mars <- train(
  x = subset(ames_train, select = -Sale_Price),
  y = ames_train$Sale_Price,
  method = "earth",
  metric = "RMSE",
  trControl = trainControl(method = "cv", number = 10),
  tuneGrid = hyper_grid
)


# View results
cv_mars$bestTune
##    nprune degree
## 16     56      2

cv_mars$results %>%
  filter(nprune == cv_mars$bestTune$nprune, degree == cv_mars$bestTune$degree)
##   degree nprune    RMSE  Rsquared      MAE   RMSESD RsquaredSD    MAESD
## 1      2     56 26817.1 0.8838914 16439.15 11683.73 0.09785945 1678.672

ggplot(cv_mars)
```

![Cross-validated RMSE for the 30 different hyperparameter combinations in our grid search. The optimal model retains 56 terms and includes up to 2$^{nd}$ degree interactions.](https://bradleyboehmke.github.io/HOML/06b-mars_files/figure-html/grid-search-1.png)

## earth package

Sources: earth package [学习MARS的更多灵活用法](http://www.milbo.org/doc/earth-notes.pdf)





