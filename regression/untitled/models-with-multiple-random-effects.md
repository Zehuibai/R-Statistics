# Models with Multiple Random Effects

## Introduction

经常进行需要具有两个或多个随机模型效应的模型的研究。这些可以分为两大类：具有两个或多个阻断标准但只有一个尺寸实验单元的研究设计和具有一个以上尺寸实验单元的研究设计。行和列设计（例如拉丁方）是前者的最常见示例。分割图实验（也称为分层设计或多层设计）是后者的常见示例。

* study designs with two or more blocking criteria but only one size experimental unit **(**Row and column designs such as Latin squares**)**
* study designs with more than one size experimental unit.(Split-plot experiments, also called hierarchical or multilevel designs)

具有两个或多个阻塞标准但只有一个尺寸的实验单元的设计可能涉及单向或多向处理设计。具有多个尺寸实验单元的设计几乎总是涉及某种形式的因子或多因子嵌套设计

Designs with two or more blocking criteria but only one size experimental unit may involve one-way or multiway treatment designs. Designs with more than one size experimental unit almost always involve some form of factorial or multifactor nested design.

## Possible Design Structures for 2 × 2 Factorial Treatment Design

显示了进行2×2阶乘处理设计的七种不同方法，每种方法都会导致不同的模型和分析。 2×2是最简单的阶乘处理结构

![](broken-reference)

### **1.Completely Randomized Design (CRD)**&#x20;

四个A×B处理组合中的每一个都随机分配给四个单元

![](broken-reference)

```
class a b;
model y = a b a*b;
```

### **2.Randomized Complete Block**&#x20;

我们假设有一个标准可以将16个单位分为四个块，每个块有四个单元。 将处理组合随机分配给每个块中的单元，以便每个处理组合在每个块中仅出现一次。

CRD和RCB之间的区别在于，只要为每个治疗组合分配四个单位，就不会限制对CRD中单位的治疗分配，而对RCB中的治疗分配则受到限制，因此每个治疗只出现一次 在每个块中。

![](broken-reference)

```
class block a b;
model y = a b a*b;
random block;
```

### **3.**Row and Column Designs (Latin Square)

拉丁方要求分配处理，以便每个处理组合在每一行中出现一次，在每一列中出现一次。

![](broken-reference)

```
class row col a b;
model y = a b a*b;
random row col;
```

### 4.Split-Plot, Whole-Plot as CRD

分割图系列中的设计结构是不同的, 也称为分层或多级设计. 具有两个或多个尺寸的实验单位，这些尺寸是由两个或多个不同的随机化步骤得出的。难以拟合混合模型的最常见原因之一是设计结构与试图拟合的模型之间的断开。

先选A再选B

![](broken-reference)

```
class eu a b;
model y = a b a*b;
random eu(a);
```

### 5.Strip-Split-Plot

该设计与拆分图的不同之处在于，实验单元有三种尺寸，一种用于因子A的水平，一种用于因子B的水平，一种用于A×B组合。 后者是由A和B级的随机分配产生的，而不是由明显的随机化产生的。

![](broken-reference)

```
class block a b;
model y = a b a*b;
random block block*a block*b;
```

## Inference with Factorial Treatment Designs

The treatment mean is typically decomposed into main effects and interaction terms

$$
\mu_{i j}=\mu+\alpha_{i}+\beta_{j}+\alpha \beta_{i j},
$$

* where $$\mu$$ is the overall mean or intercept,&#x20;
* $$\alpha_{i}$$ and $$\beta_{j}$$ are the main effects of $$\mathrm{A}$$ and $$\mathrm{B}$$, respectively,&#x20;
* $$\alpha \beta_{i j}$$ is the $$\mathrm{A} \times \mathrm{B}$$ interaction term.

**For the CRD**, there is no blocking, the model can be written as follows:

$$
y_{i j k}=\mu_{i j}+e_{i j k}=\mu+\alpha_{i}+\beta_{j}+\alpha \beta_{i j}+e_{i j k}
$$

where $$e_{i j k}$$ is typically assumed iid $$\mathrm{N}\left(0, \sigma^{2}\right) .$$ This is the classic two-way ANOVA model.

**For the RCB**, there is blocking and there is only one size of experimental unit assigned to A × B treatment combinations, e, the experiment design component is decomposed as$$E_{i j k}=r_{k}+e_{i j k},$$ where $$r_{k}$$ denotes the $$k^{\text {th }}$$**block effect** and $$e_{i j k}$$ denotes experimental error. The model is thus

$$
y_{i j k}=\mu_{i j}+r_{k}+e_{i j k}=\mu+\alpha_{i}+\beta_{j}+\alpha \beta_{i j}+r_{k}+e_{i j k}
$$

....

For Split-Plot, Whole-Plot as RCBRD,&#x20;

