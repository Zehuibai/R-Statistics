# Regression Algorithm

## Unsupervised learning

* supervised learners which construct predictive models
* unsupervised learners which build descriptive models.

### Supervised learning

A predictive model is used for tasks that involve the prediction of a given output (or target) using other variables (or features) in the data set. 

> 预测模型用于涉及使用数据集中其他变量（或特征）预测给定输出（或目标）的任务。

* X: “Predictor variable”, “independent variable”, “attribute”, “feature”, “predictor”
* Y: “Target variable”, “dependent variable”, “response”, “outcome measurement”

### Unsupervised learning

与监督学习相反，无监督学习包括一组统计工具，可以更好地理解和描述您的数据，但无需目标变量即可执行分析。本质上，无监督学习与确定数据集中的组有关。可以通过行（即聚类 clustering）或列（即降维 dimension reduction）来定义组。但是，每种情况的动机都大不相同。

无监督学习通常是探索性数据分析（EDA）的一部分。但是，该练习往往更加主观，并且没有简单的分析目标，例如预测响应。此外，可能难以评估从无监督学习方法获得的结果的质量。这样做的原因是简单的。如果我们使用监督学习技术（即线性回归）拟合预测模型，则可以通过查看我们的模型对未用于拟合模型的观测值预测响应Y的程度来检查我们的工作。但是，在无监督学习中，由于我们不知道真正的答案，因此无法检查我们的工作

* The goal of clustering is to segment observations into similar groups based on the observed variables; for example, to divide consumers into different homogeneous groups, a process known as market segmentation. 

> 聚类的目的是根据观察到的变量将观察结果分成相似的组。例如，将消费者分为不同的同质群体，这一过程称为市场细分。

* In dimension reduction, we are often concerned with reducing the number of variables in a data set. For example, classical linear regression models break down in the presence of highly correlated features. Some dimension reduction techniques can be used to reduce the feature set to a potentially smaller set of uncorrelated variables. Such a reduced feature set is often used as input to downstream supervised learning models (e.g., principal component regression). 

> 在降维中，我们经常关注减少数据集中变量的数量。例如，经典线性回归模型在存在高度相关的特征时会崩溃。某些降维技术可用于将特征集缩减为可能较小的一组不相关变量。这种减少的特征集通常用作下游监督学习模型的输入（例如，主成分回归）。

##



## Model evaluation 

Historically, the performance of statistical models was largely based on **goodness-of-fit tests and assessment of residuals**. Unfortunately, misleading conclusions may follow from predictive models that pass these kinds of assessments (Breiman and others 2001). Today, it has become widely accepted that a more sound approach to assessing model performance is to** assess the predictive accuracy via loss functions**. Loss functions are metrics that compare the predicted values to the actual value (the output of a loss function is often referred to as the **error or pseudo residual**). When performing **resampling **methods, we assess the **predicted values for a validation set compared to the actual target value**. For example, in regression, one way to measure error is to take the difference between the actual and predicted value for a given observation (this is the usual definition of a residual in ordinary linear regression). The overall validation error of the model is computed by aggregating the errors across the entire validation data set.

> 统计模型的性能主要基于拟合优度检验和残差评估。不幸的是，通过这些评估的预测模型可能会得出误导性的结论（Breiman等人，2001）。如今，一种更为合理的评估模型性能的方法已被广泛接受，即通过**损失函数**评估预测准确性。**损失函数是将预测值与实际值进行比较的度量（损失函数的输出通常称为误差或伪残差）**。在执行重采样方法时，我们将评估验证集的预测值与实际目标值进行比较。例如，在回归中，一种测量误差的方法是获取给定观察值的实际值与预测值之差（这是普通线性回归中对残差的通常定义）。通过汇总整个验证数据集中的误差来计算模型的总体验证误差。

There are many loss functions to choose from when assessing the performance of a predictive model, each providing a unique understanding of the predictive accuracy and differing between regression and classification models. Furthermore, the way a loss function is computed will tend to emphasize certain types of errors over others and can lead to drastic differences in how we interpret the “optimal model”. Its important to consider the problem context when identifying the preferred performance metric to use. And when comparing multiple models, we need to compare them across the same metric.

> 在评估预测模型的性能时，有很多损失函数可供选择，每种函数都提供对预测准确性的独特理解，并且回归模型和分类模型之间也有所不同。此外，损失函数的计算方式将倾向于强调某些类型的错误，而不是其他类型的错误，并且可能导致我们解释“最佳模型”的方式发生巨大差异。在确定要使用的首选性能指标时，考虑问题上下文非常重要。当比较多个模型时，我们需要在同一指标上进行比较。

### Regression models

* **MSE**: Mean squared error is the average of the squared error $$MSE = \frac{1}{n} \sum^n_{i=1}(y_i - \hat y_i)^2$$ . The squared component results in larger errors having larger penalties. This (along with RMSE) is the most common error metric to use. \
  **Objective: minimize**\
  ****
* **RMSE**: Root mean squared error. This simply takes the square root of the MSE metric ( $$RMSE = \sqrt{\frac{1}{n} \sum^n_{i=1}(y_i - \hat y_i)^2}$$ ) so that your error is in the same units as your response variable. If your response variable units are dollars, the units of MSE are dollars-squared, but the RMSE will be in dollars. \
  **Objective: minimize**\
  ****
* **Deviance**: Short for mean residual deviance. In essence, it provides a degree to which a model explains the variation in a set of data when using maximum likelihood estimation. Essentially this compares a saturated model (i.e. fully featured model) to an unsaturated model (i.e. intercept only or average). If the response variable distribution is Gaussian, then it will be approximately equal to MSE. When not, it usually gives a more useful estimate of error. Deviance is often used with classification models. \
  提供了使用最大似然估计时模型解释一组数据中的变化的程度。 本质上，这会将饱和模型（即功能齐全的模型）与非饱和模型（即仅截取或平均截取）进行比较。 如果响应变量分布是高斯分布，则它将近似等于MSE。 否则，它通常会提供更有用的错误估计。\
  **Objective: minimize**\
  ****
* **MAE**: Mean absolute error. Similar to MSE but rather than squaring, it just takes the mean absolute difference between the actual and predicted values ( $$MAE = \frac{1}{n} \sum^n_{i=1}(\vert y_i - \hat y_i \vert)$$ ). This results in less emphasis on larger errors than MSE. \
  与MSE相比，这导致对较大错误的重视程度降低。\
  **Objective: minimize**\
  ****
* **RMSLE**: Root mean squared logarithmic error. Similar to RMSE but it performs a `log()` on the actual and predicted values prior to computing the difference ( $$RMSLE = \sqrt{\frac{1}{n} \sum^n_{i=1}(log(y_i + 1) - log(\hat y_i + 1))^2}$$ ). When your response variable has a wide range of values, large response values with large errors can dominate the MSE/RMSE metric. RMSLE minimizes this impact so that small response values with large errors can have just as meaningful of an impact as large response values with large errors. \
  当响应变量具有宽范围的值时，具有大误差的大响应值可以主导MSE / RMSE指标。 RMSLE最小化了这种影响，因此，具有大误差的小响应值可以具有与具有大误差的大响应值一样有意义的影响。\
  **Objective: minimize**\
  ****
* $$R^2$$: This is a popular metric that represents the proportion of the variance in the dependent variable that is predictable from the independent variable(s). Unfortunately, it has several limitations. For example, two models built from two different data sets could have the exact same RMSE but if one has less variability in the response variable then it would have a lower $$R^2$$ than the other. You should not place too much emphasis on this metric. \
  表示可从自变量预测的因变量中方差的比例。不幸的是，它有几个局限性。 例如，从两个不同数据集构建的两个模型可能具有完全相同的RMSE，但如果一个模型的响应变量变异性较小，则R2会比另一个模型低。 您不应过多地强调此指标。\
  **Objective: maximize**

Most models we assess in this book will report most, if not all, of these metrics. We will emphasize MSE and RMSE but it’s important to realize that certain situations warrant emphasis on some metrics more than others. 重点介绍**MSE和RMSE**，但重要的是要意识到某些情况比某些情况更需要强调某些指标。

### Classification models

* **Misclassification**: This is the overall error. **Objective: minimize**\
  ****
* **Mean per class error**: This is the average error rate for each class.这是每个类别的平均错误率. **Objective: minimize**\
  ****
* **MSE**: Mean squared error. Computes the distance from 1.0 to the probability suggested.计算从1.0到建议概率的距离\
  **Objective: minimize**\
  ****
* **Cross-entropy (aka Log Loss or Deviance)**: Similar to MSE but it incorporates a log of the predicted probability multiplied by the true class. Consequently, this metric disproportionately punishes predictions where we predict a small probability for the true class, which is another way of saying having high confidence in the wrong answer is really bad. \
  包含了预测概率乘以真实类别的对数。 因此，该度量标准不成比例地惩罚了我们预测真实类别可能性很小的预测，这是对错误答案高度信任的另一种说法。\
  **Objective: minimize**\
  ****
* **Gini index**: Mainly used with tree-based methods and commonly referred to as a measure of _purity_ where a small value indicates that a node contains predominantly observations from a single class. \
  主要用于基于树的方法，通常称为纯度度量，其中较小的值表示节点主要包含来自单个类别的观测值。\
  **Objective: minimize**\
  ****
* **Confusion matrix:**_ _When applying classification models, we often use a _confusion matrix_ to evaluate certain performance measures. A confusion matrix is simply a matrix that **compares actual categorical levels (or events) to the predicted categorical levels**. \

  * When we predict the right level, we refer to this as a _true positive_. 
  * However, if we predict a level or event that did not happen this is called a _false positive_. 
  * Alternatively, when we do not predict a level or event and it does happen that this is called a _false negative_.

> 应用分类模型时，我们经常使用混淆矩阵来评估某些绩效指标。 混淆矩阵只是将实际分类级别（或事件）与预测分类级别进行比较的矩阵。 当我们预测正确的水平时，我们将其称为真正的积极因素。 但是，如果我们预测未发生的级别或事件，则称为误报。 或者，当我们无法预测等级或事件时，确实发生了这种情况，即称为假阴性



$$
\begin{array}{c|cc} 
& \begin{array}{c}
\text { predicted } \\
\text { events }
\end{array} & \begin{array}{c}
\text { predicted } \\
\text { non-events }
\end{array} \\
\hline \text { actual events } & \begin{array}{c}
\text { correctly } \\
\text { forecasted } \\
\text { events }
\end{array} & \begin{array}{c}
\text { missed } \\
\text { events }
\end{array} \\
\text { actual non-events } & \begin{array}{c}
\text { missed } \\
\text { non-events }
\end{array} & \begin{array}{c}
\text { correctly } \\
\text { forecasted } \\
\text { non-events }
\end{array}
\end{array}
$$

$$
\Downarrow
$$

$$
\begin{array}{c|cc} 
& \begin{array}{c}
\text { predicted } \\
\text { events }
\end{array} & \begin{array}{c}
\text { predicted } \\
\text { non-events }
\end{array} \\
\hline \text { actual events } & \begin{array}{c}
\text { True } \\
\text { Positive }
\end{array} & \begin{array}{c}
\text { False } \\
\text { Negative }
\end{array} \\
\text { actual non-events } & \begin{array}{c}
\text { False } \\
\text { Positive }
\end{array} & \begin{array}{c}
\text { True } \\
\text { Negative }
\end{array}
\end{array}
$$

extract different levels of performance for binary classifiers. For example, given the classification (or confusion) matrix we can assess the following:

$$
\begin{array}{c|cc} 
& \begin{array}{c}
\text { predicted } \\
\text { events }
\end{array} & \begin{array}{c}
\text { predicted } \\
\text { non-events }
\end{array} \\
\hline \text { actual events } & 100 & 5 \\
\text { actual non-events } & 10 & 50
\end{array}
$$

* **Accuracy**: Overall, how often is the classifier correct? Opposite of misclassification above. Example: $$\frac{TP + TN}{total} = \frac{100+50}{165} = 0.91$$ . **Objective: maximize**\
  ****
* **Precision**: How accurately does the classifier predict events? This metric is concerned with maximizing the true positives to false positive ratio. In other words, for the number of predictions that we made, how many were correct? Example: $$\frac{TP}{TP + FP} = \frac{100}{100+10} = 0.91$$ . **Objective: maximize**\
  ****
* **Sensitivity (aka recall)**: How accurately does the classifier classify actual events? This metric is concerned with maximizing the true positives to false negatives ratio. In other words, for the events that occurred, how many did we predict? Example: $$\frac{TP}{TP + FN} = \frac{100}{100+5} = 0.95$$ . **Objective: maximize**\
  ****
* **Specificity**: How accurately does the classifier classify actual non-events? Example: $$\frac{TN}{TN + FP} = \frac{50}{50+10} = 0.83$$ . **Objective: maximize**\
  ****
* **AUC**: Area under the curve. A good binary classifier will have high precision and sensitivity. This means the classifier does well when it predicts an event will and will not occur, which minimizes false positives and false negatives. To capture this balance, we often use a ROC curve that plots the **false positive rate along the x-axis** and the **true positive rate along the y-axis**. A line that is diagonal from the lower-left corner to the upper right corner represents a random guess. The higher the line is in the upper left-hand corner, the better. AUC computes the area under this curve. \
  一个好的二进制分类器将具有很高的精度和灵敏度。 这意味着分类器在预测事件将要发生和将不会发生时表现良好，从而最大程度地减少了误报和误报。 为了获得这种平衡，我们经常使用ROC曲线，该曲线绘制沿x轴的假阳性率和沿y轴的真实阳性率。 从左下角到右上角的对角线代表随机猜测。 该线在左上角越高，越好。\
  **Objective: maximize**
