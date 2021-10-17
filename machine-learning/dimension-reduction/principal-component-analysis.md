# Principal Component Analysis (PCA)

## Introduction

* 聚类分析，它可以将相似的观测 归成一类
* 主成分分析(PCA)，它可以对相关变量进行归类，从而降低数据维度，提高对数据的理解

### Component

成分就是特征的规范化线性组合(James，2012)。在一个数据集中，**第一主成分就 是能够最大程度解释数据中的方差的特征线性组合**。**第二主成分是另一种特征线性组合，它在方 向与第一主成分垂直这个限制条件下，最大程度解释数据中的方差**。其后的每一个主成分(可以 构造与变量数相等数目的主成分)都遵循同样的规则。

* 线性组合：如果你试图在一个变量之间基本不相关的数据集上使用PCA，很可能会得到一个毫无意义的分析结果
* 变量的均值和方差是充分统计量。也就是说，数据应该服从正态分布，这样协方差矩阵即可充分 描述数据集。换言之，数据要满足多元正态分布。PCA对于非正态分布的数据具有相当强的鲁棒 性，甚至可以和二值变量一起使用，所以结果具有很好的解释性。



### PCA Idee

PCA顾名思义，就是找出数据里最主要的方面，用数据里最主要的方面来代替原始数据。具体的，假如我们的数据集是n维的，共有m个数据 $$(x^{(1)},x^{(2)},...,x^{(m)})$$ 。我们希望将这m个数据的维度从n维降到n'维，希望这m个n'维的数据集尽可能的代表原始数据集。我们知道数据从n维降到n'维肯定会有损失，但是我们希望损失尽可能的小。如何让这n'维的数据尽可能表示原来的数据

最简单的情况，n=2，n'=1,也就是将数据从二维降维到一维。图中列了两个向量方向，$$\mu1, \mu2$$,直观上也可以看出$$\mu_1$$好于$$\mu_2$$

![](<../../.gitbook/assets/image (55).png>)

有两种解释:

1. 样本点到这个直线的距离足够近
2. 样本点在这个直线上的投影能尽可能的分开。

假如我们把n'从1维推广到任意维，则我们的希望降维的标准为：样本点到这个超平面的距离足够近,或者说样本点在这个超平面上的投影能尽可能的分开。基于上面的两种标准，我们可以得到PCA的两种等价推导。

### PCA的推导:基于最小投影距离

第一种解释的推导，即样本点到这个超平面的距离足够近。假设m个n维数据 $$(x^{(1)}, x^{(2)},...,x^{(m)})$$ 都已经进行了中心化，即 $$\sum\limits_{i=1}^{m}x^{(i)}=0$$ 。经过投影变换后得到的新坐标系为 $$\{w_1,w_2,...,w_n\}$$,其中w是标准正交基，即 $$||w||_2=1, w_i^Tw_j=0$$ 如果我们将数据从n维降到n'维，即丢弃新坐标系中的部分坐标，则新的坐标系为 $$\{w_1,w_2,...,w_{n'}\}$$, 样本点$$x^{(i)}$$在n'维坐标系中的投影为： $$z^{(i)} = (z_1^{(i)}, z_2^{(i)},...,z_{n'}^{(i)})^T$$ .其中， $$z_j^{(i)} = w_j^Tx^{(i)}$$ 是$$x^{(i)}$$在低维坐标系里第j维的坐标。

$$
\overline{x}^{(i)} = \sum\limits_{j=1}^{n'}z_j^{(i)}w_j = Wz^{(i)}
$$

其中，W为标准正交基组成的矩阵。当考虑整个样本集，我们希望所有的样本到这个超平面的距离足够近，即最小化下式：

$$
\sum\limits_{i=1}^{m}||\overline{x}^{(i)} - x^{(i)}||_2^2
$$

$$
\begin{align} \sum\limits_{i=1}^{m}||\overline{x}^{(i)} - x^{(i)}||_2^2 & = \sum\limits_{i=1}^{m}|| Wz^{(i)} - x^{(i)}||_2^2 \\& = \sum\limits_{i=1}^{m}(Wz^{(i)})^T(Wz^{(i)}) - 2\sum\limits_{i=1}^{m}(Wz^{(i)})^Tx^{(i)} + \sum\limits_{i=1}^{m} x^{(i)T}x^{(i)} \\& = \sum\limits_{i=1}^{m}z^{(i)T}z^{(i)} - 2\sum\limits_{i=1}^{m}z^{(i)T}W^Tx^{(i)} +\sum\limits_{i=1}^{m} x^{(i)T}x^{(i)} \\& = \sum\limits_{i=1}^{m}z^{(i)T}z^{(i)} - 2\sum\limits_{i=1}^{m}z^{(i)T}z^{(i)}+\sum\limits_{i=1}^{m} x^{(i)T}x^{(i)}  \\& = - \sum\limits_{i=1}^{m}z^{(i)T}z^{(i)} + \sum\limits_{i=1}^{m} x^{(i)T}x^{(i)}  \\& =   -tr( W^T（\sum\limits_{i=1}^{m}x^{(i)}x^{(i)T})W)  + \sum\limits_{i=1}^{m} x^{(i)T}x^{(i)} \\& =  -tr( W^TXX^TW)  + \sum\limits_{i=1}^{m} x^{(i)T}x^{(i)}  \end{align}
$$

最小化上式等价于：

$$
\underbrace{arg\;min}_{W}\;-tr( W^TXX^TW) \;\;s.t. W^TW=I
$$

利用拉格朗日函数可以得到

$$
J(W) = -tr( W^TXX^TW + \lambda(W^TW-I))
$$

对W求导有 $$-XX^TW+\lambda W=0$$, 整理下即为： 

$$
XX^TW=\lambda W
$$

这样可以更清楚的看出，W为 $$XX^T$$ 的n'个特征向量组成的矩阵，而λ为$$XX^T$$的若干特征值组成的矩阵，特征值在主对角线上，其余位置为0。当我们将数据集从n维降到n'维时，需要找到最大的n'个特征值对应的特征向量。这n'个特征向量组成的矩阵W即为我们需要的矩阵。对于原始数据集，我们只需要用 $$z^{(i)}=W^Tx^{(i)}$$ ,就可以把原始数据集降维到最小投影距离的n'维数据集。

### PCA的推导:基于最大投影方差

对于任意一个样本 $$x^{(i)},$$ 在新的坐标系中的投影为 $$W^{T} x^{(i)}$$,在新坐标系中的投影方差为 $$x^{(i) T} W W^{T} x^{(i)},$$ 要使所有的样本的投影方差和最大, 也就是 最大化 $$\sum_{i=1}^{m} W^{T} x^{(i)} x^{(i) T} W$$ 的迹,即:

$$
\underbrace{\arg \max }_{W} \operatorname{tr}\left(W^{T} X X^{T} W\right) \text { s.t. } W^{T} W=I
$$

观察第二节的基于最小投影距离的优化目标, 可以发现完全一样, 只是一个是加负号的最小化, 一个是最大化。。 利用拉格朗日函数可以得到

$$
J(W)=\operatorname{tr}\left(W^{T} X X^{T} W+\lambda\left(W^{T} W-I\right)\right)
$$

对W求导有 $$X X^{T} W+\lambda W=0,$$ 整理下即为：

$$
X X^{T} W=(-\lambda) W
$$

和上面一样可以看出, W为 $$X X^{T}$$ 的n'个特征向量组成的矩阵, 而一 $$\lambda$$ 为 $$X X^{T}$$ 的若干特征值组成的矩阵, 特征值在主对角线上, 其余位置为0。当我们 将数据集从n维降到n'维时，需要找到最大的n'个特征值对应的特征向量。这n'个特征向量组成的矩阵W即为我们需要的矩阵。对于原始数据集, 我们只需要用 $$z^{(i)}=W^{T} x^{(i)}$$,就可以把原始数据集降维到最小投影距离的n'维数据集。

### PCA算法

从上面两节我们可以看出, 求样本 $$x^{(i)}$$ 的n'维的主成分其实就是求样本集的协方差矩阵 $$X X^{T}$$ 的前n'个特征值对应特征向量矩阵W，然后对于每个样本 $$x^{(i)}$$,做如下变换 $$z^{(i)}=W^{T} x^{(i)},$$ 即达到降维的PCA目的。

具体的算法流程。

* 输入：n维样本集 $$D=\left(x^{(1)}, x^{(2)}, \ldots, x^{(m)}\right),$$ 要降维到的维数n'.
* 输出：降维后的样本集 $$D^{\prime}$$



1. 对所有的样本进行中心化: $$x^{(i)}=x^{(i)}-\frac{1}{m} \sum_{j=1}^{m} x^{(j)}$$
2. 计算样本的协方差矩阵 $$X X^{T}$$
3. 对矩阵 $$X X^{T}$$ 进行特征值分解
4. 取出最大的n'个特征值对应的特征向量 $$\left(w_{1}, w_{2}, \ldots, w_{n^{\prime}}\right),$$ 将所有的特征向量标准化后, 组成特征向量矩阵W。。
5. 对样本集中的每一个样本 $$x^{(i)}$$,转化为新的样本 $$z^{(i)}=W^{T} x^{(i)}$$
6.  得到输出样本集 $$D^{\prime}=\left(z^{(1)}, z^{(2)}, \ldots, z^{(m)}\right)$$

    有时候, 我们不指定降维后的n'的值, 而是换种方式, 指定一个降维到的主成分比重间值t。这个间值t在 (0,1]之间。假如我们的n个特征值为 $$\lambda_{1} \geq \lambda_{2} \geq \ldots \geq \lambda_{n}$$,则n'可以通过下式得到:

    $$
    \frac{\sum_{i=1}^{n^{\prime}} \lambda_{i}}{\sum_{i=1}^{n} \lambda_{i}} \geq t
    $$

### 实例

假设我们的数据集有10个二维数据(2.5,2.4), (0.5,0.7), (2.2,2.9), (1.9,2.2), (3.1,3.0), (2.3, 2.7), (2, 1.6), (1, 1.1), (1.5, 1.6), (1.1, 0.9)，需要用PCA降到1维特征。

1. 首先我们对样本中心化，这里样本的均值为(1.81, 1.91),所有的样本减去这个均值向量后，即中心化后的数据集为(0.69, 0.49), (-1.31, -1.21), (0.39, 0.99), (0.09, 0.29), (1.29, 1.09), (0.49, 0.79), (0.19, -0.31), (-0.81, -0.81), (-0.31, -0.31), (-0.71, -1.01)。
2. 求样本的协方差矩阵，由于我们是二维的，则协方差矩阵为：\
    $$\mathbf{XX^T} = \left( \begin{array}{ccc} cov(x_1,x_1) & cov(x_1,x_2)\\   cov(x_2,x_1) & cov(x_2,x_2) \end{array} \right)$$\
    $$\mathbf{XX^T} = \left( \begin{array}{ccc} 0.616555556 & 0.615444444\\    0.615444444 & 0.716555556 \end{array} \right)$$ 
3. 求出特征值为（0.0490833989， 1.28402771）
4. 对应的特征向量分别为： $$(0.735178656, 0.677873399)^T\;\; (-0.677873399, -0.735178656)^T$$ ,由于最大的k=1个特征值为1.28402771，对于的k=1个特征向量为(−0.677873399,−0.735178656)T. 则我们的W=(−0.677873399,−0.735178656)T
5. 对所有的数据集进行投影 $$z^{(i)}=W^Tx^{(i)}$$ 
6. 得到PCA降维后的10个一维数据集为：(-0.827970186， 1.77758033， -0.992197494， -0.274210416， -1.67580142， -0.912949103， 0.0991094375， 1.14457216, 0.438046137， 1.22382056)

### 主成分旋转

**旋转方法有两种**

* **正交旋转**(rotate="varimax")\
  使得成分保持不相关（正交旋转）
* **斜交旋转**(rotate="promax")\
  使得成分相关\
  \
  最流行的正交旋转是 方差极大旋转，它试图对载荷的列进行去噪， 使得每个成分只由一组有限的变量来解释（即载荷阵每列只有少数几个很大的载荷，其他的都是很小的载荷）

## Kernelized PCA

PCA算法中，假设存在一个线性的超平面，可以让我们对数据进行投影。但是有些时候，数据不是线性的，不能直接进行PCA降维。

这里就需要用到和支持向量机一样的核函数的思想，先把数据集从n维映射到线性可分的高维N>n,然后再从N维降维到一个低维度n', 这里的维度之间满足n'\<n\<N。

使用了核函数的主成分分析一般称之为核主成分分析(Kernelized PCA, 以下简称KPCA。假设高维空间的数据是由n维空间的数据通过映射$$\phi$$产生。则对于n维空间的特征分解：

$$
\sum\limits_{i=1}^{m}x^{(i)}x^{(i)T}W=\lambda W
$$

映射为：

$$
\sum\limits_{i=1}^{m}\phi(x^{(i)})\phi(x^{(i)})^TW=\lambda W
$$

通过在高维空间进行协方差矩阵的特征值分解，然后用和PCA一样的方法进行降维。

## Application

### 数据准备

```
library(ggplot2) #support scatterplot
# library(GPArotation) #support rotation
library(psych) #PCA package

test <- read.csv("~/Library/Mobile Documents/com~apple~CloudDocs/02. Programm/01 R_lernen/00 Data/NHLtest.csv", header=T)
train <- read.csv("~/Library/Mobile Documents/com~apple~CloudDocs/02. Programm/01 R_lernen/00 Data/NHLtrain.csv", header=T)
train.scale <- scale(train[, -1:-2])
```

### 模型构建

对于模型构建过程，我们按照以下几个步骤进行:

* 抽取主成分并决定保留的数量;
* 对留下的主成分进行旋转;
* 对旋转后的解决方案进行解释;
* 生成各个因子的得分;
* 使用得分作为输入变量进行回归分析，并使用测试数据评价模型效果。

#### 主成分抽取

```
## 通过psych包抽取主成分要使用principal()函数，语法中要包括数据和是否要进行主成分旋转:
pca <- principal(train.scale, rotate="none")

## 使用碎石图可以帮助你评估能解释大部分数据方差的主成分
## 它用X轴表示主成分的数量，用Y轴表示相应的特征值:
plot(pca$values, type="b", ylab="Eigenvalues", xlab="Component")

## 需要在碎石图中找出使变化率降低的那个点，也就是我们常说的统计图中的“肘点”或弯曲 点。
## 在统计图中，肘点表示在这个点上新增加一个主成分时，对方差的解释增加得并不太多。
## 换句话说，这个点就是曲线由陡变平的转折点
```

![](https://firebasestorage.googleapis.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-MRt1rIudcHYDxTcux_Q%2Fuploads%2FxFcdoVMAG1vZJa0Q0n1f%2Ffile.png?alt=media)

#### 正交旋转与解释

```
## 旋转背后的意义是使变量在某个主成分上的载荷最大化
## 可以减少(或消灭)主成分之间的相关性，有助于对主成分的解释。
## 进行正交旋转的方法称为“方差最大法”。
## 还有其他非正交旋转方法，这种方法允许主成分(因子)之间存在相关性
## 设定使用5个主成分，并需要进行正交旋转

pca.rotate <- principal(train.scale, nfactors = 5, rotate = "varimax")
pca.rotate
```

```
## Principal Components Analysis
## Call: principal(r = train.scale, nfactors = 5, rotate = "varimax")
## Standardized loadings (pattern matrix) based upon correlation matrix
##                 RC1   RC2   RC5   RC3   RC4   h2   u2 com
## Goals_For     -0.21  0.82  0.21  0.05 -0.11 0.78 0.22 1.3
## Goals_Against  0.88 -0.02 -0.05  0.21  0.00 0.82 0.18 1.1
## Shots_For     -0.22  0.43  0.76 -0.02 -0.10 0.81 0.19 1.8
## Shots_Against  0.73 -0.02 -0.20 -0.29  0.20 0.70 0.30 1.7
## PP_perc       -0.73  0.46 -0.04 -0.15  0.04 0.77 0.23 1.8
## PK_perc       -0.73 -0.21  0.22 -0.03  0.10 0.64 0.36 1.4
## CF60_pp       -0.20  0.12  0.71  0.24  0.29 0.69 0.31 1.9
## CA60_sh        0.35  0.66 -0.25 -0.48 -0.03 0.85 0.15 2.8
## OZFOperc_pp   -0.02 -0.18  0.70 -0.01  0.11 0.53 0.47 1.2
## Give          -0.02  0.58  0.17  0.52  0.10 0.65 0.35 2.2
## Take           0.16  0.02  0.01  0.90 -0.05 0.83 0.17 1.1
## hits          -0.02 -0.01  0.27 -0.06  0.87 0.83 0.17 1.2
## blks           0.19  0.63 -0.18  0.14  0.47 0.70 0.30 2.4
## 
##                        RC1  RC2  RC5  RC3  RC4
## SS loadings           2.69 2.33 1.89 1.55 1.16
## Proportion Var        0.21 0.18 0.15 0.12 0.09
## Cumulative Var        0.21 0.39 0.53 0.65 0.74
## Proportion Explained  0.28 0.24 0.20 0.16 0.12
## Cumulative Proportion 0.28 0.52 0.72 0.88 1.00
## 
## Mean item complexity =  1.7
## Test of the hypothesis that 5 components are sufficient.
## 
## The root mean square of the residuals (RMSR) is  0.08 
##  with the empirical chi square  28.59  with prob <  0.19 
## 
## Fit based upon off diagonal values = 0.91
```

#### 根据主成分建立因子得分

```
pca.scores <- data.frame(pca.rotate$scores)
head(pca.scores)

## 得到每个球队在每个因子上的得分，这些得分的计算非常简单，
   每个观测的变量值乘以载荷 然后相加即可。现在可以将响应变量(ppg)作为一列加入数据
pca.scores$ppg <- train$ppg
```

#### 回归分析

```
nhl.lm <- lm(ppg ~ ., data = pca.scores)
summary(nhl.lm)

nhl.lm2 <- lm(ppg ~ RC1 + RC2, data = pca.scores)
summary(nhl.lm2)

## 评价模型误差
sqrt(mean(nhl.lm2$residuals^2))
```

```
## 模型在样本外数据上的效果
test.scores <- data.frame(predict(pca.rotate, test[, c(-1:-2)]))
test.scores$pred <- predict(nhl.lm2, test.scores)

test.scores$ppg <- test$ppg
test.scores$Team <- test$Team

## 评价模型误差
sqrt(mean(nhl.lm2$residuals^2))
```
