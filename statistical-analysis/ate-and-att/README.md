# ATE & ATT



随机对照试验（RCT）被认为是评估治疗，干预措施和暴露（以下简称治疗）对结局影响的金标准方法。随机分配治疗可确保治疗状态不会与测量或未测量基线特征相混淆。因此，可以通过直接比较治疗和未治疗受试者之间的治疗效果来评估治疗对治疗效果的影响（Greenland，Pearl和Robins，1999）。在观察性研究中，治疗选择通常受受试者特征的影响。结果，经治疗的受试者的基线特征经常与未经治疗的受试者的系统特征不同。因此，在评估治疗对预后的影响时，必须考虑治疗与未治疗受试者之间基线特征的系统差异。从历史上看，应用研究人员一直依靠回归调整来解释治疗和未治疗受试者之间基线特征的差异。近来，人们对基于倾向得分的方法的兴趣日益浓厚，这些方法可以减少或消除使用观测数据时混淆的影响。

### ATE & ATT

#### 平均治疗效果（ATE）& 被治疗者的平均治疗效果（ATT）

ATE = E \[Yi（1）-Yi（0）] ATT = E \[Y（1）−7（0）| Z = 1] 在RCT中，这两种治疗效果的测量值是一致的，因为由于随机化，治疗后的人群平均而言不会与总体人群发生系统性差异。

应用研究人员应确定ATE或ATT在他们的特定研究背景下是否具有更大的用途或兴趣。在评估密集的，结构化的戒烟计划的有效性时，ATT可能比ATE更受关注。由于参与和完成戒烟计划的潜在障碍很大，如果将该计划应用于所有当前吸烟者，则估计该计划的效果可能是不现实的。取而代之的是，该计划对选择参加该计划的当前烟民的影响可能更大。相反，在估计家庭医生给当前吸烟者的信息手册对戒烟的影响时，ATE可能比ATT更具吸引力。分发信息手册的成本和工作量相对较低，并且患者接受手册的障碍很小

Randomized Controlled Trials

在RCT中，治疗是通过随机分配的。由于随机化，可以直接从研究数据中计算出ATE的无偏估计。 ATE的无偏估计为E \[Yi，（1）-Yi，（0）] = E \[Y（1）]-E \[Y（0）]（Lunceford＆Davidian，2004）

Observational Studies

在观察性研究中，接受治疗的受试者通常与未经治疗的受试者系统地不同。因此，总的来说，我有E \[Y（1）| Z = 1]≠E \[Y（1）], 因此，不能通过直接比较两个治疗组之间的结果来获得平均治疗效果的无偏估计

The propensity score was defined by Rosenbaum and Rubin (1983a) to be the probability of treatment assignment conditional on observed baseline covariates: ei = Pr(Zi = 1|Xi). The propensity score is a balancing score: conditional on the propensity score, the distribution of measured baseline covariates is similar between treated and untreated subjects. Thus, in a set of subjects all of whom have the same propensity score, the distribution of observed baseline covariates will be the same between the treated and untreated subjects.

Rosenbaum和Rubin（1983a）将倾向评分定义为以观察到的基线协变量为条件的治疗分配概率: 倾向评分是一个平衡评分：在倾向评分的条件下，所测基线协变量的分布在治疗和未治疗受试者之间相似。因此，在一组所有人均具有相同倾向得分的受试者中，在治疗和未治疗的受试者之间观察到的基线协变量的分布将相同。 在随机实验中，真实倾向得分是已知的，并由研究设计定义。在观察性研究中，通常不了解真实倾向得分。但是，可以使用研究数据进行估算。在实践中，倾向评分通常是使用逻辑回归模型估算的，其中治疗状态根据观察到的基线特征进行回归。估计的倾向得分是从拟合回归模型得出的预期治疗概率。尽管逻辑回归似乎是估计倾向得分的最常用方法，但使用套袋或增强方法; 归分区或基于树的方法，随机森林和神经网络用于评估倾向得分

### Two Conditions

> 第一个条件表示治疗分配与观察到的基线协变量有关的潜在结果无关 \
> 第二个条件是，每个受试者都有接受任何一种治疗的非零概率。他们证明，如果对治疗的分配很不了解，则对倾向评分进行调节就可以使人们获得对平均治疗效果的无偏估计。 

* The first condition says that treatment assignment is independent of the potential outcomes conditional on the observed baseline covariates. 
* The second condition says that every subject has a nonzero probability to receive either treatment. They demonstrated that if treatment assignment is strongly ignorable, conditioning on the propensity score allows one to obtain unbiased estimates of average treatment effects.







Sensitivity analysis for ATE

（1）观测数据的灵活模型，以及（2）敏感模型中已识别和未识别部分的清晰分离

用未确定的选择函数来表示未观察到的潜在结果的分布，这些函数在治疗分配指标和观察到的潜在结果之间存在不确定的关系。该框架中的灵敏度参数易于解释

* Franks, A., D’Amour, A., & Feller, A. (2019). Flexible sensitivity analysis for observational studies without observable implications\*. Journal of the American Statistical Association, 1–38. doi:10.1080/01621459.2019.1604369
