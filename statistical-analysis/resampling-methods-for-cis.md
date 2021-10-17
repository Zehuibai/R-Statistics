# Resampling Methods for CIs

假设有一个随机样本的观测值。感兴趣的问题是估计基础分布的统计θ。该统计信息可以是单变量（例如均值，方差，中位数，众数），也可以是双变量（例如相关系数）。如果基本分布是已知的，那么我们可以应用发达的理论来找到采样分布，并在参数设置中给出点估计和置信区间。但是，如果对分布情况一无所知，则必须采用非参数方法。考虑两种相关技术：**jackknife and bootstrap.**

当θ是观测值的平滑函数（例如均值，方差或相关系数）时，**jackknife **法效果很好。\
****但是，如果该统计信息不平滑，例如中位数或四分位数，则建议改用**bootstrap**法。此外，**bootstrap**函数在提供θ的整个采样分布估计值（而不仅仅是间隔估计值）的意义上更具通用性。当应用自举方法时，有关采样分布的所有信息都来自样本内部，而没有任何其他假设。

## Jackknife

### Estimation procedure

Let $$\theta$$ denote the sample statistic for the original sample $$x_{1}, \ldots, x_{n} .$$ For a fixed $$j, j=1, \ldots, n$$, introduce the jackknife replicate $$\theta_{j}$$ as the sample statistic computed for the sample with $$x_{j}$$ removed. Define the pseudovalues by $$\widehat{\theta}_{j}^{*}=n \widehat{\theta}-(n-1) \widehat{\theta}_{j}, j=1, \ldots, n$$. The jackknife estimate of $$\theta$$ is the average of the pseudovalues, $$\widehat{\theta}_{J}=\frac{1}{n} \sum_{i=1}^{n} \widehat{\theta}_{j}^{*} .$$ The variance of the pseudovalues is $$\frac{\sum_{i=1}^{n}\left(\widehat{\theta}_{j}^{*}-\widehat{\theta}_{J}\right)^{2}}{(n-1)} .$$ An approximate $$100(1-\alpha) \%$$ confidence interval for $$\theta$$ based on the $$t$$ -distribution is computed as

$$
\widehat{\theta}_{J} \pm t_{\alpha / 2, n-1} \sqrt{\frac{\sum_{i=1}^{n}\left(\widehat{\theta}_{j}^{*}-\widehat{\theta}_{J}\right)^{2}}{n(n-1)}} .
$$

### SAS Implementation

* Step 1a. If we need to compute a univariate statistic such as mean or median, we use the UNIVARIATE procedure, Next, we invoke the CALL SYMPUT routine to create macro variables containing the values of $$n$$ and $$\hat θ$$ as they will be needed later
* Step 1b. If the desired statistic θ is a correlation coefficient, then the CORR procedure may be called
* ```
  PROC UNIVARIATE DATA=data name;
  VAR variable name;
  OUTPUT OUT=outdata name N=n name statistic keyword=statistic name;
  RUN;

  DATA NULL ;
  SET outdata name;
  CALL SYMPUT (’n macro name’, n name);
  CALL SYMPUT (’statistic macro name’, statistic name);
  RUN;


  PROC CORR DATA=data name OUTP=statistic data name;
  VAR variable1 name variable2 name;
  RUN;

  DATA NULL ;
  SET statistic data name;
  IF TYPE =’N’ THEN CALL SYMPUT (’n macro name’, variable1 name);
  IF ( TYPE =’CORR’ AND NAME =’variable1 name’)
  THEN CALL SYMPUT (’correlation macro name’, variable2 name);
  RUN;
  ```
* Step 2. We generate the jackknife samples by removing one observation at a time.
* ```


  DATA jackknife samples name;
  DO sample count=1 TO &n macro name;
  DO record count=1 TO &n macro name;
  SET data name POINT=record count;
  IF sample count NE record count THEN OUTPUT;
  END;
  END;
  STOP;
  RUN;
  ```
* Step 3. We compute the jackknife replicates $$\widehat{\theta}_{j}, j=1, \ldots, n$$ 
* ```
  ## The syntax for the UNIVARIATE procedure is
  PROC UNIVARIATE DATA=jackknife samples name;
  VAR variable name;
  BY sample count;
  OUTPUT OUT=jackknife replicates name statistic keyword=statistic name;
  RUN;

  ## The syntax for the procedure CORR is
  PROC CORR DATA=jackknife samples name; OUTP=jackknife replicates name;
  VAR variable1 name variable2 name;
  BY sample count;
  RUN;
  ```
* Step 4. We calculate the pseudovalues $$\widehat{\theta}_{j}^{*}, j=1, \ldots, n$$ 
* ```
  ## The syntax for the UNIVARIATE procedure is
  DATA jackknife replicates name;
  SET jackknife replicates name;
  pseudovalue name = &n macro name * statistic macro name- (&n macro name - 1) * statistic name;
  BY sample count;
  KEEP pseudovalue name;
  RUN;

  ## The syntax for the procedure CORR is
  DATA jackknife replicates name;
  SET jackknife replicates name;
  IF ( TYPE =’CORR’ AND NAME =’variable1 name’);
  pseudovalue name = &n macro name * correlation macro name-(&n macro name - 1) * variable2 name;
  KEEP pseudovalue name;
  RUN;
  ```
* We construct a $$100(1-\alpha) \%$$ confidence interval for $$\theta$$ based on the $$t$$ $$\underline{\text { Step }}$$ distribution. The syntax is
* ```
  PROC MEANS DATA=jackknife replicates name ALPHA=value;
  VAR pseudovalue name;
  OUTPUT OUT=confidence interval name LCLM=lower limit name
  UCLM=upper limit name;
  RUN;

  PROC PRINT data=confidence interval name;
  RUN;
  ```

#### Examples

```
data snack_shack;
input revenue @@;
datalines;
437.94 387.51 400.48 403.16 350.87 408.43 275.94
470.83 173.96 423.90 173.70 462.77 343.58 425.04
168.63 392.00 368.24 310.25 403.15 177.03 408.19
175.33 320.00 185.09 462.46 197.78 276.34 392.71
435.85 283.82 383.30 188.31 460.30 180.14 473.08
177.94 457.38 185.24 352.75 400.32 371.00 372.95
425.95 358.55 380.65 377.22 375.36 280.11 450.68
410.33 370.11 380.32 343.44 400.26 227.33 440.37
405.25 425.57 333.21 200.34 433.23 293.51 458.92
190.42 358.06 373.27 373.83 182.70 463.49 350.00
400.04 367.26 167.29 460.23 167.22 400.34 180.03
442.55 190.44 463.85 283.61 350.64 197.66 428.97
183.94 413.37 183.18 465.96 420.45 393.85 433.92
183.60 453.68 203.80 418.52 443.48 407.45 413.35
395.71 410.32 272.41 458.21 283.21 450.92 195.69
223.75 412.15 213.03 240.43 287.79 297.32 296.89
;


proc univariate data=snack_shack;
var revenue;
output out=stats n=n_obs mean=mean_all;
run;
data _null_;
set stats;
call symput (’n_obs’, n_obs);
call symput (’mean_all’, mean_all);
run;
data jackknife_samples;
do sample=1 to &n_obs;
do record=1 to &n_obs;
set snack_shack point=record;
if sample ne record then output;
end;
end;
stop;
run;

proc univariate data=jackknife_samples;
var revenue;
by sample;
output out=jackknife_replicates mean=mean_revenue;
run;
data jackknife_replicates;
set jackknife_replicates;
pseudomean=&n_obs*&mean_all-(&n_obs-1)*mean_revenue;
by sample;
keep pseudomean;
run;

proc means data=jackknife_replicates;
var pseudomean;
output out=CI LCLM=CI_95_lower UCLM=CI_95_upper;
run;
proc print data=CI;
run;
```

## **Bootstrap**

### Estimation Procedure

自举算法规定从原始样本x1，...，xn中抽取大小为n的B个随机样本作为替换 ，因此创建了B个引导程序样本。 对于这些样本中的每一个，我们都计算感兴趣的统计量$$\hat θ_b，b = 1，...，B$$，称为引导复制。

A confidence interval for $$\theta$$ may be constructed directly based on the bootstrap replicates. Denote by $$\widehat{\theta}_{L, \alpha / 2}$$ and $$\widehat{\theta}_{U, \alpha / 2}$$ the $$\alpha / 2$$ and $$1-\alpha / 2$$ percentiles of the bootstrap replicates, respectively. The $$100(1-\alpha) \%$$ **Efron's percentile confidence interval** is

$$
\left(\widehat{\theta}_{L, \alpha / 2}, \widehat{\theta}_{U, \alpha / 2}\right)
$$

At least 1,000 bootstrap samples are required for a $$95 \%$$ confidence interval (**至少需要1,000个引导程序样本**), and at least $$5,000,$$ for a $$99 \%$$ confidence interval.

### SAS Implementation

```
data snack_shack;
input revenue @@;
datalines;
437.94 387.51 400.48 403.16 350.87 408.43 275.94
470.83 173.96 423.90 173.70 462.77 343.58 425.04
168.63 392.00 368.24 310.25 403.15 177.03 408.19
175.33 320.00 185.09 462.46 197.78 276.34 392.71
435.85 283.82 383.30 188.31 460.30 180.14 473.08
177.94 457.38 185.24 352.75 400.32 371.00 372.95
425.95 358.55 380.65 377.22 375.36 280.11 450.68
410.33 370.11 380.32 343.44 400.26 227.33 440.37
405.25 425.57 333.21 200.34 433.23 293.51 458.92
190.42 358.06 373.27 373.83 182.70 463.49 350.00
400.04 367.26 167.29 460.23 167.22 400.34 180.03
442.55 190.44 463.85 283.61 350.64 197.66 428.97
183.94 413.37 183.18 465.96 420.45 393.85 433.92
183.60 453.68 203.80 418.52 443.48 407.45 413.35
395.71 410.32 272.41 458.21 283.21 450.92 195.69
223.75 412.15 213.03 240.43 287.79 297.32 296.89
;


*** We resort to the SURVEYSELECT procedure to draw bootstrap samples.
    This procedure is designed to draw random samples by a variety of methods. For
    bootstrapping, we need the method of sampling with replacement;
proc surveyselect data=snack_shack out=bootstrap_samples
outhits seed=3027574 method=urs samprate=1 rep=1000;
run;

proc univariate noprint data=bootstrap_samples;
var revenue;
by replicate;
output out=bootstrap_replicates mean=mean_revenue;
run;

proc univariate data=bootstrap_replicates;
var mean_revenue;
output out=CI pctlpre=CI_95_ pctlpts=2.5 97.5
pctlname=lower upper;
run;

proc print data=CI;
run;
```
