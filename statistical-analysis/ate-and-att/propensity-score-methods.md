# Propensity Score

Source: [An Introduction to Propensity Score Methods for Reducing the Effects of Confounding in Observational Studies Peter C. Austin](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3144483/)

## Four methods:

* matching on the propensity score, 
* stratification on the propensity score, 
* inverse probability of treatment weighting using the propensity score, 
* covariate adjustment using the propensity score. 

> 倾向得分匹配\
> 倾向得分分层 \
> 使用倾向得分对治疗加权的逆概率\
> 使用倾向得分进行协变量调整

### Propensity Score Matching

Propensity score matching entails forming matched sets of treated and untreated subjects who share a similar value of the propensity score (Rosenbaum & Rubin, 1983a, 1985).

一旦形成了匹配的样本，就可以通过直接比较匹配样本中已治疗和未治疗对象之间的结果来评估治疗效果。如果结果是连续的（例如抑郁量表），则可以将治疗效果评估为匹配样本中治疗对象的平均结果与未治疗对象的平均结果之间的差异（Rosenbaum＆Rubin，1983a）。如果结果是二分法（自我报告是否存在抑郁症），则可以将治疗效果评估为两组中每组（治疗组与未治疗组）经历该事件的受试者 with 匹配的样本 之间比例的差异。对于二元结局，也可以使用相对风险或NNT来描述治疗效果

统计显着性: Schafer和Kang（2008）提出，在匹配的样本中，治疗和未治疗的受试者应视为独立受试者。与此相反，Imbens（2004）建议，当使用匹配的估计量时，应使用适用于配对实验的方法来计算方差。我认为倾向得分匹配的样本不包含独立的观察结果。而是，在相同匹配集中的已治疗和未治疗的受试者具有相似的倾向得分值。因此，他们观察到的基线协变量来自相同的多元分布。在存在混淆的情况下，基线协变量与结果相关。因此，与随机选择的受试者相比，匹配的受试者更有可能具有相似的结果。在评估治疗效果差异时，应考虑倾向得分匹配样本缺乏独立性。最近使用蒙特卡洛模拟进行的研究表明，在多种情况下，用于匹配的方差估计量可以更准确地反映出估计的治疗效果的样本可变性（Austin 2009c，印刷中）。因此，配对t检验可用于评估连续治疗结果的统计学意义。同样，McNemar的检验可用于评估 dichotomous outcome 的比例差异的统计显着性。

倾向得分匹配样本的分析可以模仿RCT的分析：可以直接比较倾向得分匹配样本中已治疗和未治疗受试者之间的结果。在RCT的背景下，人们期望平均而言，治疗组之间协变量的分布将相似。但是，在各个RCT中，治疗组之间可能存在基线协变量的残留差异。回归调整可用于减少由于治疗组之间观察到的基线协变量中的残留差异引起的偏差。(Regression adjustment can be used to reduce bias due to residual differences in observed baseline covariates between treatment groups. ) 回归调整可以提高连续结果的准确性，并提高连续，二进制和事件发生时间的统计能力（Steyerberg，2009）。同样，在倾向得分匹配的样本中，协变量平衡是一个较大的样本属性。倾向得分匹配可以与其他预后因素匹配或回归调整相结

#### 替换和不替换匹配

当使用匹配而不替换时，一旦选择了未治疗的对象与给定的治疗对象匹配，则该未治疗的对象不再可被考虑作为后续治疗对象的潜在匹配。结果，每个未治疗的受试者最多包含在一个匹配组中。相反，与替换匹配允许将给定的未经治疗的受试者包括在一个以上的匹配集中。当使用替换匹配时，方差估计(variance estimation)必须考虑以下事实：同一未治疗的受试者可能在多个匹配集中（Hill＆Reiter，2006）。

#### 贪婪和最佳匹配 greedy and optimal matching (Rosenbaum, 2002)

在贪婪匹配中，首先随机选择一个治疗对象。选择倾向得分最接近于该随机选择的治疗对象的倾向得分的未治疗对象以与该治疗对象匹配。然后重复该过程，直到未治疗的受试者已经与所有治疗的受试者匹配，或者直到一个人用尽了可以找到匹配的未治疗的受试者的治疗受试者的列表。该过程被称为贪婪，因为在该过程的每个步骤中，选择最接近的未治疗对象以与给定的治疗对象匹配，即使该未治疗对象最好用作后续治疗对象的匹配对象。贪婪匹配的替代方法是最佳匹配，其中形成匹配以使倾向得分的总对内差异最小 (minimize the total within-pair difference of the propensity score)。

#### 主要方法

两种主要方法：最接近邻居匹配和指定卡尺距离内的最接近邻居匹配 Two primary methods for this: **nearest neighbor matching **and **nearest neighbor matching within a specified caliper distance **(Rosenbaum & Rubin, 1985). 最近邻居匹配选择匹配给定治疗对象的倾向得分最接近治疗对象的倾向的未治疗对象。如果多个未经治疗的受试者的倾向得分与经治疗的受试者的倾向得分相等，则随机选择这些未经治疗的受试者之一。重要的是要注意，两个匹配对象的倾向评分之间的最大可接受差异没有限制。

在指定卡尺距离内的最近邻居匹配与最近邻居匹配相似，但进一步的限制是，匹配对象的倾向得分的绝对差必须低于某个预定阈值（卡尺距离）。因此，对于给定的治疗对象，人们将识别其倾向得分在治疗对象的指定距离之内的所有未治疗对象。从这组受限制的未经治疗的受试者中，倾向得分最接近于经治疗的受试者的未经治疗的受试者将被选择来与该经治疗的受试者匹配。如果没有未治疗的受试者的倾向得分在治疗的受试者的倾向得分的指定卡尺距离内，则该治疗的受试者不会与任何未治疗的受试者匹配。然后将不匹配的治疗对象从所得匹配样品中排除。

#### many-to-one (M: l) matching

多对一（M：l）匹配中，将M个未治疗的受试者与每个治疗的受试者进行匹配。 Ming and Rosenbaum（2000）通过允许将可变数量的未经治疗的受试者与每个经过治疗的受试者进行匹配来修改此方法。他们发现，与固定数量的控件匹配时，与可变数量的控件匹配时，可以获得更好的偏差降低。

### Stratification on the Propensity Score

倾向分数的分层涉及根据主体的估计倾向分数将受试者分为互斥的子集。根据受试者的倾向得分对其进行排名。然后根据估计的倾向得分的先前定义的阈值将受试者分层为子集。一种常见的方法是使用估计的倾向得分的五分位数将对象分为五个相等大小的组。 Cochran（1968）证明，在一个连续混杂变量的五分位数上进行分层可以消除由于该变量引起的大约90％的偏差。 。虽然随着层数的增加，边际的边际减少会降低，但增加的层数应可改善偏见的减少程度（Cochran，1968； Huppler Hullsiek＆Louis，2002）。在每个倾向得分层中，接受治疗和未经接受治疗的受试者的倾向得分值大致相似。因此，当正确指定了倾向得分时，在同一层次内的已治疗和未治疗受试者之间，测量的基线协变量的分布将近似相似 

### Inverse Probability of Treatment Weighting Using the Propensity Score

Inverse probability of treatment weighting (IPTW) using the propensity score uses weights based on the propensity score to create a synthetic sample in which the distribution of measured baseline covariates is independent of treatment assignment. The use of IPTW is similar to the use of survey sampling weights that are used to weight survey samples so that they are representative of specific populations (Morgan & Todd, 2008).

使用倾向评分的治疗加权加权的逆概率（IPTW）使用基于倾向评分的权重来创建合成样本，其中测量的基线协变量的分布与治疗分配无关。 IPTW的使用类似于用于对调查样本加权的调查抽样权重，以便它们代表特定人群（Morgan＆Todd，2008）。

Estimator Formula

$${\displaystyle {\hat {\mu }}_{a,n}^{IPWE}=n^{-1}\Sigma _{i=1}^{n}Y_{i}1_{A_{i}=a}/{\hat {p}}_{n}(A_{i}=a|X_{i})}$$

et Zi be an indicator variable denoting whether or not the ith subject was treated; furthermore, let ei denote the propensity score for the ith subject. Weights can be defined as

$$W_i=\frac{Z_i}{e_i}+\frac{1-Z_i}{1-e_i}$$

An external file that holds a picture, illustration, etc. Object name is hmbr46-399\_inline1.jpg

\[marginal structural model] Joffe，Ten Have，Feldman和Kimmel（2004）描述了如何通过治疗的逆概率对回归模型进行加权，以估计治疗的因果关系。当在这种情况下使用IPTW时，它是更大的因果方法系列的一部分，被称为边缘结构模型（Hernan，Brumback和Robins，2000，2002）。重要的是要注意，方差估计必须考虑到合成样本的加权性质，而健壮的方差估计通常用于说明样本权重（Joffe等，2004）。

### Covariate Adjustment Using the Propensity Score

Using this approach, the outcome variable is regressed on an indicator variable denoting treatment status and the estimated propensity score. 使用这种方法，将结果变量回归到指示治疗状态和估计倾向得分的指标变量上。回归模型的选择将取决于结果的性质。对于连续的结果，将选择线性模型。对于二分结果，可以选择逻辑回归模型。使用拟合回归模型中的估计回归系数确定治疗效果。对于线性模型，治疗效果是调整后的均值差，而对于逻辑模型，则是调整后的优势比。在这四种倾向评分方法中，这是唯一一种需要指定将结果与治疗状态相关联的回归模型和协变量（倾向评分）的方法。此外，该方法假定倾向得分与结果之间关系的性质已正确建模。

### Comparison

* 倾向得分匹配消除了在治疗和未治疗受试者之间基线特征的系统性差异所占的比例，比对倾向得分进行分层或使用倾向得分进行协变量调整的效果更大（Austin，2009a； Austin，Grootendorst和Anderson， 2007； Austin＆Mamdani，2006）。
* 在某些情况下，倾向得分匹配和IPTW在相当程度上消除了治疗对象和未治疗对象之间的系统差异；但是，在某些情况下，倾向得分匹配比IPTW适度消除了更多的失衡（Austin，2009a）。 
* Lunceford和Davidian（2004）证明，分层估计的平均治疗效果比各种加权估计量的偏差更大。
* 前三种方法将研究的设计与研究的分析分开了。当使用倾向得分进行协变量调整时，不会发生这种分离。倾向得分匹配，倾向得分分层和IPTW，只要人们对倾向得分模型的规格感到满意，就可以直接估算出治疗对匹配，分层或加权样本中结果的影响。没有必要指定与治疗结果相关的回归模型。相反，当使用倾向得分进行协变量调整时，一旦人们已经满意地指定了倾向得分模型，就必须拟合一种回归模型，该模型将结果与表示治疗状态和倾向得分的指标变量相关联。在指定回归模型时，必须正确模拟倾向得分与结果之间的关系（例如，指定关系是线性还是非线性）。

## Balance Diagnostics

真实倾向评分是一个平衡评分：以真实倾向评分为条件，测得的基线协变量的分布与治疗分配无关。在一项观察性研究中，真正的倾向得分未知。必须使用研究数据进行估算。任何倾向得分分析的重要组成部分是检查倾向得分模型是否已正确指定。( examining whether the propensity score model has been adequately specified.)

评估倾向得分模型是否已适当指定的适当方法包括检查在具有相同估计倾向得分的已治疗和未治疗受试者之间测得的基线协变量的分布是否相似。如果在对倾向得分进行调节后，如果治疗和未治疗的受试者之间的基线协变量仍然存在系统性差异，则可能表明没有正确指定倾向得分模型。

倾向得分匹配，评估倾向得分模型是否已正确指定涉及在倾向得分匹配样本中比较已治疗和未治疗的受试者。对于IPTW，此评估包括比较样本中已处理和未处理的受试者的权重，该权重由处理的逆概率决定。为了对倾向得分进行分层，此评估需要在倾向得分的层次内比较已治疗和未治疗的受试者。

比较匹配样本中已处理和未处理对象的相似性，应先比较连续协变量的均值或中位数以及已处理和未处理对象之间的同类对应物的分布。标准化差异可用于比较治疗组之间连续变量和二元变量的平均值. 标准化差比较以合并标准差为单位的均值差。此外，它不受样本大小的影响，并允许比较以不同单位测量的变量的相对平衡。尽管尚无关于标准差的阈值可用来表示重要失衡的普遍共识标准，但已采用小于0.1的标准差来表示治疗之间协变量的均值或普遍性差异可忽略不计组（Normand等，2001）。但是，对倾向得分匹配的样本中的治疗对象和未治疗对象的可比性进行彻底检查不应以均值和患病率的比较来结束。真实倾向评分是一个平衡评分：在与真实倾向评分匹配的层次中，观察到的基线协变量的分布与治疗状态无关。因此，基线协变量的整个分布，而不仅仅是均值和患病率，在匹配样本的治疗组之间应该相似。因此，应在治疗组之间比较协变量的较高阶矩和协变量之间的相互作用（Austin，2009b； Ho，Imai，King和Stuart，2007； Imai，King和Stuart，2008； Morgan和Todd，2008）。同样，可以使用图形化方法（如并排箱线图，分位数-分位数图，累积分布函数和经验非参数密度图 side-by-side boxplots, quantile-quantile plots, cumulative distribution functions, and empirical nonparametric density plots）来比较倾向得分匹配样本中各治疗组之间连续基线协变量的分布（Austin, 2009b).

the use of statistical significance testing to assess balance in propensity score matched samples is discouraged. 不鼓励使用统计显着性检验来评估倾向得分匹配样本中的平衡。-----------------------倾向得分匹配的样本几乎总是小于原始样本。因此，依靠显着性检验来发现不平衡可能会产生误导性的结果。组之间无显着差异的发现可能仅是由于匹配样本的样本量减少了（此外，对于大样本，统计学上的显着差异可能仅是由于协变量均值微不足道时检验的高功效）

## Variable selection of propensity score model

倾向评分模型中可能包含的变量集包括：所有测得的基线协变量，与治疗分配相关的所有基线协变量，影响结果的所有协变量（即潜在的混杂因素）以及影响两者的所有协变量治疗分配和结果（即真正的混杂因素）。倾向评分定义为治疗分配的概率（ei，= Pr（Zi，= 1 | Xi））。

* all measured baseline covariates,
* all baseline covariates that are associated with treatment assignment, 
* all covariates that affect the outcome (i.e., the potential confounders), 
* all covariates that affect both treatment assignment and the outcome (i.e., the true confounders). 

最近的一项研究（Austin，Grootendorst和Anderson，2007年）研究了将倾向评分模型中先前所述的不同基线协变量集包括在内的相对好处。结果表明，在倾向评分模型中只包括潜在混杂因素或真正混杂因素是有好处的。在倾向评分匹配的背景下，倾向评分模型中使用四个不同的协变量集中的任何一组都会导致所有预后重要变量在匹配样本中的治疗和未治疗受试者之间达到平衡。当倾向评分模型仅包括潜在混杂因素或真正混杂因素时，在治疗和未治疗受试者之间不平衡的变量是那些影响治疗分配但与结果无关的变量。但是，与使用两个替代性倾向评分模型相比，使用这两个倾向评分模型形成的匹配对数量更多。此外，这两个倾向得分模型（即潜在的混杂因素或真实的混杂因素）导致无效治疗效果的估计，与使用其他两个倾向得分模型获得的估计相比，其均方差更低。因此，使用这两个倾向评分模型不会导致引入额外的偏差，但会导致以更高的精度估算治疗效果。 Brookhart等人也观察到了类似的发现。 （2006年），他建议，不影响暴露但影响结果的变量应始终包括在倾向评分模型中。此外，他们指出，包括影响暴露但不影响结果的变量将增加估计治疗效果的方差，而不会同时减少偏倚。

在实践中，可能难以将基准变量准确地分类为真正的混杂因素，即仅影响结果的因素，仅影响暴露的因素以及不影响治疗或结果的因素。在特定情况下，已发表的文献可能会为确定影响结果的变量提供一些指导。实际上，在许多情况下，大多数受试者水平的基线协变量可能会影响治疗分配和结果。因此，在许多情况下，很可能可以将所有测得的基线特征安全地包括在倾向评分模型中。可能需要更多调查的变量是与政策相关的变量或表示不同时间段的变量。

应该强调的是，倾向评分模型应仅包括在基线时测量的变量，而不应包括可能受治疗影响或修改的基线后协变量。

## Propensity score methods versus regression adjustmen

### Conditional Versus Marginal Estimates of Treatment Effect

A conditional treatment effect is the average effect of treatment on the individual. A marginal treatment effect is the average effect of treatment on the population. A measure of treatment effect is said to be collapsible if the conditional and marginal effects coincide. 有条件的治疗效果是治疗对个体的平均效果。边际治疗效果是治疗对人群的平均效果。如果条件效应和边际效应重合，则认为治疗效果的度量是可折叠的。

在治疗组之间所有协变量均平衡的随机对照试验中，均值的粗差与均值的调整差将重合。倾向得分方法可以估算边缘治疗效果（Rosenbaum，2005）。因此，在一项观察性研究中，（a）没有不可估量的混淆，（b）结果是连续的，并且（c）真实结果模型是已知的，边际估计和条件估计将重合 (Thus, in an observational study in which (a) there was no unmeasured confounding, (b) the outcome was continuous, and (c) the true outcome model was known, the marginal and conditional estimates would coincide.)。假设正确指定了结果回归模型和倾向评分模型，则倾向评分方法应得出与使用线性回归调整获得的结论相似的结论。

如果结果本质上是二进制 binary 或事件发生时间 time-to-event，并且如果将比值比 odds ratio 或风险比 hazard ratio 用作衡量治疗效果的指标，那么即使在没有混淆的情况下，对结果的边际和条件治疗效估计也不重合（Gail，Wieand，＆Piantadosi，1984; Greenland，1987）。因此，在一项观察性研究中，即使在没有无法衡量的混杂因素的情况下，即使知道了真实的结果回归模型，条件比值比或条件危险比也不必与使用倾向评分法获得的估计值一致。

### Practical Concerns

在使用观察数据评估治疗效果时，有很多实际原因会偏重基于倾向评分的方法而不是基于回归的方法。

1. 首先，确定是否正确指定了倾向评分模型比评估是否已经正确指定了将治疗分配和基线协变量与结果相关的回归模型更容易。倾向评分是一个平衡评分：在倾向评分的条件下，所测基线协变量的分布在治疗和未治疗受试者之间相似。拟合优度度量（例如模型R2）不提供对是否正确指定结果模型的检验。此外，拟合优度检验不允许人们确定拟合回归模型已成功消除已治疗和未治疗受试者之间系统性差异的程度。
2. 其次，这些方法使人们可以将研究设计与研究分析分开。这类似于首先设计研究的RCT。只有在研究完成后，才能估计治疗对结果的影响。当使用倾向得分匹配，倾向得分的分层以及使用倾向得分的IPTW时，可以估算倾向得分，并且可以构建匹配，分层或加权的样本，而无需任何参考结果。只有在测量的基线协变量中达到可接受的平衡后，才能评估治疗对预后的影响。但是，当使用回归调整时，结果总是可见的，研究人员面临着不断修改回归模型直到实现所需关联。当使用倾向得分进行匹配，分层或加权时，可以使用后续回归调整来消除对预后重要的协变量的残留不平衡。但是，如在RCT中一样，可以在分析之前指定此回归。
3. 第三，当结果（本质上是二进制或事件发生时间 time-to-event ）很少且治疗很普遍时，灵活性可能会增加（Braitman＆Rosenbaum，2002）。当结果本质上是二进制或事件发生时间时，先前的研究表明，对于进入回归模型的每个协变量，至少应观察到10个事件（Peduzzi，Concato，Feinstein和＆Holford，1995； Peduzzi， Concato，Kemper，Holford和Feinstein，1996年）。因此，在某些情况下，可能会观察到不足的结果，以至于无法对一个人希望包含在回归模型中的所有基线变量进行适当调整。但是，如果治疗或不治疗的发生比结局更为普遍，则在建模倾向评分时可能会增加灵活性。
4. 第四，可以明确检查两个治疗组之间基线协变量分布的重叠程度。当使用倾向得分匹配和分层时，一个明确地比较观察基线协变量分布相似的已治疗和未治疗受试者之间的结局。如果治疗和未治疗的受试者之间的基线协变量存在实质性差异，这将通过匹配受试者的数量较少或观察到大多数阶层主要由治疗受试者或主要由未治疗受试者组成的观察来证明。当面对已治疗和未治疗的受试者之间稀疏重叠时，分析师面临两种选择之间的选择：首先，将分析限制为比较具有相似协变量模式的少数已治疗和未治疗受试者之间的结果，其次，为了中止分析，我们得出结论，接受治疗和未经治疗的受试者差异如此之大，以至于无法对两组之间的结果进行有意义的比较。当使用基于回归的方法时，可能难以评估两组基线协变量分布之间的重叠程度。

## PSM using SAS

[https://blog.csdn.net/Noob_daniel/article/details/76546723](https://blog.csdn.net/Noob_daniel/article/details/76546723)
