# Hierarchical Clustering

## 层次聚类

层次聚类算法的基础是观测之间的相异度测量。层次聚类(Hierarchical Clustering)是聚类算法的一种，通过计算不同类别数据点间的相似度来创建一棵有层次的嵌套聚类树。在聚类树中，不同类别的原始数据点是树的最低层，树的顶层是一个聚类的根节点。创建聚类树有自下而上合并和自上而下分裂两种方法

分层聚类可能比k均值有一些好处，例如不必预先指定聚类的数量，以及它可以产生很好的聚类的分层图示（这对于较小的数据集很有用）。但是，从实际的角度来看，层次聚类分析仍然涉及许多决策，这些决策可能会对结果的解释产生重大影响。

1. 首先，像k均值一样，您仍然需要对要使用的相异性度量进行决策。 make a decision on the dissimilarity measure to use.
2. 其次，需要确定链接方法。每种链接方法在对观察结果进行分组的方式上都有不同的系统趋势（或偏差），并且可能导致明显不同的结果。\
   Each linkage method has different systematic tendencies (or biases) in the way it groups observations and can result in significantly different results.\
   例如，质心法倾向于产生不规则形状的簇。 Ward的方法往往会产生具有大致相同数量观察值的聚类，并且其提供的解决方案往往会被异常值严重扭曲。鉴于这种趋势，所选算法与数据的基础结构之间应该存在匹配（例如样本大小，观察值分布以及包括哪些类型的变量-标称，有序，比率或区间）。
3. 第三，尽管我们不需要预先指定簇的数量，但是我们经常仍然需要决定在哪里切割树状图以获得最终要使用的簇。\
   not need to pre-specify the number of clusters, we often still need to decide where to cut the dendrogram in order to obtain the final clusters to use.

### Hierarchical clustering algorithms

Hierarchical clustering can be divided into two main types:

1. **Agglomerative clustering:** Commonly referred to as AGNES (AGglomerative NESting) works in a bottom-up manner. That is, each observation is initially considered as a single-element cluster (leaf). At each step of the algorithm, the two clusters that are the most similar are combined into a new bigger cluster (nodes). This procedure is iterated until all points are a member of just one single big cluster (root). The result is a tree that can be displayed using a dendrogram.
2. **Divisive hierarchical clustering:** Commonly referred to as DIANA (DIvise ANAlysis) works in a top-down manner. DIANA is like the reverse of AGNES. It begins with the root, in which all observations are included in a single cluster. At each step of the algorithm, the current cluster is split into two clusters that are considered most heterogeneous. The process is iterated until all observations are in their own cluster.

> 聚集群集：通常称为AGNES（聚集嵌套）以自下而上的方式工作。 也就是说，每个观察值最初都被视为一个单元素簇（叶 leaf）。 在算法的每个步骤中，最相似的两个群集将合并为一个新的较大群集（节点 nodes）。 重复此过程，直到所有点仅是一个大群集（根 root）的成员为止。其结果是其可以使用树状显示的树。 
>
> 分裂式层次聚类：通常称为DIANA（DIvise ANAlysis）以自上而下的方式工作。 DIANA就像AGNES的反面。 它从根开始，其中所有观测值都包含在一个群集中。 在算法的每个步骤中，当前群集都被分为两个群集，这两个群集被认为是最异构的。 重复此过程，直到所有观测值都在其自己的群集中。

###  Measure the dissimilarity between two clusters of observations

与k均值类似，使用距离量度（例如，欧几里得距离，曼哈顿距离等）来测量观测值的（非相似性）;欧几里得距离是最常用的默认。但是，层次聚类中的一个基本问题是：我们如何测量两个观察聚类之间的差异？已经开发出许多不同的簇集方法 **cluster agglomeration methods**（即，链接方法 **Linkage**）来回答这个问题。

* **Maximum or complete linkage clustering:** Computes all pairwise dissimilarities between the elements in cluster 1 and the elements in cluster 2, and considers the largest value of these dissimilarities as the distance between the two clusters. It tends to produce more **compact **clusters.
* **Minimum or single linkage clustering:** Computes all pairwise dissimilarities between the elements in cluster 1 and the elements in cluster 2, and considers the smallest of these dissimilarities as a linkage criterion. It tends to produce long, “**loose**” clusters.
* **Mean or average linkage clustering:** Computes all pairwise dissimilarities between the elements in cluster 1 and the elements in cluster 2, and considers the average of these dissimilarities as the distance between the two clusters. Can vary in the compactness of the clusters it creates.
* **Centroid linkage clustering:** Computes the dissimilarity between the centroid for cluster 1 (a mean vector of length p, one element for each variable) and the centroid for cluster 2.
* **Ward’s minimum variance method:** Minimizes the total within-cluster variance. At each step the pair of clusters with the smallest between-cluster distance are merged. Tends to produce more compact clusters.

> 最大或完整的链接聚类：计算聚类1中的元素与聚类2中的元素之间的所有成对相异性，并将这些相异性的最大值视为两个聚类之间的距离。它倾向于产生更紧凑的簇。 
>
> 最小或单个链接聚类：计算群集1中的元素与群集2中的元素之间的所有成对差异，并将这些差异中的最小值视为链接标准。它倾向于产生长的“松散”簇。 
>
> 均值或平均连锁聚类：计算聚类1中的元素与聚类2中的元素之间的所有成对差异，并将这些差异的平均值视为两个聚类之间的距离。可以改变它创建的集群的紧凑性。 
>
> 质心连锁聚类：计算聚类1的质心和聚类2的质心之间的差异（长度的平均向量 p ，每个变量一个元素。 
>
> Ward的最小方差方法：最小化集群内的总方差。在每个步骤中，将群集间距离最小的一对群集合并。倾向于生产更紧凑的簇。

### **Agglomerative clustering**

1. 首先，所有观测都是自己本身的一 个簇；
2. 然后，算法开始在所有的**两点组合之中进行迭代搜索，找出最相似的两个 簇**，将它们聚集成一个簇。所以，第一次迭代之后，有n 1个簇；第二次迭代 之后，有n 2个簇，依此类推
3. 进行迭代时，除了距离测量之外，还有一个重要问题是需要确定观测组之间的测距方式，不 同类型的数据集需要使用不同的簇间测距方式。常用的测距方式类型:
   1. Ward距离, 使总的簇内方差最小，使用簇中的点到质心的误差平方和作为测量方式
   2. 最大距离（Complete linkage), 两个簇之间的距离就是两个簇中的观测之间的最大距离
   3. 质心距离（Centroid linkage), 两个簇之间的距离就是两个簇的质心之间的距离

> dist()函数计算距离: 默认方式欧氏距离, 可以在这个函数中指定其他距离计算方式（如最大值距离、 曼哈顿距离、堪培拉距离、二值距离和闵可夫斯基距离）。
>
> 需要注意的是，要对你的数据进行标准化的缩放操作，使数据的均值为0， 标准差为1，这样在计算距离时才能进行比较。否则，变量的测量值越大，对距 离的影响就越大。



## Application

* stats::hclust() and cluster::agnes() for agglomerative hierarchical clustering (HC)
* cluster::diana() for divisive HC.

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

### Cluster Analysis

要在R中建立层次聚类模型，可以使用stats包中的hclust()函数。这个函数需要两个基本输 入:距离矩阵和聚类方法。使用dist()函数可以轻松生成距离矩阵，我们使用的是欧氏距离。 可以使用的聚类方法有若干种，hclust()函数使用的默认方法是最大距离法

30种不同的聚类有效性指标。表现最好的前5种指标是**CH指数、Duda指数、Cindex、Gamma 和Beale指数**。另外一种确定簇数目的著名方法是**gap统计量. **在R中，我们可以使用NbClust包中的NbClust()函数，求出23种聚类有效性指标的结果，包 括Miligan和Cooper论文中最好的5种和gap统计量

```
## 使用NbClust()时，需要指定簇的最小值和最大值、距离类型、测距方式和有效性指标。
## 函数指定使用欧氏距离，簇的最小数量为2，最大数量为6，测距方式为 最大距离法，并使用所有有效性指标。
numComplete <- NbClust(df, distance = "euclidean", 
                       min.nc = 2, max.nc = 6, 
                       method = "complete", index = "all")
```

![](https://firebasestorage.googleapis.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-MRt1rIudcHYDxTcux_Q%2Fuploads%2FLmkHOKW6tsBDw7pUasml%2Ffile.png?alt=media)

```
## The Hubert index is a graphical method of determining the number of clusters.
## In the plot of Hubert index, we seek a significant knee that corresponds to a 
## ignificant increase of the value of the measure i.e the significant peak in Hubert
## index second differences plot. 

Hubert指数图: 在左侧的图中，你要找出一 个明显的拐点;在右侧的图中，你要找到峰值.
左图在3个簇的地方有个拐点，右图在3个簇的时候达到峰值
Dindex图:提供了同样的信息
```

![](https://firebasestorage.googleapis.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-MRt1rIudcHYDxTcux_Q%2Fuploads%2FUzVcTd1tyjOGYEDilRf2%2Ffile.png?alt=media)

```
## The D index is a graphical method of determining the number of clusters. 
##                 In the plot of D index, we seek a significant knee (the significant peak in Dindex
##                 second differences plot) that corresponds to a significant increase of the value of
##                 the measure. 
##  
## ******************************************************************* 
## * Among all indices:                                                
## * 1 proposed 2 as the best number of clusters 
## * 11 proposed 3 as the best number of clusters 
## * 6 proposed 5 as the best number of clusters 
## * 5 proposed 6 as the best number of clusters 
## 
##                    ***** Conclusion *****                            
##  
## * According to the majority rule, the best number of clusters is  3 
```

```
```

```
## 每种有效性指标的最优簇数 量和与之对应的指标值
## 第一个指标(KL)的最优簇数量是5，第二个指标(CH)的最优簇数量是3
numComplete$Best.nc

##                      KL      CH Hartigan   CCC    Scott      Marriot   TrCovW
## Number_clusters  5.0000  3.0000   3.0000 5.000   3.0000 3.000000e+00     3.00
## Value_Index     14.2227 48.9898  27.8971 1.148 340.9634 6.872632e+25 22389.83
##                   TraceW Friedman   Rubin Cindex     DB Silhouette   Duda
## Number_clusters   3.0000   3.0000  5.0000 3.0000 6.0000     3.0000 5.0000
## Value_Index     256.4861  10.6941 -0.1489 0.3551 1.6018     0.2038 0.8856
##                 PseudoT2  Beale Ratkowsky     Ball PtBiserial Frey McClain
## Number_clusters   5.0000 5.0000    3.0000   3.0000     6.0000    1  2.0000
## Value_Index       6.3314 1.1253    0.3318 462.0304     0.5877   NA  0.7687
##                   Dunn Hubert SDindex Dindex   SDbw
## Number_clusters 6.0000      0  6.0000      0 6.0000
## Value_Index     0.1892      0  0.9951      0 0.5031
```

```
## 选择3个簇进行聚类，现在计算距离矩阵，并建立层次聚类模型
dis <- dist(df, method = "euclidean")
hc <- hclust(dis, method = "complete")

## 可视化的通用方式是画出树状图，可以用plot函数实现。
## 注意，参数hang=-1表示 将观测排列在图的底部
## 树状图表明了观测是如何聚集的，图中的连接(也可称为分支)告诉我们哪些观测是相似的。
## 分支的高度表示观测之间相似或相异的程度
plot(hc, hang = -1,labels = FALSE, main = "Complete-Linkage")

## 想使聚类可视化效果更好，可以使用sparcl包生成彩色树状图。要对合适数目的簇上色， 
## 需要使用cutree()函数对树状图进行剪枝，以得到合适的簇的数目。
## 这个函数还可以为每个观测生成簇标号:
comp3 <- cutree(hc, 3)
ColorDendrogram(hc, y = comp3, main = "Complete", branchlength = 50)
```

![](https://firebasestorage.googleapis.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-MRt1rIudcHYDxTcux_Q%2Fuploads%2Fo2ktzsQKMMnr9FmTgCFZ%2Ffile.png?alt=media)

```
## Ward距离法。代码和前面的一样，首先确定簇的数目，应该将method的值改为 Ward.D2
numWard <- NbClust(df, diss = NULL, distance = "euclidean", 
        min.nc = 2, 
        max.nc = 6, 
        method= "ward.D2", 
        index = "all")
## * According to the majority rule, the best number of clusters is  3         
```

```
## *** : The D index is a graphical method of determining the number of clusters. 
```

```
## 并建立层次聚类模型
hcWard <- hclust(dis, method = "ward.D2")
plot(hcWard, hang = -1, labels = FALSE, main = "Ward's-Linkage")

## 图中显示的3个簇区别十分明显，每个簇中观测的数量大致相同。
## 计算每个簇的大小，并与品种等级标号进行比较
ward3 <- cutree(hcWard, 3)
table(ward3, wine$Class)  
```
