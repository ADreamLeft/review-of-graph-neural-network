# Introduction

从数据中进行图学习对于统计图学习和图信号处理领域来说一直是一个至关重要的问题（Friedman et al., 2008; Lake and Tenenbaum, 2010; Witten et al., 2011; Kalofolias, 2016; Egilmez et al., 2016）。 ，2017；Pavez 等，2018；Zhao 等，2019），对无监督学习、聚类等应用领域产生直接影响（Hsieh 等，2012；Sun 等，2014；Tan 等， 2015；Nie 等人，2016；Hao 等人，2018；Kumar 等人，2020, 2019a)，应用金融（Mantegna，1999；de Prado，2016；Marti 等人，2017a），网络拓扑推理（ Segarra 等人，2017；Mateos 等人，2019；Coutino 等人，2019；Shafipour 和 Mateos，2020），社区检测（Fortunato，2010；Li 等人，2018；Chen 等人，2019），和图神经网络（Wu et al., 2019；Pal et al., 2019）。

图学习背后的基本思想是回答以下问题：给定一个数据矩阵，其列表示在图节点测量的信号（观测值），如何设计一种“最”能拟合该数据矩阵的图表示，而无需任何（或最多有部分）底层图结构的知识？ “图表示”或“图结构”通常被理解为图的拉普拉斯矩阵、邻接矩阵或关联矩阵，甚至是更通用的图移位算子（Marques et al., 2016）。此外，观察到的信号不需要存在于规则的有序空间中，并且可以采用任意值，例如分类值和数值值，因此数据的概率分布可能是高度未知的。图 1 说明了这种设置。

![image-20240305225929092](C:\Users\ADL\AppData\Roaming\Typora\typora-user-images\图结构概率分布未知.png)

另一方面，源自股票和外汇等金融工具的数据是在众所周知的有序时域（日内等距、每日、每周、每月等）上定义的并采用通常由高斯过程建模其回报的实际值。那么，问题是：图学习如何成为金融数据分析的有用工具？事实上，Mantegna（1999）在他的开创性工作中表明，在股票市场背景下学习拓扑排列（例如图表）可以提供揭示影响价格数据演变的经济因素的关键信息。历来研究广泛讨论了图形表示给金融股票应用带来的好处。一个非详尽但具有代表性的示例单包括：(i) 通过资产树图识别“正常”和“崩溃”期（Onnela 等人，2003b,a），(ii) 通过拓扑了解投资组合动态模拟最小生成树的属性（Bonanno 等人，2003 年、2004 年；Mantegna 和 Stanley，2004 年），(iii) 基于图表构建公司网络（Onnela 等人，2004 年），(iv) 了解与投资组合相关的风险（Malevergne 和 Sornette，2006），(v) 将学习图的属性利用到后续任务中，例如分层投资组合设计（de Prado，2016；Raffinot，2018a,b），(vi) 挖掘投资者之间的关系结构（Yang 等人，2020），（vii）探索图形属性，例如市场崩溃检测和投资组合构建的度中心性和特征向量中心性（Millington 和 Niranjan，2020），以及（viii）金融股票市场中的社区检测（Ramakrishna 等人）等人，2020；de M. Cardoso 和 Palomar，2020）。

尽管已经得到广泛应用，但学习通用图形模型的结构是一个 NP 难题（Anandkumar et al., 2012），其重要性对于可视化、理解和利用此类结构中数据所包含的全部潜力至关重要。尽管如此，大多数现有的图学习技术通常无法强加特定的图结构，因为它们无法在学习过程中结合先验信息。更令人惊讶的是，正如本篇研究所指出的，最先进的学习算法在估计具有某些属性（例如 k 分量）的图时存在不足。

此外，图学习框架的设计假设是观测到的图信号呈高斯分布（Friedman et al., 2008；Lake and Tenenbaum, 2010；Dong et al., 2016；Kalofolias, 2016；Egilmez et al., 2017） ；Zhao 等人，2019；Kumar 等人，2020；Ying 等人，2020b），本质上忽略了可能存在异常值的情况。因此，这些方法可能无法成功地完全捕获底层图表的有意义的表示，尤其是金融工具的数据，这些数据众所周知是重尾和倾斜的（Gourieroux 和 Monfort，1997 年；Cont，2001 年；Tsay，2010 年） ；Harvey，2013；Feng 和 Palomar，2015；Liu 等人，2019）。

虽然连通图的估计量已经被提出（Dong et al., 2016；Kalofolias, 2016；Egilmez et al., 2017；Zhao et al., 2019），但其一些属性，例如稀疏性，仍在研究中（Ying等人，2020b,a)，直到最近 Kumar 等人。 (2020) 提出了用于学习更一般图形结构的估计器，例如 k 分量、二分图和 k 分量二分图。然而，(Kumar et al., 2020) 的一个主要缺点是缺乏对节点度的限制。正如我们在这项工作中所展示的，控制节点度数的能力是在学习 k 分量图时避免平凡解决方案的关键。

最近，聂等人。 （2016）；库马尔等人。 (2020, 2019a) 提出了用于学习 k 分量图类的优化程序，且由于拉普拉斯矩阵的谱特性，此图类对于聚类任务来说是一个有吸引力的模型。从金融视角来看，对金融时间序列（例如股票收益数据）进行聚类一直是一个活跃的研究课题（Mantegna，1999；Dose 和 Cincotti，2005；Marti 等人，2016，2017a，b）。然而，这些工作主要依赖于层次聚类技术，并假设底层图具有树结构，这确实因其层次聚类特性带来了优势，但也被证明是不稳定的（Carlsson 和 Me ́moli，2010； Lemieux 等人，2014；Marti 等人，2015），并且当数据不是高斯分布时不适合（Donnat 等人，2016）。相反，在这项工作中，我们从概率的角度解决股票聚类问题，类似于 Kumar 等人提出的方法。 （2019a，2020），其中假设 k 分量图的拉普拉斯矩阵对股票之间的成对条件相关性进行建模。这种方法的一个关键优势是我们可以考虑更现实的概率假设，例如重尾。

在实践中，有关股票集群的先验信息可以通过行业分类系统获得，例如全球行业分类标准（GICS）（标准普尔，2006 年；摩根士丹利资本国际和标准普尔道琼斯，2018 年）或行业分类基准（ICB） ）（施赖纳，2019）。然而，股票往往会对多个行业产生影响，例如亚马逊、苹果、谷歌和 Facebook 等科技公司的明显例子，它们对价格的影响不仅影响其所在行业的股票，而且影响整个行业的股票。多个部门。造成这种现象的原因之一是这些公司提供的服务种类繁多，导致难以准确确定它们应该属于哪个股票市场板块。

受金融领域实际应用（例如金融工具聚类和网络估计）的启发，我们研究了学习结构遵循无向加权图的拉普拉斯矩阵结构的图矩阵的问题。

我们论文的主要贡献包括：

- 据笔者所知，我们首次从股市数据的角度对图的拉普拉斯约束进行解释。这些解释自然会为从金融数据中学习图表所需的数据预处理提供有意义且直观的指南。
- 我们证明，仅靠排名约束（最先进的方法经常使用的做法）不足以学习非平凡的 k 分量图。我们通过利用图的节点度的线性约束来实现没有孤立节点的 k 分量图的学习。
- 我们提出了学习 k 分量图和重尾图的新公式，这些公式通过精心设计的交替方向乘子法 (ADMM) 算法来解决。此外，我们为所提出的算法建立了理论收敛保证，并对其经验收敛性进行了实验。所提出的算法可以很容易地扩展以解决图权重的额外线性约束。
- 我们提出了广泛的实际结果，展示了所提出的算法中使用的操作假设与最先进的方法相比的优势。除了本文提出的方法之外，我们还发布了一个 R 包，名为 fingraph ，包含实现所提出算法的快速、经过单元测试的代码，并且可在以下位置公开获取：https://github.com/mirca/fingraph。

# Background and Related Works

无向加权图通常表示为三元组 $\mathcal{G} = (\mathcal{V}, \mathcal{E}, \mathbf{W} )$，其中 $\mathcal{V }= \{1, 2, \cdots , p\}$ 是顶点（或节点）集合，$\mathcal{E} ⊆ \{\{u, v\} : u, v ∈ \mathcal{V}\} $是边集合，即所有可能的无序对 $p$ 节点的集合的子集，使得 ${u, v} ∈ \mathcal{E}$ 当且仅当节点 $u$ 和 $v$ 已连接。 $W ∈ \mathbb{R}^{p×p}_ +$ 是对称加权邻接矩阵，满足 $W_{ii} = 0, W_{ij} > 0 \iff {i, j} ∈ \mathcal{E}$ 且 $W_{ij} = 0$，否则。我们将图表示为 4 元组 $G = (\mathcal{V}, \mathcal{E}, W , f_t)$，其中 $f_t : \mathcal{V} → {1, 2, \cdots, t}$ 是将单个类型（标签）关联到图的每个顶点的函数，其中 $t$ 是可能类型的数量。此扩展对于计算某些实际感兴趣的图属性（例如图模块化）是必要的。我们用 $|\mathcal{E}|$ 表示 $\mathcal{E}$ 中的元素数量。组合非归一化图拉普拉斯矩阵 $L$ 像往常一样定义为 $L\triangleq  D − W $，其中 $D \triangleq Diag(W \mathcal{1})$ 是度矩阵。

秩为 $p − k、k ≥ 1$ 的不当高斯马尔可夫随机场 (IGMRF)（Rue 和 Held，2005 年；Slawski 和 Hein，2015 年）被表示为 $p$ 维、实值、高斯随机变量 $x$，其平均值向量 $\mathbb{E} [x] \triangleq μ$ 和欠定精度矩阵 $\mathbb{E}\left[(x-\boldsymbol{\mu})(x-\boldsymbol{\mu})^\top\right]^\dagger\triangleq\Xi.$。 $x$ 的概率密度函数如下
$$
p(\boldsymbol{x})\propto\sqrt{\det^*(\boldsymbol{\Xi})}\exp\left\{-\frac{1}{2}(\boldsymbol{x}-\boldsymbol{\mu})^\top\Xi(\boldsymbol{x}-\boldsymbol{\mu})\right\},
$$
其中 $\det^*(\boldsymbol{\Xi})$ 是 $\boldsymbol{\Xi}$ 的伪（也称为广义）行列式，即其正特征值的乘积（Knill，2014）

数据生成过程被假设为零均值，IGMRF $x ∈ \mathbb{R}^p$，使得 xi 是生成在节点 $i$ 处测量的信号的随机变量，其欠定精度矩阵被建模为图拉普拉斯矩阵。该模型也称为拉普拉斯约束高斯马尔可夫随机场 (LGMRF)（Ying 等人，2020b）。假设我们得到 $x$ 的 $n$ 个观测值，即 $\boldsymbol{X}=\left[\boldsymbol{x}_{1,*}^{\top},\boldsymbol{x}_{2,*}^{\top},\ldots,\boldsymbol{x}_{n,*}^{\top}\right]^{\top},\boldsymbol{X}\in\mathbb{R}^{n\times p},\boldsymbol{x}_{i,*}\in\mathbb{R}^{p\times1}$。图学习算法的目标是学习拉普拉斯矩阵，或等效的邻接矩阵，仅给定数据矩阵 $X$，即通常不了解 $\mathcal{E}$ 和 $f_t$。

为此，基于观测数据 $X，x$ 的拉普拉斯约束精度矩阵的经典惩罚最大似然估计器 (MLE) 可以表示为以下优化程序：
$$
\begin{array}{cc}\underset{L\succeq\mathbf{0}}{\operatorname*{\mathrm{minimize}}}&\mathrm{tr}\left(LS\right)-\log\det^{*}\left(L\right)+h_{\alpha}(L),\\\mathrm{subject~to}&L1=0,&L_{ij}=L_{ji}\leq0,\end{array}
$$
其中 $S$ 是相似矩阵，例如样本协方差（或相关）矩阵 $S ∝ X^\top X$，$h_α(L)$ 是正则化函数，具有超参数向量 $α$，以促进 $L$ 上的某些属性，例如稀疏性或低秩性。

问题（2）是图信号处理领域的一个基本问题，它已成为许多扩展的基石，主要是那些涉及将结构包含到 $L$ 上的扩展（Egilmez 等人，2017 年；Pavez 等人，2018 年；Kumar 等人）等人，2019a，b，2020）。即使问题 (2) 是凸的，只要我们假设 $h_α(·)$ 的凸选择，它就不足以通过严格凸规划 (DCP) 语言来解决，例如 cvxpy（Diamond 和 Boyd，2016），特别是由于与 $\log \det^*(L)$ 计算相关的可扩展性问题（Egilmez 等人，2017；Zhao 等人，2019）。事实上，最近，人们在设计基于块坐标下降 (BCD) (Wright, 2015)、Majorization-Minimization (MM) (Sun et al., 2017) 和 ADMM (Boyd) 的可扩展迭代算法方面付出了巨大的努力。 et al., 2011）以有效的方式解决问题（2），例如（Egilmez et al., 2017）、（Zhao et al., 2019）、（Kumar et al., 2020）和（Ying et al., 2020） al., 2020b)，仅举几例。

为了规避与 $\log \det^*(L)$ 计算相关的一些可扩展性问题，Lake 和 Tenenbaum (2010) 提出了以下带有“1-范数惩罚”的宽松版本：
$$
\begin{aligned}
&\begin{array}{l}{\mathsf{maximize}}\\{\tilde{L}\succ\mathbf{0},W,\sigma>0}\\\end{array} -\mathrm{tr}\left(\tilde{L}S\right)+\log\det\left(\tilde{L}\right)-\alpha\|\boldsymbol{W}\|_{1},  \\
&\text{subject to} \tilde{\boldsymbol{L}}=\mathrm{Diag}(\boldsymbol{W}1)-\boldsymbol{W}+\sigma\boldsymbol{I}_p,  \\
&\operatorname{diag}(\boldsymbol{W})=\boldsymbol{0},W_{ij}=W_{ji}\geq0,\forall i,j\in\{1,2,...,p\}.
\end{aligned}
$$
换言之，问题（3）通过引入项 $σI_p$ 强制精度矩阵为正定，从而放松了原始问题，这将 $\tilde{L}$ 的最小特征值限制为至少为 $σ$，从而可以替换广义行列式为正常的行列式。尽管与广义行列式相关的技术问题似乎已经得到解决（尽管是以间接的方式），但在这个公式中，需要估计的变量是其两倍，这在设计实用的、可扩展的算法，以及 cvx 的适用性时被证明是令人望而却步的，就像（Lake 和 Tenenbaum，2010）中所做的那样，只有在小规模（p ≈ 50）场景中才有可能。

为了解决这个可扩展性问题，Hassan-Moghaddam 等人。 (2016) 和 Egilmez 等人。 (2017) 提出了定制的 BCD 算法（Shalev-Shwartz 和 Tewari，2011；Saha 和 Tewari，2013；Wright，2015）来解决问题 (2)，假设采用 `1-范数正则化，即 $h_{\alpha}(L)\triangleq\alpha\sum_{i\neq j}|L_{ij}|=-\alpha\sum_{i\neq j}L_{ij}$，以促进所得估计拉普拉斯矩阵的稀疏性。然而，正如最近 Ying 等人所表明的那样。 (2020b,a)，与常见做法相反，“1-范数惩罚”令人惊讶地导致图形更加密集，以至于对于较大的 $α$ 值，所得图形将与均匀分布的图形权重完全连接。

另一方面，由于处理 $\log \det^*(L)$ 项时涉及到这些麻烦，一些工作完全背离了 LGMRF 公式。相反，他们关注的是图表中的基础信号是平滑的假设（Kalofolias，2016；Dong 等人，2016；Cepuri 等人，2017）。在最简单的形式中，从数据矩阵 $X ∈ \mathbb{R}^{n×p}$ 学习平滑图相当于找到一个使狄利克雷能量最小化的邻接矩阵 $W$，即：
$$
\begin{array}{cl}\underset {W}{\operatorname*{\mathrm{minimize}}}&\frac12\sum_{i,j}W_{ij}\left\|x_i-x_j\right\|_2^2,\\ \text{subject to}&W_{ij}=W_{ji}\geq0,\mathrm{~diag}(W)=0.\end{array}
$$
问题(4)也可以等价地用拉普拉斯矩阵表示：
$$
\begin{array}{cc}\underset{L\succeq0}{\operatorname*{\mathrm{minimize}}}&\mathrm{tr}\left(XLX^\top\right),\\\mathrm{subject~to}&L1=0,L_{ij}=L_{ji}\leq0.\end{array}
$$


为了明确问题(4)和(5)，即避免平凡解 $W = 0 (L = 0)$，文献中提出了一些约束。例如，董等人。 (2016) 提出了以下非凸优化程序作为图拉普拉斯的第一类估计之一：
$$
\begin{aligned}&\underset{L\succeq\mathbf{0},Y\in\mathbb{R}^{n\times p}}{\text{minimize}}\quad\|X-Y\|_{\mathrm{F}}^2+\alpha\mathrm{tr}\left(YLY^\top\right)+\eta\|L\|_{\mathrm{F}}^2,\\&\text{subject to}\quad L1=0,L_{ij}=L_{ji}\leq0,\mathrm{tr}\left(L\right)=p,\end{aligned}
$$
其中施加约束 $\operatorname{tr}\left(L\right)=p$ 来固定图的度数之和，$α$ 和 $η$ 是正的实值超参数，用于控制估计图中的稀疏程度。更准确地说，对于给定的固定 $α$，增加（减少）$η$ 会导致图更稀疏（更稠密）。董等人。 （2016）采用迭代交替最小化方案来找到问题（6）的最佳点，即在每次迭代时固定一个变量，同时找到另一个变量的解。然而，公式（6）的主要缺点是，由于变量 $Y ∈\mathbb{R}^{n×p}$ 的更新，它不能很好地适应大数据集，而变量 $Y ∈\mathbb{R}^{n×p}$ 是观测值数量 $n$ 的函数。在金融市场的一些图学习问题中，价格记录的数量可能比节点（金融资产）的数量大几个数量级，特别是在高频交易场景中（Kirilenko et al., 2017）。

而根据平滑信号假设，Kalofolias (2016) 提出了如下凸公式：
$$
\begin{array}{rl}\underset{\boldsymbol{W}}{\operatorname*{\mathrm{minimize}}}&\frac{1}{2}\mathrm{tr}\left(\boldsymbol{W}\boldsymbol{Z}\right)-\alpha\boldsymbol{1}^{\top}\log(\boldsymbol{W}\boldsymbol{1})+\frac{\eta}{2}\left\|\boldsymbol{W}\right\|_{\mathrm{F}}^{2},\\\mathrm{subject~to}&W_{ij}=W_{ji}\geq0,\mathrm{~diag}(\boldsymbol{W})=\boldsymbol{0},\end{array}
$$
其中 $Z_{ij}\triangleq\|x_{*,i}-x_{*,j}\|_{2}^{2}$, $α$ 和 $η$ 是正实值超参数，控制估计图中的稀疏量，其解释与问题 (6) 相同，并且$\log(W 1)$ 假设按元素进行评估，是为避免图的度数变为零而添加的正则化项。

问题 (7) 是凸的，可以通过原始对偶、类似 ADMM 的算法来解决（Komodakis 和 Pesquet，2015）。可见，问题（7）中的目标函数实际上是问题（2）的目标函数的近似。根据 Hadamard 不等式（R ́o ̇ za ́ nski et al., 2017），我们有
$$
\log\det^*(L)\leq\log\prod\limits_{i=1}^pL_{ii}=\sum\limits_{i=1}^p\log L_{ii}=1^\top\log(W1).
$$
因此，问题（7）可以被认为是带有 Frobenius 范数正则化的惩罚最大似然估计的近似，以便限制图权重。

前面讨论的图学习公式仅适用于学习连通图。学习具有先验结构的图（例如 k 分量图）则提出了相当高的挑战，因为拉普拉斯矩阵 $L$ 的零空间的维数等于图的分量数量（Chung，1997）。因此，算法必须保证第0个特征值的代数重数等于k。然而，后一个条件不足以排除“平凡”k 分量图的空间，即具有孤立节点的图。除了 L 上的秩（或零化度）约束之外，还需要指定图的度数约束

最近人们努力将谱图理论（Chung，1997）的理论结果引入实际的优化程序中。例如，（Nie et al., 2016）提出了一种基于平滑信号方法估计 k 分量图的公式。更准确地说，他们提出了约束拉普拉斯秩（CLR）算法，该算法分两个阶段进行。在第一阶段，它使用问题（7）的解来估计连通图，然后在第二阶段，它启发式地将图投影到维度为 p、秩为 p − k 的拉普拉斯矩阵集合上，其中 k 是给定数量的图形组件。该方法概括为以下两个阶段：

- 获得初始亲和矩阵 $A^*$作为最佳值：

$$
\begin{array}{rl}\underset{A}{\operatorname*{\mathrm{minimize}}}&\frac{1}{2}\mathrm{tr}\left(A\boldsymbol{Z}\right)+\frac{\eta}{2}\left\|\boldsymbol{A}\right\|_{\mathrm{F}}^{2},\\\mathrm{subject~to}&\mathrm{diag}\left(\boldsymbol{A}\right)=\boldsymbol{0},\boldsymbol{A}\boldsymbol{1}=\boldsymbol{1},\boldsymbol{A}_{ij}\geq0\mathrm{~}\forall i,j\end{array}
$$

- 求满足 $L^\star=\mathrm{Diag}(\frac{B^{\star\top}+B^\star}2)-\frac{B^{\star\top}+B^\star}2$ 的秩为 p − k 的预测$A^*$：

$$
\begin{aligned}
&\operatorname*{minimize}_{B,L\succeq0}&& \|B-A^{\star}\|_{\mathrm{F}}^{2},  \\
&\text{subject to}&& B\mathbf{1}=\mathbf{1},\mathrm{rank}(\mathbf{L})=p-k,  \\
&&&L=\mathrm{Diag}(\frac{B^{\mathsf{T}}+B}2)-\frac{B^{\mathsf{T}}+B}2
\end{aligned}
$$

其中 k 是所需的图分量数量

拉普拉斯矩阵上的谱约束是恢复 k 分量图的直观方法，因为其零特征值的重数（即 L 的零化度）决定了图的分量数量。 Kumar 等人提出了第一个在 LGMRF 模型下对估计拉普拉斯矩阵施加结构的框架。 (Kumar et al., 2019a, 2020)，通过使用如下谱约束：
$$
\begin{aligned}
&\underset{L,U,\lambda}{\operatorname*{minimize}} &&\mathrm{tr}\left(\boldsymbol{L}\boldsymbol{S}\right)-\sum_{i=1}^{p}\log\left(\lambda_{i}\right)+\frac\eta2\left\Vert\boldsymbol{L}-\boldsymbol{U}\mathrm{Diag}(\boldsymbol{\lambda})\boldsymbol{U}^{\top}\right\Vert_{\mathrm{F}}^{2},  \\
&\text{subject to}  &&L\succeq0,L1=0,L_{ij}=L_{ii}<0,\\

&&&U^{\top}U=I,U\in\mathbb{R}^{p\times(p-k)}, \\
&&&\lambda\in\mathbb{R}_{+}^{p-k},c_{1}<\lambda_{1}<\cdots<\lambda_{p-k}<c_{2}.
\end{aligned}
$$
其中 $\frac\eta2\left\|L-U\text{Diag}(\lambda)U^\top\right\|_{\mathrm{F}}^2$ 项（通常称为谱正则化）作为惩罚项添加，以间接提升 $L$ 与 $U Diag(λ)U^\top$ 具有相同的秩，即 p − k，k 是先验选择的图的分量数量，$η > 0$ 是控制 L 谱分解惩罚的超参数，c1 和 c2 是正实值常数用于提升 L 特征值的界限。

注意到问题（11）无需两阶段算法即可学习 k 分量图。但是，此公式的一个明显的限制是，它不控制图中节点的度数，这可能会导致包含孤立节点的简单解决方案，结果对聚类任务没有用处，特别是当应用于噪声数据集或不显着高斯分布的数据集时。此外，选择超参数 η、c1 和 c2 的值通常是一项复杂的任务。