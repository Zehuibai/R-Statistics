# Bayesian Theory

## Introduction

一组随机样本，频率学派认为总体的参数是不变的，样本是随机获取的； 而贝叶斯学派认为总体参数是随机的，而获样本是不变的.

* 如果总体参数固定，那么随机获得的样本就是理想实体的不完美映射。相应的，如果获得了这些样本证据 ,利用极大似然法，推断出获得这些样本最有可能的参数。
* 贝叶斯不关心正确的参数到底是多少，而是需要通过获取的数据加上先验知识得出后验 概率进行统计推断

The posterior distribution summarises our uncertainty over the value of a parameter. If the distribution is narrower, then this indicates that we have greater confidence in our estimates of the parameter’s value. More narrow posterior distributions can be obtained by collecting more data.

* 先验是一种概率分布，描述了我们在收集和分析数据之前对假设的信念。
* 在贝叶斯推理中，我们对该假设的分析后信念为后验。 后验概率分布 $$\mathrm{P}(\theta | \text{data})$$是贝叶斯推理的主要目标。 后验分布总结了我们对参数值的不确定性。如果分布范围更窄，则表明我们对参数值的估计更有信心。通过收集更多数据可以获得更窄的后验分布。后验分布还用于预测实验的未来结果和模型测试。

### Bayes’ Rule

p(θ|data)\[**Bayes**] = p(data|θ)\[**Likelihood=L(θ|data)**] p(θ)/p(data)

$$
P(H\mid E)={\frac {P(E\mid H)\cdot P(H)}{P(E)}}
$$

$$P(H)$$ 先验概率，是在观察到当前数据E之前，假设概率H的估计. 边缘概率（又称先验概率）：某个事件发生的概率。边缘概率是这样得到的：在联合概率中，把最终结果中那 些不需要的事件通过合并成它们的全概率，而消去它们（对离散随机变量用求和得全概率，对连续随机变量用 积分得全概率），这称为边缘化（marginalization），比如A的边缘概率表示为P(A)，B的边缘概率表示为P(B)。

$$\textstyle P(H\mid E)$$ 后验概率：事件A在另外一个事件B已经发生条件下的发生概率。条件概率表示为P(A|B)， 读作“在B条件下**A**的概率”,。

$$\textstyle P(E\mid H)$$ 贝叶斯方法的核心就是通过先验知识不断更新后验概率密度来分析参数的可能性分布，如果继续进行实验 ，之前的后验概率密度就变成了先验知识，这样最终就会越来越接近参数的真实分布。需要注意的是，一 般来讲如果当前的样本量比先验知识的样本量大很多，那么先验知识就可以忽略不计。



### Bootstrap

Bootstrap 是一种用小样本来估计大样本的统计方法，斯坦福统计系主任的Bradley Efron在70年代提出。 中心思想是通过从样本中重抽样（resample），构建某个估计的置信区间。抽象的说，通过样本得到的估 计并没有榨干样本中的信息，bootstrap利用重抽样，把剩余价值发挥在了构建置信区间上。

* 首先，Bootstrap通过重抽样，可以避免了Cross-Validation造成的样本减少问题
* 其次，Bootstrap也可以用于创造数据的随机性。比如，我们所熟知的随机森林算法第一步就是从原始训 练数据集中，应用bootstrap方法有放回地随机抽取k个新的自助样本集，并由此构建k棵分类回归树。

### Bootstrap VS Monte Carlo

* Bootstrap是对现有的数据，不断再随机取小的样本，对每个小样处理数据,得到estimator.从而来了解 estimator 的variation or distribution.
* Monte Carlo 是用一个algorithm,依次输出数组，然后对这些数组处理，得到想要的结果。数组之间的 关系由algorithm来决定。Monte Carlo 的概念更广泛。Bootstrap 其实是一种Monte Carlo. {从某种意义上，Mente Carlo是一种计算积分的方法，期望，方差等等都是在概率空间的积分，先采样， 再加和。一般用在两个地方，一是在closed form拿不到的情况下来做的，二是空间维度非常高，因为 Mente Carlo的收敛阶依赖于采样点的个数（半阶收敛）而和空间维数没有关系，在特别高维的问题中计算 积分，Mente Carlo甚至成为了唯一可行的方法。}

## Parameter estimates

想要确定数据对应的概率密度分布，就需要确定两个东西：概率密度函数的形式和概率密度函数的参数。

* 有时可能知道的是概率密度函数的形式(高斯、瑞利等等)，但是不知道具体的参数，例如均值或者方差；
* 有时候可能不知道概率密度的类型，但是知道一些估计的参数，比如均值和方差。

在一个基础模型之下我们需要去一估计些未知的参数（比如在linear regression, 需要去计算**W这个向量**), 但在贝叶斯模型下我们需要去计算的是**W的分布（而不是W的点估计**)，用其分布直接计算对y的预测值p(y|x,D)，所以我们需要去需要整合W，也就是说我们把**所有可能的W向量都会去考虑**.这也为什么贝叶斯模型通常棘手的。所以我们需要用**MCMC**，不是直接用优化的方法。

在贝叶斯模型之下， 随着我们观察到越来越多的数据， 我们会对**W向量的分布会有更清晰的推断**，这其实就是**posterior inference**. 我们比较三种预测

1. Maximum likelihood estimation (ML)           (point estimation)
2. Maximum a posteriori estimation（MAP)   (point estimation) 
3. Bayesian Model                                                 (distribution estimation)

### Maximum likelihood estimation

极大似然估计是最简单的point estimation, 也就是我们需要去计算P(D|W), 从而找到最优化的W. 它的缺点就是数据比较少的时候往往过度拟合

基本的假设

* 第一：假设M个类别的数据子集的概率密度函数形式一样，只是参数的取值不同；
* 第二：假设类别i中的数据和类别j中的数据是相互独立抽样的，即类别j的参数仅仅根据类别j的数据就可以估计出来，类别i的数据并不能影响类别j的参数估计，反之亦然；
* 第三：每个类别内的样本之间具有统计独立性，即每个类别内的样本之间是**独立同分布** (iid) 的。

设x1,x2,...,xN 是从概率密度函数p(x;θ) 中随机抽取的样本，那么就可以得到联合概率密度函数 p(X;θ)，

$$
p(X;\theta)\equiv p(x_1,x_2,...,x_N;\theta)=\prod_{k=1}^Np(x_k;\theta)
$$

此时，就可以使用最大似然估计(Maximum Likelihood,ML)来估计参数θ：

$$
\hat{\theta}_{ML}=arg\max_{\theta}\prod_{k=1}^Np(x_k;\theta)
$$

为了得到最大值，$$\theta^{\text{ML}}$$ 必须满足的必要条件是,似然函数对θ的梯度必须为0，即：

$$
\frac{\partial \prod_{k=1}^Np(x_k;\theta)}{\partial\theta}=0
$$

一般我们取其对数形式：

$$
L(\theta)\equiv ln\prod_{k=1}^Np(x_k;\theta)
$$

$$
\frac{\partial L(\theta)}{\partial \theta}=\sum_{k=1}^N  \frac{\partial ln p(x_k;\theta)}{\partial \theta}=\sum_{k=1}^N\frac{1}{p(x_k;\theta)} \frac{\partial p(x_k;\theta)}{\partial \theta}=0
$$

极大似然估计有两个非常重要的性质：使得极大似然估计的成为了非常简单而且实用的参数估计方法。这里假设$$θ_0$$是密度函数p(x;θ) 中未知参数的准确值。

* 渐进无偏
* 渐进一致性， 有了这两个性质，

极大似然估计是渐进无偏的，即：$$\lim_{N \to \infty}E[\hat{\theta}_{ML}]=\theta_0$$, 也就是说，这里认为估计值** θ^ML本身是一个随机变量**（因为不同的样本集合X会得到不同的 θ^ML）， 那么其**均值就是未知参数的真实值**，这就是渐进无偏。

极大似然估计是渐进一致的，即：$$\lim_{N \to \infty}prob\{ \lVert \hat{\theta}_{ML}- \theta_0 \rVert \leqslant \epsilon\} = 1$$,这个公式还可以表示为: $$\lim_{N \to \infty} E \lVert \hat{\theta}_{ML}- \theta_0 \rVert^2 = 0$$, 对于一个估计器而言，一致性是非常重要的，因为存在满足无偏性，但是不满足一致性的情况，比如，θ^ML^ 在 θ-0-周围震荡。如果不满足一致性，那么就会出现很大的方差. 以上两个性质，都是在渐进的前提下（$$N \to \infty$$）才能讨论的，即只有N足够大时，上面两个性质才能成立.

### Maximum a posteriori estimation

在最大似然估计中，将θ看做是未知的参数，说的通俗一点，**最大似然估计是θ的函数**，其求解过程就是找到使得最大似然函数最大的那个参数θ。 **从最大后验估计开始，将参数θ看成一个随机变量**，并在已知样本集{x1,x2,...,xN}的条件下，估计参数θ。

* 在**最大似然估计中，参数θ是一个定值，只是这个值未知**，最大似然函数是θ的函数，这里θ 是没有概率意义的。
* 在最大后验估计中，**θ是有概率意义的，θ有自己的分布**，而这个分布函数，需要**通过已有的样本集合X得到**，即最大后验估计需要计算的是 **p(θ|X)**. Maximum a posteriori estimation (MAP). 他是在ML的基础上加了prior, 也就是假定了一些P(θ)的分布根据贝叶斯理论：

$$
p(\theta|X)=\frac{p(\theta)p(X|\theta)}{p(X)}
$$

这就是参数θ关于已有数据集合X的后验概率，**要使得这个后验概率最大，和极大似然估计一样，这里需要对后验概率函数求导**。由于分子中的p(X)相对于θ是独立的，随意可以直接忽略掉p(X)。

$$
\hat{\theta}_{MAP}=arg\max_{\theta}p(\theta|X)=arg\max_{\theta}p(\theta)p(X|\theta)
$$

为了得到参数θ，和ML一样，需要对p(θ|X)求梯度，并使其等于0：

$$
\frac{p(\theta|X)}{\partial\theta}=\frac{p(\theta)p(X|\theta）}{\partial\theta}=0
$$

这里**p(X|θ)和极大似然估计中的似然函数p(X;θ)** 是一样的，只是记法不一样。MAP和ML的区别是：MAP是在ML的基础上加上了p(θ). 

在MAP中，p(θ)称为θ的先验，假设其服从均匀分布，即对于所有θ取值，p(θ)都是同一个常量，则MAP和ML会得到相同的结果。当然了，如果p(θ)的方差非常的小，也就是说，**p(θ)是近似均匀分布的话，MAP和ML的结果自然也会非常的相似**。

### Bayesian Model

MAP的一些limitation 贝叶斯可以帮我们解决. 贝叶斯的特点就是**考虑整个X的分布**，而不是**通过已有的样本集合X得到得到的θ的**分布函数。 所以自然而然也就是防止overfitting.

防止标号混淆，这里定义已有的样本集合为D，而不是之前的X。 样本集合D中的样本都是从一个 固定但是未知的概率密度函数p(x)中独立抽取出来的，要求根据这些样本估计x的概率分布，**记为p(x|D)**，并且使得p(x|D)尽量的接近p(x)，这就是贝叶斯估计的核心问题。在这里我们是计算x的分布，而不是x的一个最优化的值！

虽然p(x)是未知的，但是前面提到过，一个密度分布的两个要素为：形式和参数， 我们可以假设p(x)的形式已知，但是参数θ的取值未知。这里就有了贝叶斯估计的第一个重要元素**p(x|θ)**, 这是一个条件概率密度函数，准确的说，是一个类条件概率密度函数. p(x|θ)的形式是已知的，只是参数θ的取值未知。由于这里的x可以看成一个测试样本， 所以这个条件密度函数，从本质上讲，是θ在点 x 处的似然估计。

由于参数θ的取值未知，且，我们将θ看成一个随机变量，那么，在观察到具体的训练样本之前， 关于θ的全部知识，可以用一个先验概率密度函数p(θ)来表示，对于训练样本的观察， 使得我们能够把这个先验概率密度转化成为后验概率密度函数p(θ|D)， 根据后验概率密度相关的论述知道，我们希望p(θ|D)在θ的真实值附近有非常显著的尖峰。 这里的这个后验概率密度**p(θ|D)**，就是贝叶斯估计的第二个主要元素。

现在，将贝叶斯估计核心问题p(x|D)，和贝叶斯估计的两个重要元素：p(x|θ)、p(θ|D)联系起来

$$
p(x|D)=\int p(x,\theta|D) d\theta=\int p(x|\theta,D)p(\theta|D)d\theta
$$

* x 是测试样本
* D 是训练集，x和D的选取是独立进行的, 因此，p(x|θ,D)可以写成p(x|θ), 所以，贝叶斯估计的核心问题就是下面这个公式：

$$
p(x|D)=\int p(x|\theta)p(\theta|D)d\theta
$$

* p(x|D) 根据这些样D本估计x的概率分布
*   p(x|θ) 假设p(x)的形式已知，但是参数θ的取值未知, p(x|θ)的形式是已知的，只是参数θ的取值未知, 这里p(x|θ)

    是θ关于测试样本x这一个点的似然估计
*   p(θ|D) 在观察到具体的训练样本之前，关于θ的全部知识，可以用一个先验概率密度函数p(θ)来表示，对于训练样本的观察，使得我们能够把这个先验概率密度转化成为后验概率密度函数p(θ|D) (p(θ|D)是θ在已有样本集合上的后验概率)

    $$
    p(\theta|D)=\frac{p(D|\theta)p(\theta)}{p(D)}=\frac{p(D|\theta)p(\theta)}{\int p(D|\theta)p(\theta)d\theta}
    $$

    $$
    p(D|\theta)=\prod_{k=1}^N p(x_k|\theta)
    $$

#### Full-Bayesian

与最大似然估计，最大后验估计（MAP）不同之处在于它得到的是测试数据在整个空间上的一个概率分布，而不单纯是一个点估计。它的精髓就在于这个**加权积分：考虑到了参数的所有情况，并且加以不同的权重（后验分布的值）**，自然就避免了过拟合。此外，很多情况下比起单纯的点估计，我们更需要一个分布来获得更多的信息

* Step 1: 用训练数据得到似然函数likelihood，再加上一个先验分布prior，得到一个后验分布posterior.
* Step 2: 对于一个新的测试数据x，用之前得到的posterior作为权重在整个参数空间里计算一个加权积分，得到一个预测分布。

在实际中，除了少数情况（比如先验和似然函数都是高斯分布），那个后验分布的形式一般都很复杂，第二步里的积分是积不出来的。这时候就要采取一些近似方法，近似方法又分为两大类：

* 简化复杂的后验分布，然后就能算出积分的解析形式了。具体方法有变分推断，Laplace近似等。这类方法的特点是人算起来困难，机器跑起来快。
* 用采样的方法搞定后验分布。具体方法有Gibbs采样，HMC采样等。这类方法反过来了，人算起来简单，但是机器跑起来慢。采样方法还有一个好处，就是精度算得比谁都高







## Non-informativ Priors

确定先验主要有**四个方向:**

1. 无信息先验分布
2. 共轭先验分布
3. 经验Bayes方法
4. 专家验定先验分布

通常在贝叶斯分析中，我们需要指定一个先验，但事实在很多前提下，我们是不知道其先验的，这时我们就可以采用无信息先验分布来进行分析计算。

### Jeffreys Prior

* Jeffreys在他的书里提出了Jeffreys 先验， 其最主要性质就是不变性（invariant），即先验的形式不随着参数形式变化而变化。
* 较好地解决了无信息先验中的一个矛盾：若对参数θ选用均匀分布，则其函数g(θ)往往不是均匀分布。
* 采用Fisher信息阵的平方根作为θ的无信息先验分布

### Reference Prior

* Reference prior解决了Jeffreys prior在多元情况下的一些问题
* Reference prior是在极大样本下使得先验分布和后验分布的Kullback–Leibler divergence最大的reference prior。 先验分布和后验分布的Kullback–Leibler
* divergence可以被理解成这两个分布之间的信息差，而这个信息差显然来自于样本的信息，使得这个信息差最大就说明后验分布主要体现的是样本的信息，reference prior对我们得到的信息几乎没有影响。所以这种先验分布可以叫做noninformative prior。

## Conjugate

共轭（conjugate）是贝叶斯方法中很常见的一个词，结合贝叶斯定理，我们可以将“共轭”理解为后验和先验是同一种分布。

### **泊松分布-伽马分布模型**

假设有一组观测样本 ![\[公式\]](https://www.zhihu.com/equation?tex=x\_1%2C...%2Cx_n) 独立同分布于泊松分布，即 ![\[公式\]](https://www.zhihu.com/equation?tex=x_i%5Csim%5Ctext%7BPoisson%7D%28%5Clambda%29) ，则

![](https://www.zhihu.com/equation?tex=p%28x_i%7C%5Clambda%29%3D%5Cfrac%7Be%5E%7B-%5Clambda%7D%5Clambda%5E%7Bx_i%7D%7D%7Bx_i%21%7D)

从而，可以很轻松地写出相应的**似然**（likelihood）：

![](https://www.zhihu.com/equation?tex=%5Cmathcal%7BL%7D%28x\_1%2C...%2Cx_n%7C%5Clambda%29%3D%5Cprod\_%7Bi%3D1%7D%5E%7Bn%7D%7B%5Cfrac%7Be%5E%7B-%5Clambda%7D%5Clambda%5E%7Bx_i%7D%7D%7Bx_i%21%7D%7D%3D%5Cfrac%7Be%5E%7B-n%5Clambda%7D%5Clambda%5E%7B%5Csum\_%7Bi%3D1%7D%5E%7Bn%7D%7Bx_i%7D%7D%7D%7B%5Cprod\_%7Bi%3D1%7D%5E%7Bn%7D%7Bx_i%21%7D%7D)

其中， ![\[公式\]](https://www.zhihu.com/equation?tex=%5Clambda%3E0) 是一个未知的参数，进一步假设其服从伽马分布，给定**先验** ![\[公式\]](https://www.zhihu.com/equation?tex=%5Clambda%5Csim%5Ctext%7BGamma%7D%28%5Calpha%2C%5Cbeta%29) ，则

![](https://www.zhihu.com/equation?tex=p%28%5Clambda%7C%5Calpha%2C%5Cbeta%29%3D%5Cfrac%7B%5Cbeta%5E%5Calpha%7D%7B%5CGamma%28%5Calpha%29%7De%5E%7B-%5Cbeta%5Clambda%7D%5Clambda%5E%7B%5Calpha-1%7D)

其中，需要说明的是，伽马分布中的 ![\[公式\]](https://www.zhihu.com/equation?tex=%5Calpha) 表示形状（shape）参数， ![\[公式\]](https://www.zhihu.com/equation?tex=%5Cbeta) 表示比率（rate）参数， ![\[公式\]](https://www.zhihu.com/equation?tex=%5CGamma%28%5Ccdot%29) 表示伽马函数 在贝叶斯分析中，有了先验和似然，则后验正比于先验和似然的乘积

![](https://www.zhihu.com/equation?tex=%5Ctext%7Bposterior%7D%5Cpropto%5Ctext%7Bprior%7D%5Ctimes%5Ctext%7Blikelihood%7D)

![](https://www.zhihu.com/equation?tex=p%28%5Clambda%7Cx\_1%2C...%2Cx_n%2C%5Calpha%2C%5Cbeta%29%5Cpropto+p%28%5Clambda%7C%5Calpha%2C%5Cbeta%29%5Cmathcal%7BL%7D%28x\_1%2C...%2Cx_n%7C%5Clambda%29)

![](https://www.zhihu.com/equation?tex=%5Cpropto+e%5E%7B-%28%5Cbeta%2Bn%29%5Clambda%7D%5Clambda%5E%7B%5Calpha%2B%5Csum\_%7Bi%3D1%7D%5E%7Bn%7D%7Bx_i%7D-1%7D)

![](https://www.zhihu.com/equation?tex=%5CRightarrow+%28%5Clambda%7Cx\_1%2C...%2Cx_n%2C%5Calpha%2C%5Cbeta%29%5Csim%5Ctext%7BGamma%7D%28%5Calpha%2B%5Csum\_%7Bi%3D1%7D%5E%7Bn%7D%7Bx_i%7D%2C%5Cbeta%2Bn%29)

因此，假设一组观测样本独立同分布于参数为 ![\[公式\]](https://www.zhihu.com/equation?tex=%5Clambda) 的泊松分布，则伽马分布是参数 ![\[公式\]](https://www.zhihu.com/equation?tex=%5Clambda) 的**共轭先验**（conjugate prior）

### **正态分布-正态分布模型**

假设一组观测样本 ![\[公式\]](https://www.zhihu.com/equation?tex=x\_1%2C...%2Cx_n) 独立同分布于正态分布，即 ![\[公式\]](https://www.zhihu.com/equation?tex=x_i%5Csim%5Cmathcal%7BN%7D%28%5Cmu%2C%5Csigma%5E2%29) ，其中， ![\[公式\]](https://www.zhihu.com/equation?tex=%5Cmu) 是未知的， ![\[公式\]](https://www.zhihu.com/equation?tex=%5Csigma%5E2) 是已知的（不需要假设先验），则**似然**为

![](https://www.zhihu.com/equation?tex=%5Cmathcal%7BL%7D%28x\_1%2C...%2Cx_n%7C%5Cmu%29%3D%5Cprod\_%7Bi%3D1%7D%5E%7Bn%7D%5Cfrac%7B1%7D%7B%5Csqrt%7B2%5Cpi%7D%5Csigma%7D%5Cexp%5Cleft%5C%7B-%5Cfrac%7B1%7D%7B2%5Csigma%5E2%7D%28x_i-%5Cmu%29%5E2%5Cright%5C%7D)

![](https://www.zhihu.com/equation?tex=%5Cpropto%5Cexp%5Cleft%5C%7B-%5Cfrac%7B1%7D%7B2%5Csigma%5E2%7D%5Csum\_%7Bi%3D1%7D%5E%7Bn%7D%7B%28x_i-%5Cmu%29%5E2%7D%5Cright%5C%7D)

进一步，假设参数 ![\[公式\]](https://www.zhihu.com/equation?tex=%5Cmu) 的**先验**为 ![\[公式\]](https://www.zhihu.com/equation?tex=%5Cmu%5Csim%5Cmathcal%7BN%7D%28%5Cmu\_0%2C%5Csigma\_0%5E2%29) ，即

![](https://www.zhihu.com/equation?tex=p%28%5Cmu%7C%5Cmu\_0%2C%5Csigma\_0%29%3D%5Cfrac%7B1%7D%7B%5Csqrt%7B2%5Cpi%7D%5Csigma\_0%7D%5Cexp%5Cleft%5C%7B-%5Cfrac%7B1%7D%7B2%5Csigma\_0%5E2%7D%28%5Cmu-%5Cmu\_0%29%5E2%5Cright%5C%7D)

从而，可以推导出**后验**为

![](https://www.zhihu.com/equation?tex=p%28%5Cmu%7Cx\_1%2C...%2Cx_n%2C%5Cmu\_0%2C%5Csigma\_0%29%5Cpropto+p%28%5Cmu%7C%5Cmu\_0%2C%5Csigma\_0%29%5Cmathcal%7BL%7D%28x\_1%2C...%2Cx_n%7C%5Cmu%29)

![](https://www.zhihu.com/equation?tex=%5Cpropto%5Cexp%5Cleft%5C%7B-%5Cfrac%7B1%7D%7B2%7D%5Cleft%5B%5Cfrac%7B1%7D%7B%5Csigma%5E2%7D%5Csum\_%7Bi%3D1%7D%5E%7Bn%7D%7B%28x_i-%5Cmu%29%5E2%7D%2B%5Cfrac%7B1%7D%7B%5Csigma\_0%5E2%7D%28%5Cmu-%5Cmu\_0%29%5E2%5Cright%5D%5Cright%5C%7D)

![](https://www.zhihu.com/equation?tex=%5Cpropto+%5Cexp%5Cleft%5C%7B-%5Cfrac%7B1%7D%7B2%7D%5Cleft%28%5Cfrac%7B1%7D%7B%5Csigma\_0%5E2%7D%2B%5Cfrac%7Bn%7D%7B%5Csigma%5E2%7D%5Cright%29%5Cleft%28%5Cmu-%5Cfrac%7B%5Cfrac%7B%5Cmu\_0%7D%7B%5Csigma\_0%5E2%7D%2B%5Cfrac%7Bn%5Cbar%7Bx%7D%7D%7B%5Csigma%5E2%7D%7D%7B%5Cfrac%7B1%7D%7B%5Csigma\_0%5E2%7D%2B%5Cfrac%7Bn%7D%7B%5Csigma%5E2%7D%7D%5Cright%29%5E2%5Cright%5C%7D+)

因此，后验也服从正态分布，其均值为

![](https://www.zhihu.com/equation?tex=%5Cfrac%7B%5Cfrac%7B%5Cmu\_0%7D%7B%5Csigma\_0%5E2%7D%2B%5Cfrac%7Bn%5Cbar%7Bx%7D%7D%7B%5Csigma%5E2%7D%7D%7B%5Cfrac%7B1%7D%7B%5Csigma\_0%5E2%7D%2B%5Cfrac%7Bn%7D%7B%5Csigma%5E2%7D%7D)

方差为

![](https://www.zhihu.com/equation?tex=%5Cleft%28%5Cfrac%7B1%7D%7B%5Csigma\_0%5E2%7D%2B%5Cfrac%7Bn%7D%7B%5Csigma%5E2%7D%5Cright%29%5E%7B-1%7D)

一般而言，有了先验和似然，计算后验能够方便我们实现变分推断和Gibbs采样等

## MCMC

MCMC由两个MC组成，即蒙特卡罗方法（Monte Carlo Simulation，简称MC）和马尔科夫链（Markov Chain ，也简称MC）

{% embed url="https://www.cnblogs.com/pinard/p/6645766.html" %}

### Monte Carlo Simulation

蒙特卡罗原来是一个赌场的名称，用它作为名字大概是因为蒙特卡罗方法是一种随机模拟的方法，这很像赌博场里面的扔骰子的过程。最早的蒙特卡罗方法都是为了求解一些不太好求解的求和或者积分问题。比如积分： $$\theta = \int_a^b f(x)dx$$ 

如果我们可以得到x在\[a,b]的概率分布函数p(x)，那么我们的定积分求和可以这样进行：

$$
\theta = \int_a^b f(x)dx =  \int_a^b \frac{f(x)}{p(x)}p(x)dx \approx \frac{1}{n}\sum\limits_{i=0}^{n-1}\frac{f(x_i)}{p(x_i)}
$$

最右边的这个形式就是蒙特卡罗方法的一般形式。当然这里是连续函数形式的蒙特卡罗方法，但是在离散时一样成立。假设x在\[a,b]之间是均匀分布的时候， $$p(x_i) = 1/(b-a)$$ ，带入我们有概率分布的蒙特卡罗积分的上式，可以得到：

$$
\frac{1}{n}\sum\limits_{i=0}^{n-1}\frac{f(x_i)}{1/(b-a)} = \frac{b-a}{n}\sum\limits_{i=0}^{n-1}f(x_i)
$$

### 概率分布采样

蒙特卡罗方法的**关键是得到x的概率分布**。如果求出了x的概率分布，我们可以基于概率分布去采样基于这个概率分布的n个x的样本集，带入蒙特卡罗求和的式子即可求解。

还有一个关键的问题需要解决，即**如何基于概率分布去采样基于这个概率分布的n个x的样本集**。

对于常见的均匀分布uniform(0,1)是非常容易采样样本的，一般通过线性同余发生器可以很方便的生成(0,1)之间的伪随机数样本。而其他常见的概率分布，无论是离散的分布还是连续的分布，它们的样本都可以通过uniform(0,1)的样本转换而得。 

* 比如二维正态分布的样本$$(Z_1,Z_2)$$ 可以通过通过独立采样得到的uniform(0,1)样本对$$(X_1,X_2)$$通过如下的式子转换而得：

$$
Z_1 = \sqrt{-2 ln X_1}cos(2\pi X_2)
$$

$$
Z_2 = \sqrt{-2 ln X_1}sin(2\pi X_2)
$$

* 其他一些常见的连续分布，比如t分布，F分布，Beta分布，Gamma分布等，都可以通过类似的方式从uniform(0,1)得到的采样样本转化得到。

### 接受-拒绝采样

对于概率分布不是常见的分布，一个可行的办法是采用接受-拒绝采样来得到该分布的样本。既然 p(x) 太复杂在程序中没法直接采样，那么我**设定一个程序可采样的分布 q(x)** 比如高斯分布，然后按照一定的方法**拒绝某些样本，以达到接近 p(x) 分布的目的**，其中**q(x)叫做 proposal distribution**。

![](https://images2015.cnblogs.com/blog/1042406/201703/1042406-20170327143755811-993574578.png)

具体采用过程如下，设定一个方便采样的常用概率分布函数 q(x)，以及一个常量 k，使得 p(x) 总在 kq(x) 的下方。如上图。首先，采样得到q(x)的一个样本z0，采样方法如第三节。然后，从均匀分布 $$(0, kq(z_0))$$ 中采样得到一个值u. 如果u落在了上图中的灰色区域，则拒绝这次抽样，否则接受这个样本z0。重复以上过程得到n个接受的样本z0,z1,...zn−1,则最后的蒙特卡罗方法求解结果为：

$$
\frac{1}{n}\sum\limits_{i=0}^{n-1}\frac{f(z_i)}{p(z_i)}
$$

### Markov Chain 

马尔科夫链定义本身比较简单，它假设某一时刻状态转移的概率只依赖于它的前一个状态。如果用精确的数学定义来描述，则假设我们的序列状态是 $$...X_{t-2}, X_{t-1}, X_{t},  X_{t+1},...$$ 那么我们的在时刻Xt+1的状态的条件概率仅仅依赖于时刻Xt，即：

$$
P(X_{t+1} |...X_{t-2}, X_{t-1}, X_{t} ) = P(X_{t+1} | X_{t})
$$

既然某一时刻状态转移的概率只依赖于它的前一个状态，那么我们只要能求出系统中任意两个状态之间的转换概率，这个马尔科夫链的模型就定了。

![](https://images2015.cnblogs.com/blog/1042406/201703/1042406-20170328104002045-972507912.png)

这个马尔科夫链是表示股市模型的，共有三种状态：牛市（Bull market）, 熊市（Bear market）和横盘（Stagnant market）。每一个状态都以一定的概率转化到下一个状态。比如，牛市以0.025的概率转化到横盘的状态。这个状态概率转化图可以以矩阵的形式表示。如果我们定义矩阵阵P某一位置$$P(i,j)$$的值为 $$P(j|i)$$ 即从状态i转化到状态j的概率，并定义牛市为状态0， 熊市为状态1, 横盘为状态2. 这样我们得到了马尔科夫链模型的状态转移矩阵为：

$$
P=\left( \begin{array}{ccc} 0.9&0.075&0.025 \\ 0.15&0.8& 0.05 \\ 0.25&0.25&0.5 \end{array} \right)
$$

尽管这次采用了不同初始概率分布，最终状态的概率分布趋于同一个稳定的概率分布\[0.625 0.3125 0.0625]， 也就是说我们的**马尔科夫链模型的状态转移矩阵收敛到的稳定概率分布与我们的初始状态概率分布无关**。也就是说，如果我们得到了这个稳定概率分布对应的马尔科夫链模型的状态转移矩阵，则我们可以用任意的概率分布样本开始，带入马尔科夫链模型的状态转移矩阵，这样经过一些序列的转换，最终就可以得到符合对应稳定概率分布的样本。

### **马尔科夫链的收敛性质:**

如果一个非周期的马尔科夫链有状态转移矩阵P, 并且它的任何两个状态是连通的，那么 $$\lim_{n \to \infty}P_{ij}^n$$ 与i无关，我们有：

1. $$\lim_{n \to \infty}P_{ij}^n = \pi(j)$$ 
2. $$\lim_{n \to \infty}P^n = \left( \begin{array}{ccc} \pi(1)&\pi(2)&\ldots&\pi(j)&\ldots \\ \pi(1)&\pi(2)&\ldots&\pi(j)&\ldots \\ \ldots&\ldots&\ldots&\ldots&\ldots \\ \pi(1)&\pi(2)&\ldots&\pi(j)&\ldots \\ \ldots&\ldots&\ldots&\ldots&\ldots \end{array} \right)$$ 
3. $$\pi(j) = \sum\limits_{i=0}^{\infty}\pi(i)P_{ij}$$ 
4. $$\pi$$是方程的 $$\pi P = \pi$$ 唯一非负解，其中： $$\pi = [\pi(1),\pi(2),...,\pi(j),...]\;\; \sum\limits_{i=0}^{\infty}\pi(i) = 1$$ 

上面的性质中需要解释的有：

1. 非周期的马尔科夫链：这个主要是指马尔科夫链的状态转化不是循环的，如果是循环的则永远不会收敛。幸运的是我们遇到的马尔科夫链一般都是非周期性的。用数学方式表述则是：对于任意某一状态i，d为集合 $$\{n \mid n \geq 1,P_{ii}^n>0 \}$$ 的最大公约数，如果 d=1d=1 ，则该状态为非周期的
2. 任何两个状态是连通的：这个指的是从任意一个状态可以通过有限步到达其他的任意一个状态，不会出现条件概率一直为0导致不可达的情况。
3. 马尔科夫链的状态数可以是有限的，也可以是无限的。因此可以用于连续概率分布和离散概率分布。
4. $$\pi$$通常称为马尔科夫链的平稳分布。

### 基于马尔科夫链采样

如果我们得到了某个平稳分布所对应的马尔科夫链状态转移矩阵，我们就很容易采用出这个平稳分布的样本集。假设我们任意初始的概率分布是 $$\pi_0(x)$$ , 经过第一轮马尔科夫链状态转移后的概率分布是$$\pi_1(x)$$，。。。第i轮的概率分布是$$\pi_i(x)$$。假设经过n轮后马尔科夫链收敛到我们的平稳分布$$\pi(x)$$，即：

$$
\pi_n(x) = \pi_{n+1}(x) = \pi_{n+2}(x) =... = \pi(x)
$$

对于每个分布$$\pi_i(x)$$，我们有：

$$
\pi_i(x) = \pi_{i-1}(x)P = \pi_{i-2}(x)P^2 = \pi_{0}(x)P^i
$$

现在我们可以开始采样了，首先，基于初始任意简单概率分布比如高斯分布π0(x)采样得到状态值x0，基于条件概率分布 $$P(x|x_0)$$ 采样状态值x1，一直进行下去，当状态转移进行到一定的次数时，比如到n次时，我们认为此时的采样集 $$(x_n,x_{n+1},x_{n+2},...)$$ 即是符合我们的平稳分布的对应样本集，可以用来做蒙特卡罗模拟求和了。

总结下基于马尔科夫链的采样过程: 

* 1\) 输入马尔科夫链状态转移矩阵 $$P$$, 设定状态转移次数间值 $$n_{1},$$ 需要的样本个数 $$n_{2}$$ 
* 2\) 从任意简单概率分布采样得到初始状态值 $$x_{0}$$ 
* 3\) for $$t=0$$ to $$n_{1}+n_{2}-1:$$ 从条件概率分布 $$P\left(x \mid x_{t}\right)$$ 中采样得到样本 $$x_{t+1}$$ 样本集 $$\left(x_{n_{1}}, x_{n_{1}+1}, \ldots, x_{n_{1}+n_{2}-1}\right)$$ 即为我们需要的平稳分布对应的样本集。

如果假定我们可以得到我们需要采样样本的平稳分布所对应的马尔科夫链状态转移矩阵，那么我们就可以用马尔科夫链采样得到我们需要的样本集，进而进行蒙特卡罗模拟。但是一个重要的问题是，随意给定一个平稳分布π,如何得到它所对应的马尔科夫链状态转移矩阵P呢？这是个大问题。我们绕了一圈似乎还是没有解决任意概率分布采样样本集的问题。幸运的是，MCMC采样通过迂回的方式解决了上面这个大问题.

### MCMC采样

#### 马尔科夫链的细致平稳条件

在解决从平稳分布π, 找到对应的马尔科夫链状态转移矩阵P之前，我们还需要先看看马尔科夫链的细致平稳条件》如果非周期马尔科夫链的状态转移矩阵P和概率分布π(x)对于所有的i,j满足：

$$
\pi(i)P(i,j) = \pi(j)P(j,i)
$$

则称概率分布π(x)是状态转移矩阵P的平稳分布。证明如下

$$
\sum\limits_{i=1}^{\infty}\pi(i)P(i,j)  = \sum\limits_{i=1}^{\infty} \pi(j)P(j,i) =  \pi(j)\sum\limits_{i=1}^{\infty} P(j,i) =  \pi(j)
$$

将上式用矩阵表示即为： $$\pi P = \pi$$ 

不过不幸的是，仅仅从细致平稳条件还是很难找到合适的矩阵P。比如我们的目标平稳分布是π(x),随机找一个马尔科夫链状态转移矩阵Q,它是很难满足细致平稳条件的，即：

$$
\pi(i)Q(i,j) \neq \pi(j)Q(j,i)
$$

可以对上式做一个改造，使细致平稳条件成立。方法是引入一个 $$\alpha(i,j)$$ ,使上式可以取等号，即：

$$
\pi(i)Q(i,j)\alpha(i,j) = \pi(j)Q(j,i)\alpha(j,i)
$$

$$\alpha(i,j)$$满足下两式即可：

$$
\alpha(i,j) = \pi(j)Q(j,i)
$$

$$
\alpha(j,i) = \pi(i)Q(i,j)
$$

我们就得到了我们的分布π(x)对应的马尔科夫链状态转移矩阵P，满足：

$$
P(i,j) = Q(i,j)\alpha(i,j)
$$

$$\alpha(i,j)$$一般称之为**接受率**。取值在\[0,1]之间，可以理解为一个概率值。即目标矩阵P可以通过任意一个马尔科夫链状态转移矩阵Q以一定的接受率获得

#### 总结下MCMC的采样过程

* 1\) 输入我们任意选定的马尔科夫链状态转移矩阵 $$Q,$$ 平稳分布 $$\pi(x),$$ 设定状态转移次数间值 $$n_{1},$$ 需要的样本个数 $$n_{2}$$
* 2\) 从任意简单概率分布采样得到初始状态值 $$x_{0}$$
*   3\) for $$t=0$$ to $$n_{1}+n_{2}-1$$ :

    * a) 从条件概率分布 $$Q\left(x \mid x_{t}\right)$$ 中采样得到样本 $$x_{*}$$
    * b) 从均匀分布采样 $$u \sim$$ $$uniform [0,1]$$
    * c) 如果 $$u<\alpha\left(x_{t}, x_{*}\right)=\pi\left(x_{*}\right) Q\left(x_{*}, x_{t}\right),$$ 则接受转移 $$x_{t} \rightarrow x_{*},$$ 即 $$x_{t+1}=x_{*}$$
    *   d) 否则不接受转移, 即 $$x_{t+1}=x_{t}$$



    样本集 $$\left(x_{n_{1}}, x_{n_{1}+1}, \ldots, x_{n_{1}+n_{2}-1}\right)$$ 即为我们需要的平稳分布对应的样本集。

但是这个采样算法还是比较难在实际中应用，问题在上面第三步的c步骤，由于 $$\alpha(x_t,x_{*})$$可能非常的小，比如0.1，导致我们大部分的采样值都被拒绝转移，采样效率很低。有可能我们采样了上百万次马尔可夫链还没有收敛，也就是上面这个n1要非常非常的大，这让人难以接受，怎么办呢？这时就轮到我们的**M-H采样**出场了。

### M-H采样

M-H采样是Metropolis-Hastings采样的简称，这个算法首先由Metropolis提出，被Hastings改进，因此被称之为Metropolis-Hastings采样或M-H采样. M-H采样解决了我们上一节MCMC采样接受率过低的问题。

回到MCMC采样的细致平稳条件： $$\pi(i)Q(i,j)\alpha(i,j) = \pi(j)Q(j,i)\alpha(j,i)$$ 

我们采样效率低的原因是 $$\alpha(i,j)$$ 太小了，比如为0.1，而$$\alpha(j,i)$$为0.2。即： $$\pi(i)Q(i,j)\times 0.1 = \pi(j)Q(j,i)\times 0.2$$ , 这时我们可以看到，如果两边同时扩大五倍，接受率提高到了0.5，但是细致平稳条件却仍然是满足的，即： $$\pi(i)Q(i,j)\times 0.5 = \pi(j)Q(j,i)\times 1$$ 这样我们的接受率可以做如下改进，即：

$$
\alpha(i,j) = min\{ \frac{\pi(j)Q(j,i)}{\pi(i)Q(i,j)},1\}
$$

通过这个微小的改造，我们就得到了可以在实际应用中使用的M-H采样算法过程如下：

* 1\) 输入我们任意选定的马尔科夫链状态转移矩阵 $$Q,$$ 平稳分布 $$\pi(x),$$ 设定状态转移次数间值 $$n_{1},$$ 需要的样本个数 $$n_{2}$$
* 2\) 从任意简单概率分布采样得到初始状态值 $$x_{0}$$
* 3\) for $$t=0$$ to $$n_{1}+n_{2}-1$$ :
  * a) 从条件概率分布 $$Q\left(x \mid x_{t}\right)$$ 中采样得到样本 $$x_{*}$$
  * b) 从均匀分布采样 $$u \sim$$ uniform \[0,1]
  * c) 如果 $$u<\alpha\left(x_{t}, x_{*}\right)=\min \left\{\frac{\pi(j) Q(j, i)}{\pi(i) Q(i, j)}, 1\right\},$$ 则接受转移 $$x_{t} \rightarrow x_{*},$$ 即 $$x_{t+1}=x_{*}$$
  * d) 否则不接受转移， 即 $$x_{t+1}=x_{t}$$

样本集 $$\left(x_{n_{1}}, x_{n_{1}+1}, \ldots, x_{n_{1}+n_{2}-1}\right)$$ 即为我们需要的平稳分布对应的样本集。 很多时候, 我们选择的马尔科夫链状态转移矩阵Q如果是对称的, 即满足 $$Q(i, j)=Q(j, i)$$,这时我们的接受率可以进一步简化为：

$$
\alpha(i, j)=\min \left\{\frac{\pi(j)}{\pi(i)}, 1\right\}
$$

M-H采样完整解决了使用蒙特卡罗方法需要的任意概率分布样本集的问题，因此在实际生产环境得到了广泛的应用. 但是在大数据时代，M-H采样面临着两大难题：

1. 我们的数据特征非常的多，M-H采样由于接受率计算式$$\frac{\pi(j)Q(j,i)}{\pi(i)Q(i,j)}$$的存在，在高维时需要的计算时间非常的可观，算法效率很低。同时$$\alpha(i,j)$$一般小于1，有时候辛苦计算出来却被拒绝了。能不能做到不拒绝转移呢？
2. 由于特征维度大，很多时候我们甚至很难求出目标的各特征维度联合分布，但是可以方便求出各个特征之间的条件概率分布。这时候我们能不能只有各维度之间条件概率分布的情况下方便的采样呢？

Gibbs采样解决了上面两个问题

### Gibbs采样

M-H采样已经可以很好的解决蒙特卡罗方法需要的任意概率分布的样本集的问题。但是M-H采样有两个缺点：一是需要计算接受率，在高维时计算量大。并且由于接受率的原因导致算法收敛时间变长。二是有些高维数据，特征的条件概率分布好求，但是特征的联合分布不好求。因此需要一个好的方法来改进M-H采样

细致平稳条件：如果非周期马尔科夫链的状态转移矩阵P和概率分布π(x)对于所有的i,j满足：$$\pi(i)Q(i,j)\alpha(i,j) = \pi(j)Q(j,i)\alpha(j,i)$$则称概率分布π(x)是状态转移矩阵P的平稳分布。现在我们换一个思路, 重新寻找合适的细致平稳条件

从二维的数据分布开始，假设$$\pi(x_1,x_2)$$是一个二维联合数据分布，观察第一个特征维度相同的两个点$$A(x_1^{(1)},x_2^{(1)})$$和$$B(x_1^{(1)},x_2^{(1)})$$，容易发现下面两式成立：

$$
\pi(x_1^{(1)},x_2^{(1)}) \pi(x_2^{(2)} | x_1^{(1)}) = \pi(x_1^{(1)})\pi(x_2^{(1)}|x_1^{(1)}) \pi(x_2^{(2)} | x_1^{(1)})
$$

$$
\pi(x_1^{(1)},x_2^{(2)}) \pi(x_2^{(1)} | x_1^{(1)}) = \pi(x_1^{(1)}) \pi(x_2^{(2)} | x_1^{(1)})\pi(x_2^{(1)}|x_1^{(1)})
$$

由于两式的右边相等，因此我们有：

$$
\pi(x_1^{(1)},x_2^{(1)}) \pi(x_2^{(2)} | x_1^{(1)})  = \pi(x_1^{(1)},x_2^{(2)}) \pi(x_2^{(1)} | x_1^{(1)})
$$

$$
\pi(A) \pi(x_2^{(2)} | x_1^{(1)})  = \pi(B) \pi(x_2^{(1)} | x_1^{(1)})
$$

观察上式再观察细致平稳条件的公式, 我们发现在 $$x_{1}=x_{1}^{(1)}$$ 这条直线上, 如果用条件概率分布 $$\pi\left(x_{2} \mid x_{1}^{(1)}\right.)$$ 作为马尔科夫链的状态转移概率, 则任意两 个点之间的转移满足细致平稳条件! 同样的道理, 在在 $$x_{2}=x_{2}^{(1)}$$ 这条直线上, 如果用条件概率分布 $$\pi\left(x_{1} \mid x_{2}^{(1)}\right.)$$ 作为马尔科夫链的状态 转移概率, 则任意两个点之间的转移也满足细致平稳条件。那是因为假如有一点 $$C\left(x_{1}^{(2)}, x_{2}^{(1)}\right)$$,我们可以得到:

$$
\pi(A) \pi\left(x_{1}^{(2)} \mid x_{2}^{(1)}\right)=\pi(C) \pi\left(x_{1}^{(1)} \mid x_{2}^{(1)}\right)
$$

基于上面的发现, 我们可以这样构造分布 $$\pi\left(x_{1}, x_{2}\right)$$ 的马尔可夫链对应的状态转移矩阵 $$P$$ :

$$
\begin{array}{c}
P(A \rightarrow B)=\pi\left(x_{2}^{(B)} \mid x_{1}^{(1)}\right) \text {  if  } x_{1}^{(A)}=x_{1}^{(B)}=x_{1}^{(1)} \\
P(A \rightarrow C)=\pi\left(x_{1}^{(C)} \mid x_{2}^{(1)}\right) \text {  if  } x_{2}^{(A)}=x_{2}^{(C)}=x_{2}^{(1)} \\
P(A \rightarrow D)=0 \text { else }
\end{array}
$$

有了上面这个状态转移矩阵，我们很容易验证平面上的任意两点E,F，满足细致平稳条件：

$$
\pi(E)P(E \to F)  = \pi(F)P(F \to E)
$$

#### 二维Gibbs采样

利用状态转移矩阵，我们就得到了二维Gibbs采样，这个采样需要两个维度之间的条件概率。具体过程如下：

* 1\) 输入平稳分布 $$\pi\left(x_{1}, x_{2}\right),$$ 设定状态转移次数间值 $$n_{1},$$ 需要的样本个数 $$n_{2}$$
* 2\) 随机初始化初始状态值 $$x_{1}^{(0)}$$ 和 $$x_{2}^{(0)}$$
* 3\) for $$t=0$$ to $$n_{1}+n_{2}-1$$:
  * a) 从条件概率分布 $$P\left(x_{2} \mid x_{1}^{(t)}\right)$$ 中采样得到样本 $$x_{2}^{t+1}$$
  * b) 从条件概率分布 $$P\left(x_{1} \mid x_{2}^{(t+1)}\right)$$ 中采样得到样本 $$x_{1}^{t+1}$$

样本集 $$\left\{\left(x_{1}^{\left(n_{1}\right)}, x_{2}^{\left(n_{1}\right)}\right),\left(x_{1}^{\left(n_{1}+1\right)}, x_{2}^{\left(n_{1}+1\right)}\right), \ldots,\left(x_{1}^{\left(n_{1}+n_{2}-1\right)}, x_{2}^{\left(n_{1}+n_{2}-1\right)}\right)\right\}$$ 即为我们需要的平稳分布对应的样本集。

整个采样过程中，我们通过轮换坐标轴, 采样的过程为：

$$
\left(x_{1}^{(1)}, x_{2}^{(1)}\right) \rightarrow\left(x_{1}^{(1)}, x_{2}^{(2)}\right) \rightarrow\left(x_{1}^{(2)}, x_{2}^{(2)}\right) \rightarrow \ldots \rightarrow\left(x_{1}^{\left(n_{1}+n_{2}-1\right)}, x_{2}^{\left(n_{1}+n_{2}-1\right)}\right)
$$

用下图可以很直观的看出，采样是在两个坐标轴上不停的轮换的。当然，坐标轴轮换不是必须的，我们也可以每次随机选择一个坐标轴进行采样。不过常用的Gibbs采样的实现都是基于坐标轴轮换的。

![](https://images2015.cnblogs.com/blog/1042406/201703/1042406-20170330161210195-1389960329.png)

由于Gibbs采样在高维特征时的优势，目前我们通常意义上的MCMC采样都是用的Gibbs采样。当然Gibbs采样是从M-H采样的基础上的进化而来的，同时Gibbs采样要求数据至少有两个维度，一维概率分布的采样是没法用Gibbs采样的,这时M-H采样仍然成立。有了Gibbs采样来获取概率分布的样本集，有了蒙特卡罗方法来用样本集模拟求和，他们一起就奠定了MCMC算法在大数据时代高维数据模拟求和时的作用。

## Bayesian linear regression

如果要将极大似然估计应用到线性回归模型中，模型的复杂度会被两个因素所控制：基函数的数目和样本的数目。尽管为对数极大似然估计加上一个正则项（或者是参数的先验分布），在一定程度上可以限制模型的复杂度，防止过拟合，但基函数的选择对模型的性能仍然起着决定性的作用。

\----由于极大似然估计总是会使得模型过于的复杂以至于产生过拟合的现象，所以单纯的适用极大似然估计并不是特别的有效。

\----交叉验证是一种有效的限制模型复杂度，防止过拟合的方法，但是交叉验证需要将数据分为训练集合测试集，对数据样本的浪费也是非常的严重的。

贝叶斯线性回归不仅可以解决极大似然估计中存在的过拟合的问题，而且，它对数据样本的利用率是100%，仅仅使用训练样本就可以有效而准确的确定模型的复杂度。线性回归模型是一组输入变量x的基函数的线性组合，在数学上其形式如下：

$$
y(x,w)=w_{0}+\sum_{j=1}^{M}\omega_{j}\phi_{j}(x)
$$

这里$$\phi_j(x)$$就是基函数(输入向量x的基函数f(x)的线性组合)，总共的基函数的数目为M个，如果定义$$ϕ_0(x)=1$$的话，那个上面的式子就可以简单的表示为：

$$
y(x,w)=\sum_{j=0}^{M}\omega_{j}\phi_{j}(x)=w^T\phi(x)
$$

$$
w=(w_{0},w_{1},w_{2},...,w_{M})
$$

$$\phi=(\phi_{0},\phi_{1},\phi_{2},...,\phi_{M})$$ 则线性模型的概率表示如下

$$
p(t|x,w,\beta)=N(t|y(x,w),\beta^{-1}I)
$$

假设参数w满足高斯分布，这是一个先验分布：

$$
p(w)=N(w|0,\alpha^{-1}I)
$$

一般来说，我们称p(w)为共轭先验(conjugate prior)。这里t是x对应的目标输出. $$\beta^{-1},\alpha^{-1}$$别对应于样本集合和w的高斯分布的方差，w是参数， 那么，线性模型的对数后验概率函数：

$$
ln p(\theta|D)=ln p(w|T)=-\frac{\beta}{2}\sum_{n=1}^N\{y(x_{n},w)-t_{n}\}^2+\frac{\alpha}{2}w^Tw+const
$$

这里T是数据样本的目标值向量，T={t1,t2,...,tn}，const是和参数w无关的量。



