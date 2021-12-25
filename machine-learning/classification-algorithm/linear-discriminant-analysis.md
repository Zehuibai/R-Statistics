---
description: 线性判别分析
---

# Linear Discriminant Analysis (LDA)

使用基于概率的线性模型预测定性响应变量，三种方法:

* **Logistic regression**
* **Linear discriminant analysis (线性判别分析)**
* **Multivariate adaptive regression spline (多元自适应回归样条)**

## Discriminant analysis

判别分析又称**费舍尔判别分析**，也是一项常用的分类技术. 因为，**分类结果很确定时**，逻辑斯蒂回归的估计结果可能是不稳定的， 即**置信区间很宽**，不同样本之间的估计值会有很大变化. 判别分析 不会受到这个问题的困扰，泛化能力更强。 反之，如果特征 和结果变量之间具有错综复杂的关系，判别分析在分类任务上的表现就会非常差。

判别分析使用**贝叶斯定理确定每个观测属于某个类别的概率**。 如果你有两个类别，比如良性和恶性， 判别分析会计算观测分别属于两个类别的概率，然后选择高概率的类别作为正确的类别。 贝叶斯定理定义了在X已经发生的条件下Y发生的概率——等于Y和X同时发生的概率除以X 发生的概率

$$P(X|Y)=P(X+Y)/P(X)$$

同样地，分类原则认为如果X和Y的联合分布已知， 那么给定X后，决定观测属于哪个类别的最佳决策是选择那个有更大概率(后验概率)的类别

获得后验概率的过程如下所示。&#x20;

* (1) 收集已知类别的数据。&#x20;
* (2) 计算先验概率——代表属于某个类别的样本的比例。&#x20;
* (3) 按类别计算每个特征的均值。&#x20;
* (4) 计算每个特征的方差-协方差矩阵。在线性判别分析中，这会是一个所有类别的混合矩阵， 给出线性分类器;在二次判别分析中，会对每个分类建立一个方差协方差矩阵。&#x20;
* (5) 估计每个分类的正态分布(高斯密度)。&#x20;
* (6) 计算discriminant函数，作为一个新对象的分类原则。&#x20;
* (7) 根据discriminant函数，将观测分配到某个分类。

第k个分类的先验概率: $$\pi_k=$$分类k中的样本数/总体样本数,  一个观测属于第k个分类的概率密度函数。假设概率服从正态分布(高斯分布)。如果有多个特征，假设概率服从多元高斯分布$$f_k(X)=P(X=x|Y=k)$$

给定一个观测的特征值时，这个观测属于第k个分类的后验概率:

$$
P_x(X)=-\frac{\pi_kf_k(X)}{\sum_{l=1}^k\pi_lf_l(X)}
$$

判别分析会生成k-1个决策边界，也就是说，如果有三个类别(k = 3)，那么就会有两个决策边界。 当k=2时，生成1个决策边界： z.B，当$$\pi_1=\pi_2$$时，if $$2x(\mu_1-\mu_2)\gt\mu_1^2-\mu_2^2$$, 观测值被分配到第3个分类中，否则就被分配到第二个分类

### Idee

LDA是一种监督学习的降维技术，也就是说它的数据集的每个样本是有类别输出的。这点和PCA不同。PCA是不考虑样本类别输出的无监督降维技术。DA的思想可以用一句话概括，就是“**投影后类内方差最小，类间方差最大**”。------- 将数据在低维度上进行投影，投影后希望每一种类别数据的投影点尽可能的接近，而不同类别的数据的类别中心之间的距离尽可能的大。

### 瑞利商（Rayleigh quotient）

瑞利商是指这样的函数 $$R(A,x)$$&#x20;

$$
R(A,x) = \frac{x^HAx}{x^Hx}
$$

其中 $$x$$ 为非零向量, 而 $$A$$ 为 $$n \times n$$ 的Hermitan矩阵。所谓的Hermitan矩阵就是满足共轩转置矩阵和自己相等的矩阵, 即 $$A^{H}=A_{\circ}$$ 如果我们的矩阵A 是实矩阵, 则满足 $$A^{T}=A$$ 的矩阵即为Hermitan矩阵。

瑞利商 $$R(A, x)$$ 有一个非常重要的性质, 即它的最大值等于矩阵 $$A$$ 最大的特征值, 而最小值等于矩阵 $$A$$ 的最小的特征值, 也就是满足

$$
\lambda_{\min } \leq \frac{x^{H} A x}{x^{H} x} \leq \lambda_{\max }
$$

当向量 $$x$$ 是标准正交基时, 即满足 $$x^{H} x=1$$ 时, 瑞利商退化为: $$R(A, x)=x^{H} A x$$

### 广义瑞利商（genralized Rayleigh quotient）&#x20;

广义瑞利商是指这样的函数 $$R(A,B,x)$$&#x20;

$$
R(A,x) = \frac{x^HAx}{x^HBx}
$$

通过将其通过标准化就可以转化为瑞利商的格式。令 $$x=B^{-1/2}x'$$, 则分母转化为：

$$
x^HBx = x'^H(B^{-1/2})^HBB^{-1/2}x' = x'^HB^{-1/2}BB^{-1/2}x' = x'^Hx'
$$

而分子转化为

$$
x^HAx =  x'^HB^{-1/2}AB^{-1/2}x'
$$

此时$$R(A,B,x)$$转化为$$R(A,B,x')$$

$$
R(A,B,x') = \frac{x'^HB^{-1/2}AB^{-1/2}x'}{x'^Hx'}
$$

利用前面的瑞利商的性质， $$R(A,B,x')$$的最大值为矩阵 $$B^{-1/2}AB^{-1/2}$$ 的最大特征值，或者说矩阵 $$B^{-1}A$$ 的最大特征值，而最小值为矩阵 $$B^{-1}A$$ 的最小特征值。

### 二类LDA原理

LDA希望投影后希望同一种类别数据的投影点尽可能的接近，而不同类别的数据的类别中心之间的距离尽可能的大

假设数据集 $$D=\left\{\left(x_{1}, y_{1}\right),\left(x_{2}, y_{2}\right), \ldots,\left(\left(x_{m}, y_{m}\right)\right)\right\}$$,其中任意样本 $$x_{i}$$ 为n维向量, $$y_{i} \in\{0,1\}$$我们定义$$N_{j}(j=0,1)$$为第类样本的个数, $$X_{j}(j=0,1)$$ 为第类样本的集合, 而 $$\mu{j}(j=0,1)$$ _为第迷样本的均值向量,_ 定义 $$\Sigma{j}(j=0,1)$$ 为第迷样本的协方差矩阵 (严格说是缺少分母部分的协方差矩阵) 。 $$\mu_{j}$$ 的表达式为:

$$
\mu_{j}=\frac{1}{N_{j}} \sum_{x \in X_{j}} x(j=0,1)
$$

$$\Sigma_{j}$$ 的表达式为：

$$
\Sigma_{j}=\sum_{x \in X_{j}}\left(x-\mu_{j}\right)\left(x-\mu_{j}\right)^{T} \quad(j=0,1)
$$

由于是两类数据，因此我们只需要将数据投影到一条直线上即可。假设我们的投影直线是向量$$w$$,则对任意一个样本$$x_i$$, 它在直线$$w$$的投影为 $$w^Tx_i$$ ,对于我们的两个类别的中心点 $$\mu_0,\mu_1$$ 在直线w的投影为$$w^T\mu_0, w^T\mu_1$$。由于LDA需要让不同类别的数据的类别中心之间的距离尽可能的大，也就是我们要最大化 $$||w^T\mu_0-w^T\mu_1||_2^2$$ ,同时我们希望同一种类别数据的投影点尽可能的接近，也就是要同类样本投影点的协方差 $$w^T\Sigma_0w, w^T\Sigma_1w$$, 尽可能的小，即最小化 $$w^T\Sigma_0w+w^T\Sigma_1w$$ 。综上所述，优化目标为:

$$
\underbrace{arg\;max}_w\;\;J(w) = \frac{||w^T\mu_0-w^T\mu_1||_2^2}{w^T\Sigma_0w+w^T\Sigma_1w} = \frac{w^T(\mu_0-\mu_1)(\mu_0-\mu_1)^Tw}{w^T(\Sigma_0+\Sigma_1)w}
$$

定义类内散度矩阵$$S_w$$

$$
S_w = \Sigma_0 + \Sigma_1 = \sum\limits_{x \in X_0}(x-\mu_0)(x-\mu_0)^T + \sum\limits_{x \in X_1}(x-\mu_1)(x-\mu_1)^T
$$

定义类间散度矩阵$$S_b$$

$$
S_b = (\mu_0-\mu_1)(\mu_0-\mu_1)^T
$$

优化目标重写为(广义瑞利商)

$$
\underbrace{arg\;max}_w\;\;J(w) = \frac{w^TS_bw}{w^TS_ww}
$$

$$J(w')$$ 最大值为矩阵 $$S^{−\frac{1}{2}}_wS_bS^{−\frac{1}{2}}_w$$ 的最大特征值，而对应的$$w'$$为$$S^{−\frac{1}{2}}_wS_bS^{−\frac{1}{2}}_w$$的最大特征值对应的特征向量! 而 $$S_w^{-1}S_b$$的特征值和 $$S^{−\frac{1}{2}}_wS_bS^{−\frac{1}{2}}_w$$ 的特征值相同, $$S_w^{-1}S_b$$的特征向量$$w$$和 $$S^{−\frac{1}{2}}_wS_bS^{−\frac{1}{2}}_w$$ 的特征向量$$w′$$满足 $$w = S^{−\frac{1}{2}}_ww'$$ 的关系!

### 多类LDA原理

多类向低维投影，则此时投影到的低维空间就不是一条直线，而是一个超平面了。假设我们投影到的低维空间的维度为d，对应的基向量为 $$(w_1,w_2,...w_d)$$ ，基向量组成的矩阵为$$W$$, 它是一个$$n×d$$的矩阵. 此时我们的优化目标应该可以变成为:

$$
\frac{W^TS_bW}{W^TS_wW}
$$

$$
S_b = \sum\limits_{j=1}^{k}N_j(\mu_j-\mu)(\mu_j-\mu)^T
$$

$$
S_w =  \sum\limits_{j=1}^{k}S_{wj} = \sum\limits_{j=1}^{k}\sum\limits_{x \in X_j}(x-\mu_j)(x-\mu_j)^T
$$

常见的一个LDA多类优化目标函数定义为：

$$
\underbrace{arg\;max}_W\;\;J(W) = \frac{\prod\limits_{diag}W^TS_bW}{\prod\limits_{diag}W^TS_wW}
$$

$$\prod\limits_{diag}A$$为A的主对角线元素的乘积, $$W$$ 为$$n×d$$的矩阵。

$$
J(W) = \frac{\prod\limits_{i=1}^dw_i^TS_bw_i}{\prod\limits_{i=1}^dw_i^TS_ww_i} = \prod\limits_{i=1}^d\frac{w_i^TS_bw_i}{w_i^TS_ww_i}
$$

上式最右边就是广义瑞利商！最大值是矩阵 $$S^{−1}_wS_b$$的最大特征值,最大的d个值的乘积就是矩阵$$S^{−1}_wS_b$$的最大的d个特征值的乘积,此时对应的矩阵$$W$$为这最大的$$d$$个特征值对应的特征向量张成的矩阵.

### LDA算法流程

输入：数据集 $$D=\left\{\left(x_{1}, y_{1}\right),\left(x_{2}, y_{2}\right), \ldots,\left(\left(x_{m}, y_{m}\right)\right)\right\}$$,其中任意样本 $$x_{i}$$ 为n维向量, $$y_{i} \in\left\{C_{1}, C_{2}, \ldots, C_{k}\right\},$$ 降维到的维度$$d$$。 \
输出：降维后的样本集$$D'$$&#x20;

1. 计算类内散度矩阵 $$S_{w}$$&#x20;
2. 计算类间散度矩阵 $$S_{b}$$&#x20;
3. 计算矩阵 $$S_{w}^{-1} S_{b}$$&#x20;
4. 计算 $$S_{w}^{-1} S_{b}$$ 的最大的$$d$$个特征值和对应的$$d$$个特征向量 $$\left(w_{1}, w_{2}, \ldots w_{d}\right),$$ 得到投影矩阵$$W^{T}$$&#x20;
5. 对样本集中的每一个样本特征 $$x_{i}$$,转化为新的样本 $$z_{i}=W^{T} x_{i}$$&#x20;
6. 得到输出样本集 $$D^{\prime}=\left\{\left(z_{1}, y_{1}\right),\left(z_{2}, y_{2}\right), \ldots,\left(\left(z_{m}, y_{m}\right)\right)\right\}$$



## Application

线性判别分析假设每种类别中的观测 服从多元正态分布，并且不同类别之间的具有同样的协方差。 二次判别分析仍然假设观测服从正 态分布，但假设每种类别都有自己的协方差。线性判别分析(LDA)可以用MASS包实现

```
## Data Preparation
library(MASS)
data(biopsy)
biopsy$ID = NULL
names(biopsy) = c("thick", "u.size", "u.shape", "adhsn", 
                  "s.size", "nucl", "chrom", "n.nuc", "mit", "class")
biopsy.v2 <- na.omit(biopsy)

## 为了满 足这个要求，可以创建一个变量y，用0表示良性，用1表示恶性
## 使用ifelse()函数为y赋 值，如下所示:
y <- ifelse(biopsy.v2$class == "malignant", 1, 0)

set.seed(123) #random number generator
ind <- sample(2, nrow(biopsy.v2), replace = TRUE, prob = c(0.7, 0.3))
train <- biopsy.v2[ind==1, ] #the training data set
test <- biopsy.v2[ind==2, ] #the test data set
trainY <- y[ind==1]
testY <- y[ind==2]




lda.fit <- lda(class ~ ., data = train)
lda.fit

## Call:
## lda(class ~ ., data = train)
## 
## Prior probabilities of groups:
##    benign malignant 
## 0.6371308 0.3628692 
## 
## Group means:
##             thick   u.size  u.shape    adhsn   s.size     nucl    chrom
## benign    2.92053 1.304636 1.413907 1.324503 2.115894 1.397351 2.082781
## malignant 7.19186 6.697674 6.686047 5.668605 5.500000 7.674419 5.959302
##              n.nuc      mit
## benign    1.225166 1.092715
## malignant 5.906977 2.639535
## 
## Coefficients of linear discriminants:
##                 LD1
## thick    0.19557291
## u.size   0.10555201
## u.shape  0.06327200
## adhsn    0.04752757
## s.size   0.10678521
## nucl     0.26196145
## chrom    0.08102965
## n.nuc    0.11691054
## mit     -0.01665454
```

```
## 在分组先验概率中，良性概率大约为64%，恶性概率大约为36%
## 线性判别系数是标准线性组合，用来确定观 测的判别评分的特征。评分越高，越可能被分入恶性组
## 画出判别评分的直方图和密度图
## 组间有些重合，这表明有些观测被错误分类
par(mar=c(2,2,2,2))
plot(lda.fit, type="both")
dev.off()
```

![](https://firebasestorage.googleapis.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-MRt1rIudcHYDxTcux\_Q%2Fuploads%2FsHP3KgD7FD26yaq2z6v0%2Ffile.png?alt=media)

```
## 在训练集上表现
## LDA模型可以用predict()函数得到3种元素(class、posterior和x)的列表。
## class元素是对 良性或恶性的预测，posterior是值为x的评分可能属于某个类别的概率
## x是线性判别评分, 仅提取恶性观测的概率
library(InformationValue)
train.lda.probs <- predict(lda.fit)$posterior[, 2]
misClassError(trainY, train.lda.probs)
confusionMatrix(trainY, train.lda.probs)
```

|   | 0\<int> | 1\<int> |
| - | ------- | ------- |
| 0 | 296     | 13      |
| 1 | 6       | 159     |



```
## 在测试集上的表现
test.lda.probs <- predict(lda.fit, newdata = test)$posterior[, 2]
misClassError(testY, test.lda.probs)
confusionMatrix(testY, test.lda.probs)
```

|   | 0\<int> | 1\<int> |
| - | ------- | ------- |
| 0 | 140     | 6       |
| 1 | 2       | 61      |



```
## 二次判别分析(QDA)模型来拟合数据, QDA也是MASS包的一部分
qda.fit <- qda(class ~ ., data =train)
qda.fit

## Call:
## qda(class ~ ., data = train)
## 
## Prior probabilities of groups:
##    benign malignant 
## 0.6371308 0.3628692 
## 
## Group means:
##             thick   u.size  u.shape    adhsn   s.size     nucl    chrom
## benign    2.92053 1.304636 1.413907 1.324503 2.115894 1.397351 2.082781
## malignant 7.19186 6.697674 6.686047 5.668605 5.500000 7.674419 5.959302
##              n.nuc      mit
## benign    1.225166 1.092715
## malignant 5.906977 2.639535
```

```
## 结果中有分组均值，和LDA一样;但是没有系数，因为这是二次函数
train.qda.probs <- predict(qda.fit)$posterior[, 2]
misClassError(trainY, train.qda.probs)
confusionMatrix(trainY, train.qda.probs)
```

```
test.qda.probs <- predict(qda.fit, newdata = test)$posterior[, 2]
misClassError(testY, test.qda.probs)
confusionMatrix(testY, test.qda.probs)
```
