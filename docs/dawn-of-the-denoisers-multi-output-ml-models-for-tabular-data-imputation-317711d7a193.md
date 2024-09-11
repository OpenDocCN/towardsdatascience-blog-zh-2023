# **去噪声器的黎明：用于表格数据插补的多输出机器学习模型**

> 原文：[https://towardsdatascience.com/dawn-of-the-denoisers-multi-output-ml-models-for-tabular-data-imputation-317711d7a193?source=collection_archive---------6-----------------------#2023-06-28](https://towardsdatascience.com/dawn-of-the-denoisers-multi-output-ml-models-for-tabular-data-imputation-317711d7a193?source=collection_archive---------6-----------------------#2023-06-28)

## 方法概述与实际应用

[](https://kcpub21.medium.com/?source=post_page-----317711d7a193--------------------------------)[![Chinmay Kakatkar](../Images/873531f24cd4ee8a927669a211d61ae8.png)](https://kcpub21.medium.com/?source=post_page-----317711d7a193--------------------------------)[](https://towardsdatascience.com/?source=post_page-----317711d7a193--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----317711d7a193--------------------------------) [Chinmay Kakatkar](https://kcpub21.medium.com/?source=post_page-----317711d7a193--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F87ab8c40e0ed&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdawn-of-the-denoisers-multi-output-ml-models-for-tabular-data-imputation-317711d7a193&user=Chinmay+Kakatkar&userId=87ab8c40e0ed&source=post_page-87ab8c40e0ed----317711d7a193---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----317711d7a193--------------------------------) · 11 分钟阅读 · 2023年6月28日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F317711d7a193&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdawn-of-the-denoisers-multi-output-ml-models-for-tabular-data-imputation-317711d7a193&user=Chinmay+Kakatkar&userId=87ab8c40e0ed&source=-----317711d7a193---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F317711d7a193&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdawn-of-the-denoisers-multi-output-ml-models-for-tabular-data-imputation-317711d7a193&source=-----317711d7a193---------------------bookmark_footer-----------)![](../Images/3ac7476e11ca163995f5a03ae562d2f0.png)

图片由 [Jon Tyson](https://unsplash.com/ja/@jontyson?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

处理表格数据中的缺失值是数据科学中的一个基本问题。如果由于某种原因不能忽略或省略缺失值，那么我们可以尝试*填补*它们，即用其他值替代缺失值。填补有几种简单（但过于简单）的方式和几种先进的（更准确但复杂且可能需要大量资源）方式。本文介绍了一种表格数据填补的新方法，旨在实现简洁性和实用性之间的平衡。

具体来说，我们将看到如何利用*去噪*的概念（通常与非结构化数据相关联）来迅速将几乎任何多输出机器学习算法转变为适用于实际的表格数据填补器。我们将首先介绍一些关于去噪、填补和多输出算法的基本概念，然后深入探讨如何使用去噪将多输出算法转变为填补器。接着，我们将简要地看一下这种新颖的方法如何在实际中应用，并以行业中的一个例子为例。最后，我们将讨论在生成式人工智能和基础模型时代，基于去噪的表格数据填补的未来相关性。为了便于解释，代码示例仅以Python语言展示，尽管概念方法本身是不依赖于语言的。

# 从去噪到填补

去噪是关于从数据中去除噪声的。去噪算法将含有噪声的数据作为输入，进行一些巧妙的处理以尽可能减少噪声，然后返回去噪后的数据。去噪的典型应用包括去除音频数据中的噪声和锐化模糊的图像。去噪算法可以通过多种方法来构建，从高斯和中值滤波器到自编码器。

尽管去噪的概念主要与涉及非结构化数据（例如，音频、图像）的应用场景相关，但对结构化表格数据的填补是一个密切相关的概念。有许多方法可以替代（或填补）表格数据中的缺失值。例如，数据可以简单地用零（或在特定上下文中等效的值）替代，或用相关行或列的一些统计数据（例如，均值、中位数、众数、最小值、最大值）替代——但这样做可能会扭曲数据，如果作为机器学习训练工作流程中的预处理步骤使用，这种简单的填补可能会对预测性能产生不利影响。其他方法如K近邻（KNN）或关联规则挖掘可能表现更好，但由于它们没有训练的概念，并直接在测试数据上工作，当测试数据量很大时，它们的速度可能会受到影响；这在需要快速在线推理的应用场景中尤其成问题。

现在，可以简单地训练一个 ML 模型，将具有缺失值的特征设置为输出，并使用其余特征作为预测器（或输入）。如果我们有多个具有缺失值的特征，构建每个特征的单输出模型可能既繁琐又昂贵，因此我们可以尝试构建一个多输出模型，一次性预测所有受影响特征的缺失值。至关重要的是，如果缺失值可以被视为噪声，那么我们可能能够应用去噪概念来填补表格数据——这是我们将在以下部分中深入探讨的关键见解。

# 多输出 ML 算法

正如其名所示，*多输出*（或*多目标*）算法可用于训练模型以同时预测多个输出/目标特征。Scikit-learn 网站提供了关于分类和回归的多输出算法的出色概述（参见 [这里](https://scikit-learn.org/stable/modules/multiclass.html)）。

虽然一些 ML 算法允许开箱即用的多输出建模，但其他算法可能仅原生支持*单输出*建模。像 Scikit-learn 这样的库提供了利用单输出算法进行多输出建模的方法，通过提供实现通常功能（如*fit* 和 *predict*）的包装器，并在后台独立应用这些包装器到单输出模型。以下示例代码展示了如何将原生仅支持单输出建模的 Scikit-learn 中的线性支持向量回归（Linear SVR）封装成一个多输出回归器，使用 *MultiOutputRegressor* 包装器。

```py
from sklearn.datasets import make_regression
from sklearn.svm import LinearSVR
from sklearn.multioutput import MultiOutputRegressor

# Construct a toy dataset
RANDOM_STATE = 100
xs, ys = make_regression(
    n_samples=2000, n_features=7, n_informative=5, 
    n_targets=3, random_state=RANDOM_STATE, noise=0.2
)

# Wrap the Linear SVR to enable multi-output modeling
wrapped_model = MultiOutputRegressor(
    LinearSVR(random_state=RANDOM_STATE)
).fit(xs, ys)
```

虽然这种封装策略至少让我们在多输出用例中使用单输出算法，但它可能无法考虑输出特征之间的相关性或依赖性（即预测的输出特征集合是否整体上有意义）。相比之下，一些原生支持多输出建模的 ML 算法似乎确实考虑了输出间的关系。例如，当使用 Scikit-learn 中的决策树根据一些输入数据建模 *n* 个输出时，所有 *n* 个输出值都存储在叶子节点中，并使用考虑所有 *n* 个输出值作为一个集合的拆分标准，例如，通过对它们进行平均（参见 [这里](https://scikit-learn.org/stable/modules/tree.html#tree)）。以下示例代码展示了如何构建一个多输出决策树回归器——你会注意到，在表面上，这些步骤与之前训练带有包装器的线性支持向量回归（Linear SVR）时的步骤非常相似。

```py
from sklearn.datasets import make_regression
from sklearn.tree import DecisionTreeRegressor

# Construct a toy dataset
RANDOM_STATE = 100
xs, ys = make_regression(
    n_samples=2000, n_features=7, n_informative=5, 
    n_targets=3, random_state=RANDOM_STATE, noise=0.2
)

# Train a multi-output model directly using a decision tree
model = DecisionTreeRegressor(random_state=RANDOM_STATE).fit(xs, ys)
```

# 将多输出 ML 模型训练为表格数据插补的去噪器

现在我们已经涵盖了去噪、插补和多输出机器学习算法的基础知识，我们准备将所有这些构建块结合起来。通常，训练多输出机器学习模型以使用去噪插补表格数据包括以下步骤。请注意，与前一节中的代码示例不同，在接下来的步骤中我们不会明确区分预测变量和目标变量——这是因为，在表格数据插补的上下文中，如果特征在数据中存在，它们可以作为预测变量，如果缺失，它们可以作为目标变量。

## 第1步：创建训练和验证数据集

将数据拆分为训练集和验证集，例如，使用80:20的拆分比例。我们将这些数据集称为*df_training*和*df_validation*。

## 第2步：创建训练和验证数据集的噪声/掩盖副本

复制*df_training*和*df_validation*，并在这些副本的数据中添加噪声，例如，通过随机掩盖值。我们将这些掩盖后的副本称为*df_training_masked*和*df_validation_masked*。掩盖函数的选择可能会影响最终训练的插补器的预测准确性，因此我们将在下一节中讨论一些掩盖策略。此外，如果*df_training*的大小较小，可以考虑将行数上采样到某个因子*k*，这样如果*df_training*有*n*行和*m*列，则上采样后的*df_training_masked*数据集将有*n*k*行（列数*m*保持不变）。

## 第3步：训练一个基于去噪的多输出模型作为插补器

选择一个你喜欢的多输出算法，并训练一个使用噪声/掩盖副本来预测原始训练数据的模型。从概念上讲，你可以进行类似*model.fit(predictors = df_training_masked, targets = df_training)*的操作。

## 第4步：将插补器应用于被掩盖的验证数据集

将*df_validation_masked*传递给训练好的模型以预测*df_validation*。从概念上讲，这将类似于*df_validation_imputed = model.predict(df_validation_masked)*。注意，某些拟合函数可能会直接将验证数据集作为参数来计算拟合过程中的验证误差（例如，在TensorFlow中的神经网络）——如果是这样，那么记得在计算验证误差时，使用噪声/掩盖验证集（*df_validation_masked*）作为预测变量，使用原始验证集（*df_validation*）作为目标变量。

## 第5步：评估验证数据集的插补准确性

通过将*df_validation_imputed*（模型预测的结果）与*df_validation*（真实值）进行比较来评估填补准确性。评估可以按列（以确定每个特征的填补准确性）或按行（以检查预测实例的准确性）进行。为了避免每列得到夸大的准确性结果，可以在计算准确性之前，过滤掉在*df_validation_masked*中未掩盖的待预测列值的行。

最后，尝试上述步骤来优化模型（例如，使用另一种掩码策略或选择不同的多输出机器学习算法）。

以下代码展示了如何实现步骤1–5的一个示例。

```py
import pandas as pd
import numpy as np
from sklearn.datasets import make_classification
from sklearn.tree import DecisionTreeClassifier

# Construct a toy dataset
RANDOM_STATE = 100
data = make_classification(n_samples=2000, n_features=7, n_classes=1, random_state=RANDOM_STATE, class_sep=2, n_informative=3)
df = pd.DataFrame(data[0]).applymap(lambda x: int(abs(x)))

#####
# Step 1: Create training and validation datasets
#####

TRAIN_TEST_SPLIT_FRAC = 0.8
n = int(df.shape[0]*TRAIN_TEST_SPLIT_FRAC)

df_training, df_validation = df.iloc[:n, :], df.iloc[n:, :].reset_index(drop=True)

#####
# Step 2: Create noisy/masked copies of training and validation datasets
#####

# Example of random masking where each decision to mask a value is framed as a coin toss (Bernoulli event)
def random_masking(value): return -1 if np.random.binomial(n=1, p=0.5) else value
df_training_masked = df_training.applymap(random_masking)
df_validation_masked = df_validation.applymap(random_masking)

#####
# Step 3: Train a multi-output model to be used as a denoising-based imputer
#####

# Notice that the masked data is used to model the original data
model = DecisionTreeClassifier(random_state=RANDOM_STATE).fit(X=df_training_masked, y=df_training)

#####
# Step 4: Apply imputer to masked validation dataset
#####

df_validation_imputed = pd.DataFrame(model.predict(df_validation_masked))

#####
# Step 5: Evaluate imputation accuracy on validation dataset
#####

# Check basic top-1 accuracy metric, accounting for inflated results
feature_accuracy_dict = {}
for i in range(df_validation_masked.shape[1]):
    # Get list of row indexes where feature i was masked, i.e., needed to be imputed
    masked_indexes = df_validation_masked.index[df_validation_masked[i] == -1]
    # Compute imputation accuracy only for those rows for feature i
    feature_accuracy_dict[i] = (df_validation_imputed.iloc[masked_indexes, i] == df_validation.iloc[masked_indexes, i]).mean()
print(feature_accuracy_dict)
```

# 数据掩码策略

通常，可以采用几种策略来掩盖训练和验证数据。从高层次来看，我们可以区分三种掩码策略：*穷举*、*随机*和*领域驱动*。

## 穷举掩码

该策略涉及为数据集中每一行生成所有可能的掩码组合。假设我们有一个包含*n*行和*m*列的数据集。那么穷举掩码将把每一行扩展到最多2^*m*行，每种掩码组合对应一行；该行的最大组合总数等于帕斯卡三角形中行*m*的和，尽管我们可能选择忽略一些对特定用例不有用的组合（例如，所有值都被掩盖的组合）。因此，最终掩盖的数据集将最多包含*n**(2^*m*)行和*m*列。虽然穷举策略具有相当全面的优点，但在*m*较大的情况下可能不太实际，因为生成的掩盖数据集可能过大，以至于大多数计算机难以处理。例如，如果原始数据集仅有1000行和50列，则穷举掩盖的数据集将大约有10¹⁸行（即一千亿亿行）。

## 随机掩码

正如其名称所示，该策略通过使用某种随机函数来掩盖值。例如，在一个简单的实现中，决定掩盖数据集中的每个值可以被视为独立的伯努利事件，掩盖的概率为*p*。随机掩码策略的明显优点是，与穷举掩码不同，掩盖数据的大小是可以管理的。然而，特别是在小数据集的情况下，为了达到足够高的填补准确性，可能需要在应用随机掩码之前对训练数据集进行上采样，以便在生成的掩盖数据集中反映更多的掩码组合。

## 领域驱动掩码

该策略旨在以一种接近现实生活中缺失值模式的方式应用掩码，即在填补器将被使用的领域或用例中。为了发现这些模式，分析定量的观察数据以及结合领域专家的见解可能会很有用。

# 实际应用

本文讨论的基于去噪的填补器可以在实践中提供一种务实的“中间路径”，其中其他方法可能过于简单或过于复杂且资源密集。除了在更大的 ML 工作流中作为数据清洗的预处理步骤外，基于去噪的表格数据填补还可能用于推动某些实际用例中的核心产品功能。

AI 辅助完成在线表单是行业中的一个例子。随着各种业务流程的数字化，纸质表单正被数字在线版本所取代。提交工作申请、创建采购申请、企业旅行预订和注册活动等流程通常涉及填写某种在线表单。手动完成这样的表单可能会很繁琐、耗时且可能容易出错，尤其是当表单有多个需要填写的字段时。然而，在 AI 助手的帮助下，通过根据可用的上下文信息向用户提供输入建议，完成这样的在线表单可以变得更加轻松、快捷和准确。例如，当用户开始填写表单上的某些字段时，AI 助手可以推断剩余字段最可能的值，并实时向用户建议这些值。这样的用例可以被自然地框架为基于去噪的多输出填补问题，其中噪声/掩码数据由表单的当前状态（有些字段填写了，其他字段为空/缺失）给出，目标是预测缺失的字段。可以根据需要调整模型以满足各种用例要求，包括预测准确性和端到端响应时间（用户感知的）。

# 在生成性人工智能和基础模型时代的相关性

随着生成性人工智能和基础模型的最新进展，以及即使在非技术观众中也越来越意识到它们的潜力，自从 ChatGPT 于 2022 年末亮相以来，探讨基于去噪的填补器在未来的相关性是合理的。例如，大型语言模型（LLMs）可以在理论上处理表格数据的填补任务。毕竟，预测句子中的缺失标记是用于训练像**双向编码器表示（BERT）**这样的 LLMs 的典型学习目标。

然而，基于去噪的插补方法——或现今存在的其他更简单的表格数据插补方法——在生成式 AI 和基础模型的时代，不太可能很快变得过时。了解这一点的原因可以通过考虑2010年代末期的情况来感受，那时神经网络已成为在几个以前依赖简单算法（如逻辑回归、决策树和随机森林）的用例中，技术上更可行且经济上更具吸引力的选项。虽然神经网络确实在某些需要大规模训练数据且训练和维护成本被认为是合理的高端用例中取代了这些其他算法，但许多其他用例仍未受到影响。实际上，神经网络的普及得益于存储和计算资源成本的降低，而这种降低也使得其他简单算法受益。从这个角度来看，成本、复杂性、解释性需求、实时用例的快速响应时间以及对可能出现的寡头垄断外部预训练模型提供商的锁定威胁等因素，都似乎指向一个未来，在这个未来中，像基于去噪的表格数据插补这样的务实创新有可能与生成式 AI 和基础模型有意义地共存，而不是被它们取代。
