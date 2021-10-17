---
description: Rule- and Instance-Based Regression Modeling
---

# Cubist Model

## Introduction

Cubist is a rule–based model that is an extension of Quinlan’s M5 model tree. A tree is grown where the terminal leaves contain linear regression models. These models are based on the predictors used in previous splits. Also, there are intermediate linear models at each step of the tree. A prediction is made using the linear regression model at the terminal node of the tree, but is “smoothed” by taking into account the prediction from the linear model in the previous node of the tree (which also occurs recursively up the tree). The tree is reduced to a set of rules, which initially are paths from the top of the tree to the bottom. Rules are eliminated via pruning and/or combined for simplification.

> Cubist是基于规则的模型，是Quinlan M5模型树的扩展。在末端叶子包含线性回归模型的地方生长一棵树。这些模型基于先前拆分中使用的预测变量。而且，在树的每个步骤中都有中间线性模型。使用树的末端节点处的线性回归模型进行预测，但通过考虑树的前一节点中的线性模型进行的预测（也可以在树上递归地进行）来“平滑”预测。将树简化为一组规则，这些规则最初是从树的顶部到底部的路径。通过修剪消除规则和/或将其组合以简化。

### Cubist package

| [cubist](https://rdrr.io/rforge/Cubist/man/cubist.html)                       | Fit a Cubist model                                         |
| ----------------------------------------------------------------------------- | ---------------------------------------------------------- |
| [cubistControl](https://rdrr.io/rforge/Cubist/man/cubistControl.html)         | Various parameters that control aspects of the Cubist fit. |
| [dotplot.cubist](https://rdrr.io/rforge/Cubist/man/dotplot.cubist.html)       | Visualization of Cubist Rules and Equations                |
| [exportCubistFiles](https://rdrr.io/rforge/Cubist/man/exportCubistFiles.html) | Export Cubist Information To the File System               |
| [predict.cubist](https://rdrr.io/rforge/Cubist/man/predict.cubist.html)       | Predict method for cubist fits                             |
| [summary.cubist](https://rdrr.io/rforge/Cubist/man/summary.cubist.html)       | Summarizing Cubist Fits                                    |

## Application

### Data Preparation

```
library("Cubist")
library("mlbench")

## Data Preparation

data(BostonHousing)
BostonHousing$chas <- as.numeric(BostonHousing$chas) - 1

set.seed(1)
inTrain <- sample(1:nrow(BostonHousing), floor(.8*nrow(BostonHousing)))

## Predictors
train_pred <- BostonHousing[ inTrain, -14]
test_pred  <- BostonHousing[-inTrain, -14]

## Responder variable
train_resp <- BostonHousing$medv[ inTrain]
test_resp  <- BostonHousing$medv[-inTrain]
```

### Fit Continious Outcome

The modelTree method for Cubist shows the usage of each variable in either the rule conditions or the (terminal) linear model. In actuality, many more linear models are used in prediction that are shown in the output. Because of this, the variable usage statistics shown at the end of the output of the summary function will probably be inconsistent with the rules also shown in the output. At each split of the tree, Cubist saves a linear model (after feature selection) that is allowed to have terms for each variable used in the current split or any split above it. Quinlan (1992) discusses a smoothing algorithm where each model prediction is a linear combination of the parent and child model along the tree. As such, the final prediction is a function of all the linear models from the initial node to the terminal node. The percentages shown in the Cubist output reflects all the models involved in prediction (as opposed to the terminal models shown in the output).

> Cubist的modelTree方法显示了规则条件或（最终）线性模型中每个变量的用法。实际上，在预测中使用了更多的线性模型，这些模型显示在输出中。因此，摘要功能输出末尾显示的变量使用情况统计信息可能与输出中也显示的规则不一致。在树的每个分割处，Cubist都会保存一个线性模型（在特征选择之后），该线性模型允许对当前分割处或其上方任何分割处使用的每个变量都具有术语。 Quinlan（1992）讨论了一种平滑算法，其中每个模型预测都是沿着树的父模型和子模型的线性组合。这样，最终预测是从初始节点到终端节点的所有线性模型的函​​数。立体派输出中显示的百分比反映了预测中涉及的所有模型（与输出中显示的终端模型相对）。

```
## Continious Outcome
## Building mOdel
model_tree <- cubist(x = train_pred, y = train_resp)
summary(model_tree)
```

```
## 
## Call:
## cubist.default(x = train_pred, y = train_resp)
## 
## 
## Cubist [Release 2.07 GPL Edition]  Thu Mar 18 12:33:43 2021
## ---------------------------------
## 
##     Target attribute `outcome'
## 
## Read 404 cases (14 attributes) from undefined.data
## 
## Model:
## 
##   Rule 1: [77 cases, mean 14.04, range 5 to 27.5, est err 2.14]
## 
##     if
##  nox > 0.668
##     then
##  outcome = -2.18 + 3.47 dis + 21.6 nox - 0.32 lstat + 0.0089 b
##            - 0.12 ptratio - 0.02 crim - 0.005 age
## 
##   Rule 2: [167 cases, mean 19.30, range 7 to 31, est err 2.15]
## 
##     if
##  nox <= 0.668
##  lstat > 9.53
##     then
##  outcome = 43.02 - 0.94 ptratio - 0.27 lstat - 0.84 dis - 0.034 age
##            + 0.0082 b - 0.1 indus + 0.4 rm
## 
##   Rule 3: [29 cases, mean 25.08, range 18.2 to 50, est err 2.64]
## 
##     if
##  rm <= 6.226
##  lstat <= 9.53
##     then
##  outcome = -18.07 + 3.91 crim - 1.67 lstat + 0.0834 b + 3.3 rm
## 
##   Rule 4: [143 cases, mean 29.49, range 16.5 to 50, est err 2.39]
## 
##     if
##  dis > 2.3999
##  lstat <= 9.53
##     then
##  outcome = -28.26 + 11.1 rm + 0.62 crim - 0.58 lstat - 0.017 tax
##            - 0.059 age + 11.3 nox - 0.58 dis - 0.52 ptratio
## 
##   Rule 5: [14 cases, mean 40.54, range 22 to 50, est err 5.28]
## 
##     if
##  rm > 6.226
##  dis <= 2.3999
##  lstat <= 9.53
##     then
##  outcome = -5.27 + 3.14 crim - 5.18 dis - 1.22 lstat + 9.6 rm
##            - 0.0141 tax - 0.031 age - 0.39 ptratio
## 
## 
## Evaluation on training data (404 cases):
## 
##     Average  |error|               2.13
##     Relative |error|               0.31
##     Correlation coefficient        0.96
## 
## 
##  Attribute usage:
##    Conds  Model
## 
##     82%   100%    lstat
##     57%    51%    nox
##     37%    93%    dis
##     10%    82%    rm
##            93%    age
##            93%    ptratio
##            63%    b
##            61%    crim
##            39%    indus
##            37%    tax
## 
## 
## Time: 0.0 secs
```

```
## Make the prediction on the test datasets
Cubist.probs <- predict(model_tree, test_pred)

## Test set RMSE
sqrt(mean((Cubist.probs - test_resp)^2))

## Test set R^2
cor(Cubist.probs, test_resp)^2
```

#### Variable Importance

the variable importance is a linear combination of the usage in the rule conditions and the model.

```
model_tree <- cubist(x = train_pred, y = train_resp)
library("caret")
varImp(model_tree)
```

#### Summary display

The tidyRules function in the tidyrules package returns rules in a tibble (an extension of dataframe) with one row per rule. The tibble provides these information about the rule: support, mean, min, max, error, LHS, RHS and committee.

```
library("tidyrules")
tr <- tidyRules(model_tree)
tr
```

| id\<int> | LHS\<chr>                                  |   |
| -------- | ------------------------------------------ | - |
| 1        | nox > 0.668                                |   |
| 2        | nox <= 0.668 & lstat > 9.53                |   |
| 3        | rm <= 6.226 & lstat <= 9.53                |   |
| 4        | dis > 2.3999 & lstat <= 9.53               |   |
| 5        | rm > 6.226 & dis <= 2.3999 & lstat <= 9.53 |   |

#### specific parts

These results can be used to look at specific parts of the data. For example, the 4th rule predictions are:

```
library("rlang")
library("dplyr")
char_to_expr <- function(x, index = 1, model = TRUE) {
  x <- x %>% dplyr::slice(index) 
  if (model) {
    x <- x %>% dplyr::pull(RHS) %>% rlang::parse_expr()
  } else {
    x <- x %>% dplyr::pull(LHS) %>% rlang::parse_expr()
  }
  x
}

rule_expr  <- char_to_expr(tr, 4, model = FALSE)
model_expr <- char_to_expr(tr, 4, model = TRUE)
```

### Ensembles By Committees

The Cubist model can also use a boosting–like scheme called committees where iterative model trees are created in sequence. The first tree follows the procedure described in the last section. Subsequent trees are created using adjusted versions to the training set outcome: if the model over–predicted a value, the response is adjusted downward for the next model (and so on). Unlike traditional boosting, stage weights for each committee are not used to average the predictions from each model tree; the final prediction is a simple average of the predictions from each model tree.

> 其中按顺序创建迭代模型树。 第一棵树遵循最后一部分中描述的过程。 使用对训练集结果的调整后的版本来创建后续树：如果模型预测值过高，则针对下一个模型向下调整响应（依此类推）。 与传统的提升不同，每个委员会的阶段权重不会用于平均每个模型树的预测； 最终预测是来自每个模型树的预测的简单平均值。

#### Nearest–neighbors Adjustmemt

Another innovation in Cubist using nearest–neighbors to adjust the predictions from the rule–based model. First, a model tree (with or without committees) is created. Once a sample is predicted by this model, Cubist can find it’s nearest neighbors and determine the average of these training set points.

> 使用最近邻来调整基于规则的模型中的预测。 首先，创建一个模型树（有或没有委员会）。 通过该模型预测样本后，Cubist可以找到其最近的邻居，并确定这些训练设定点的平均值。

```
## Ensembles By Committees 
set.seed(1)
com_model <- cubist(x = train_pred, y = train_resp, committees = 5)
summary(com_model)
```

```
## Nearest–neighbors Adjustmemt
inst_pred <- predict(com_model, test_pred, neighbors = 5)
## RMSE
sqrt(mean((inst_pred - test_resp)^2))
## R^2
cor(inst_pred, test_resp)^2
```

### Optimize parameters

To tune the model over different values of neighbors and committees, the train function in the \`caret package can be used to optimize these parameters.

```
## Optimize parameters
library("caret")
grid <- expand.grid(committees = c(1, 10, 50, 100),
                    neighbors = c(0, 1, 5, 9))
set.seed(1)
boston_tuned <- train(
  x = train_pred,
  y = train_resp,
  method = "cubist",
  tuneGrid = grid,
  trControl = trainControl(method = "cv")
)
boston_tuned
```

```
## Cubist 
## 
## 404 samples
##  13 predictor
## 
## No pre-processing
## Resampling: Cross-Validated (10 fold) 
## Summary of sample sizes: 364, 364, 364, 363, 364, 363, ... 
## Resampling results across tuning parameters:
## 
##   committees  neighbors  RMSE      Rsquared   MAE     
##     1         0          3.486995  0.8696332  2.410782
##     1         1          3.476423  0.8766470  2.353796
##     1         5          3.226631  0.8905880  2.172835
##     1         9          3.273334  0.8863545  2.200032
##    10         0          2.984583  0.9030420  2.107548
##    10         1          2.881682  0.9119488  2.071019
##    10         5          2.726023  0.9194164  1.907319
##    10         9          2.758086  0.9165772  1.910888
##    50         0          3.010918  0.9003286  2.114696
##    50         1          2.902492  0.9105268  2.061518
##    50         5          2.707697  0.9195486  1.881237
##    50         9          2.742850  0.9165186  1.900488
##   100         0          2.971313  0.9033629  2.093351
##   100         1          2.871736  0.9128822  2.043151
##   100         5          2.671169  0.9221820  1.862776
##   100         9          2.709607  0.9189411  1.879577
## 
## RMSE was used to select the optimal model using the smallest value.
## The final values used for the model were committees = 100 and neighbors = 5.
```

```
## The profiles of the tuning parameters 
ggplot(boston_tuned)
```

![](https://firebasestorage.googleapis.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-MRt1rIudcHYDxTcux_Q%2Fuploads%2FCQbUM3XhDboNmalKep83%2Ffile.png?alt=media)

### Logistic CV

```
library("MASS")
data(biopsy)
biopsy$ID = NULL
names(biopsy) = c("thick", "u.size", "u.shape", "adhsn", 
                  "s.size", "nucl", "chrom", "n.nuc", "mit", "class")
biopsy.v2 <- na.omit(biopsy)

## 用0表示良性，用1表示恶性
y <- ifelse(biopsy.v2$class == "malignant", 1, 0)

set.seed(123) #random number generator
ind <- sample(2, nrow(biopsy.v2), replace = TRUE, prob = c(0.7, 0.3))
train <- biopsy.v2[ind==1, ] #the training data set
test <- biopsy.v2[ind==2, ] #the test data set

trainY <- y[ind==1]
testY <- y[ind==2]



set.seed(123)
ctrl=(trainControl(method="repeatedcv", repeats=5))
c<-c(100)
n<-c(3,4)

cubit.fit<-train(as.matrix(train[-10]),
                 trainY, 
                 method="cubist",
                 preProcess = c("center", "scale"),
                 tuneGrid = expand.grid(committees=c,neighbors=n),
                 trControl = ctrl)
                
summary(cubit.fit)
```

```
dotPlot(varImp(cubit.fit), main="Cubist Predictor importance")
```

![](https://firebasestorage.googleapis.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-MRt1rIudcHYDxTcux_Q%2Fuploads%2Fr353vk4wKhp6jmzeC2lI%2Ffile.png?alt=media)

###  
