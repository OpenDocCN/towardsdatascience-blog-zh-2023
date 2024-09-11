# 使用量子退火进行 scikit-learn 特征选择

> 原文：[https://towardsdatascience.com/using-quantum-annealing-for-feature-selection-in-scikit-learn-e747d056f673?source=collection_archive---------7-----------------------#2023-04-10](https://towardsdatascience.com/using-quantum-annealing-for-feature-selection-in-scikit-learn-e747d056f673?source=collection_archive---------7-----------------------#2023-04-10)

## 对于具有大量特征的数据集，使用量子处理进行 scikit-learn 模型的特征选择

[](https://florin-andrei.medium.com/?source=post_page-----e747d056f673--------------------------------)[![Florin Andrei](../Images/372ac3e80dbc03cbd20295ec1df5fa6f.png)](https://florin-andrei.medium.com/?source=post_page-----e747d056f673--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e747d056f673--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----e747d056f673--------------------------------) [Florin Andrei](https://florin-andrei.medium.com/?source=post_page-----e747d056f673--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Faeaeb9d7d248&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fusing-quantum-annealing-for-feature-selection-in-scikit-learn-e747d056f673&user=Florin+Andrei&userId=aeaeb9d7d248&source=post_page-aeaeb9d7d248----e747d056f673---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e747d056f673--------------------------------) · 11 分钟阅读 · 2023年4月10日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe747d056f673&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fusing-quantum-annealing-for-feature-selection-in-scikit-learn-e747d056f673&user=Florin+Andrei&userId=aeaeb9d7d248&source=-----e747d056f673---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe747d056f673&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fusing-quantum-annealing-for-feature-selection-in-scikit-learn-e747d056f673&source=-----e747d056f673---------------------bookmark_footer-----------)

特征选择是机器学习中的一个广泛话题。正确实施时，它可以帮助减少过拟合、提高可解释性、降低计算负担等。用于特征选择的技术有很多种，它们的共同点在于它们会查看特征集，并尝试将那些能够带来良好结果（准确的预测、可解释的模型等）的特征与那些不利于此目标的特征分开。

特别困难的是特征数量非常大的情况。对所有特征组合进行穷举探索通常计算开销很大。像R中的`regsubsets()`函数那样的逐步搜索算法可能会通过添加/删除特征并比较结果来尝试推断出有前途的特征组合。但最终，当特征数量很大时，搜索成功的程度与寻找最佳特征组合所花费的努力之间仍然存在权衡。

一般来说，当问题的最佳解涉及到大量组合的搜索时，量子退火可能值得研究。我将展示一个使用D-Wave最近发布的scikit-learn插件进行特征选择的例子，该数据集具有数百个特征。

# D-Wave和scikit-learn

请记住，这并不是通用的门模型量子计算。这是一种算法，本质上类似于模拟退火，其中有一个目标函数，并且使用类似于模拟退火的方法来找到一个最小化目标的值组合。不同的是，这里的退火并非模拟的，而是通过编程使实际系统的物理能量与目标函数相匹配。系统的能量会降低直到达到最低点（退火），然后解就是系统的状态，这个状态会被读取并返回给用户。

[D-Wave](https://www.dwavesys.com/) 构建了量子退火器，可以有效地解决许多组合复杂度很高的问题。只要你能将问题简化为二次二进制模型（BQM），或者带约束的BQM（CQM），或者上述模型的某些离散推广（DQM），问题就可以提交给量子解算器。更多细节请参见[文档](https://docs.dwavesys.com/docs/latest/doc_getting_started.html)。

理论上，你可以将特征选择算法表述为BQM，其中特征的存在是值为1的二进制变量，而特征的缺失是值为0的变量，但这需要一些工作。D-Wave提供了一个可以直接插入到scikit-learn管道中的[scikit-learn插件](https://github.com/dwavesystems/dwave-scikit-learn-plugin)，简化了这一过程。

本文将首先展示显式地表述BQM的整个过程，然后是CQM，将模型发送到量子解算器，然后解析结果。这是解决量子退火器上优化过程的一般方法，并且可以用于许多问题。

但如果你只是想选择数据集中最好的特征，一个简单的`SelectFromQuadraticModel()`方法调用就足够了。这将整个算法压缩成一行代码。我们将在最后展示这一点。

# 问题与方法

考虑从OpenML下载的[场景识别数据集](https://www.openml.org/d/312)，最初由[Boutell, M., Luo, J., Shen, X., Brown, C. (2004)](https://www.sciencedirect.com/science/article/abs/pii/S0031320304001074)创建，并由其作者在CC BY许可证下分发。这是一个二分类数据集，具有一个依赖变量（Urban），并有近300个特征。响应变量的值（二元：城市或非城市）需要基于特征的组合进行预测。一个简单的分类模型如`RandomForestClassifier()`应该表现良好，前提是特征选择得当。

```py
>>> dataset.get_data()[0]

         attr1     attr2     attr3     attr4     attr5     attr6     attr7     attr8  ...   attr293   attr294  Beach  Sunset  FallFoliage  Field  Mountain  Urban
0     0.646467  0.666435  0.685047  0.699053  0.652746  0.407864  0.150309  0.535193  ...  0.014025  0.029709      1       0            0      0         1      0
1     0.770156  0.767255  0.761053  0.745630  0.742231  0.688086  0.708416  0.757351  ...  0.082672  0.036320      1       0            0      0         0      1
2     0.793984  0.772096  0.761820  0.762213  0.740569  0.734361  0.722677  0.849128  ...  0.112506  0.083924      1       0            0      0         0      0
3     0.938563  0.949260  0.955621  0.966743  0.968649  0.869619  0.696925  0.953460  ...  0.049780  0.090959      1       0            0      0         0      0
4     0.512130  0.524684  0.520020  0.504467  0.471209  0.417654  0.364292  0.562266  ...  0.164270  0.184290      1       0            0      0         0      0
...        ...       ...       ...       ...       ...       ...       ...       ...  ...       ...       ...    ...     ...          ...    ...       ...    ...
2402  0.875782  0.901653  0.926227  0.721366  0.795826  0.867642  0.794125  0.899067  ...  0.254413  0.134350      0       0            0      0         0      1
2403  0.657706  0.669877  0.692338  0.713920  0.727374  0.750354  0.684372  0.718770  ...  0.048747  0.041638      0       0            0      0         0      1
2404  0.952281  0.944987  0.905556  0.836604  0.875916  0.957034  0.953938  0.967956  ...  0.017547  0.019734      0       0            0      0         0      1
2405  0.883990  0.899004  0.901019  0.904298  0.846402  0.858145  0.851362  0.852472  ...  0.226332  0.223070      0       0            0      0         0      1
2406  0.974915  0.866425  0.818144  0.936140  0.938583  0.935087  0.930597  1.000000  ...  0.025059  0.004033      0       0            0      0         0      1

[2407 rows x 300 columns]
```

一种选择特征的方法，描述在[Milne, Rounds, 和Goddard (2018)的论文](https://1qbit.com/whitepaper/optimal-feature-selection-in-credit-scoring-classification-using-quantum-annealer/)中，适合量子退火机，并可以总结如下：

将数据集拆分为特征矩阵和响应向量——即scikit-learn中熟悉的(X, y)元组。使用这些，构造一个相关矩阵C，其中Cii表示特征与响应的相关性，Cij是特征之间的相互相关性。对于与响应高度相关的特征，我们希望尽可能多地捕获。对于彼此相关的特征，我们希望尽可能少地捕获，但不影响与响应强相关的特征。显然，我们希望在所有这些标准之间取得平衡。

设Xi为二元变量，指示特征i是否被选择到最终数据集中。如果选择了特征i，则Xi = 1，否则Xi = 0。问题变成了等同于寻找极值：

![](../Images/ed33a2a33134634f69b8ea136b01c8f1.png)

目标函数

第一个项的总和表示来自特征的个别贡献——我们称之为线性项。第二个项的总和可以说包含了二次交互项。alpha是一个偏置系数，它控制目标函数中我们允许的特征之间的交互量；其值需要探索以找到最佳结果。找到最小化目标函数的Xi集合等同于特征选择。

目标函数实际上是一个二次二元模型——BQM。它是二元的，因为Xi可以是0或1。它是二次的，因为最高阶的项是二次交互项。这可以很容易地在量子退火机上解决。我们需要应用的唯一约束是，等于1的变量Xi的数量不能超过我们愿意接受的特征总数。

# 用困难的方法进行特征选择

让我们来解决这个问题。下面的代码块导入了我们需要的所有库，下载了数据集，并实例化了一个分类模型。

```py
import numpy as np
import openml
from sklearn.ensemble import RandomForestClassifier
from sklearn.model_selection import cross_val_score
import plotly.express as px
from plotly.subplots import make_subplots
import dimod
from dwave.system import LeapHybridCQMSampler

dataset = openml.datasets.get_dataset(312)

X, y, categorical_indicator, attribute_names = dataset.get_data(
    target=dataset.default_target_attribute, dataset_format="dataframe"
)
X = X.astype(float)
y = y.values.astype(int)

clf = RandomForestClassifier()
```

我们关注两件事：

+   每个特征的“相关性”，即它与响应的相关强度

+   特征的“冗余性”，即相关矩阵中的非对角项（二次交互项的权重）

让我们构建几个函数来可视化相关性和冗余性，将它们应用于没有特征选择的整个数据集，然后在整个数据集上进行交叉验证（5 折）模型的性能。

```py
def evaluate_model(m, X, y, indices=None):
    if not indices is None:
        X_filtered = X.iloc[:, indices]
    else:
        X_filtered = X
    acc = np.mean(cross_val_score(clf, X_filtered, y, cv=5))
    return acc

def show_relevance_redundancy(X, y, indices=None, title=""):
    if not indices is None:
        X = X.iloc[:, indices].copy()
    y = y
    fig = make_subplots(
        rows=1,
        cols=2,
        column_widths=[0.68, 0.32],
        column_titles=["relevance", "redundancy"],
    )
    trace_rel = px.bar(np.array([abs(np.corrcoef(x, y)[0, 1]) for x in X.values.T]))
    trace_red = px.imshow(abs(np.corrcoef(X.values, rowvar=False)))
    fig.add_trace(trace_rel.data[0], row=1, col=1)
    fig.add_trace(trace_red.data[0], row=1, col=2)
    fig.update_layout(width=1200, height=480, title=title)
    fig.show()

show_relevance_redundancy(
    X,
    y,
    None,
    f"full dataset: acc={evaluate_model(clf, X, y, None):.4f}",
)
```

结果如下：

![](../Images/d404dc2909d215a51faa41f8e31b8544.png)

基线性能

模型的准确性为 0.9082。特征在相关性方面变化显著。有一些冗余的区域，特征之间似乎存在强相关性。

为了最小化目标函数，让我们创建一个带有约束的二次模型，并将其发送到量子退火器。代码将在下面解释。

```py
k = 30
# Pearson correlation
correlation_matrix = abs(np.corrcoef(np.hstack((X, y[:, np.newaxis])), rowvar=False))
# fix the alpha parameter from the Milne paper
# to account for the numerous quadratic terms that are possible
beta = 0.5
alpha = 2 * beta * (k - 1) / (1 - beta + 2 * beta * (k - 1))
# generate weights for linear and quadratic terms, per Milne algorithm
Rxy = correlation_matrix[:, -1]
Q = correlation_matrix[:-1, :-1] * (1 - alpha)
np.fill_diagonal(Q, -Rxy * alpha)
# create binary quadratic model from the linear and quadratic weights
bqm = dimod.BinaryQuadraticModel(Q, "BINARY")
# create constrained quadratic model
cqm = dimod.ConstrainedQuadraticModel()
# the objective function of the CQM is the same as BQM
cqm.set_objective(bqm)
# constraint: limit the number of features to k
cqm.add_constraint_from_iterable(
    ((i, 1) for i in range(len(cqm.variables))), "==", rhs=k
)
# the sampler that will be used is the hybrid sampler in the DWave cloud
sampler = LeapHybridCQMSampler()
# solve the problem
sampleset = sampler.sample_cqm(cqm)
```

这里发生了很多事情。让我们解释每一步。

我们将特征数量限制为k=30。这是模型的主要限制。

我们稍微偏离了论文中描述的目标函数。我们没有直接定义 alpha，而是使用了一个等效参数 beta，它具有相同的作用。然后我们以一种保持交互项贡献受控的方式定义 alpha——如果特征数量极大，这将确保交互项不会压倒目标函数中的线性项。

我们以一种可以直接构建 BQM 的方式塑造相关矩阵。我们将 BQM 限制在不超过 k=30 个特征的条件下，因此我们获得一个受限的二次模型 (CQM)。我们将 CQM 发送到量子退火器并收集结果。

应该注意的是，量子部分代码的运行时间约为 10 秒。对于许多问题，这是一个典型的基准时长，即使变量数量很大。运行时间往往随着问题复杂性的增加而增加，但这种增加可能不会像你从经典求解器中预期的那样急剧。

我们还没有完成。D-Wave 硬件通常会对解决方案空间进行采样，并返回大量可能的可行解决方案。这是因为硬件的工作方式：一个退火事件运行迅速且容易，因此重复进行退火是有意义的，这里自动发生了。如果生成足够多的样本，其中一些将是最佳解决方案。

所以我们需要对解决方案进行排序并挑选最佳方案。“最佳”意味着——它在目标函数中具有最低的值，D-Wave 称之为“能量”，因为它确实是量子处理器的物理能量。

```py
# postprocess results
feasible = sampleset.filter(lambda s: s.is_feasible)
if feasible:
    best = feasible.first
else:
    assert len(cqm.constraints) == 1
    best = sorted(
        sampleset.data(),
        key=lambda x: (list(cqm.violations(x.sample).values())[0], x.energy),
    )[0]
assert list(best.sample.keys()) == sorted(best.sample.keys())
is_selected = np.array([bool(val) for val in best.sample.values()])
features = np.array([i for i, val in enumerate(is_selected) if val])
best_energy = best.energy
```

`features` 是一个包含量子退火器选定特征索引的数组。这是特征选择过程的结果。显然，它的长度将是 k=30。

让我们在特征选择后测量模型的准确性：

```py
show_relevance_redundancy(
    X,
    y,
    features,
    f"explicit optimization: acc={evaluate_model(clf, X, y, features):.4f}",
)
```

![](../Images/1bdf597311e331f7e012d682fb9a9c3b.png)

显式优化

特征选择后的模型准确度为 0.9381。我们获得了确切数量的特征。大多数特征具有很高的相关性。特征之间的相关值通常较低。模型表现更好，可能更容易解释，并且选择正确特征所花费的时间并不长。

但是这个过程很长且繁琐，如果你只想选择一些特征的话。幸运的是，现在有一种更简单的方法。

# 轻松的特征选择方法

如果你安装了 D-Wave scikit-learn 插件，你只需执行以下操作：

```py
X_new = SelectFromQuadraticModel(num_features=30, alpha=0.5).fit_transform(X, y)
```

这就是全部了。后台代码创建了二次模型，对其进行约束，发送到量子求解器，获取结果，再进行解析，最后选择最佳结果以 NumPy 数组格式返回，这是 scikit-learn 所期望的格式。运行时间大致相同。但让我们看看结果是什么样的：

```py
X_new_df = pd.DataFrame(data=X_new, columns=list(range(X_new.shape[1])))

show_relevance_redundancy(
    X_new_df,
    y,
    None,
    f"plugin optimization: acc={evaluate_model(clf, X_new_df, y, None):.4f}",
)
```

![](../Images/a40d30fc00a2ef4480f93234552d20a2.png)

插件优化

模型的准确度为 0.9369，本质上与我们通过显式优化获得的性能相同（在各种随机组件的典型波动范围内）。

选择的特征集略有不同。这可能是由于我的手动过程和库中的自动化实现之间的微小差异造成的。

在任何一种情况下，我们都将模型的性能从良好提升到了卓越，通过一种不进行启发式猜测的算法，而是考虑了所有特征的总和。

# 待探索的进一步主题

alpha 参数会使特征选择算法偏向于减少冗余但可能质量较低（alpha=0），或者偏向于提高质量但代价是增加冗余（alpha=1）。最佳值取决于你要解决的问题。

`SelectFromQuadraticModel()` 方法有一个名为 `time_limit` 的参数，正如名称所示：它控制求解器上的最大时间。你为所用的时间付费，因此这里的高值可能会很昂贵。另一方面，如果量子退火似乎未能收敛到足够好的解决方案，这里较高的值可能值得探索。这里展示的问题对量子处理器而言相当简单，所以我们仅使用了默认值。

请记住，你的计算机与云端量子求解器之间的数据交换可能并非简单，因此在时间敏感的应用程序（如金融）中，连接的延迟可能非常重要。

# 参考资料

D-Wave 网络研讨会介绍了 scikit-learn 插件。[https://www.youtube.com/watch?v=VHEpe00AXPI](https://www.youtube.com/watch?v=VHEpe00AXPI)

Milne, A., Rounds, M., & Goddard, P. (2018). *使用量子退火器进行信用评分和分类的最佳特征选择*。1QBit.com。[1QBit.com上的白皮书](https://1qbit.com/whitepaper/optimal-feature-selection-in-credit-scoring-classification-using-quantum-annealer/)

Boutell, M., Luo, J., Shen, X., Brown, C. (2004). 学习多标签场景分类。《模式识别》期刊，ScienceDirect。[文章链接](https://www.sciencedirect.com/science/article/abs/pii/S0031320304001074)

入门指南：[D-Wave 求解器](https://docs.dwavesys.com/docs/latest/doc_getting_started.html)

D-Wave scikit-learn 插件：[D-Wave scikit-learn 插件](https://github.com/dwavesystems/dwave-scikit-learn-plugin)

D-Wave 示例：CQM 特征选择：[D-Wave 示例：CQM 特征选择](https://github.com/dwave-examples/feature-selection-cqm)

OpenML 场景识别数据集：[OpenML 场景识别数据集](https://www.openml.org/d/312)

为本文创作的代码和工件：[为本文创作的代码和工件](https://github.com/FlorinAndrei/misc/tree/master/dwave-scikit-learn-feature-selection)

感谢罗斯-霍尔曼科技学院的**马修·布特尔博士**授权我在创作共用许可证下访问场景特征数据集。

所有图像均由作者创作。
