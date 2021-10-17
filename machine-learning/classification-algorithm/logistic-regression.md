# Logistic Regression

## Introduction

使用基于概率的线性模型预测定性响应变量，三种方法:

* **Logistic regression**
* **Linear discriminant analysis (线性判别分析)**
* **Multivariate adaptive regression spline (多元自适应回归样条)**

### Functions

|                                   |                                                           |
| --------------------------------- | --------------------------------------------------------- |
| Compare models                    | anova(fit.model1, fit.model2, test = "Chisq")             |
| Interpret coefficients Odds       | <p>coef(fit.reduced) <br>exp(coef(fit.reduced))</p>       |
| Predict using new datasets        | predict(fit.model, newdata = testdata, type = "response") |
| CIs using profiled log-likelihood | confint(fit.model)                                        |
| CIs using standard errors         | confint.default(fit.model)                                |
| VIF statistics                    | <p>library(car) <br>vif(fit.model)</p>                    |
| Change Referenz                   | relevel(factor(school2$RANK),ref=4)                       |
| Wald Test                         | <p>library(aod) <br>wald.test()</p>                       |
| Log likelihood ratio test         | logLik()                                                  |

### Model training

```
数据集包含699个患者的组织样本，保存在有11个变量的数据框中，如下所示。

 ID:样本编码
 V1:细胞浓度
 V2:细胞大小均匀度
 V3:细胞形状均匀度
 V4:边缘黏着度
 V5:单上皮细胞大小
 V6:裸细胞核(16个观测值缺失) 8  V7:平和染色质
 V8:正常核仁
 V9:有丝分裂状态
 class:肿瘤诊断结果，良性或恶性;这就是我们要预测的结果变量。
确认数据结构，将变量重命名为有意义的名称， 删除有缺失项的观测，然后即可开始对数据进行可视化探索。

library(MASS)
data(biopsy)
biopsy$ID = NULL
names(biopsy) = c("thick", "u.size", "u.shape", "adhsn", 
                  "s.size", "nucl", "chrom", "n.nuc", "mit", "class")
biopsy.v2 <- na.omit(biopsy)

## 为了满 足这个要求，可以创建一个变量y，用0表示良性，用1表示恶性
## 使用ifelse()函数为y赋 值，如下所示:
y <- ifelse(biopsy.v2$class == "malignant", 1, 0)

library(reshape2)
library(ggplot2)
## 将数据按值融合成一个总体特征，并按照class变量进行分组
biop.m <- melt(biopsy.v2, id.var = "class")
ggplot(data = biop.m, aes(x = class, y = value)) + 
  geom_boxplot() +
  facet_wrap(~ variable, ncol = 3)
  
## 加载corrplot包，检查特征之间的 相关性
library(corrplot)
bc <- cor(biopsy.v2[ ,1:9]) #create an object of the features
corrplot.mixed(bc)

## 从相关系数可以看出，我们是否会遇到共线性问题
## 作为逻辑斯蒂回归建模过程的一部分，VIF分析是必不可少的
## 在机器学习中，我们不应该过于关心对已使用的观测的预测有多好
## 应该把重点 放在那些建立算法时没有使用过的观测的预测上面
## 我们要使用训练数据建立并选择一个 对测试数据能做出最好预测的最优算法
## 有多种方式可以将数据恰当地划分成训练集和测试集:50/50、60/40、70/30、80/20
## 本例中，选择按照70/30的比例划分

set.seed(123) #random number generator
ind <- sample(2, nrow(biopsy.v2), replace = TRUE, prob = c(0.7, 0.3))
train <- biopsy.v2[ind==1, ] #the training data set
test <- biopsy.v2[ind==2, ] #the test data set
str(test) #confirm it worked
```

#### Model construction

```
## R中的glm()函数可 以拟合广义线性模型，这是一系列模型，其中包括逻辑斯蒂回归模型
full.fit <- glm(class ~ ., family = binomial, data = train)
summary(full.fit)
confint(full.fit)

## 计算优势比
exp(coef(full.fit))

## 同线性回归一样，在逻辑斯蒂回 归中也可以算出VIF统计量
library(car)
vif(full.fit)

## 建立一个向量，表示预测概率
train.probs <- predict(full.fit, type = "response")
```

### Model evaluation

混淆矩阵的作用：

１）用于观察模型在各个类别上的表现，可以计算模型对应各个类别的准确率，召回率；

２）通过混淆矩阵可以观察到类别直接哪些不容易区分，比如Ａ类别中有多少被分到了Ｂ类别，这样可以有针对性的设计特征等，使得类别更有区分性；

```
## 评价模型在训练集上执行的效果，然后再评价它在测试集上的拟合程度。
## 快速实现评价的方法是生成一个混淆矩阵
## 混淆矩阵可以由InformationValue包
## 0和1来表示结果。函数区别良性结果 和恶性结果使用的默认值是0.50
## 也就是说，当概率大于等于0.50时，就认为这个结果是恶性的
library(InformationValue)
trainY <- y[ind==1]
testY <- y[ind==2]
confusionMatrix(trainY, train.probs)

##            0      1
##     0     297     7
##     1      8     165

## 矩阵的行表示预测值，列表示实际值。对角线上的元素是预测正确的分类
## 右上角的值7表示误为恶性的预测数, 左下角的值8表示误为良性的预测数
## optimalCutoff(trainY, train.probs)

## 查看错误预测的比例, 训练集上只有3.16%的预测错误率, 看上去不错
misClassError(trainY, train.probs)

## 在测试集上建立混淆矩阵的方法和在训练集上一样
test.probs <- predict(full.fit, newdata = test, type = "response")
misClassError(testY, test.probs)

confusionMatrix(testY, test.probs)
```

### Cross-validation

交叉验证的目的是提高测试集上的预测正确率，以及尽可能避免过拟合。K折交叉验证的做 法是将数据集分成K个相等的等份，每个等份称为一个K子集(K-set)。算法每次留出一个子集， 使用其余K-1个子集拟合模型，然后用模型在留出的那个子集上做预测。将上面K次验证的结果 进行平均，可以使误差最小化，并且获得合适的特征选择。

留一交叉验证方法，这 里的K等于N。模拟表明，LOOCV可以获得近乎无偏的估计，但是会有很高的方差。所以，大多 数机器学习专家都建议将K的值定为5或10。bestglm包可以自动进行交叉验证，这个包依赖于在线性回归中使用过的leaps包

```
## Paste Formular
formula.name <- names(which(LR.best.fit.result$which[15,],arr.ind = TRUE))[-1]
formula <- as.formula(paste("diagnosis", paste(formula.name, collapse=" + "), sep=" ~ "))
formula

## BIC Bestsubset
LR.bestBIC.fit <- glm(formula, family = binomial, data = biopsy.zs.train)
```

```
## 加载程序包之后，需要将结果编码成0或1。如果结果仍为因子，程序将不起作用。
## 使用这个 程序包的另外一个要求是结果变量(或y)必须是最后一列，而且要删除所有没有用的列。
## 通过 以下代码新建一个数据框即可快速实现上述要求
library(leaps)
library(bestglm)

X <- train[, 1:9]
Xy <- data.frame(cbind(X, trainY))
## CVArgs是我们要使用的交叉验证参数。HTF方法就是K折交叉验证，数字K = 10指定了均分的份数
## 参数REP = 1告诉程序随机使用等份并且只迭代一次。像glm() 函数一样
## 需要指定参数family = binomial。如果指定参数family = gaussian
## 可以使用bestglm()函数进行线性回归
bestglm(Xy = Xy, IC = "CV", 
        CVArgs = list(Method = "HTF", K = 10, REP = 1), 
        family=binomial)
```

```
## CV(K = 10, REP = 1)
## BICq equivalent for q in (7.16797006619085e-05, 0.273173435514245)
## Best Model:
##               Estimate Std. Error   z value     Pr(>|z|)
## (Intercept) -7.8147191 0.90996494 -8.587934 8.854687e-18
## thick        0.6188466 0.14713075  4.206100 2.598159e-05
## u.size       0.6582015 0.15295415  4.303260 1.683031e-05
## nucl         0.5725902 0.09922549  5.770596 7.899178e-09
```

```
## 把这些特征放到glm()函数中，看看模型在测试集上表现如何。
## predict()函数不能用于bestglm生成的模型
reduce.fit <- glm(class ~ thick + u.size + nucl, family = binomial, data = train)

## 在测试集上比较预测值和实际值
test.cv.probs = predict(reduce.fit, newdata = test, type = "response")
misClassError(testY, test.cv.probs)
confusionMatrix(testY, test.cv.probs)
```

