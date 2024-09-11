# 测试mlscorecheck包的报告的机器学习性能一致性

> 原文：[https://towardsdatascience.com/testing-the-consistency-of-reported-machine-learning-performance-scores-by-the-mlscorecheck-package-e1baaba57610?source=collection_archive---------6-----------------------#2023-11-12](https://towardsdatascience.com/testing-the-consistency-of-reported-machine-learning-performance-scores-by-the-mlscorecheck-package-e1baaba57610?source=collection_archive---------6-----------------------#2023-11-12)

![](../Images/fa56cf93c52f06cc70ff8f8ba26f6c98.png)

AI (Dall-E) 生成的主题描述

## 为了实现可复现的机器学习科学迈出了一小步

[](https://medium.com/@gyuriofkovacs?source=post_page-----e1baaba57610--------------------------------)[![Gyorgy Kovacs](../Images/aa5d1fcc59d738acc1056de3f0cbe7ca.png)](https://medium.com/@gyuriofkovacs?source=post_page-----e1baaba57610--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e1baaba57610--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----e1baaba57610--------------------------------) [Gyorgy Kovacs](https://medium.com/@gyuriofkovacs?source=post_page-----e1baaba57610--------------------------------)

·

[跟进](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4563dd81810c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftesting-the-consistency-of-reported-machine-learning-performance-scores-by-the-mlscorecheck-package-e1baaba57610&user=Gyorgy+Kovacs&userId=4563dd81810c&source=post_page-4563dd81810c----e1baaba57610---------------------post_header-----------) 发布在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----e1baaba57610--------------------------------) ·11分钟阅读·2023年11月12日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe1baaba57610&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftesting-the-consistency-of-reported-machine-learning-performance-scores-by-the-mlscorecheck-package-e1baaba57610&user=Gyorgy+Kovacs&userId=4563dd81810c&source=-----e1baaba57610---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe1baaba57610&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftesting-the-consistency-of-reported-machine-learning-performance-scores-by-the-mlscorecheck-package-e1baaba57610&source=-----e1baaba57610---------------------bookmark_footer-----------)

在本文中，我们探讨了如何使用Python包[mlscorecheck](https://github.com/FalseNegativeLab/mlscorecheck)来测试报告的机器学习性能分数与实验设置描述之间的一致性。

*免责声明：本文的作者是mlscorecheck包的作者。*

# **什么是性能分数的一致性测试？**

假设你遇到一个二分类问题的准确率（0.9494）、敏感性（0.8523）和特异性（0.9765）分数，这个测试集由100个正样本和1000个负样本组成。你能相信这些分数吗？你如何检查这些分数是否真的可能是所宣称的实验结果？这就是`mlscorecheck`包可以通过提供一致性测试功能来帮助你的地方。在这个具体的例子中，可以利用

```py
from mlscorecheck.check.binary import check_1_testset_no_kfold

result = check_1_testset_no_kfold(
    testset={'p': 100, 'n': 1000},
    scores={'acc': 0.8464, 'sens': 0.81, 'f1': 0.4894},
    eps=1e-4
)
result['inconsistency']
#False
```

如果结果的`'insconsistency'`标志为`False`，则表明这些分数可能是从实验中得出的。（这是真的，因为这些分数对应于81个真实的正样本和850个真实的负样本。）如果准确率分数0.8474是由于意外的打印错误而报告的呢？

```py
result = check_1_testset_no_kfold(
    testset={'p': 100, 'n': 1000},
    scores={'acc': 0.8474, 'sens': 0.81, 'f1': 0.4894},
    eps=1e-4
)
result['inconsistency']
#True
```

测试调整后的设置时，结果显示不一致：这些分数可能不是实验的结果。要么分数错误，要么假定的实验设置不正确。

在接下来的内容中，我们将详细查看[mlscorecheck](https://github.com/FalseNegativeLab/mlscorecheck)包的主要特性和使用案例。

# 介绍

在研究和应用中，监督学习方法通常通过在一些实验中计算的性能分数进行排名（[二分类](https://en.wikipedia.org/wiki/Evaluation_of_binary_classifiers)，[多分类](/micro-macro-weighted-averages-of-f1-score-clearly-explained-b603420b292f)，[回归](https://scikit-learn.org/stable/modules/model_evaluation.html#regression-metrics)）。由于出版物中的打印错误，[不当使用的统计数据](https://doi.org/10.1055/s-0033-1359421)，[数据泄漏](https://www.cell.com/patterns/abstract/S2666-3899(23)00159-9)以及[伪装](https://doi.org/10.1371/journal.pone.0005738)，许多情况下报告的性能分数是不可靠的。除了对机器学习和人工智能中的[可重复性危机](https://en.wikipedia.org/wiki/Replication_crisis)做出贡献外，不切实际的高性能分数的影响通常还会被[出版偏倚](https://en.wikipedia.org/wiki/Publication_bias)进一步放大，最终[扭曲整个领域](https://doi.org/10.1016/j.artmed.2020.101987)的研究。

[mlscorecheck](https://github.com/FalseNegativeLab/mlscorecheck)包的目标是提供数值技术**以测试一组报告的性能分数是否可能是假定实验设置的结果**。

## **一致性测试的操作**

一致性测试的理念是，在给定的实验设置中，性能分数不能独立地取任何值：

+   例如，在一个二元分类测试集中有100个正样本时，[灵敏度](https://zh.wikipedia.org/wiki/灵敏度和特异度)分数只能取值为0.0, 0.01, 0.02, …, 1.0，但不能是0.8543。

+   当报告多个性能分数时，它们需要彼此一致。例如，[准确率](https://zh.wikipedia.org/wiki/ROC曲线)是[灵敏度和特异度](https://zh.wikipedia.org/wiki/ROC曲线)的加权平均，因此，在一个由100个正样本和100个负样本组成的二元分类问题中，得分acc = 0.96，sens = 0.91，spec = 0.97是不可能的。

在更复杂的实验设置中（涉及[k折交叉验证](https://zh.wikipedia.org/wiki/交叉验证_(统计学))等），跨多个折叠/数据集的分数聚合，等等，约束条件变得更加先进，但它们仍然存在。[mlscorecheck](https://github.com/FalseNegativeLab/mlscorecheck)包实现了数值测试，以检查假设从实验中得出的分数是否满足相应的约束条件。

**测试是数值化的，确定性地识别出不一致之处**。用统计假设检验作类比，零假设是没有不一致，一旦发现某种不一致，就提供了反对零假设的证据，但作为数值测试，这种证据是无可争议的。

各种实验设置对性能分数施加各种约束，需要专门的解决方案。该包中实施的测试基于三个原则：通过区间计算加快的详尽枚举；线性整数规划；分数之间的分析关系。测试的灵敏度高度依赖于实验设置和数值不确定性：大数据集、大数值不确定性和少数报告的分数减少了测试识别偏离假设评估协议的能力。尽管如此，正如我们后面所看到的，这些测试在许多现实场景中仍然适用。有关测试数学背景的进一步详细信息，请参阅[预印本](https://www.researchgate.net/publication/374845553_Testing_the_Consistency_of_Performance_Scores_Reported_for_Binary_Classification_Problems)和[文档](https://mlscorecheck.readthedocs.io/en/latest/)。

# **用例**

现在，我们探讨一些示例，说明包的使用，但首先，我们讨论测试的一般要求和描述实验使用的一些术语。

## **要求**

一致性测试有三个要求：

+   **报告的性能分数的收集**；

+   分数的**估计数值不确定性**（当分数被截断为*4*位小数时，可以假设实际值在报告值的0.0001范围内，这是分数的数值不确定性） — 这通常是测试的eps参数，只需检查分数即可推断。

+   **实验的细节**（涉及的数据集统计信息，交叉验证方案，聚合模式）。

## **术语表**

实验规范中使用的术语：

+   *得分均值*（MoS）：为每个折叠/数据集计算得分，然后平均以获得报告的得分；

+   *均值得分*（SoM）：首先对折叠/数据集级别的原始数字（例如混淆矩阵）进行平均，然后从平均数字计算得分；

+   *微平均*：多类问题的评估通过将每个类别的表现与其他所有类别（作为二元分类）进行比较来完成，类级别的结果以*均值得分*的方式汇总；

+   *宏平均*：与微平均相同，但类级别分数以*得分均值*的方式汇总；

+   *折叠配置*：当使用 k 折交叉验证时，测试通常依赖于线性整数编程。了解折叠中类的样本数可以用于形成线性程序。这些折叠级别类样本计数称为折叠配置。

## **二元分类**

在开始时，我们已经说明了在单个测试集上计算的二元分类分数时使用包的情况。现在，我们来看一些更高级的例子。

除了我们详细调查的两个示例外，该软件包还支持共 10 种用于二元分类的实验设置，其列表可以在[文档](https://mlscorecheck.readthedocs.io/en/latest/)中找到，并在[示例笔记本](https://github.com/FalseNegativeLab/mlscorecheck/tree/main/notebooks/illustration)中提供更多示例。

**N 个测试集，均值得分汇总**

在本例中，我们假设有 N 个测试集，不涉及 k 折交叉验证，但分数以均值得分的方式汇总，即为每个测试集确定原始真正例和真负例数字，然后从总（或平均）真正例和真负例数字计算性能分数。可用的分数被认为是[准确率](https://en.wikipedia.org/wiki/Evaluation_of_binary_classifiers)，[负预测值](https://en.wikipedia.org/wiki/Evaluation_of_binary_classifiers)和[F1分数](https://en.wikipedia.org/wiki/Evaluation_of_binary_classifiers)。

例如，在实践中，对存储在一个张量中的 N 个测试图像进行图像分割技术的评估通常会导致这种情况。

软件包的设计是这样的，实验设置的细节被编码在测试函数的名称中，引导用户在选择适当的测试时注意所有可用的实验细节。在本例中，适当的测试是`mlscorecheck.check.binary`模块中的`check_n_testsets_som_no_kfold`函数，其中`'som'`代表聚合模式（均值分数）：

```py
from mlscorecheck.check.binary import check_n_testsets_som_no_kfold

scores = {'acc': 0.4719, 'npv': 0.6253, 'f1': 0.3091}
testsets = [
    {'p': 405, 'n': 223}, 
    {'p': 3, 'n': 422}, 
    {'p': 109, 'n': 404}
]

result = check_n_testsets_som_no_kfold(
    testsets=testsets,
    scores=scores,
    eps=1e-4
)
result['inconsistency']
# False
```

结果表明，分数可能是实验的结果。毫不奇怪，这些分数是通过对测试集的真正阳性和真正阴性计数进行抽样，并按指定的方式计算得出的。然而，如果其中一个分数略有变化，例如F1修改为0.3191，则配置变得不一致：

```py
scores['f1'] = 0.3191

result = check_n_testsets_som_no_kfold(
    testsets=testsets,
    scores=scores,
    eps=1e-4
)
result['inconsistency']
# True
```

进一步分析的详细信息，例如，关于可行性的证据可以从测试函数返回的字典中提取。关于输出的结构，同样，请参阅[文档](https://mlscorecheck.readthedocs.io/en/latest/)。

**1 数据集，k折交叉验证，分数均值聚合**

在这个例子中，我们假设有一个数据集，对其中的二元分类器进行了分层重复k折交叉验证（2折，3次重复），并报告了各折产生的分数的均值。

这个实验设置可能是监督式机器学习中最常用的。

我们强调了*知道*和*不知道**折叠配置*之间的区别。通常，MoS测试依赖于线性整数规划，并且需要折叠配置来制定线性整数规划。折叠配置可以通过列出折叠的统计数据来指定，或者可以引用导致确定性折叠统计的折叠策略，例如*分层*。后来，我们展示了即使在不知道折叠配置的情况下，也可以进行测试，不过在这种情况下，会测试所有可能的折叠配置，这可能会带来巨大的计算需求。

再次强调，第一步是选择要使用的适当测试。在这种情况下，正确的测试是`check_1_dataset_known_folds_mos`函数，其中`mos`表示聚合模式，`known_folds`表示由于分层而知道*折叠配置*。测试的执行如下：

```py
from mlscorecheck.check.binary import check_1_dataset_known_folds_mos

scores = {'acc': 0.7811, 'sens': 0.5848, 'spec': 0.7893}
dataset = {'p': 21, 'n': 500}
folding = {
    'n_folds': 2, 
    'n_repeats': 3, 
    'strategy': 'stratified_sklearn'
}

result = check_1_dataset_known_folds_mos(
    dataset=dataset,
    folding=folding,
    scores=scores,
    eps=1e-4
)
result['inconsistency']
# False
```

类似于之前的例子，不存在不一致性，因为性能分数准备构成一个一致的配置。然而，如果其中一个分数略有变化，测试就会检测到不一致：

```py
scores['acc'] = 0.79

result = check_1_dataset_known_folds_mos(
    dataset=dataset,
    folding=folding,
    scores=scores,
    eps=1e-4,
    verbosity=0
)
result['inconsistency']
# True
```

在前面的例子中，我们假设了折叠配置是已知的。然而，在许多情况下，确切的折叠配置并不知道，也没有指定分层。在这些情况下，可以依赖于系统地测试所有可能的折叠配置的测试，如下例所示。这次，适当的测试在其名称中具有 `'unknown_folds'` 标记，表示将测试所有可能的折叠配置：

```py
from mlscorecheck.check.binary import check_1_dataset_unknown_folds_mos

folding = {'n_folds': 2, 'n_repeats': 3}
result = check_1_dataset_unknown_folds_mos(
    dataset=dataset,
    folding=folding,
    scores=scores,
    eps=1e-4,
    verbosity=0
)
result['inconsistency']
# False
```

与以前一样，测试正确地识别出没有不一致性：在评估所有可能的折叠配置过程中，它测试了实际的分层配置，显示出一致性，并且凭借这一证据停止了对剩余配置的测试。

在实践中，在使用未知折叠进行测试之前，建议对可能要测试的折叠配置数量进行估计：

```py
from mlscorecheck.check.binary import estimate_n_evaluations

estimate_n_evaluations(
    dataset=dataset, 
    folding=folding, 
    available_scores=['acc', 'sens', 'spec']
)
# 4096
```

在最坏的情况下，解决 4096 个小型线性整数规划问题仍然可行，但是对于更大的数据集，潜在的折叠配置数量可能会迅速变得棘手。

## **多类分类**

测试多类分类场景类似于二元情况，因此我们不会像在二元情况下那样进入太多细节。

在该包支持的6个实验设置中，我们选择了一个常用的用于说明的设置：假设有一个多类数据集（4类），并且使用了4折重复分层k折交叉验证。我们还知道分数是以*宏平均*的方式聚合的，即，在每个折叠中，针对每个类别的性能以二元分类方式评估所有其他类别，然后在类别和折叠上进行平均。

再次，第一步是选择合适的测试函数，在这种情况下，选择了来自`mlscorecheck.check.multiclass`模块的`check_1_dataset_known_folds_mos_macro`。名称中的 `'mos’` 和 `'macro’` 表示实验中使用的聚合方式。

```py
from mlscorecheck.check.multiclass import check_1_dataset_known_folds_mos_macro

scores = {'acc': 0.626, 'sens': 0.2483, 'spec': 0.7509}
dataset = {0: 149, 1: 118, 2: 83, 3: 154}
folding = {
    'n_folds': 4, 
    'n_repeats': 2, 
    'strategy': 'stratified_sklearn'
}

result = check_1_dataset_known_folds_mos_macro(
    dataset=dataset,
    folding=folding,
    scores=scores,
    eps=1e-4,
    verbosity=0
)
result['inconsistency']
# False
```

类似于前面的情况，通过手工制作的一组一致分数，测试检测到没有不一致性。然而，一个小的改变，例如，将准确度修改为0.656，就会使配置变得不可行。

## **回归**

[mlscorecheck](https://github.com/FalseNegativeLab/mlscorecheck)包支持的最后一个监督学习任务是回归。测试回归问题是最困难的，因为对测试集的预测可以取任何值，因此实验可以产生任何分数值。回归测试唯一可以依赖的是当前支持的*平均绝对误差 (mae)*、*均方误差 (mse)* 和 *r平方* (r2) 之间的数学关系。

在以下示例中，我们假设*mae*和*r2*分数是针对测试集报告的，并且我们知道其主要统计数据（样本数量和方差）。然后，可以执行一致性测试，如下所示：

```py
from mlscorecheck.check.regression import check_1_testset_no_kfold

var = 0.0831
n_samples = 100
scores =  {'mae': 0.0254, 'r2': 0.9897}

result = check_1_testset_no_kfold(
    var=var,
    n_samples=n_samples,
    scores=scores,
    eps=1e-4
)
result['inconsistency']
# False
```

再次地，测试正确显示没有不一致（分数是通过实际评估准备的）。但是，如果*r2*分数稍微改变，例如，变为0.9997，配置将变得不可行。

# **测试包**

为了使针对流行的、广泛研究的问题报告的分数的一致性测试更容易，[mlscorecheck](https://github.com/FalseNegativeLab/mlscorecheck)包包括了多个被认为是某些问题标准的实验设置的规范。

## **DRIVE数据集上的视网膜血管分割**

在视网膜图像分析领域，存在一个[歧义](https://www.researchgate.net/publication/350236730_A_new_baseline_for_retinal_vessel_segmentation_Numerical_identification_and_correction_of_methodological_inconsistencies_affecting_100_papers)的问题：作者可以自由选择是否考虑视野圆形区域之外的像素，而这一选择在出版物中很少被指明。这种歧义可能导致基于不可比性能分数的算法排名。在[mlscorecheck](https://github.com/FalseNegativeLab/mlscorecheck)包中实现的功能适合识别作者是否使用了视野之外的像素进行评估。

最广泛研究的问题之一是基于[DRIVE](https://ieeexplore.ieee.org/document/1282003)数据集的血管分割。为了避免查找图像统计数据和构建实验设置的繁琐任务，包中包含了数据集的统计数据，并提供了两个高级功能来测试图像级和聚合分数的歧义。例如，拥有测试图像‘03’的[DRIVE](https://ieeexplore.ieee.org/document/1282003)数据集的图像级准确性、敏感性和特异性分数三元组，可以利用包进行如下操作：

```py
from mlscorecheck.check.bundles.retina import check_drive_vessel_image

scores = {'acc': 0.9323, 'sens': 0.5677, 'spec': 0.9944}

result = check_drive_vessel_image(
    scores=scores,
    eps=10**(-4),
    image_identifier='03',
    annotator=1
)
result['inconsistency']
# {'inconsistency_fov': False, 'inconsistency_all': True}
```

结果表明，对于该图像的分数必须仅通过使用视野内的(*fov*)像素进行评估，因为这些分数与这一假设是一致的，但与使用*所有*像素进行评估的替代假设是不一致的。

## **进一步的测试包**

[mlscorecheck](https://github.com/FalseNegativeLab/mlscorecheck)包中支持的所有流行研究问题和对应的公开数据集的列表如下：

+   视网膜图像分析：

    - 血管分割： [DRIVE](https://ieeexplore.ieee.org/document/1282003), [STARE](https://pubmed.ncbi.nlm.nih.gov/9929355/), [HRF](https://pubmed.ncbi.nlm.nih.gov/24416040/), [CHASE_DB1](https://ieeexplore.ieee.org/document/6224174);

    - 视网膜病变识别：[DIARETDB0](https://www.semanticscholar.org/paper/DIARETDB-0-%3A-Evaluation-Database-and-Methodology-Kauppi-Kalesnykiene/bd7d2380e76fb9dfd367d669e311d4913f67f7d2)，[DIARETDB1](https://www.researchgate.net/publication/221259835_DIARETDB1_diabetic_retinopathy_database_and_evaluation_protocol)；

    - 视盘/杯分割：[DRISHTI_GS](https://ieeexplore.ieee.org/document/6867807)；

    - 渗出物分割：[DIARETDB1](https://www.researchgate.net/publication/221259835_DIARETDB1_diabetic_retinopathy_database_and_evaluation_protocol)；

+   皮肤病变分类：[ISIC2016](https://arxiv.org/abs/1605.01397)，[ISIC2017](https://arxiv.org/abs/1710.05006)；

+   从电生理图信号预测足月分娩： [TPEHG](https://pubmed.ncbi.nlm.nih.gov/18437439/)。

## **征稿启事**

欢迎各领域的专家提交更多测试包，以促进对机器学习性能评分在各研究领域的验证！

# **结论**

对机器学习研究的荟萃分析不包括很多技术，超出了对论文的全面评估和可能尝试重新实现所提出的方法以验证声明结果的范围。[mlscorecheck](https://github.com/FalseNegativeLab/mlscorecheck)包提供的功能使得对机器学习研究的荟萃分析更为简明、数值化，有助于维护各研究领域的完整性。

# **进一步阅读**

如需更多信息，我们建议查看：

+   [mlscorecheck](https://github.com/FalseNegativeLab/mlscorecheck)包的README，

+   套件中提供的[说明性笔记本](https://github.com/FalseNegativeLab/mlscorecheck/tree/main/notebooks/illustration)，

+   详细[文档](https://mlscorecheck.readthedocs.io/en/latest/)，

+   描述数值方法的[预印本](https://www.researchgate.net/publication/374845553_Testing_the_Consistency_of_Performance_Scores_Reported_for_Binary_Classification_Problems)。
