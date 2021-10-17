# Wilcoxon rank-sum test

## Wilcoxon rank-sum test/Mann–Whitney U test

Wilcoxon符号秩检验是非独立样本t检验的一种非参数替代方法。它适用于两组成对数据和无法保证正态性假设的情境。 调用格式与Mann–Whitney U检验完全相同，不过还可以添加参数 paired=TRUE。

A very general formulation is to assume that:

1. All the observations from both groups are independent of each other,
2. The responses are ordinal (i.e., one can at least say, of any two observations, which is the greater),
3. Under the null hypothesis H0H0, the distributions of both populations are equal.
4. The alternative hypothesis H1H1 is that the distributions are not equal.

### Ordered categorical responses

computes sample size for the two-sided Wilcoxon test when applied to two independent samples with ordered categorical responses.

* power: required Power
* alpha: required two-sided Type-I-error level
* t: sample size fraction n/N, where n is sample size of group B and N is the total sample size
* p: vector of expected proportions of the categories in group A, should sum to 1
* q: vector of expected proportions of the categories in group B, should be of equal length as p and should sum to 1

```
library("samplesize")
library("samplesize")
n.wilcox.ord(power = 0.8, alpha = 0.05, 
             t = 0.53, 
             p = c(0.66, 0.15, 0.19), 
             q = c(0.61, 0.23, 0.16))
```

```
## $`total sample size`
## [1] 8390
## 
## $m
## [1] 3943
## 
## $n
## [1] 4447
```

### Precise and Accurate Power for a Continuous Variable

**shiehpow**

perform a power analysis for a one or two-sided Wilcoxon-Mann-Whitney test using the method developed by Shieh and colleagues.

* n: Sample size of first sample (numeric)
* m: Sample size of second sample (numeric)
* p: Effect size, P(X\<Y) (numeric)
* dist: The distribution type for the two groups (“exp”, “dexp”, or “norm”) (string)
* sides: Options are “two.sided” and “one.sided” (string)

> 我们要计算统计功效以比较DNA突变之间的距离分成两组人。 每个组（X和Y）有10个人。 我们假设第一组突变之间的距离以比率3指数分布。我们假定第一组突变的距离小于第二个突变突变的概率（即P（X \<Y））为0.8 。 所需的I型错误为0.05。

```
library("wmwpow")
shiehpow(n = 10, m = 10, p = 0.80, alpha = 0.05, dist = "exp", sides = "two.sided")
```

```
## Distribution: exp
## Sample sizes: 10 and 10
## p: 0.8
## WMW odds: 4
## sides: Two-sided
## alpha: 0.05
## 
## Shieh Power: 0.65
```

**wmwpowd**

Precise and Accurate Monte Carlo Power Calculation by Inputting Distributions F and G (wmwpowd)

1. Calculate the power for a one-sided or two-sided Wilcoxon-Mann-Whitney test with an empirical p-value given two user specified distributions.
2. Calculate p, the P(X\<Y), where X represents random draws from one continuous probability distribution and Y represents random draws from another distribution; p is useful for quantifying the effect size that the Wilcoxon-Mann-Whitney test is assessing.

> 给定两个用户指定的分布，使用经验p值计算单面或双面Wilcoxon-Mann-Whitney检验的功效。计算p，即P（X \<Y），其中X代表来自一个连续概率分布的随机抽取，Y代表来自另一分布的随机抽取； p对量化Wilcoxon-Mann-Whitney检验正在评估的效应大小很有用。

```
wmwpowd(106, 53, 
        "norm(3,1.8)", "norm(2,1.8)", 
        "two.sided", alpha = 0.05, nsims = 100)
```

```
## Supplied distribution 1: norm(3,1.8); n = 106
## Supplied distribution 2: norm(2,1.8); m = 53
## 
## p: 0.347
## WMW odds: 0.531
## Number of simulated datasets: 100
## Two-sided WMW test (alpha = 0.05)
## 
## Empirical power: 0.9
```

```
## The output WMWOdds is p expressed as odds p/(1-p)
```

**wmwpowp**

Precise and Accurate Monte Carlo Power Calculation by Inputting P, where p is the effect size, i.e., the probability that the first random variable is less than the second random variable (P(X\<Y)) (numeric)

> 通过输入P进行精确精确的蒙特卡洛幂计算，其中p是效果大小，即第一随机变量小于第二随机变量（P（X \<Y））的概率（数字）

```
wmwpowp(n = 10, m = 10, 
        distn = "exp(3)", p = 0.8, 
        sides = "two.sided", alpha = 0.05)
```

```
## Supplied distribution: exp(3); n = 10
## Shifted distribution: exp(0.75); m = 10
## 
## p: 0.8
## WMW odds: 4
## Number of simulated datasets: 10000
## Two-sided WMW test (alpha = 0.05)
## 
## Empirical power: 0.65
```

#### Prior

calculates the sample size for a given power, type-I error rate and allocation rate t = n1/N.

* x: prior information for the first group
* y: prior information for the second group
* alpha: two sided type I error rate
* t: proportion of subjects in the first group; or use t = “min” to use optimal proportion rate

```
set.seed(12345)
x <- rnorm(100, mean = 3, sd = 1.8)
y <- rnorm(100, mean = 2, sd = 1.8)

library("WMWssp")
ssp <- WMWssp(x, y, alpha = 0.05, power = 0.9, t = 2/3)
summary(ssp)
```

```
## Wilcoxon-Mann-Whitney Sample Size Calculation
##  
## Summary
## Call: WMWssp
## Type-I error (two-sided): 0.05
## Power: 0.9
## 
##                                 Results
## alpha (2-sided)               0.0500000
## Power                         0.9000000
## Estimated relative effect p   0.3088000
## N (total sample size needed) 98.7383955
## t=n1/N                        0.6666667
## n1 in Group 1                65.8255970
## n2 in Group 2                32.9127985
## N rounded                    99.0000000
## n1 rounded                   66.0000000
## n2 rounded                   33.0000000
```
