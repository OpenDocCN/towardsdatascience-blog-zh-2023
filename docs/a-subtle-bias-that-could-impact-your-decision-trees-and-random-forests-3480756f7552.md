# 可能影响你的决策树和随机森林的微妙偏差

> 原文：[https://towardsdatascience.com/a-subtle-bias-that-could-impact-your-decision-trees-and-random-forests-3480756f7552?source=collection_archive---------5-----------------------#2023-12-28](https://towardsdatascience.com/a-subtle-bias-that-could-impact-your-decision-trees-and-random-forests-3480756f7552?source=collection_archive---------5-----------------------#2023-12-28)

## 这可以很容易地被消除

[](https://medium.com/@gyuriofkovacs?source=post_page-----3480756f7552--------------------------------)[![Gyorgy Kovacs](../Images/aa5d1fcc59d738acc1056de3f0cbe7ca.png)](https://medium.com/@gyuriofkovacs?source=post_page-----3480756f7552--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3480756f7552--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----3480756f7552--------------------------------) [Gyorgy Kovacs](https://medium.com/@gyuriofkovacs?source=post_page-----3480756f7552--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4563dd81810c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-subtle-bias-that-could-impact-your-decision-trees-and-random-forests-3480756f7552&user=Gyorgy+Kovacs&userId=4563dd81810c&source=post_page-4563dd81810c----3480756f7552---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----3480756f7552--------------------------------) ·10分钟阅读·2023年12月28日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F3480756f7552&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-subtle-bias-that-could-impact-your-decision-trees-and-random-forests-3480756f7552&user=Gyorgy+Kovacs&userId=4563dd81810c&source=-----3480756f7552---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F3480756f7552&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-subtle-bias-that-could-impact-your-decision-trees-and-random-forests-3480756f7552&source=-----3480756f7552---------------------bookmark_footer-----------)![](../Images/97c9e93a77a35e474acf2ef359534ccd.png)

Dall-E 生成的艺术决策树

你的决策树和随机森林可能会受到一些小的偏差影响，这种偏差可以轻松消除，几乎不需要成本。这就是我们在本文中探讨的内容。

*免责声明：这篇文章讨论了作者最近进行的研究。*

# 介绍

决策树和随机森林是机器学习中广泛应用的分类和回归技术。决策树因其可解释性而受到青睐，而随机森林则作为高度竞争且通用的最先进技术脱颖而出。常用的 CART 实现，如 Python 包中的 [sklearn](https://scikit-learn.org/stable/index.html) 和 R 包中的 [tree](https://cran.r-project.org/web/packages/tree/index.html) 及 [caret](https://cran.r-project.org/web/packages/caret/index.html)，假设所有特征都是连续的。尽管存在这种隐含的连续性假设，这两种技术仍然被广泛应用于具有各种特征类型的数据集。

在一篇最近的论文中，我们研究了违反连续性假设的实际影响，并发现它会导致偏差。重要的是，这些假设在实际中几乎总是被违反。在这篇文章中，我们展示并讨论了我们的发现，说明和解释了背景，并提出了一些简单的技术来减轻这种偏差。

# 一个激励性示例

让我们通过 UCI 资源库中的 [CPU 性能](https://archive.ics.uci.edu/dataset/29/computer+hardware) 数据集来进行示例。我们将通过 [common-datasets](https://pypi.org/project/common-datasets/) 包导入数据，以简化预处理过程，避免特征编码和缺失数据插补的需求。

```py
import numpy as np

from sklearn.ensemble import RandomForestRegressor
from sklearn.model_selection import RepeatedKFold, cross_validate
from scipy.stats import wilcoxon

from common_datasets.regression import load_cpu_performance

dataset = load_cpu_performance()
X = dataset['data']
y = dataset['target']

# a cross-validation wrapper to simplify the code
def cv_rf(X, y, regressor=RandomForestRegressor):
    return cross_validate(
        estimator=regressor(max_depth=11),
        X=X, y=y,
        cv=RepeatedKFold(n_splits=5, n_repeats=400, random_state=5),
        scoring='r2'
    )['test_score']

r2_original = cv_rf(X, y)
r2_mirrored = cv_rf(-X, y)
```

在实验中，我们评估了随机森林回归器在原始数据及其镜像版本（每个特征乘以 -1）上的表现。回归器的超参数（`max_depth=11`）是在专门的模型选择步骤中选择的，通过在合理的深度范围内最大化 r2 得分来确定。用于评估的交叉验证比机器学习中通常使用的交叉验证要全面得多，总共进行了 2000 次折叠。

```py
print(f'original r2: {np.mean(r2_original):.4f}')
print(f'mirrored r2: {np.mean(r2_mirrored):.4f}')
print(f'p-value: {wilcoxon(r2_original, r2_mirrored, zero_method="zsplit").pvalue:.4e}')
# original r2: 0.8611
# mirrored r2: 0.8595
# p-value: 6.2667e-04
```

在 r2 得分方面，当属性被镜像时，我们观察到降低了 0.2 个百分比点。此外，这一差异在传统水平下统计上显著（p << 0.01）。

结果有些令人惊讶且不符合直觉。机器学习技术通常对某些类型的变换具有*不变性*。例如，k 最近邻对任何正交变换（如旋转）不变，线性技术通常对属性的缩放不变。由于决策树中的空间划分是轴对齐的，因此不能期望它对旋转不变。然而，它对缩放是不变的：对任何特征应用任何正的乘数都会导致完全相同的树。因此，轴的镜像处理一定存在一些问题。

一个有趣的问题出现了：如果镜像坐标轴可以带来更好的结果怎么办？我们是否应该在模型选择中考虑另一个自由度（乘以 -1），除了确定最优深度之外？好吧，在接下来的文章中我们将搞清楚这里发生了什么！

# **二叉决策树生成与推理**

现在，让我们简要回顾一下使用**二叉分类与回归树（CART）**的构建和推理的一些重要特征，这些特征被大多数实现所使用。与其他树生成技术如 [ID3](https://en.wikipedia.org/wiki/ID3_algorithm) 和 [C4.5](https://en.wikipedia.org/wiki/C4.5_algorithm) 相比，CART 树的一个显著不同之处在于它们不会以任何特殊方式处理类别特征。**CART 树假设所有特征都是连续的**。

给定一个训练集（分类或回归），决策树通过递归地使用诸如 *feature < threshold* 或 *features <= threshold* 的条件来划分训练集和特征空间来生成。**条件选择通常是实现的一个固有属性**。例如，Python 包 [sklearn](https://scikit-learn.org/stable/index.html) 使用形式为 *feature <= threshold* 的条件，而 R 包 [tree](https://cran.r-project.org/web/packages/tree/index.html) 使用 *feature < threshold*。请注意，这些条件与所有特征都是连续的假设相一致。

然而，假设特征是连续的并不是一个限制。整数特征、通过某些编码的类别特征或二元特征仍然可以输入到这些树中。让我们以一个假设的贷款审批场景中的示例树为例（一个二元分类问题），基于三个属性：

+   graduated (binary): 如果申请人未毕业则为 0，如果申请人毕业则为 1；

+   income (float): 申请人的年收入；

+   dependents (int): 依赖人数；

目标变量是二元的：申请人是否违约（1）或偿还（0）。

![](../Images/22da8afdd9c919cd18b2cf30bf16d644.png)

为假设的贷款审批场景构建的决策树

树的结构以及节点中的条件（即哪个特征的阈值）是从训练数据中推断出来的。有关树生成的更多细节，请参考 [维基百科上的决策树学习](https://en.wikipedia.org/wiki/Decision_tree_learning)。

给定这样一棵树，新记录的推理是通过从叶子节点开始，递归应用条件，并将记录路由到条件输出对应的分支。当遇到叶子节点时，记录在叶子节点中的标签（或最终分布）将被返回作为预测。

# **条件设置与阈值**

一个有限的训练记录集不能暗示特征空间的唯一分割。例如，上图中的树可能是从数据中归纳出的，其中没有毕业=0且收入在]60k，80k[范围内的记录。树的归纳方法确定应在收入值60k和80k之间进行分割。在没有更多信息的情况下，中点（70k）被用作阈值。一般来说，也可以是65k或85k。使用未标记区间的中点是一种常见的做法和合理的选择：与连续特征的假设一致，将未标记区间的50%分配到左分支，50%分配到右分支。

由于使用中点作为阈值，**树的归纳完全独立于条件操作符的选择**：使用<=和<都会导致相同的树结构，阈值也相同，只是在条件操作符上有所不同。

然而，**推断确实依赖于条件操作符**。在示例中，如果要推断一个收入为70k的申请人记录，则在所示的设置中，它将被路由到左分支。然而，使用<操作符时，它将被路由到右分支。对于真正的连续特征，收入恰好为70k的记录被推断的可能性微不足道。然而，实际上，收入可能以1k、5k或最终10k为单位，这使得条件操作符的选择对预测有显著影响。

# **条件操作符和镜像的关系**

*为什么我们谈论条件操作符，而我们观察到的问题是特征的镜像？* 这两者基本上是相同的。条件“feature < threshold”与条件“-feature <= -threshold”在本质上是等价的，因为它们导致相同但镜像的实数轴分割。也就是说，在这两种情况下，如果特征值等于阈值，该值位于特征值大于阈值的相同分区。例如，比较下面的两棵树，一棵是我们之前用于说明的树，除了所有条件操作符都改为<，另一棵是保持操作符不变，但树是镜像的：可以很容易看到，对于任何记录，它们都会导致相同的预测。

![](../Images/35dbb474d158758d688420288278fe48.png)

上一个使用条件操作符<的树

![](../Images/b2462e58505a5011b91b009749a92bfb.png)

基于镜像数据构建的树（仍然使用条件操作符≤）

由于树生成与条件选择无关，**在镜像数据上构建树，然后预测镜像向量，相当于使用非默认的条件运算符（<）对非镜像记录进行推断**。当森林的树拟合到镜像数据时，即使*sklearn*使用‘<=’运算符进行条件处理，它也会像使用‘<’运算符一样工作。因此，**我们发现镜像处理的性能下降是由于阈值与特征值重合**，导致在测试集评估过程中预测结果不同。

为了完整性，我们注意到树生成某些步骤中的随机化可能会导致在拟合镜像数据时出现略微不同的树。然而，这些差异在随机森林中平滑，特别是在2k折的评估中。观察到的性能下降是阈值与特征值重合的系统性效应的结果。

# **这种情况何时会发生？**

主要有两种情况会增加这种现象的可能性：

+   **当特征域包含高概率的等距值**：这为阈值（作为两个观察值的中点）与特征值重合提供了条件。

+   **建立了相对较深的树**：通常情况下，树的深度增加时，训练数据在节点处变得更加稀疏。当某些观察值在更深的层次上缺失时，阈值可能会落在这些值上。

有趣的是，许多领域中具有少量等距值的特征非常普遍。例如：

+   医疗数据集中的年龄特征。

+   四舍五入的十进制数字（观察到第2位数字的，将形成一个网格）。

+   财务数据集中以百万或十亿为单位引用的货币金额。

此外，在`sklearn.datasets`中的玩具回归和分类数据集中，几乎97%的特征都是这种情况。因此，毫不夸张地说，高概率的等距特征几乎无处不在。因此，作为经验法则，**建立更深的树或森林，阈值干扰特征值的可能性就越大**。

# **这是一种偏差，模型选择无济于事！**

我们已经看到，这两个条件运算符（由数据镜像模仿的非默认运算符）可能导致具有统计显著性的不同预测结果。这两个预测不能同时无偏。因此，我们认为，无论使用哪种形式的条件，当阈值与特征值重合时，都引入了偏差。

另外，考虑到某种形式的条件可能与数据巧妙对齐并提升性能，这是一种诱人的想法。因此，可以使用模型选择来选择最合适的条件形式（或是否应该镜像数据）。然而，在特定的模型选择场景中，使用一些 k 折交叉验证方案，我们只能测试在例如，20% 的数据被移除（5 折）用于训练和评估时，哪种操作符通常是更有利的。**当模型在所有数据上训练时，其他阈值可能会干扰特征值，我们没有信息来判断哪种条件会改善性能。**

# **减轻随机森林中的偏差**

消除偏差的自然方法是去除条件操作符选择的影响。这涉及使用两种操作符进行预测并平均结果。

在实践中，通过随机森林，利用数据镜像与改变条件操作符的等效性，这可以基本上以零成本近似完成。与其使用 N_e 个估计器的森林，不如构建两个规模为一半的森林，一个拟合原始数据，另一个拟合镜像数据，并取结果的平均值。请注意，这种方法适用于任何随机森林实现，仅需边际附加成本（如将数据乘以 -1 并平均结果）。

例如，我们在下面的 Python 代码中实现了这一策略，旨在从*sklearn*随机森林中去除偏差。

```py
from sklearn.base import RegressorMixin

class UnbiasedRandomForestRegressor(RegressorMixin):

    def __init__(self, **kwargs):
        # determining the number of estimators used in the
        # two subforests (with the same overall number of trees)
        self.n_estimators = kwargs.get('n_estimators', 100)

        n_leq = int(self.n_estimators / 2)
        n_l = self.n_estimators - n_estimators_leq

        # instantiating the subforests
        self.rf_leq = RandomForestRegressor(**(kwargs | {'n_estimators': n_leq}))
        self.rf_l = RandomForestRegressor(**(kwargs | {'n_estimators': n_l}))

    def fit(self, X, y, sample_weight=None):
        # fitting both subforests
        self.rf_leq.fit(X, y, sample_weight)
        self.rf_l.fit(-X, y, sample_weight)

        return self

    def predict(self, X):
        # taking the average of the predictions
        return np.mean([self.rf_leq.predict(X), self.rf_l.predict(-X)], axis=0)

    def get_params(self, deep=False):
        # returning the parameters
        return self.rf_leq.get_params(deep) | {'n_estimators': self.n_estimators}
```

接下来，我们可以执行与之前相同的实验，使用完全相同的折叠：

```py
r2_unbiased = cv_rf(X, y, UnbiasedRandomForestRegressor)
```

让我们比较一下结果！

```py
print(f'original r2: {np.mean(r2_original):.4f}')
print(f'mirrored r2: {np.mean(r2_mirrored):.4f}')
print(f'unbiased r2: {np.mean(r2_unbiased):.4f}')
# original r2: 0.8611
# mirrored r2: 0.8595
# unbiased r2: 0.8608
```

根据预期，无偏森林的 r2 分数介于原始森林的镜像数据有无之间的分数之间。消除偏差可能对性能有害；然而，我们再次强调，一旦森林用所有数据拟合，关系可能会颠倒，原始模型可能会比镜像模型产生更差的预测。**通过去除对条件操作符的依赖来消除偏差，从而消除了由于依赖默认条件操作符而导致性能恶化的风险。**

# **结论**

已经确认并展示了与条件选择和特征取等距值的交互相关的偏差的存在。考虑到这类特征的普遍存在，这种偏差很可能出现在足够深的决策树和随机森林中。通过对两个条件操作符进行预测平均，可以消除潜在的有害影响。有趣的是，对于随机森林而言，这几乎没有成本。在我们使用的例子中，改进达到了 `0.1–0.2` 个百分比点的 r2 分数水平。最后，我们强调这些结果同样适用于分类问题和单一决策树（见 [预印本](https://www.researchgate.net/publication/376591586_The_Conditioning_Bias_in_Binary_Decision_Trees_and_Random_Forests_and_Its_Elimination)）。

# **进一步阅读**

对于更多细节，我推荐：

+   我们详细讨论此主题的预印本，包含更多测试、插图和替代偏差缓解技术：[预印本](https://www.researchgate.net/publication/376591586_The_Conditioning_Bias_in_Binary_Decision_Trees_and_Random_Forests_and_Its_Elimination)。

+   包含可重复分析的 GitHub 仓库：[https://github.com/gykovacs/conditioning_bias](https://github.com/gykovacs/conditioning_bias)

+   本文背后的笔记本：[https://github.com/gykovacs/conditioning_bias/blob/main/blogpost/001-analysis.ipynb](https://github.com/gykovacs/conditioning_bias/blob/main/blogpost/001-analysis.ipynb)

*如果你对类似内容感兴趣，不要忘记订阅！你还可以在* [LinkedIn](https://www.linkedin.com/in/gyorgy-kovacs-a9799727/)*上找到我！*
