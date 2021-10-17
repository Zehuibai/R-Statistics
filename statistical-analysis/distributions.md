# Distributions

## Gamma distribution

**Gamma分布即为多个独立且相同分布（iid）的指数分布变量的和的分布。**

在概率论和统计学中，伽马分布是连续概率分布的两参数族。 指数分布，Erlang分布和卡方分布是伽马分布的特例。 

#### Characterization using shape α and rate β

$$
{\displaystyle X\sim \Gamma (\alpha ,\beta )\equiv \operatorname {Gamma} (\alpha ,\beta )}
$$

$$
{\displaystyle {\begin{aligned}f(x;\alpha ,\beta )&={\frac {\beta ^{\alpha }x^{\alpha -1}e^{-\beta x}}{\Gamma (\alpha )}}\quad {\text{ for }}x>0\quad \alpha ,\beta >0,\\[6pt]\end{aligned}}}
$$

* If X \~ Gamma(1, 1/λ) (shape–scale parametrization), then X has an exponential distribution with rate parameter λ.
* If X \~ Gamma(ν/2, 2)(shape–scale parametrization), then X is identical to χ2(ν), the chi-squared distribution with ν degrees of freedom. 

**Gamma的重要性质包括下面几条： **

1. 递推公式: $$\Gamma(x+1)=x \Gamma(x)$$ 
2. 对于正整数n, 有 $$\Gamma(n+1)=n !$$ 因此可以说Gamma函数是阶乘的推广
3. $$\Gamma(1)=1$$ 
4. $$\Gamma\left(\frac{1}{2}\right)=\sqrt{\pi}$$

**Gamma分布 & 指数分布**

从意义来看：指数分布解决的问题是“要等到一个随机事件发生，需要经历多久时间”，伽玛分布解决的问题是“要等到n个随机事件都发生，需要经历多久时间”。

从公式来看: $$\mathrm{X} \sim \operatorname{Gamma}(\alpha, \lambda),$$ 概率公式如下

$$
f_{X}(x)=\left\{\begin{array}{ll}
\frac{\lambda^{a} x^{a-1} e^{-\lambda x}}{\Gamma(\alpha)} & x>0 \\
0 & \text { otherwise }
\end{array}\right.
$$

$$\alpha$$代表上述的n, $$\alpha$$=1时，就变成了指数分布:

$$
f_{X}(x)=\left\{\begin{array}{ll}
\lambda e^{-\lambda x} & x>0 \\
0 & \text { otherwise }
\end{array}\right.
$$

从统计指标来看.

$$
E X=\frac{\alpha}{\lambda}, \quad \operatorname{Var}(X)=\frac{\alpha}{\lambda^{2}}
$$

这就是 $$\mathrm{n}(\alpha)$$ 倍的指数分布的期望

## Weibull Distribution

韦氏分布或威布尔分布，是可靠性分析和寿命检验的理论基础。威布尔分布在可靠性工程中被广泛应用，尤其适用于机电类产品的磨损累计失效的分布形式。由于它可以利用概率值很容易地推断出它的分布参数，被广泛应用于各种寿命试验的数据处理。

$$
{\displaystyle f(x;\lambda ,k)={\begin{cases}{\frac {k}{\lambda }}\left({\frac {x}{\lambda }}\right)^{k-1}\mathrm {e} ^{-(x/\lambda )^{k}}&x\geq 0,\\0&x<0,\end{cases}}}
$$

* λ>0是比例参数 scale parameter
* k>0是形状参数 shape parameter

它的累积分布函数是扩展的指数分布函数，而且，Weibull distribution与很多分布都有关系。

* k=1，它是指数分布；
* k=2且时，是Rayleigh distribution（[瑞利分布](https://baike.baidu.com/item/%E7%91%9E%E5%88%A9%E5%88%86%E5%B8%83)）。

The **moment generating function** of the logarithm of a Weibull distributed random variable is given by

$$
{\displaystyle \operatorname {E} \left[e^{t\log X}\right]=\lambda ^{t}\Gamma \left({\frac {t}{k}}+1\right)}{\displaystyle \operatorname {E} \left[e^{t\log X}\right]=\lambda ^{t}\Gamma \left({\frac {t}{k}}+1\right)}
$$

The** mean and variance** of a Weibull random variable can be expressed as

$$
{\displaystyle \operatorname {E} (X)=\lambda \Gamma \left(1+{\frac {1}{k}}\right)\,}
$$

$$
{\displaystyle \operatorname {var} (X)=\lambda ^{2}\left[\Gamma \left(1+{\frac {2}{k}}\right)-\left(\Gamma \left(1+{\frac {1}{k}}\right)\right)^{2}\right]\,.}
$$



## Beta Distribution

{% embed url="https://zhuanlan.zhihu.com/p/69606875" %}

Beta分布是一种**连续型概率密度分布**，表示为 ![\[公式\]](https://www.zhihu.com/equation?tex=x+%5Csim+Beta%28a%2Cb%29) ，由两个参数 ![\[公式\]](https://www.zhihu.com/equation?tex=a%2Cb) 决定，称为形状参数。由于其定义域为(0,1)，一般被用于建模**伯努利试验事件成功的概率**的概率分布：

> 对于硬币或者骰子这样的简单实验，我们事先能很准确地掌握系统成功的概率。然而通常情况下，系统成功的概率是未知的，但是根据频率学派的观点，我们可以通过频率来估计概率。
>
> 为了测试系统的成功概率，我们做n次试验，统计成功的次数k，于是很直观地就可以计算出。然而由于系统成功的概率是未知的，这个公式计算出的只是系统成功概率的最佳估计。也就是说实际上也可能为其它的值，只是为其它的值的概率较小。因此我们并不能完全确定硬币出现正面的概率就是该值，所以也是一个随机变量，它符合Beta分布，其取值范围为0到1

* 先验概率就是事情尚未发生前，我们对该事发生概率的估计。利用过去历史资料计算得到的先验概率，称为客观先验概率； 当历史资料无从取得或资料不完全时，凭人们的主观经验来判断而得到的先验概率，称为主观先验概率。例如抛一枚硬币头向上的概率为0.5，这就是主观先验概率。
* 后验概率是指通过调查或其它方式获取新的附加信息，利用贝叶斯公式对先验概率进行修正，而后得到的概率。
* 先验概率和后验概率的区别：先验概率不是根据有关自然状态的全部资料测定的，而只是利用现有的材料(主要是历史资料)计算的；后验概率使用了有关自然状态更加全面的资料，既有先验概率资料，也有补充资料。另外一种表述：先验概率是在缺乏某个事实的情况下描述一个变量；而后验概率（Probability of outcomes of an experiment after it has been performed and a certain event has occured.）是在考虑了一个事实之后的条件概率。

### **Beta分布及其函数公式推导**

如果随机变量 ![\[公式\]](https://www.zhihu.com/equation?tex=X) 服从参数为 ![\[公式\]](https://www.zhihu.com/equation?tex=n) 和 ![\[公式\]](https://www.zhihu.com/equation?tex=q) 的二项分布，那么它的概率由概率质量函数（对于连续随机变量，则为概率密度函数）为：

![](https://www.zhihu.com/equation?tex=p%28x%29%3D%5Cbegin%7Bpmatrix%7Dn%5C%5Cx%5C%5C+%5Cend%7Bpmatrix%7Dq%5Ex%281-q%29%5E%7Bn-x%7D%5Ctag%7B1%7D)

把 ![\[公式\]](https://www.zhihu.com/equation?tex=%281%29) 表示为变量 ![\[公式\]](https://www.zhihu.com/equation?tex=q) 的函数，即只有 ![\[公式\]](https://www.zhihu.com/equation?tex=q) 这一个变量，写成如下形式

![](https://www.zhihu.com/equation?tex=f%28q%29%5Cvarpropto+q%5Ea%281-q%29%5Eb%5Ctag%7B2%7D)

其中 ![\[公式\]](https://www.zhihu.com/equation?tex=a) 和 ![\[公式\]](https://www.zhihu.com/equation?tex=b) 是常量， ![\[公式\]](https://www.zhihu.com/equation?tex=q%5Cin%280%2C1%29)

为了把 ![\[公式\]](https://www.zhihu.com/equation?tex=%282%29) 变成一个分布，可以给它乘上一个因子，使它对 ![\[公式\]](https://www.zhihu.com/equation?tex=q) 从0到1积分为1即可，即

![](https://www.zhihu.com/equation?tex=f%28q%29+%3D+kq%5Ea%281-q%29%5Eb%5Ctag%7B3%7D)

令其积分为1

![](https://www.zhihu.com/equation?tex=%5Cint\_0%5E1+f%28q%29%5Cmathbf+dq+%3D+%5Cint\_0%5E1+kq%5Ea%281-q%29%5Eb+%5Cmathbf+dq%3Dk%5Cint\_0%5E1+q%5Ea%281-q%29%5Eb+%5Cmathbf+dq%3D1+%5Ctag%7B4%7D)

则

![](https://www.zhihu.com/equation?tex=k%3D%5Cfrac%7B1%7D%7B%5Cint\_0%5E1+q%5Ea%281-q%29%5Eb+%5Cmathbf+dq%7D)

记 ![\[公式\]](https://www.zhihu.com/equation?tex=B%28a%2B1%2Cb%2B1%29%3D%5Cint\_0%5E1+q%5Ea%281-q%29%5Eb+%5Cmathbf+dq) ，则 ![\[公式\]](https://www.zhihu.com/equation?tex=k%3DB%28a%2B1%2Cb%2B1%29%5E%7B-1%7D) ，所以

那么规范化后的 (2) 就是一个分布了

![](https://www.zhihu.com/equation?tex=f%28q%3Ba%2B1%2Cb%2B1%29+%3D+%5Cfrac%7B1%7D%7BB%28a%2B1%2Cb%2B1%29%7Dq%5Ea%281-q%29%5Eb%5Ctag%7B5%7D)

这就是Beta分布的最原始的来源

对（5）进行适当的改造：取 ![\[公式\]](https://www.zhihu.com/equation?tex=%5Calpha%3Da%2B1%2C%5Cbeta%3Db%2B1) ，并将积分 ![\[公式\]](https://www.zhihu.com/equation?tex=B%28a%2B1%2Cb%2B1%29%3D%5Cint\_0%5E1+q%5Ea%281-q%29%5Eb+%5Cmathbf+dq) 中的 ![\[公式\]](https://www.zhihu.com/equation?tex=q) 改为 ![\[公式\]](https://www.zhihu.com/equation?tex=t) ，我们就得到了我们在教材上看到的Beta函数了：

![](https://www.zhihu.com/equation?tex=B%28%5Calpha%2C%5Cbeta%29%3D%5Cint\_0%5E1+t%5E%7B%5Calpha-1%7D%281-t%29%5E%7B%5Cbeta-1%7D+%5Cmathbf+dt%5Ctag%7B6%7D)

另外，将（5）中的q改为x，则我们就得到了我们在教材上看到的Beta分布的函数：

![](https://www.zhihu.com/equation?tex=f%28x%3B%5Calpha%2C%5Cbeta%29+%3D+%5Cfrac%7B1%7D%7BB%28%5Calpha%2C%5Cbeta%29%7Dx%5E%7B%5Calpha-1%7D%281-x%29%5E%7B%5Cbeta-1%7D%5Ctag%7B7%7D)

到这里我们已经完整地推出了Beta函数（公式(6)）和Beta分布（公式(7)）

### **Beta 函数和 Gamma 函数 **

假设向长度为1的桌子上扔一个红球，它会落在0到1这个范围内，设这个长度值为 ![\[公式\]](https://www.zhihu.com/equation?tex=x) ，再向桌上扔一个白球，那么这个白球落在红球左边的概率即为 ![\[公式\]](https://www.zhihu.com/equation?tex=x) 。 若一共扔了 ![\[公式\]](https://www.zhihu.com/equation?tex=n) 次白球，其中每一次都是相互独立的，假设落在红球左边的白球数量为 ![\[公式\]](https://www.zhihu.com/equation?tex=k) ，那么随机变量 ![\[公式\]](https://www.zhihu.com/equation?tex=K) 服从参数为 ![\[公式\]](https://www.zhihu.com/equation?tex=n) 和 ![\[公式\]](https://www.zhihu.com/equation?tex=x) 的二项分布，即 ![\[公式\]](https://www.zhihu.com/equation?tex=K%E2%88%BCBinomial%28n%2Cx%29) ，有

![](https://www.zhihu.com/equation?tex=P%28K%3Dk%7Cx%29%3D%5Cbegin%7Bpmatrix%7Dn%5C%5Ck%5C%5C+%5Cend%7Bpmatrix%7Dx%5Ek%281-x%29%5E%7Bn-k%7D%5Ctag%7B1%7D)

![\[公式\]](https://www.zhihu.com/equation?tex=X) 服从 ![\[公式\]](https://www.zhihu.com/equation?tex=%5B0%2C1%5D) 上的均匀分布，即 ![\[公式\]](https://www.zhihu.com/equation?tex=X%E2%88%BCU%5B0%2C1%5D)

![\[公式\]](https://www.zhihu.com/equation?tex=K) 对每一个 ![\[公式\]](https://www.zhihu.com/equation?tex=x) 都有上面的分布，对于所有可能的 ![\[公式\]](https://www.zhihu.com/equation?tex=x) ， ![\[公式\]](https://www.zhihu.com/equation?tex=K) 的分布为

![](https://www.zhihu.com/equation?tex=P%28K%3Dk%29%3D%5Cint\_0%5E1+%5Cbegin%7Bpmatrix%7Dn%5C%5Ck%5C%5C+%5Cend%7Bpmatrix%7Dx%5Ek%281-x%29%5E%7Bn-k%7D%5Cmathbf+dx+%3D%5Cbegin%7Bpmatrix%7Dn%5C%5Ck%5C%5C+%5Cend%7Bpmatrix%7D%5Cint\_0%5E1+x%5Ek%281-x%29%5E%7Bn-k%7D%5Cmathbf+dx%5Ctag%7B2%7D)

换一种方式来丢球： 先将这 ![\[公式\]](https://www.zhihu.com/equation?tex=n%2B1) 个球都丢出来，再选择一个球作为红球，任何一个球被选中的概率均为 ![\[公式\]](https://www.zhihu.com/equation?tex=%5Cdisplaystyle+1+%5Cover+n%2B1) ，此时红球左边有 ![\[公式\]](https://www.zhihu.com/equation?tex=0%2C1%2C2...n) 个球的概率均为 ![\[公式\]](https://www.zhihu.com/equation?tex=%5Cdisplaystyle+1+%5Cover+n%2B1) ，有

![](https://www.zhihu.com/equation?tex=P%28K%3Dk%29%3D%5Cint\_0%5E1+%5Cbegin%7Bpmatrix%7Dn%5C%5Ck%5C%5C+%5Cend%7Bpmatrix%7Dx%5Ek%281-x%29%5E%7Bn-k%7D%5Cmathbf+dx+%3D%5Cbegin%7Bpmatrix%7Dn%5C%5Ck%5C%5C+%5Cend%7Bpmatrix%7D%5Cint\_0%5E1+x%5Ek%281-x%29%5E%7Bn-k%7D%5Cmathbf+dx%3D%5Cfrac%7B1%7D%7Bn%2B1%7D)

 ![\[公式\]](https://www.zhihu.com/equation?tex=%5CGamma) 函数的定义：

![\[公式\]](https://www.zhihu.com/equation?tex=%5CGamma%28m%29+%3D+%5Cint\_0%5E%7B%2B%5Cinfty%7D+e%5E%7B-x%7D+x%5E%7Bm-1%7D+%5Cmathbf+dx%3D%28m-1%29%21%5Ctag%7B4%7D)

（4）在定义域为整数时，即 ![\[公式\]](https://www.zhihu.com/equation?tex=m+%5Cin+Z) 时，才满足等于 ![\[公式\]](https://www.zhihu.com/equation?tex=%28m-1%29%21)

现在我们就可以推导出 ![\[公式\]](https://www.zhihu.com/equation?tex=%5CGamma) 函数与Beta函数的关系了：

由于

![](https://www.zhihu.com/equation?tex=B%28%5Calpha%2C%5Cbeta%29%3D%5Cint\_0%5E1+t%5E%7B%5Calpha-1%7D%281-t%29%5E%7B%5Cbeta-1%7D+%5Cmathbf+dt)

根据(3)，可令 ![\[公式\]](https://www.zhihu.com/equation?tex=k%3D%5Calpha-1%2Cn-k%3D%5Cbeta-1%5Cquad+%5CRightarrow+%5Cquad+n%3Da%2Bb-2) ，则

![](https://www.zhihu.com/equation?tex=B%28%5Calpha%2C%5Cbeta%29%3D%5Cint\_0%5E1+t%5E%7B%5Calpha-1%7D%281-t%29%5E%7B%5Cbeta-1%7D+%5Cmathbf+dt%3D%5Cfrac%7B%28%5Calpha-1%29%21%28%5Cbeta-1%29%21%7D%7B%28%5Calpha%2B%5Cbeta-1%29%21%7D)

又由于(4)，可得

![](https://www.zhihu.com/equation?tex=B%28%5Calpha%2C%5Cbeta%29%3D%5Cfrac%7B%5CGamma%28%5Calpha%29%5CGamma%28%5Cbeta%29%7D%7B%5CGamma%28%5Calpha%2B%5Cbeta%29%7D)

因此，Beta分布也可以写成下面的形式：

![](https://www.zhihu.com/equation?tex=f%28x%3B%5Calpha%2C%5Cbeta%29+%3D+%5Cfrac%7B1%7D%7BB%28%5Calpha%2C%5Cbeta%29%7Dx%5E%7B%5Calpha-1%7D%281-x%29%5E%7B%5Cbeta-1%7D%3D%5Cfrac%7B%5CGamma%28%5Calpha%2B%5Cbeta%29%7D%7B%5CGamma%28%5Calpha%29%5CGamma%28%5Cbeta%29%7Dx%5E%7B%5Calpha-1%7D%281-x%29%5E%7B%5Cbeta-1%7D)

### **Beta 分布的期望与方差**

****

* Beta 分布的期望

![](https://www.zhihu.com/equation?tex=%5Cbegin%7Baligned%7D+++++%26%5Cquad+E%5BX%5D+%5C%5C+++++%26%3D+%5Cint\_0%5E1+x+f%28x%3B%5Calpha%2C%5Cbeta%29+%5C%5C+++++%26%3D+%5Cint\_0%5E1+x+%5Cfrac%7Bx%5E%7B%5Calpha-1%7D%281-x%29%5E%7B%5Cbeta-1%7D%7D+%7BB%28%5Calpha%2C%5Cbeta%29+%7D++%5Cmathbf+dx+%5C%5C+++++%26%3D+%5Cfrac%7B1%7D+%7BB%28%5Calpha%2C%5Cbeta%29%7D+%5Cint\_0%5E1+x%5E%7B%5Calpha%7D%281-x%29%5E%7B%5Cbeta-1%7D+%5Cmathbf+dx+%5C%5C+++++%26%3D+%5Cfrac%7BB%28%5Calpha%2B1%2C%5Cbeta%29%7D%7BB%28%5Calpha%2C%5Cbeta%29%7D+%5C%5C+++++%26%3D+%5Cfrac%7B%5CGamma%28%5Calpha%2B1%29%5CGamma%28%5Cbeta%29%7D%7B%5CGamma%28%5Calpha%2B%5Cbeta%2B1%29%7D%5Cfrac%7B%5CGamma%28%5Calpha%2B%5Cbeta%29%7D%7B%5CGamma%28%5Calpha%29%5CGamma%28%5Cbeta%29%7D+%5C%5C+++++%26%3D+%5Cfrac%7B%5Calpha%7D%7B%5Calpha%2B%5Cbeta%7D+++++%5Cend%7Baligned%7D)

* Beta 分布的方差

由于 ![\[公式\]](https://www.zhihu.com/equation?tex=Var%5BX%5D%3DE%28X%5E2%29-E%28X%29%5E2)

> ![\[公式\]](https://www.zhihu.com/equation?tex=%5Cbegin%7Baligned%7D%26%5Cquad+Var%5BX%5D+%5C%5C+%26%3DE%5Cleft+%5B%28X-E%5BX%5D%29%5E2+%5Cright+%5D%5C%5C+%26%3DE+%5Cleft+%5BX%5E2-2XE%28X%29%2BE%5BX%5D%5E2+%5Cright+%5D+%5C%5C+%26%3D+E%5BX%5E2%5D-2E%5BX%5DE%5BX%5D%2BE%5BX%5D%5E2+%5C%5C+%26%3D+E%28X%5E2%29-E%28X%29%5E2%5Cend%7Baligned%7D)

那么，先求 ![\[公式\]](https://www.zhihu.com/equation?tex=E%28X%5E2%29)

![](https://www.zhihu.com/equation?tex=%5Cbegin%7Baligned%7D+++++%26%5Cquad+E%28X%5E2%29+%5C%5C+++++%26%3D+%5Cint\_%7B-%5Cinfty%7D%5E%7B%5Cinfty%7D+x%5E2f%28x%29dx+%5C%5C+++++%26%3D+%5Cint\_0%5E1+x%5E2+%5Cfrac%7B1%7D%7BB%28%5Calpha%2C%5Cbeta%29%7Dx%5E%7B%5Calpha-1%7D%281-x%29%5E%7B%5Cbeta-1%7Ddx+%5C%5C+++++%26%3D+%5Cfrac%7B1%7D%7BB%28%5Calpha%2C%5Cbeta%29%7D+%5Cint\_0%5E1+x%5E%7B%5Calpha%2B1%7D%281-x%29%5E%7B%5Cbeta-1%7Ddx+%5C%5C+++++%26%3D+%5Cfrac%7BB%28%5Calpha%2B2%2C%5Cbeta%29%7D%7BB%28%5Calpha%2C%5Cbeta%29%7D+%5C%5C+++++%26%3D+%5Cfrac%7B%5CGamma%28%5Calpha%2B2%29%5CGamma%28%5Cbeta%29%7D%7B%5CGamma%28%5Calpha%2B%5Cbeta%2B2%29%7D%5Cfrac%7B%5CGamma%28%5Calpha%2B%5Cbeta%29%7D%7B%5CGamma%28%5Calpha%29%5CGamma%28%5Cbeta%29%7D+%5C%5C+++++%26%3D+%5Cfrac%7B%5B%5CGamma%28%5Calpha%29%28%5Calpha%2B1%29%5Calpha%5D%5CGamma%28%5Cbeta%29%7D%7B%5CGamma%28%5Calpha%2B%5Cbeta%29%28%5Calpha%2B%5Cbeta%2B1%29%28%5Calpha%2B%5Cbeta%29%7D%5Cfrac%7B%5CGamma%28%5Calpha%2B%5Cbeta%29%7D%7B%5CGamma%28%5Calpha%29%5CGamma%28%5Cbeta%29%7D+%5C%5C+++++%26%3D+%5Cfrac%7B%28%5Calpha%2B1%29%5Calpha%7D%7B%28%5Calpha%2B%5Cbeta%2B1%29%28%5Calpha%2B%5Cbeta%29%7D+++++%5Cend%7Baligned%7D)

接着求 ![\[公式\]](https://www.zhihu.com/equation?tex=Var%5BX%5D)

![](https://www.zhihu.com/equation?tex=%5Cbegin%7Baligned%7D+++++%26%5Cquad+Var%5BX%5D+%5C%5C+++++%26%3D+E%5BX%5E2%5D-E%5BX%5D%5E2+%5C%5C+++++%26%3D+%5Cfrac%7B%28%5Calpha%2B1%29%5Calpha%7D%7B%28%5Calpha%2B%5Cbeta%2B1%29%28%5Calpha%2B%5Cbeta%29%7D+-+%5Cleft+%28%5Cfrac%7B%5Calpha%7D%7B%5Calpha%2B%5Cbeta%7D+%5Cright+%29%5E2+%5C%5C+++++%26%3D+%5Cfrac%7B%28%5Calpha%2B1%29%28%5Calpha%2B%5Cbeta%29%5Calpha-%28%5Calpha%2B%5Cbeta%2B1%29%5Calpha%5E2%7D%7B%28%5Calpha%2B%5Cbeta%2B1%29%28%5Calpha%2B%5Cbeta%29%5E2%7D+%5C%5C+++++%26%3D+%5Cfrac%7B%5Calpha%5Cbeta%7D%7B%28%5Calpha%2B%5Cbeta%2B1%29%28%5Calpha%2B%5Cbeta%29%5E2%7D+++++%5Cend%7Baligned%7D)





