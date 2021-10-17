# K-means Clustering

## K均值聚类

像其他聚类算法一样，k均值试图将观察分为互斥组（或簇），以使同一聚类中的观察尽可能相似（即，类内相似度很高），而来自不同聚类的观察则尽可能不相似（即，类间相似度低）。在k均值聚类中，每个聚类都由其中心（即质心）表示，该中心对应于分配给该聚类的观测值的平均值。查找这些聚类的过程类似于k最近邻居（KNN）算法。

但是，K-Means是无监督学习的聚类算法，没有样本输出；而KNN是监督学习的分类算法，有对应的类别输出。KNN基本不需要训练，对测试集里面的点，只需要找到在训练集中最近的k个点，用这最近的k个点的类别来决定测试点的类别。而K-Means则有明显的训练过程，找到k个类别的最佳质心，从而决定样本的簇类别。两者也有一些相似点，两个算法都包含一个过程，即找出和某一个点最近的点。两者都利用了最近邻(nearest neighbors)的思想

### Idee

K-Means算法的思想很简单，对于给定的样本集，按照样本之间的距离大小，将样本集划分为K个簇。让簇内的点尽量紧密的连在一起，而让簇间的距离尽量的大。如果用数据表达式表示，假设簇划分为 $$(C_1,C_2,...C_k)$$ ，则我们的目标是最小化平方误差E：

$$
E = \sum\limits_{i=1}^k\sum\limits_{x \in C_i} ||x-\mu_i||_2^2
$$

其中$$u_i$$是簇$$C_i$$的均值向量，有时也称为质心，表达式为：

$$
\mu_i = \frac{1}{|C_i|}\sum\limits_{x \in C_i}x
$$

直接求上式的最小值并不容易，只能采用启发式的迭代方法。假设k=2。随机选择了两个k类所对应的类别质心，然后分别求样本中所有点到这两个质心的距离，并标记每个样本的类别为和该样本距离最小的质心的类别，经过计算样本和红色质心和蓝色质心的距离，我们得到了所有样本点的第一轮迭代后的类别。分别求其新的质心，新的质心位置已经发生了变动，即将所有点的类别标记为距离最近的质心的类别并求新的质心。

### Algorithm

对于K-Means算法，首先要注意的是k值的选择，一般来说，我们会根据对数据的先验经验选择一个合适的k值，如果没有什么先验知识，则可以通过交叉验证选择一个合适的k值。在确定了k的个数后，我们需要选择k个初始化的质心 (随机质心)。由于我们是启发式方法，k个初始化的质心的位置选择对最后的聚类结果和运行时间都有很大的影响，因此需要选择合适的k个质心，最好这些质心不能太近。

初始分配是随机的，所以会造成每次聚类结果不一致。因此，重要 的一点是，要进行多次初始分配，让软件找出最优的解。

迭代过程: 输入是样本集 $$D=\left\{x_{1}, x_{2}, \ldots x_{m}\right\}$$,聚类的族树k,最大迭代次数N 输出是族划分 $$C=\left\{C_{1}, C_{2}, \ldots C_{k}\right\}$$

1. 从数据集D中随机选择k个样本作为初始的k个质心向量： $$\left\{\mu_{1}, \mu_{2}, \ldots, \mu_{k}\right\}$$
2.  对于 $$n=1,2, \ldots, N$$

     a. 将族划分C初始化为 $$C_{t}=\varnothing t=1,2 \ldots k$$

     b. 对于 $$i=1,2 \ldots$$, 计算样本 $$x_{i}$$ 和各个质心向量 $$\mu_{j}(j=1,2, \ldots k)$$ 的距离: $$d_{i j}=\left\|x_{i}-\mu_{j}\right\|_{2}^{2},$$ 将 $$x_{i}$$ 标记最小的为 $$d_{i j}$$ 所对应的类别 $$\lambda_{i_{\circ}}$$ 此时 更新 $$C_{\lambda_{i}}=C_{\lambda_{i}} \cup\left\{x_{i}\right\}$$

     c. 对于 $$j=1,2, \ldots, k,$$ 对 $$C_{j}$$ 中所有的样本点重新计算新的质心 $$\mu_{j}=\frac{1}{\left|C_{j}\right|} \sum_{x \in C_{j}} x$$

     d. 如果所有的k个质心向量都没有发生变化, 则转到步骤3)
3. 输出族划分 $$C=\left\{C_{1}, C_{2}, \ldots C_{k}\right\}$$

### K-Means++

个初始化的质心的位置选择对最后的聚类结果和运行时间都有很大的影响，因此需要选择合适的k个质心。如果仅仅是完全随机的选择，有可能导致算法收敛很慢。K-Means++算法就是对K-Means随机初始化质心的方法的优化。

K-Means++的对于初始化质心的优化策略如下：

* a. 从输入的数据点集合中随机选择一个点作为第一个聚类中心 $$\mu_{1}$$ 
* b. 对于数据集中的每一个点 $$x_{i},$$ 计算它与已选择的聚类中心中最近聚类中心的距离

$$
D\left(x_{i}\right)=\arg \min \left\|x_{i}-\mu_{r}\right\|_{2}^{2} r=1,2, \ldots k_{\text {selected }}
$$

* c. 选择一个新的数据点作为新的聚类中心, 选择的原则是: $$D(x)$$ 较大的点, 被选取作为聚类中心的概率较大 
* d. 重复b和c直到选择出k个聚类质心 
* e. 利用这k个质心来作为初始化质心去运行标准的K-Means算法

### elkan K-Means

统的K-Means算法中，我们在每轮迭代时，要计算所有的样本点到所有的质心的距离，这样会比较的耗时。elkan K-Means算法就是从这块入手加以改进。它的目标是减少不必要的距离的计算。如何定义不需要计算的距离。

elkan K-Means利用了两边之和大于等于第三边,以及两边之差小于第三边的三角形性质, 来减少距离的计算。 

1. 第一种规律是对于一个样本点 $$x$$ 和两个质心 $$\mu_{j_{1}}, \mu_{j_{2} \circ}$$ 如果我们预先计算出了这两个质心之间的距离 $$D\left(j_{1}, j_{2}\right),$$ 则如果计算发现 $$2 D\left(x, j_{1}\right) \leq D\left(j_{1}, j_{2}\right)$$,我们立即就可以知道 $$D\left(x, j_{1}\right) \leq D\left(x, j_{2}\right)$$ 。此时我们不需要再计算 $$D\left(x, j_{2}\right)$$,也就是说省了一步距离计算。 
2.  第二种规律是对于一个样本点 $$x$$ 和两个质心 $$\mu_{j_{1}}, \mu_{j_{2} \circ}$$ 我们可以得到 $$D\left(x, j_{2}\right) \geq \max \left\{0, D\left(x, j_{1}\right)-D\left(j_{1}, j_{2}\right)\right\}_{\circ}$$ 这个从三角形的性质也很容易得到。

    　　　　

利用上边的两个规律，elkan K-Means比起传统的K-Means迭代速度有很大的提高。但是如果我们的样本的**特征是稀疏的**，**有缺失值的话**，这个方法就不使用了，此时某些距离无法计算，则不能使用该算法。

### Mini Batch K-Means

传统的K-Means算法中，要计算所有的样本点到所有的质心的距离。如果样本量非常大，比如达到10万以上，特征有100以上，此时用传统的K-Means算法非常的耗时，就算加上elkan K-Means优化也依旧。在大数据时代，这样的场景越来越多。此时Mini Batch K-Means应运而生。

Mini Batch，也就是用样本集中的一部分的样本来做传统的K-Means，这样可以避免样本量太大时的计算难题，算法收敛速度大大加快。当然此时的代价就是我们的聚类的精确度也会有一些降低。一般来说这个降低的幅度在可以接受的范围之内。

在Mini Batch K-Means中，我们会选择一个合适的批样本大小batch size，我们仅仅用batch size个样本来做K-Means聚类。batch size一般是通过无放回的随机采样得到的。为了增加算法的准确性，一般会多跑几次Mini Batch K-Means算法，用得到不同的随机采样集来得到聚类簇，选择其中最优的聚类簇。

## Application

### Data Preparation

```
## 聚类分析
library(cluster)       # conduct cluster analysis
library(compareGroups) # build descriptive statistic tables
library(HDclassif)     # contains the dataset
library(NbClust)       # cluster validity measures
library(sparcl)        # colored dendrogram

## 数据集位于HDclassif包
## 数据包括178种葡萄酒，有13个变量表示酒中的化学成分，还有一个标号变量Class，表示品 种等级或葡萄种植品种。在聚类过程中，我们不会使用这个标号变量，而是用它验证模型性能。
data(wine)
str(wine)

## 'data.frame':    178 obs. of  14 variables:
##  $ class: int  1 1 1 1 1 1 1 1 1 1 ...
##  $ V1   : num  14.2 13.2 13.2 14.4 13.2 ...
##  $ V2   : num  1.71 1.78 2.36 1.95 2.59 1.76 1.87 2.15 1.64 1.35 ...
##  $ V3   : num  2.43 2.14 2.67 2.5 2.87 2.45 2.45 2.61 2.17 2.27 ...
##  $ V4   : num  15.6 11.2 18.6 16.8 21 15.2 14.6 17.6 14 16 ...
##  $ V5   : int  127 100 101 113 118 112 96 121 97 98 ...
##  $ V6   : num  2.8 2.65 2.8 3.85 2.8 3.27 2.5 2.6 2.8 2.98 ...
##  $ V7   : num  3.06 2.76 3.24 3.49 2.69 3.39 2.52 2.51 2.98 3.15 ...
##  $ V8   : num  0.28 0.26 0.3 0.24 0.39 0.34 0.3 0.31 0.29 0.22 ...
##  $ V9   : num  2.29 1.28 2.81 2.18 1.82 1.97 1.98 1.25 1.98 1.85 ...
##  $ V10  : num  5.64 4.38 5.68 7.8 4.32 6.75 5.25 5.05 5.2 7.22 ...
##  $ V11  : num  1.04 1.05 1.03 0.86 1.04 1.05 1.02 1.06 1.08 1.01 ...
##  $ V12  : num  3.92 3.4 3.17 3.45 2.93 2.85 3.58 3.58 2.85 3.55 ...
##  $ V13  : int  1065 1050 1185 1480 735 1450 1290 1295 1045 1045 ...

names(wine) <- c("Class", "Alcohol", "MalicAcid", "Ash", "Alk_ash",
                 "magnesium", "T_phenols", "Flavanoids", "Non_flav",
                 "Proantho", "C_Intensity", "Hue", "OD280_315", "Proline")
df <- as.data.frame(scale(wine[, -1]))             
table(wine$Class)      

##  1  2  3 
## 59 71 48           
```

### K 均值聚类

```
## 函数中将method的值设定为kmeans即可，同时将最大簇数目放大到15
numKMeans <- NbClust(df, min.nc = 2, max.nc = 15, method = "kmeans")
```

![](https://firebasestorage.googleapis.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-MRt1rIudcHYDxTcux_Q%2Fuploads%2FqqUSkAEw6HGeIkUxWR5b%2Ffile.png?alt=media)

```
## *** : The Hubert index is a graphical method of determining the number of clusters.
##                 In the plot of Hubert index, we seek a significant knee that corresponds to a 
##                 significant increase of the value of the measure i.e the significant peak in Hubert
##                 index second differences plot. 
## 
```

![](https://firebasestorage.googleapis.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-MRt1rIudcHYDxTcux_Q%2Fuploads%2FWVz3ocZWPesYMZphztqT%2Ffile.png?alt=media)

```
## *** : The D index is a graphical method of determining the number of clusters. 
##                 In the plot of D index, we seek a significant knee (the significant peak in Dindex
##                 second differences plot) that corresponds to a significant increase of the value of
##                 the measure. 
##  
## ******************************************************************* 
## * Among all indices:                                                
## * 2 proposed 2 as the best number of clusters 
## * 19 proposed 3 as the best number of clusters 
## * 1 proposed 14 as the best number of clusters 
## * 1 proposed 15 as the best number of clusters 
## 
##                    ***** Conclusion *****                            
##  
## * According to the majority rule, the best number of clusters is  3 
##  
##  
## *******************************************************************
```

```
## 3个簇再次成为最优解
set.seed(1234)
km <- kmeans(df, 3, nstart = 25)
table(km$cluster)

##  1  2  3 
## 62 65 51
```

```
## 簇之间的观测数量分布得非常均衡。对聚类结果的另一种分析方式是查看簇中心点矩阵，
   它保存了每个簇中每个变量的中心点值:
km$centers

##      Alcohol  MalicAcid        Ash    Alk_ash   magnesium   T_phenols
## 1  0.8328826 -0.3029551  0.3636801 -0.6084749  0.57596208  0.88274724
## 2 -0.9234669 -0.3929331 -0.4931257  0.1701220 -0.49032869 -0.07576891
## 3  0.1644436  0.8690954  0.1863726  0.5228924 -0.07526047 -0.97657548
##    Flavanoids    Non_flav    Proantho C_Intensity        Hue  OD280_315
## 1  0.97506900 -0.56050853  0.57865427   0.1705823  0.4726504  0.7770551
## 2  0.02075402 -0.03343924  0.05810161  -0.8993770  0.4605046  0.2700025
## 3 -1.21182921  0.72402116 -0.77751312   0.9388902 -1.1615122 -1.2887761
##      Proline
## 1  1.1220202
## 2 -0.7517257
## 3 -0.4059428
```

### 果瓦系数和PAM

无论层次聚类还是K均值聚类，都不是为分析混合数据(既包括定量数据又包括定性数据)而专门设计的。使用果瓦相异度系数将混合数据转换为适当的特征 空间。在这种方法中，甚至可以使用因子作为输入变量处理混合数据，比 如可以先进行主成分分析, 建立潜变量，然后使用潜变量作为聚类的输入

聚类算法使用 PAM聚类算法（Partitioning Around Medoid），而不是K均值, PAM和K均值很相似, 有两个明显的优点 1. PAM可以接受相异度矩阵作为输入，这样即可处理混合数据 2. PAM对于异常值和不对称数据的鲁棒性更好，因为它最小化的是相异度总和，而不是欧氏距离的平方和

#### 果瓦系数

果瓦系数比较两个成对的实例，并计算它们之间的相异度，实质上就是每个变量的贡献的加权平均值。对于两个实例i与j，果瓦系数定义如下

#### PAM

中心点是簇内所有观测中，使相异度(使用果瓦系数表示)最小的那个观测。所以，同K均值一样，如果指定5个簇，就可以将数据划分为5份。PAM算法的目标是，使所有观测与离它们最近的中心点的相异度最小。该算法按照下面的步骤迭代:

```
1. 随机选择k个观测作为初始中心点;
2. 将每个观测分配至最近的中心点;
3. 用非中心点观测替换中心点，并计算相异度的变化;
4. 选择能使总相异度最小的配置;
5. 重复第(2)步~第(4)步，直至中心点不再变化。
```

果瓦系数和PAM都可以使用R中的cluster包实现。使用daisy()函数计算相异度矩阵，从而 计算果瓦系数，然后使用pam()函数进行实际的数据划分

```
## 处理因子变量，所以可以将酒 精函数转换为因子，它有两个水平:高/低
wine$Alcohol <- as.factor(ifelse(df$Alcohol > 0, "High", "Low"))

## 建立相异度矩阵，使用cluster包中的daisy()函数
disMatrix <- daisy(wine[, -1], metric = "gower")  
set.seed(123)
pamFit <- pam(disMatrix, k = 3)

table(pamFit$clustering)
##  1  2  3 
## 62 71 45

table(pamFit$clustering, wine$Class)
##      1  2  3
##   1 57  5  0
##   2  2 64  5
##   3  0  2 43
```

```
wine$cluster <- pamFit$clustering

## 使用compareGroups包建立一张描述性统计表
group <- compareGroups(cluster ~ ., data = wine) 
clustab <- createTable(group) 
clustab

## --------Summary descriptives table by 'cluster'---------
## 
## _________________________________________________________ 
##                  1           2           3      p.overall 
##                N=62        N=71        N=45               
## ¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯ 
## Class       1.08 (0.27) 2.04 (0.31) 2.96 (0.21)  <0.001   
## Alcohol:                                         <0.001   
##     High     62 (100%)   1 (1.41%)  29 (64.4%)            
##     Low      0 (0.00%)  70 (98.6%)  16 (35.6%)            
## MalicAcid   2.00 (0.83) 1.95 (0.91) 3.41 (1.09)  <0.001   
## Ash         2.42 (0.27) 2.28 (0.30) 2.44 (0.18)   0.002   
## Alk_ash     17.2 (2.75) 20.2 (3.20) 21.6 (2.26)  <0.001   
## magnesium   105 (11.7)  95.6 (16.8) 99.1 (10.8)   0.001   
## T_phenols   2.83 (0.36) 2.20 (0.56) 1.71 (0.37)  <0.001   
## Flavanoids  2.96 (0.42) 1.99 (0.76) 0.81 (0.31)  <0.001   
## Non_flav    0.29 (0.07) 0.36 (0.12) 0.46 (0.12)  <0.001   
## Proantho    1.89 (0.43) 1.60 (0.60) 1.18 (0.43)  <0.001   
## C_Intensity 5.44 (1.29) 3.17 (1.01) 7.51 (2.36)  <0.001   
## Hue         1.07 (0.13) 1.03 (0.21) 0.69 (0.13)  <0.001   
## OD280_315   3.12 (0.36) 2.75 (0.58) 1.70 (0.26)  <0.001   
## Proline     1070 (279)   535 (167)   635 (119)   <0.001   
## ¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯

# export2csv(clustab,file = "wine_clusters.csv")
# export2pdf(clustab, file = "wine_clusters.pdf")
```
