# Random Forest



## Random Forest 

### Bootstrap (Bagging)

随机采样(bootstrap)就是从我们的训练集里面采集固定个数的样本，但是每采集一个样本后，都将样本放回。也就是说，之前采集到的样本在放回后有可能继续被采集到。Bagging算法，一般会随机采集和训练集样本数m一样个数的样本。

* Gradient Boosting Decision Tree Algorithm  GBDT的子采样是无放回采样
* Bagging的子采样是放回采样。

对于一个样本，它在某一次含m个样本的训练集的随机采样中，每次被采集到的概率是 $$\frac{1}{m}$$ 。不被采集到的概率为$$1-\frac{1}{m}$$。如果m次采样都没有被采集中的概率是 $$(1-\frac{1}{m})^m$$ . 当 $$m \to \infty$$ $$(1-\frac{1}{m})^m \to \frac{1}{e} \simeq 0.368$$ 。

也就是说，在bagging的每轮随机采样中，训练集中大约有36.8%的数据没有被采样集采集中。对于这部分大约36.8%的没有被采样到的数据，我们常常称之为**袋外数据(Out Of Bag, 简称OOB)**。这些数据没有参与训练集模型的拟合，因此可以用来检测模型的泛化能力。



```
# Helper packages
library(dplyr)       # for data wrangling
library(ggplot2)     # for awesome plotting
library(doParallel)  # for parallel backend to foreach
library(foreach)     # for parallel processing with for loops

# Modeling packages
library(caret)       # for general model fitting
library(rpart)       # for fitting decision trees
library(ipred)       # for fitting bagged decision trees

# make bootstrapping reproducible
set.seed(123)

# train bagged model
ames_bag1 <- bagging(
  formula = Sale_Price ~ .,
  data = ames_train,
  nbagg = 100,  
  coob = TRUE,
  control = rpart.control(minsplit = 2, cp = 0)
)

ames_bag1
## Out-of-bag estimate of root mean squared error:  25528.78
```

```
 ## Apply bagging within caret and use 10-fold CV
 
 ames_bag2 <- train(
  Sale_Price ~ .,
  data = ames_train,
  method = "treebag",
  trControl = trainControl(method = "cv", number = 10),
  nbagg = 200,  
  control = rpart.control(minsplit = 2, cp = 0)
)
ames_bag2
## Bagged CART 
## 
## 2054 samples
##   80 predictor
## 
## No pre-processing
## Resampling: Cross-Validated (10 fold) 
## Summary of sample sizes: 1849, 1848, 1848, 1849, 1849, 1847, ... 
## Resampling results:
## 
##   RMSE      Rsquared   MAE     
##   26957.06  0.8900689  16713.14
```

### bagging算法流程

输入为样本集 $$D=\left\{\left(x, y_{1}\right),\left(x_{2}, y_{2}\right), \ldots\left(x_{m}, y_{m}\right)\right\},$$ 弱学习器算法, 弱分类器迭代次数 。 输出为最终的强分类器 $$f(x)$$

1. 对于t $$=1,2 \ldots, T:$$
   * a 对训练集进行第t次随机采样, 共采集m次, 得到包含m个样本的采样集 $$D_{t}$$
   * b 用采样集 $$D_{t}$$ 训练第t个弱学习器 $$G_{t}(x)$$
2. 如果是分类算法预测, 则T个弱学习器投出最多票数的类别或者类别之一为最终类别。如果是回归算法, T个弱学习器得到的回归结果进行算术平均得 到的值为最终的模型输出。

### Random Forest Algorithm

随机森林是Bagging算法的进化版, 首先，RF使用了CART决策树作为弱学习器. 第二，在使用决策树的基础上，RF对决策树的建立做了改进，对于普通的决策树，我们会在节点上所有的n个样本特征中选择一个最优的特征来做决策树的左右子树划分，但是RF通过随机选择节点上的一部分样本特征，这个数字小于n，假设为 $$n_{sub}$$ 个样本特征中，选择一个最优的特征来做决策树的左右子树划分。这样进一步增强了模型的泛化能力。

* $$n_{sub}$$越小，则模型约健壮，当然此时对于训练集的拟合程度会变差。也就是说模型的方差会减小，但是偏倚会增大。
* 在实际案例中，一般会通过交叉验证调参获取一个合适的$$n_{sub}$$

随机森林技术在模型构建过程中使用两种奇妙的方法

1. 第一个方法称为自助聚集，或称装袋。在装袋法中，使用数据集的一次随机抽样建立一个独立树，抽样的数量大概为全部观测的2/3(请记住，剩下的1/3被称为袋外数据，out-of-bag)。这个过程重复几十次或上百次，最后取平均结果。其中每个树都任其生长，不进行任何基于误差测量的剪枝，这意味着每个独立树的方差都很大。但是，通过对结果的平均化处理可以降低方差，同时又不增加偏差。
2. 另一个在随机森林中使用的方法是，对数据进行随机抽样(装袋)的同时，独立树每次分裂时对输入特征也进行随机抽样。在randomForest包中，我们使用随机抽样数的默认值来对预测特征进行抽样。对于分类问题，默认值为所有预测特征数量的平方根;对于回归问题，默认值为所有预测 特征数量除以3。

通过每次分裂时对特征的随机抽样以及由此形成的一套方法，你可以减轻高度相关的预测特 征的影响，这种预测特征在由装袋法生成的独立树中往往起主要作用。这种方法还使你不必思索 如何减少装袋导致的高方差。独立树彼此之间的相关性减少之后，对结果的平均化可以使泛化效 果更好，对于异常值的影响也更加不敏感，比仅进行装袋的效果要好。

#### RF算法

输入为样本集 $$D=\left\{\left(x, y_{1}\right),\left(x_{2}, y_{2}\right), \ldots\left(x_{m}, y_{m}\right)\right\},$$ 弱分类器迭代次数T。

输出为最终的强分类器 $$f(x)$$

1. 对于t $$=1,2 \ldots, T:$$
   * a)对训练集进行第t次随机采样，共采集m次, 得到包含m个样本的采样集 $$D_{t}$$
   *   b)用采样集 $$D_{t}$$ 训练第t个决策树模型 $$G_{t}(x),$$ 在训练决策树模型的节点的时候, 在节点上所有的样本特征中选择一部分样本特征, 在这些随机选

       择的部分样本特征中选择一个最优的特征来做决策树的左右子树划分
2. 如果是分类算法预测, 则T个弱学习器投出最多票数的类别或者类别之一为最终类别。如果是回归算法, T个弱学习器得到的回归结果进行算术平均得 到的值为最终的模型输出。

### Random forest promotion

RF在实际应用中的良好特性，基于RF，有很多变种算法，应用也很广泛，不光可以用于分类回归，还可以用于特征转换，异常点检测等。

#### Extra trees

extra trees是RF的一个变种, 原理几乎和RF一模一样，仅有区别有：

　　　　1） 对于每个决策树的训练集，RF采用的是随机采样bootstrap来选择采样集作为每个决策树的训练集，而extra trees一般不采用随机采样，即每个决策树采用原始训练集。

　　　　2） 在选定了划分特征后，RF的决策树会基于基尼系数，均方差之类的原则，选择一个最优的特征值划分点，这和传统的决策树相同。但是extra trees比较的激进，他会随机的选择一个特征值来划分决策树。

从第二点可以看出，由于随机选择了特征值的划分点位，而不是最优点位，这样会导致生成的决策树的规模一般会大于RF所生成的决策树。也就是说，模型的方差相对于RF进一步减少，但是偏倚相对于RF进一步增大。在某些时候，extra trees的泛化能力比RF更好。

#### Totally Random Trees Embedding

Totally Random Trees Embedding(以下简称 TRTE)是一种非监督学习的数据转化方法。它将低维的数据集映射到高维，从而让映射到高维的数据更好的运用于分类回归模型。我们知道，在支持向量机中运用了核方法来将低维的数据集映射到高维，此处TRTE提供了另外一种方法。

TRTE在数据转化的过程也使用了类似于RF的方法，建立T个决策树来拟合数据。当决策树建立完毕以后，数据集里的每个数据在T个决策树中叶子节点的位置也定下来了。比如我们有3颗决策树，每个决策树有5个叶子节点，某个数据特征xx划分到第一个决策树的第2个叶子节点，第二个决策树的第3个叶子节点，第三个决策树的第5个叶子节点。则x映射后的特征编码为(0,1,0,0,0,     0,0,1,0,0,     0,0,0,0,1), 有15维的高维特征。这里特征维度之间加上空格是为了强调三颗决策树各自的子编码。

映射到高维特征后，可以继续使用监督学习的各种分类回归算法了。

#### Isolation Forest

Isolation Forest（以下简称IForest）是一种异常点检测的方法。它也使用了类似于RF的方法来检测异常点。对于在T个决策树的样本集，IForest也会对训练集进行随机采样,但是采样个数不需要和RF一样，对于RF，需要采样到采样集样本个数等于训练集个数。但是IForest不需要采样这么多，一般来说，采样个数要远远小于训练集个数？为什么呢？因为我们的目的是异常点检测，只需要部分的样本我们一般就可以将异常点区别出来了。

对于每一个决策树的建立， IForest采用随机选择一个划分特征，对划分特征随机选择一个划分阈值。这点也和RF不同。另外，IForest一般会选择一个比较小的最大决策树深度max_depth,原因同样本采集，用少量的异常点检测一般不需要这么大规模的决策树。

对于异常点的判断, 则是将测试样本点 $$x$$ 拟合到T颗决策树。计算在每颗决策树上该样本的早子节点的深度 $$h_{t}(x)$$ 。, 从而可以计算出平均高度h $$(\mathrm{x})$$ 。此 时我们用下面的公式计算样本点x的异常概率：

$$
s(x, m)=2^{-\frac{h(x)}{c(m)}}
$$

其中， m为样本个数。 $$c(m)$$ 的表达式为： $$c(m)=2 \ln (m-1)+\xi-2 \frac{m-1}{m},$$ $$\xi$$为欧拉常数 $$s(x, m)$$ 的取值范围是 $$[0,1],$$ 取值越接近于1，则是异常点的概率也越大。

### Package ‘randomForest’

Source: [https://cran.r-project.org/web/packages/randomForest/randomForest.pdf](https://cran.r-project.org/web/packages/randomForest/randomForest.pdf)









## Modellierung

```
## load package
library(party)
library(rpart)         # classification and regression trees
library(partykit)      # treeplots
library(MASS)          # breast and pima indian data
library(randomForest)  # random forests
library(xgboost)       # gradient boosting 
library(caret)         # tune hyper-parameters

## 准备数据：前列腺癌数据集。使用ifelse()函数将gleason评分编码为指标变量，
   划分训练数据集和测试数据集，
   训练数据集为pros.train，测试数据集为pros.test
## Load dataset
prostate <- read.delim("~/Library/Mobile Documents/com~apple~CloudDocs/02. Programm/01 R_lernen/00 Data/prostate.txt", header=T)
prostate$gleason <- ifelse(prostate$gleason == 6, 0, 1)
pros.train <- subset(prostate, train == TRUE)[, 2:10]
pros.test = subset(prostate, train == FALSE)[, 2:10]
```

### Regression tree

```
## 在训练数据集上建立回归树，使用party包中的rpart()函数
set.seed(123)
tree.pros <- rpart(lpsa ~ ., data = pros.train)
tree.pros$cptable     ## 检查每次分裂的误差，以决定最优的树分裂次数

##           CP nsplit rel error    xerror       xstd
## 1 0.35852251      0 1.0000000 1.0195606 0.17963802
## 2 0.12295687      1 0.6414775 0.8742500 0.12878275
## 3 0.11639953      2 0.5185206 0.7949473 0.10419946
## 4 0.05350873      3 0.4021211 0.7904898 0.09821670
## 5 0.01032838      4 0.3486124 0.7044000 0.09115510
## 6 0.01000000      5 0.3382840 0.7321671 0.09381916

    ## CP的第一列是成本复杂性参数
    ## 第二列nsplit是树 分裂的次数，
    ## rel error列表示相对误差，即某次分裂的RSS除以不分裂的RSS(RSS(k)/RSS(0))
    ## xerror和xstd都是基于10折交叉验证的
    ## xerror是平均误差，xstd是交叉验证过程的标准差
    ## 5次分裂在整个数据集上产生的误差最小，但使用交叉验证时，4次分裂产生的误差略微更小
    ## 可以使用plotcp()函数查看统计图,使用误差条表示树的规模和相对误差之间的关系，
       误差条和树规模是对应的
plotcp(tree.pros)

## 树的xerror可以通过剪枝达到最 小化。
## 剪枝的方法是先建立一个cp对象，将这个对象和表中第5行相关联，
   然后使用prune()函数完成剩下的工作
cp <- min(tree.pros$cptable[5, ])
prune.tree.pros <- prune(tree.pros, cp = cp)

## 可以用统计图比较完整树和剪枝树。
## 由partykit包生成的树图明显优于party包生成的
   显示了树的分裂、节点、每节点观测数，以及预测结果的箱线图
plot(as.party(tree.pros))
plot(as.party(prune.tree.pros)) 
```

![](https://firebasestorage.googleapis.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-MRt1rIudcHYDxTcux_Q%2Fuploads%2FJGGgKgfbvZAmd9QZrwUH%2Ffile.png?alt=media)

```
## Predict
## 剪枝树在测试集上表现如何。在测试数据上使用predict()函数进行预测
## 建立一个对象保存这些预测值。然后用预测值减去实际值，得到误差，最后算出误差平方的平均值
party.pros.test <- predict(prune.tree.pros, 
                           newdata = pros.test)
rpart.resid <- party.pros.test - pros.test$lpsa    ## calculate residual
mean(rpart.resid^2)
```

### Classification tree

```
## CART breast cancer 乳腺癌数据
## 删除患者ID，对特征进行重新命名，删除一些缺失值，然后建立训练数据集和测试数据集
data(biopsy)
biopsy <- biopsy[, -1]
names(biopsy) <- c("thick", "u.size", "u.shape", "adhsn", "s.size", "nucl", "chrom", "n.nuc", "mit", "class")
biopsy.v2 <- na.omit(biopsy)
set.seed(123)                       # random number generator
ind <- sample(2, nrow(biopsy.v2), replace = TRUE, prob = c(0.7, 0.3))
biop.train <- biopsy.v2[ind == 1, ] # the training data set
biop.test <- biopsy.v2[ind == 2, ]  # the test data set
str(biop.test)                      # 建立分类树之前，要确保结果变量是一个因子

## 生成树，然后检查输出中的表格，找到最优分裂次数
set.seed(123)
tree.biop <- rpart(class ~ ., data = biop.train)
tree.biop$cptable

##           CP nsplit rel error    xerror       xstd
## 1 0.79651163      0 1.0000000 1.0000000 0.06086254
## 2 0.07558140      1 0.2034884 0.2616279 0.03710371
## 3 0.01162791      2 0.1279070 0.1511628 0.02882093
## 4 0.01000000      3 0.1162791 0.1511628 0.02882093

## 交叉验证误差仅在两次分裂后就达到了最小值(第3行)。现在可以对树进行剪枝，再在图中绘制剪枝树
cp <- min(tree.biop$cptable[3, ])
prune.tree.biop = prune(tree.biop, cp <- cp)
## plot(as.party(tree.biop))
plot(as.party(prune.tree.biop))
```

![](https://firebasestorage.googleapis.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-MRt1rIudcHYDxTcux_Q%2Fuploads%2FYyikqoydg1jent1H4lyc%2Ffile.png?alt=media)

```
## 在测试集上的表现
rparty.test <- predict(prune.tree.biop, newdata = biop.test,
                       type = "class")
table(rparty.test, biop.test$class)

##            
## rparty.test benign malignant
##   benign       136         3
##   malignant      6        64

## 只有两个分支的基本树模型给出了差不多96%的正确率
(136+64)/209
## 0.9569378
```

### Random Forest for regression

建立一个随机森林对象的通用语法是使用 randomForest()函数，指定模型公式和数据集这两个基本参数。回想一下每次树迭代默认的变 量抽样数，对于回归问题，是p/3;对于分类问题，是p的平方根，p为数据集中预测变量的个数。 对于大规模数据集，就p而言，你可以调整mtry参数，它可以确定每次迭代的变量抽样数值。如 果p小于10，可以省略上面的调整过程。想在多特征数据集中优化mtry参数时，可以使用caret包， 或使用randomForest包中的tuneRF()函数。

```
set.seed(123)
rf.pros <- randomForest(lpsa ~ ., data = pros.train)
rf.pros        ## 生成了500个不同的树(默认设置)，并且在每次树分裂时随机抽出两个变量。

##  randomForest(formula = lpsa ~ ., data = pros.train) 
##                Type of random forest: regression
##                      Number of trees: 500
## No. of variables tried at each split: 2
## 
##           Mean of squared residuals: 0.6936697
##                     % Var explained: 51.73

## 结果的MSE为0.68，差不多53%的方差得到了解释
## 改善。过多的树会导致过拟合,“多大的数量是‘过多’”依赖于数据规模。
## 第一是做出rf.pros的统计图，另一件是求出最小的MSE

## 图表示MSE与模型中树的数量之间的关系。可以看出，树的数量增加时，
   一开始MSE会有显著改善，当森林中大约建立了100棵树之后，改善几乎停滞
   ## plot(rf.pros)
   ## 具体的最优树数量
   ## which.min(rf.pros$mse)
   
## 指定ntree =
set.seed(123)
rf.pros.2 <- randomForest(lpsa ~ ., data = pros.train, 
                           ntree = which.min(rf.pros$mse))
rf.pros.2

## Call:
##  randomForest(formula = lpsa ~ ., data = pros.train, ntree = which.min(rf.pros$mse)) 
##                Type of random forest: regression
##                      Number of trees: 80
## No. of variables tried at each split: 2
## 
##           Mean of squared residuals: 0.6566502
##                     % Var explained: 54.31


## 变量重要性统计图及相应的列表
## Y轴是按重要性降序排列的变量列表，X轴是MSE改善百分比
## 在分类问题中，X轴应 该是基尼指数的改善
varImpPlot(rf.pros.2, scale = TRUE,
           main = "Variable Importance Plot - PSA Score")
```

![](https://firebasestorage.googleapis.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-MRt1rIudcHYDxTcux_Q%2Fuploads%2Fo5MF5uOjwo6w4W3k2n9j%2Ffile.png?alt=media)

```
## 查看具体数据，可以 使用importance()函数
importance(rf.pros.2)    

##         IncNodePurity
## lcavol      25.011557
## lweight     15.822110
## age          7.167320
## lbph         5.471032
## svi          8.497838
## lcp          8.113947
## gleason      4.990213
## pgg45        6.663911

## 看看模型在测试数据上的表现:
rf.pros.test <- predict(rf.pros.2, newdata = pros.test)
## plot(rf.pros.test, pros.test$lpsa)
rf.resid <- rf.pros.test - pros.test$lpsa 
## calculate residual
mean(rf.resid^2)
## [1] 0.5512549
```

### Random Forest for classification

```
## 皮玛印第安人糖尿病模型:数据准备
data(Pima.tr)
data(Pima.te)
pima <- rbind(Pima.tr, Pima.te)
set.seed(502)
ind <- sample(2, nrow(pima), replace = TRUE, prob = c(0.7, 0.3))
pima.train <- pima[ind == 1, ]
pima.test <- pima[ind == 2, ]

## 建立模型
set.seed(321)
rf.pima <- randomForest(type ~ ., data = pima.train)
rf.pima

## Call:
##  randomForest(formula = type ~ ., data = pima.train) 
##                Type of random forest: classification
##                      Number of trees: 500
## No. of variables tried at each split: 2
## 
##         OOB estimate of  error rate: 20.26%
## Confusion matrix:
##      No Yes class.error
## No  235  27   0.1030534
## Yes  51  72   0.4146341

# plot(rf.pima)
## 对树的数目 进行优化
which.min(rf.pima$err.rate[,1])

set.seed(321)
rf.pima.2 <- randomForest(type ~ ., data = pima.train, ntree = which.min(rf.pima$err.rate[,1]))
rf.pima.2         ## OOB误差有些许改善

## Call:
##  randomForest(formula = type ~ ., data = pima.train, ntree = which.min(rf.pima$err.rate[,      1])) 
##                Type of random forest: classification
##                      Number of trees: 88
## No. of variables tried at each split: 2
## 
##         OOB estimate of  error rate: 19.74%
## Confusion matrix:
##      No Yes class.error
## No  236  26  0.09923664
## Yes  50  73  0.40650407


rf.pima.test <- predict(rf.pima.2, 
                        newdata = pima.test, 
                        type = "response")
table(rf.pima.test, pima.test$type)
##             
## rf.pima.test No Yes
##          No  74  17
##          Yes 19  37

## (75+33)/147
## [1] 0.7346939
## varImpPlot(rf.pima.2)
```

### 极限梯度提升

xgboost package

* nrounds:最大迭代次数(最终模型中树的数量)。
* colsample_bytree:建立树时随机抽取的特征数量，用一个比率表示，默认值为1(使用100%的特征)。 
* min_child_weight:对树进行提升时使用的最小权重，默认为1。
* eta:学习率，每棵树在最终解中的贡献，默认为0.3。
* gamma:在树中新增一个叶子分区时所需的最小减损。
* subsample:子样本数据占整个观测的比例，默认值为1(100%)。 
* max_depth:单个树的最大深度。

使用expand.grid()函数可以建立实验网格，以运行caret包的训练过程。 对于前面列出的参数，如果没有设定具体值，那么即使有默认值，运行函数时也 会收到出错信息。下面的参数取值是基于以前的一些训练迭代而设定的。可以根据实验参数调整过程。

```
## 建立一个具有24个模型的网格，caret包会运行这些模型，以确定最好的调优参数。
grid = expand.grid(
  nrounds = c(75, 100),
  colsample_bytree = 1,
  min_child_weight = 1,
  eta = c(0.01, 0.1, 0.3), #0.3 is default,
  gamma = c(0.5, 0.25),
  subsample = 0.5,
  max_depth = c(2, 3)
)
head(grid)

## 使用car包的train()函数之前，创建一个名为cntrl的对象，来设定trainControl的参数。
## 这个对象会保存要使用的方法，以训练调优参数。我们使用5折交叉验证
## 在trControl中设定了verboseIter为TURE，所以可以看到每折交叉验证中的每次训练迭代。

cntrl = trainControl(
  method = "cv",
  number = 5,
  verboseIter = TRUE,
  returnData = FALSE,
  returnResamp = "final"                                                        
)

## 设定好所需参数即可:训练数据集、 标号、训练控制对象和实验网格。设定随机数种子
set.seed(1)
train.xgb = train(
  x = pima.train[, 1:7],
  y = ,pima.train[, 8],
  trControl = cntrl,
  tuneGrid = grid,
  method = "xgbTree"
)

## Aggregating results
## Selecting tuning parameters
## Fitting nrounds = 100, max_depth = 2, eta = 0.01, gamma = 0.5, 
   colsample_bytree = 1, min_child_weight = 1, 
   subsample = 0.5 on full training set
 
## 得到最优的参数，以及每种参数设置的结果
train.xgb

## eXtreme Gradient Boosting 
## 
## No pre-processing
## Resampling: Cross-Validated (5 fold) 
## Summary of sample sizes: 308, 309, 308, 307, 308 
## Resampling results across tuning parameters:
## 
##   eta   max_depth  gamma  nrounds  Accuracy   Kappa    
##   0.01  2          0.25    75      0.7865932  0.4700801
##   0.01  2          0.25   100      0.7971195  0.5003081
##   0.01  2          0.50    75      0.7971528  0.4945394
##   ...
##   0.30  3          0.25   100      0.7637547  0.4534616
##   0.30  3          0.50    75      0.7583574  0.4403953
##   0.30  3          0.50   100      0.7427721  0.3975159


## 创建一个参数列表，供Xgboost包的训练函数xgb.train()使用
## 将数据框转换为一个输入特征矩阵，以及一个带标号的 数值型结果列表(其中的值是0和1)
## 将特征矩阵和标号列表组合成符合要求的输入，即一个xgb.Dmatrix对象
param <- list(  objective           = "binary:logistic", 
                booster             = "gbtree",
                eval_metric         = "error",
                eta                 = 0.1, 
                max_depth           = 2, 
                subsample           = 0.5,
                colsample_bytree    = 1,
                gamma               = 0.5
)

x <- as.matrix(pima.train[, 1:7])
y <- ifelse(pima.train$type == "Yes", 1, 0)
train.mat <- xgb.DMatrix(data = x, 
                         label = y)

## 创建模型
set.seed(1)
xgb.fit <- xgb.train(params = param, data = train.mat, nrounds = 75)
xgb.fit


## 查看模型效果之前，先检查变量重要性，并绘制统计图。
## gain是这个特征对其所在分支的正确率做出的改善
## cover是与这个特征相关的全体观测的相对数量
## frequency是这个特征在所有树中出现的次数百分比
impMatrix <- xgb.importance(feature_names = dimnames(x)[[2]], model = xgb.fit)
impMatrix 
xgb.plot.importance(impMatrix, main = "Gain by Feature")


## 与训练集一样，测试集数据也要转换为矩阵
library(InformationValue)
pred <- predict(xgb.fit, x)
optimalCutoff(y, pred)      
## 找出使误差最小化的最优概率阈
## [1] 0.4416743

pima.testMat <- as.matrix(pima.test[, 1:7])
xgb.pima.test <- predict(xgb.fit, pima.testMat)
y.test <- ifelse(pima.test$type == "Yes", 1, 0)
optimalCutoff(y.test, xgb.pima.test)
## [1] 0.4837992

confusionMatrix(y.test, xgb.pima.test, threshold = 0.39)
1 - misClassError(y.test, xgb.pima.test, threshold = 0.39)    ## 模型误差大概是25%
## [1] 0.7551

## ROC曲线
plotROC(y.test, xgb.pima.test)
```

![](https://firebasestorage.googleapis.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-MRt1rIudcHYDxTcux_Q%2Fuploads%2FZkYVzwQIkTjh2ymFQNv1%2Ffile.png?alt=media)
