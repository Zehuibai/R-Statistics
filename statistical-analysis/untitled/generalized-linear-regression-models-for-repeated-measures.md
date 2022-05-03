# Generalized Linear Regression Models for Repeated Measures

广义线性混合效应和GEE模型是回归模型，分别扩展了正态分布响应变量的随机斜率和截距 (_**Random Slope and Intercept Model**_) 和**GEE**模型的类别。 通用模型可用于对具有偏斜，分类或计数响应的重复测量数据进行建模。

具有**误差协方差结构**的随机斜率和截距模型 (_**Random Slope and Intercept Model with Covariance Structure**_) 不适用于这种情况，因为具有非正态分布响应的数据模型不包含误差项。

## Generalized Random Slope and Intercept Model 

### Model Definition 

广义随机斜率和截距模型（也称为广义线性混合模型）是线性混合效应模型对指数分布族中响应变量的任何分布以及相应链接函数的泛化 (exponential family of distributions, and the corresponding link function.)

* Gamma
* Binary logistic
* Poisson
* Negative binomial

Assume that the data were collected longitudinally at times $$t_{1}, \ldots, t_{p},$$ and that for the $$i$$ -th individual at time $$t_{j}$$ the observations are $$x_{1 i j}, \ldots, x_{k i j},$$ and $$y_{i j}$$ where $$i=1, \ldots, n$$ and $$j=1, \ldots, p,$$ and $$y_{i j}$$ 's have one of the aforementioned distributions. The generalized mixed-effects model in each case has two random additive terms in the linear regression expression:

$$
\beta_{0}+\beta_{1} x_{1 i j}+\cdots+\beta_{k} x_{k i j}+\beta_{k+1} t_{j}+u_{1 i}+u_{2 i} t_{j}
$$

where $$u_{1 i}$$ 's are independent $$\mathcal{N}\left(0, \sigma_{u_{1}}^{2}\right)$$ random intercepts, $$u_{2 i}$$ 's are independent $$\mathcal{N}\left(0, \sigma_{u_{2}}^{2}\right)$$ random slopes, $$\mathbb{C o v}\left(u_{1 i}, u_{2 i}\right)=\sigma_{u_{1} u_{2}},$$ and $$\mathbb{C} o v\left(u_{1 i}, u_{2 i^{\prime}}\right)=0$$ for $$i \neq i^{\prime}$$

### SAS Implementation  

```
proc glimmix data=data name method=Laplace;
    class catpredictor1 name (ref='level name') catpredictor2 name (ref='level name') . . . ;
    model response name=<list of predictors>/dist=dist name link=link type solution;
    output out=outdata pred(ilink)=predicted name;
    random intercept time name/subject=id name type=un;
    covtest/wald;
run;
```

* 选项method = Laplace指定参数估计的方法，其中输出包含-2 \* log-likelihood的值。 该值对于计算模型偏差和进行模型拟合测试是必需的
* To model level ‘1’ in logistic regression, include (event='1') after response name in the model statement.
* The choices for the distributions and corresponding links are: `dist=gamma link=log, `\
  `dist=binomial link=logit, `\
  `dist=poisson link=log, `\
  `dist=nb link=log, `\
  `dist=beta link=logit.`
* The statement covtest/wald should be added to request the Wald z-test results for the parameters of the random-effects terms.

```
data psoriasis;
input patid group$ day1 week1 week2 week5 month3 @@;
cards;
1 Tx 15 12 9 3 0 2 Tx 24 17 9 8 8
3 Cx 14 14 15 15 14 4 Cx 23 20 19 19 15
5 Tx 11 10 3 2 0 6 Cx 11 10 8 7 5
7 Tx 7 6 5 3 0 8 Tx 9 6 2 0 0
9 Cx 9 9 9 9 9 10 Cx 21 16 16 15 15
;

data longform;
set psoriasis;
array w{5}(0.14 1 2 5 13);
array x{5} day1 week1 week2 week5 month3;
do i=1 to 5;
weeks=w{i};
npatches=x{i};
output;
end;
keep patid group weeks npatches;
run;

proc glimmix method=Laplace;
class group;
model npatches=group weeks/solution
dist=poisson link=log;
random intercept weeks/subject=patid type=un;
covtest/wald;
run;
```

### R Implementation  

To fit a generalized random slope and intercept model in R, the function `glmer() `in the library `lme4 `is used

`glmer(data = , formula = , family = ,...)`

其中 `family = `有多种不同的选择(**注意是字符型的**)，分别如下:

* `binomial` - `link = “logit”`（如果自变量为0，1变量，`family`要设置为`binomial`）;
* `gaussian` - `link = "identity"`;
* `gamma` - `link = "inverse"`;
* `inverse.gaussian` - `link = "1/mu^2"`;
* `poisson` - `link = "log"`;
* `quasi` - `link = "identity", variance = "constant"`;
* `quasibinomial` - `link = "logit"`;
* `quasipoisson` - `link = "log"`;

```
load('C:/Users/zbai/Desktop/data/speed_dating.RData')

sd_model = glmer(
  decision ~ sex + samerace + attractive_sc + sincere_sc + intelligent_sc
  + (1 | iid),
  data   = speed_dating,
  family = binomial
)

summary(sd_model, correlation = FALSE)


Generalized linear mixed model fit by maximum likelihood (Laplace Approximation) ['glmerMod']
 Family: binomial  ( logit )
Formula: decision ~ sex + samerace + attractive_sc + sincere_sc + intelligent_sc +      (1 | iid)
   Data: speed_dating

     AIC      BIC   logLik deviance df.resid 
  6944.5   6992.9  -3465.3   6930.5     7359 

Scaled residuals: 
     Min       1Q   Median       3Q      Max 
-11.6136  -0.4836  -0.1212   0.4865  20.9732 

Random effects:
 Groups Name        Variance Std.Dev.
 iid    (Intercept) 2.708    1.645   
Number of obs: 7366, groups:  iid, 498

Fixed effects:
               Estimate Std. Error z value Pr(>|z|)    
(Intercept)    -0.74340    0.12128  -6.129 8.82e-10 ***
sexMale         0.15634    0.16397   0.953     0.34    
sameraceYes     0.31357    0.07481   4.192 2.77e-05 ***
attractive_sc   0.50166    0.01495  33.559  < 2e-16 ***
sincere_sc      0.08935    0.01555   5.747 9.07e-09 ***
intelligent_sc  0.14314    0.01739   8.232  < 2e-16 ***
```

## Generalized Estimating Equations Model

### Model Definition 

See Before

### SAS Implementation  

In SAS, proc genmod is used to fit GEEs. Allowable distributions and link types are: `dist=gamma link=log, dist=bin link=logit, dist=poisson link=log, and dist=nb link=log. `When the distribution is logistic, include descending in the proc statement to model the probability of` level ’1’`

```
data psoriasis;
input patid group$ day1 week1 week2 week5 month3 @@;
cards;
1 Tx 15 12 9 3 0 2 Tx 24 17 9 8 8
3 Cx 14 14 15 15 14 4 Cx 23 20 19 19 15
5 Tx 11 10 3 2 0 6 Cx 11 10 8 7 5
7 Tx 7 6 5 3 0 8 Tx 9 6 2 0 0
9 Cx 9 9 9 9 9 10 Cx 21 16 16 15 15
;
data longform;
set psoriasis;
array w{5}(0.14 1 2 5 13);
array x{5} day1 week1 week2 week5 month3;
do i=1 to 5;
weeks=w{i};
npatches=x{i};
output;
end;
keep patid group weeks npatches;
run;

/*fitting GEE with Toeplitz working
correlation matrix*/
proc genmod;
class patid group;
model npatches=group weeks/dist=poisson link=log;
repeated subject=patid/type=mdep(3) corrw;
run;
```

### R Implementation 

In R, the function geeglm() is employed for fitting a Generalized Estimating Equations model.

```
summary(ar1.fitted.model<- geeglm(npatches ~ group.rel + weeks,
data=longform.data, id=patid,
family=poisson(link="log"), corstr="ar1"))
QIC(ar1.fitted.model)
```
