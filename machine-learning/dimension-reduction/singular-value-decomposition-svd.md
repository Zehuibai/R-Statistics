---
description: >-
  奇异值分解(Singular Value Decomposition，SVD)
  https://www.cnblogs.com/pinard/p/6251584.html
---

# Singular Value Decomposition (SVD)

## Eigenvalues and eigenvectors

特征值和特征向量> 几乎所有的向量在乘以矩阵后都会改变方向，某些特殊的向量$$x$$和$$Ax$$位于同一个方向，它们称之为特征向量。

$$
Ax=\lambda x
$$

数字$$\lambda$$称为**特征值**。它告诉我们在乘以![\[公式\]](https://www.zhihu.com/equation?tex=A)后，向量是怎么被拉伸、缩小、反转或者不变的。

* ![\[公式\]](https://www.zhihu.com/equation?tex=%5Clambda+%3D+0)意味着特征向量存在于矩阵的零空间中。
* 任意向量都是单位矩阵的特征向量，因为![\[公式\]](https://www.zhihu.com/equation?tex=Ix%3Dx)，其特征值为 1。
* 要计算特征值的话，我们只需要知道![\[公式\]](https://www.zhihu.com/equation?tex=det+%28A-%5Clambda+I%29%3D0)即可。

求出特征值和特征向量有什么好处呢？ 就是我们可以将矩阵A特征分解。如果我们求出了矩阵A的n个特征值 $$\lambda_1 \leq \lambda_2 \leq ... \leq \lambda_n$$ ,以及这n个特征值所对应的特征向量$$\{w_1,w_2,...w_n\}$$，如果这n个特征向量线性无关，那么矩阵A就可以用下式的特征分解表示：

$$
A=W\Sigma W^{-1}
$$

其中$$W$$是这 $$n$$ 个特征向量所张成的 $$n \times n$$ 维矩阵, 而 $$\Sigma$$ 为这$$n$$个特征值为主对角线的 $$n \times n$$ 维矩阵。一般会把$$W$$的这$$n$$个特征向量标准化, 即满足 ||$$w_{i} \|_{2}=1,$$ 或者说 $$w_{i}^{T} w_{i}=1,$$ 此时$$W$$的$$n$$个特征向量为标准正交基, 满足$$W^{T} W=I,$$ 即$$W^{T}=W^{-1},$$ 这样我们的特征分解表达式可以写成

$$
A=W \Sigma W^{T}
$$

注意: **要进行特征分解, 矩阵**$$A$$**必须为方阵。那么如果**$$A$$**不是方阵, 即行和列不相同时, 我们还可以对矩阵进行分解:SVD。**

## SVD

### Derivation

SVD也是对矩阵进行分解, 但是和特征分解不同, SVD并不要求要分解的矩阵为方阵。假设我们的矩阵A是一个 $$m \times n$$ 的矩阵，那么我们定义矩阵$$A$$的 SVD为

$$
A=U \Sigma V^{T}
$$

其中$$U$$是一个 $$m \times m$$ 的矩阵， $$\Sigma$$ 是一个 $$m \times n$$ 的矩阵, 除了主对角线上的元素以外全为0，主对角线上的每个元素都称为奇异值, $$V$$ 是一个 $$n \times n$$ 的矩 阵。$$U$$和$$V$$满足$$U^{T} U=I, V^{T} V=I_{\circ}$$

![](https://images2015.cnblogs.com/blog/1042406/201701/1042406-20170105115457425-1545975626.png)

#### 求SVD分解后的 $$U, \Sigma, V$$&#x20;

如果我们将A的转置和A做矩阵乘法, 那么会得到 $$n \times n$$ 的一个方阵 $$A^{T} A_{\circ}$$ 既然 $$A^{T} A$$ 是方阵, 那么我们就可以进行特征分解, 得到的特征值和特征向量满足下式:这样我们就可以得到矩阵 $$A^{T} A$$ 的$$n$$个特征值和对应的$$n$$个特征向量 $$v$$ 了 。 将 $$A^{T} A$$ 的所有特征向量张成一个 $$n \times n$$ 的矩阵$$V$$，就是我们SVD公式里面的$$V$$矩 阵了。一般我们将$$V$$中的每个特征向量叫做$$A$$的右奇异向量。

$$
\left(A^{T} A\right) v_{i}=\lambda_{i} v_{i}
$$

如果我们将A和A的转置做矩阵乘法, 那么会得到 $$m \times m$$ 的一个方阵 $$A A^{T}$$ 。既然 $$A A^{T}$$ 是方阵，那么我们就可以进行特征分解, 得到的特征值和特征向 量满足下式：这样我们就可以得到矩阵 $$A A^{T}$$ 的$$m$$个特征值和对应的m个特征向量 $$u$$ 了 。 将 $$A A^{T}$$ 的所有特征向量张成一个$$m \times m$$ 的矩阵$$U$$，一般我们将$$U$$中的每个特征向量叫做$$A$$的左奇异向量。

$$
\left(A A^{T}\right) u_{i}=\lambda_{i} u_{i}
$$

由于 $$\Sigma$$ 除了对角线上是奇异值其他位置都是0，那我们只需要求出每个奇异值 $$\sigma$$ 就可以了。 我们注意到:

$$
A=U \Sigma V^{T} \Rightarrow A V=U \Sigma V^{T} V \Rightarrow A V=U \Sigma \Rightarrow A v_{i}=\sigma_{i} u_{i} \Rightarrow \sigma_{i}=A v_{i} / u_{i}
$$

这样我们可以求出我们的每个奇异值, 进而求出奇异值矩阵 $$\Sigma_{\circ}$$

#### Why $$A^{T} A$$ 的特征向量组成的就是我们SVD中的$$V$$矩阵, 而 $$A A^{T}$$ 的特征向量组成的就是我们SVD中的$$U$$矩阵？

$$V$$矩阵的证明:

$$
A=U \Sigma V^{T} \Rightarrow A^{T}=V \Sigma^{T} U^{T} \Rightarrow A^{T} A=V \Sigma^{T} U^{T} U \Sigma V^{T}=V \Sigma^{2} V^{T}
$$

上式证明使用了: $$U^{T} U=I, \Sigma^{T} \Sigma=\Sigma^{2} 。$$ 可以看出 $$A^{T} A$$ 的特征向量组成的的确就是我们SVD中的V矩阵。 进一步我们还可以看出我们的特征值矩阵等于奇异值矩阵的平方, 也就是说特征值和奇异值满足如下关系：

$$
\sigma_{i}=\sqrt{\lambda_{i}}
$$

这样也就是说, 我们可以不用 $$\sigma_{i}=A v_{i} / u_{i}$$ 来计算奇异值, 也可以通过求出 $$A^{T} A$$ 的特征值取平方根来求奇异值。

### Calculation

矩阵A定义为：

$$
\mathbf{A} = \left( \begin{array}{ccc} 0& 1\\  1& 1\\   1& 0 \end{array} \right)
$$

Then

$$
\mathbf{A^TA} = \left( \begin{array}{ccc} 0& 1 &1\\ 1&1& 0 \end{array} \right) \left( \begin{array}{ccc} 0& 1\\  1& 1\\   1& 0 \end{array} \right) = \left( \begin{array}{ccc} 2& 1 \\ 1& 2 \end{array} \right)
$$

$$
\mathbf{AA^T} =  \left( \begin{array}{ccc} 0& 1\\  1& 1\\   1& 0 \end{array} \right) \left( \begin{array}{ccc} 0& 1 &1\\ 1&1& 0 \end{array} \right) = \left( \begin{array}{ccc} 1& 1 & 0\\ 1& 2 & 1\\ 0& 1& 1 \end{array} \right)
$$

进而求出$$A^TA$$的特征值和特征向量

$$
\lambda_1= 3; v_1 = \left( \begin{array}{ccc} 1/\sqrt{2} \\ 1/\sqrt{2} \end{array} \right); \lambda_2= 1; v_2 = \left( \begin{array}{ccc} -1/\sqrt{2} \\ 1/\sqrt{2} \end{array} \right)
$$

进而求出​$$AA^T$$的特征值和特征向量

$$
\lambda_1= 3; u_1 = \left( \begin{array}{ccc} 1/\sqrt{6} \\ 2/\sqrt{6} \\ 1/\sqrt{6} \end{array} \right); \lambda_2= 1; u_2 = \left( \begin{array}{ccc} 1/\sqrt{2} \\ 0 \\ -1/\sqrt{2} \end{array} \right);  \lambda_3= 0; u_3 = \left( \begin{array}{ccc} 1/\sqrt{3} \\ -1/\sqrt{3} \\ 1/\sqrt{3} \end{array} \right)
$$

**Alternative 1:** 利用 $$Av_i = \sigma_i u_i, \ \ i=1,2$$ 求奇异值：

$$
\left( \begin{array}{ccc} 0& 1\\  1& 1\\   1& 0 \end{array} \right) \left( \begin{array}{ccc} 1/\sqrt{2} \\ 1/\sqrt{2} \end{array} \right) = \sigma_1 \left( \begin{array}{ccc} 1/\sqrt{6} \\ 2/\sqrt{6} \\ 1/\sqrt{6} \end{array} \right) \Rightarrow  \sigma_1=\sqrt{3}
$$

$$
\left( \begin{array}{ccc} 0& 1\\  1& 1\\   1& 0 \end{array} \right) \left( \begin{array}{ccc} -1/\sqrt{2} \\ 1/\sqrt{2} \end{array} \right) = \sigma_2 \left( \begin{array}{ccc} 1/\sqrt{2} \\ 0 \\ -1/\sqrt{2} \end{array} \right) \Rightarrow  \sigma_2=1
$$

**Alternative 2:** $$\sigma_i = \sqrt{\lambda_i}$$ 直接求奇异值are $$\sqrt{3},1$$

最终得到A的奇异值分解为：

$$
A=U\Sigma V^T = \left( \begin{array}{ccc} 1/\sqrt{6} & 1/\sqrt{2} & 1/\sqrt{3} \\ 2/\sqrt{6} & 0 & -1/\sqrt{3}\\ 1/\sqrt{6} & -1/\sqrt{2} & 1/\sqrt{3} \end{array} \right) \left( \begin{array}{ccc} \sqrt{3} & 0 \\  0 & 1\\ 0 & 0 \end{array} \right) \left( \begin{array}{ccc} 1/\sqrt{2}  & 1/\sqrt{2}  \\ -1/\sqrt{2}  & 1/\sqrt{2}  \end{array} \right)
$$

### Usage

在主成分分析（PCA）原理总结中，我们讲到要用PCA降维，需要找到样本协方差矩阵$$X^TX$$的最大的$$d$$个特征向量，然后用这最大的$$d$$个特征向量张成的矩阵来做低维投影降维。可以看出，在这个过程中需要先求出协方差矩阵$$X^TX$$，当样本数多样本特征数也多的时候，这个计算量是很大的。

SVD也可以得到协方差矩阵$$X^TX$$最大的$$d$$个特征向量张成的矩阵，但是SVD有个好处，有一些SVD的实现算法可以不求先求出协方差矩阵$$X^TX$$，也能求出我们的右奇异矩阵$$V$$。也就是说，我们的PCA算法可以不用做特征分解，而是做SVD来完成。这个方法在样本量很大的时候很有效。

## 矩阵向量求导

{% embed url="https://www.cnblogs.com/pinard/p/10750718.html" %}



