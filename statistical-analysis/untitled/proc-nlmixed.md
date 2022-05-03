# Proc NLMIXED

GLM (generalized linear model)对协方差结构有极端的假定。当需要估计尽可能多的方差及协方差参数时，GLM 模型有一定的局限性，而基于似然函数法原理的混合效应线性模型分析方法可满足不受协方差参数限制的要求，而且MIXED模型还可以分析观察时间点不相等的资料，也能充分利用具有缺失观察值的资料，它允许资料**存在某种相关性及协方差矩阵的多样性，更适应重复测量资料的特点**。一般线性模型(GLM)要求误差项e具有独立一致的正态分布，而许多实际资料不能满足这一假定。Mixed模型允许误差项具有更灵活的结构，包括相关性和方差的不齐 性。混合效应线性模型(proc mixed)采用最大似然估计法(maximum likelihood' ML）和约束最大似然估计法（restricted maximum likelihood，REML)原理计算协方差矩阵。

## Introduction

Introduction The NLMIXED procedure fits nonlinear mixed models—that is, models in which both fixed and random effects enter nonlinearly. These models have a wide variety of applications, two of the most common being pharmacokinetics and overdispersed binomial data. PROC NLMIXED enables you to specify a conditional distribution for your data (given the random effects) having either a standard form (normal, binomial, Poisson)

> NLMIXED过程适合非线性混合模型，即固定和随机效应都非线性输入的模型。这些模型具有广泛的应用，其中最常见的两个是药代动力学和过度分散的二项式数据。 PROC NLMIXED使您可以为数据指定条件分布（考虑到随机效应），该条件分布具有标准格式（正态，二项式，泊松）

PROC NLMIXED fits nonlinear mixed models by **maximizing an approximation to the likelihood integrated over the random effects**. Different **integral approximations** are available, the principal ones being **adaptive Gaussian quadrature** and a **first-order Taylor series approximation**. A variety of alternative optimization techniques are available to carry out the maximization; the default is a **dual quasi-Newton algorithm**.

> PROC NLMIXED通过“最大化对随机效应积分的似然性的近似值”来拟合非线性混合模型。可以使用不同的“积分近似”，主要的是“自适应高斯正交”和“一阶泰勒级数近似”。可以使用多种替代性优化技术来实现最大化。默认为“双重拟牛顿算法”。

Successful convergence of the optimization problem results in parameter estimates along with their approximate standard errors based on the second derivative matrix of the likelihood function. PROC NLMIXED enables you to use the estimated model to construct predictions of arbitrary functions by using empirical Bayes estimates of the random effects.

> 优化问题的成功收敛导致参数估计以及基于似然函数的二阶导数矩阵的近似标准误差。 PROC NLMIXED使您可以使用估计的模型通过使用随机效应的经验贝叶斯估计来构造任意函数的预测。



* [Zero-inflated Poisson and negative binomial using proc nlmixed](https://stats.idre.ucla.edu/sas/code/zero-inflated-poisson-and-negative-binomial-using-proc-nlmixed/)
* [Logistic, random intercept, and random slope regression models using nlmixed](https://stats.idre.ucla.edu/sas/code/logistic-random-intercept-and-random-slope-regression-models-using-nlmixed/)
* [Estimation options in nlmixed](https://stats.idre.ucla.edu/sas/code/estimation-options-in-nlmixed/)
