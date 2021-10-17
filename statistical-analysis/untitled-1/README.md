---
description: Power and sample size calculation
---

# Sample Size

## Basic Introduction

### Binary outcome

Binary outcome means that every subject has either (1= event) or (0= no event). The event probability is the probability that a subject has the event. The aim is to show that two groups differ with respect to the event probability.

#### Power for given sample size

Aim: to compute the power of a study to show a difference between group 1 (n=28) in which the event probability is 30% and group 2 (n=28) in which the event probability is 55%.

```
power.prop.test(n=28,p1=0.3,p2=0.55) 
```

```
## 
##      Two-sample comparison of proportions power calculation 
## 
##               n = 28
##              p1 = 0.3
##              p2 = 0.55
##       sig.level = 0.05
##           power = 0.4720963
##     alternative = two.sided
## 
## NOTE: n is number in *each* group
```

#### Sample size for given power

Aim: To compute the sample size of a study to show a difference between group 1 (n=28) in which the event probability is 30% and group 2 (n=28) in which the event probability is 55% with a power of 80%.

```
power.prop.test(power=0.8,p1=0.3,p2=0.55) 
```

```
## 
##      Two-sample comparison of proportions power calculation 
## 
##               n = 60.18568
##              p1 = 0.3
##              p2 = 0.55
##       sig.level = 0.05
##           power = 0.8
##     alternative = two.sided
## 
## NOTE: n is number in *each* group
```

### Continuous outcome

For the power and sample size calculations illustrated here the distribution of the outcome variable is characterized by its mean and standard deviation.

#### Power for given sample size

Aim: to compute the power of a study which aims to show a difference in means between group 1 (n=6) and group 2 (n=6) assuming that the magnitude of the difference is 0.3 units and the standard deviation is 0.28 units.

```
power.t.test(n=6,delta=0.3,sd=0.28,type="two.sample") 
```

```
## 
##      Two-sample t test power calculation 
## 
##               n = 6
##           delta = 0.3
##              sd = 0.28
##       sig.level = 0.05
##           power = 0.3890758
##     alternative = two.sided
## 
## NOTE: n is number in *each* group
```

#### Sample size for a given power

Aim: to compute the sample size needed to achieve a power of 90% in a study which aims to show a difference in means between two independent groups assuming that the magnitude of the difference is 0.3 units and the standard deviation is 0.28 units.

```
power.t.test(power=0.9,delta=0.3,sd=0.28,type="two.sample") 
```

```
## 
##      Two-sample t test power calculation 
## 
##               n = 19.3192
##           delta = 0.3
##              sd = 0.28
##       sig.level = 0.05
##           power = 0.9
##     alternative = two.sided
## 
## NOTE: n is number in *each* group
```











