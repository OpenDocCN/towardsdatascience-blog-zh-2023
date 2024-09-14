# 如何准备机器学习数据

> 原文：[`towardsdatascience.com/how-to-prepare-data-for-machine-learning-eb9d9973832f`](https://towardsdatascience.com/how-to-prepare-data-for-machine-learning-eb9d9973832f)

## *准备数据以进行建模是数据科学流程中最基本的第一步之一*

[](https://medium.com/@theDrewDag?source=post_page-----eb9d9973832f--------------------------------)![Andrea D'Agostino](https://medium.com/@theDrewDag?source=post_page-----eb9d9973832f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----eb9d9973832f--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----eb9d9973832f--------------------------------) [Andrea D'Agostino](https://medium.com/@theDrewDag?source=post_page-----eb9d9973832f--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----eb9d9973832f--------------------------------) ·8 分钟阅读·2023 年 4 月 19 日

--

![](img/a4201ec0fe98ad642b92ad48c8e24ac9.png)

图片由 [John Moeses Bauan](https://unsplash.com/it/@johnmoeses?utm_source=ghost&utm_medium=referral&utm_campaign=api-credit) 提供 / [Unsplash](https://unsplash.com/?utm_source=ghost&utm_medium=referral&utm_campaign=api-credit)

训练预测模型要求我们的数据格式正确。我们不能将 `.csv` 文件直接输入模型，并期望它能够正确地进行泛化。

在本文中，我们将探讨如何准备机器学习数据，从数据准备流程开始，到将数据划分为训练集、验证集和测试集。

# 数据准备流程

流程是为了准备数据以供机器学习模型处理而执行的一系列步骤。流程可以根据你拥有的数据类型有所不同，但通常包括以下步骤：

**数据收集**：第一步是收集你想用来训练机器学习模型的数据。这些数据可以来自不同的来源，如 CSV 文件、数据库、API、物联网传感器等。

**数据清理**：一旦数据被收集，重要的是要通过删除缺失数据、纠正错误以及删除重复或无关的数据来清理数据。数据清理有助于提高用于训练机器学习模型的数据质量。

**数据预处理**：数据预处理包括数据规范化、数据标准化、分类变量编码和异常值处理。这些步骤有助于准备数据，使其能够被机器学习模型处理。

**数据转换**：数据转换包括降维、特征选择和新特征的创建。这些步骤有助于减少数据噪声，提高机器学习模型做出准确预测的能力。

> *管道*一词也用于 Python 的 Scikit-learn 库中的一个编程对象。这允许我们指定数据处理步骤，以便于编码并减少错误的发生。

如果你想了解 Scikit-Learn 管道，可以阅读下面的文章

[](https://medium.com/mlearning-ai/how-to-use-sklearns-pipelines-to-optimize-your-analysis-b6cd91999be?source=post_page-----eb9d9973832f--------------------------------) [## 如何使用 Sklearn 的管道来优化你的分析

### 在数据科学和机器学习中，管道（pipeline）是一组顺序步骤，允许我们控制数据流动的…

medium.com](https://medium.com/mlearning-ai/how-to-use-sklearns-pipelines-to-optimize-your-analysis-b6cd91999be?source=post_page-----eb9d9973832f--------------------------------)

# 数据收集

从可靠且与您试图解决的问题相关的数据源中收集数据是很重要的。

例如，如果你想创建一个用于预测房价的机器学习模型，你需要收集包括房屋位置、房间数量、是否有花园、靠近公共交通等特征的数据。这些信息就像它们向我们人类提供房产价值一样，也会对预测模型提供帮助。

选择最适合你的问题和数据可用性的来源非常重要。例如，如果你想为物联网系统中的异常检测构建机器学习模型，你需要从系统中的传感器收集数据。

其他数据来源可以是

+   公共和私人 API

+   直接供应商

+   调查

此外，评估**收集的数据质量**是很重要的。数据可能包含错误、重复或遗漏，这些问题可能会对机器学习模型的质量产生负面影响。

因此，你应该进行数据清理，并检查是否有缺失、重复或不良数据。数据清理可以帮助提高机器学习模型的质量，并实现更准确的预测。

这里有一篇文章专注于数据集创建的过程

[](/building-your-own-dataset-benefits-approach-and-tools-6ab096e37f2?source=post_page-----eb9d9973832f--------------------------------) ## 自建数据集：好处、方法和工具

### 自建数据集而不是使用预建解决方案的重要性

towardsdatascience.com

## 数据收集阶段的实际例子

一个实际的数据收集例子可能是构建一个用于分类花卉的模型。在这种情况下，**我们可以收集不同花卉的数据，比如花瓣和萼片，并记录它们的长度和宽度。**

为了收集这些数据，**我们可以使用一个移动应用程序**，该程序允许我们拍摄花卉照片并记录它们的特征。或者，我们可以使用电子表格或数据库手动收集数据。

无论你使用什么数据收集方法，确保**数据能代表要解决的问题**是很重要的。在这种情况下，我们可以收集来自不同物种和不同地理区域的花卉数据，以确保数据的多样性。

# 数据预处理

一般来说，数据预处理**包括数据的归一化或标准化、类别变量的编码和异常值处理**。

**数据归一化 / 标准化**用于减少数据的尺度，以便它们之间可以相互比较。许多机器学习模型，例如 K-近邻和神经网络，要求数据进行归一化或标准化才能表现良好。

> 标准化和归一化这两个术语通常可以互换使用。但实际上，它们并不完全相同。
> 
> 我邀请有兴趣的读者阅读这个资源[解释标准化和归一化之间差异的文章](https://www.simplilearn.com/normalization-vs-standardization-article)以了解更多信息。

**类别变量编码**用于处理那些不是数字格式的变量，例如性别或眼睛颜色。类别变量编码将这些变量转换为数字，以供机器学习模型使用。

**异常值处理**用于处理那些与其余数据差异很大的数据。这些数据可能对机器学习模型产生负面影响，因此必须妥善管理。

## 数据预处理阶段的实际示例

让我们思考文本。大量的文本组织成语料库（文档列表）。在将这些数据输入机器学习模型之前，应该首先对其进行处理。

一个假设的流程可能是：

+   文本标记化

+   停用词移除

+   词干提取 / 词形还原

+   向量化

这些是文本数据预处理中的一些最常见步骤。

# 数据转换

数据转换可以包括**维度减少、特征选择和新特征的创建**。

**维度减少**用于减少数据中的特征数量。当你拥有大量数据但资源受限，例如机器学习模型的处理时间，这一步骤会非常有用。最常用的技术之一是**PCA（主成分分析）**。

**特征选择**用于选择数据中最重要的特征。当你拥有许多特征但希望只使用其中一部分来训练机器学习模型时，这一步骤会非常有用。

阅读如何在 Python 中使用一种叫做*Boruta*的技术进行特征选择

[](/feature-selection-with-boruta-in-python-676e3877e596?source=post_page-----eb9d9973832f--------------------------------) ## Python 中的 Boruta 特征选择

### 了解 Boruta 算法如何进行特征选择。解释 + 模板

[towardsdatascience.com

**创建新特征**用于从现有数据中创建新特征。这个步骤在你想要创建在原始数据中不存在但对训练机器学习模型有用的特征时特别有用。

## 数据转换阶段的实际示例

PCA 主要用于减少机器学习模型中的可用特征数量。

在 Sklearn 中实现 PCA 非常简单 —— 这段代码片段捕捉了训练和应用 PCA 的一般思路。

```py
from sklearn.decomposition import PCA

pca = PCA(n_components=2)

pca.fit(X_train)

X_train_pca = pca.transform(X_train)

# we have successfully reduced the entire feature set to just two variables
x0 = X_train_pca[:, 0]
x1 = X_train_pca[:, 1]
```

# 将数据划分为训练集、验证集和测试集

将数据划分为训练集、验证集和测试集是准备机器学习数据的重要步骤。

**训练集**：训练集用于训练机器学习模型。它包含模型将用来学习对预测有用的关系的数据。

**验证集**：验证集用于在训练过程中评估机器学习模型的性能，并测试其超参数。

**测试集**：测试集用于在训练后评估机器学习模型的性能。

将数据划分为训练集、验证集和测试集很重要，因为它可以准确评估机器学习模型的性能。

如果你使用所有数据来训练机器学习模型，然后在数据本身上评估模型性能，你可能会得到模型性能的扭曲图景。

> 正确评估模型是否学习到知识的最重要技术之一叫做**交叉验证**。
> 
> 它包括将训练集划分为折叠，以便迭代地评估模型。

如果你想了解交叉验证及其如何在你的代码中应用，阅读下面的文章。

[](/what-is-cross-validation-in-machine-learning-14d2a509d6a5?source=post_page-----eb9d9973832f--------------------------------) ## 机器学习中的交叉验证是什么

### 了解什么是交叉验证 —— 构建可泛化模型的基本技术。

[towardsdatascience.com

## 使用 Sklearn 的 `train_test_split` 的实际示例

Sklearn 提供了一个很棒的 API 来管理数据拆分。看看这段代码。

```py
from sklearn.model_selection import train_test_split

# X is out feature set, y is the target column
X_trainval, X_test, y_trainval, y_test = train_test_split(X, y, test_size=0.2, random_state=42)
X_train, X_val, y_train, y_val = train_test_split(X_trainval, y_trainval, test_size=0.25, random_state=42)

print(f"Training set shape: {X_train.shape}")
print(f"Validation set shape: {X_val.shape}")
print(f"Test set shape: {X_test.shape}")
```

在这个示例中，我们将数据按 60/20/20 的比例划分为训练集、验证集和测试集。换句话说，我们使用 60% 的数据来训练模型，20% 来验证模型在训练过程中的表现，其余的 20% 用于在训练后测试模型。

`random_state` 参数允许我们设置随机数生成种子，以便每次运行代码时都能获得相同的数据划分。

最终，我们打印了训练集、验证集和测试集的大小，以便验证数据划分是否正确。

使用 `sklearn`，将数据分为训练集、验证集和测试集变得快速而简单，使我们能够专注于设计和训练模型。

# 结论

在这篇文章中，我们查看了为机器学习准备数据所需的步骤。

我们已经看到，从可靠且与问题相关的来源收集数据以及数据清洗是确保用于训练机器学习模型的数据质量的关键。

我们还研究了数据预处理，包括数据归一化和标准化、分类变量编码和异常值处理，以及数据转换，这可能包括降维、特征选择和新特征的创建。

最终，我们了解了将数据分为训练集、验证集和测试集以准确评估机器学习模型性能的重要性。

为机器学习准备数据需要密切关注细节，但正确执行数据准备步骤可能是机器学习模型有效与否的关键。

**如果你想支持我的内容创作活动，请随时通过下面的推荐链接加入 Medium 会员计划**。我将获得您投资的一部分，您将能够以无缝的方式访问 Medium 上大量关于数据科学及更多内容的文章。

[](https://medium.com/@theDrewDag/membership?source=post_page-----eb9d9973832f--------------------------------) [## 使用我的推荐链接加入 Medium - Andrea D'Agostino

### 阅读 Andrea D'Agostino 的所有故事（以及 Medium 上成千上万的其他作者的故事）。您的会员费用直接……

medium.com](https://medium.com/@theDrewDag/membership?source=post_page-----eb9d9973832f--------------------------------)

# 推荐阅读

对有兴趣的人，这里列出了我推荐的每个与机器学习相关的主题的书籍。这些书籍在我看来是**必读**的，并且对我的职业生涯产生了很大影响。

*免责声明：这些是亚马逊会员链接。我会因推荐这些商品而从亚马逊获得少量佣金。您的体验不会改变，也不会额外收费，但这将帮助我扩大业务并制作更多关于人工智能的内容。*

+   **机器学习入门：** [*自信的数据技能：掌握数据工作基础，提升你的职业生涯*](https://amzn.to/3ZzKTz6) 作者：Kirill Eremenko

+   **Sklearn / TensorFlow：** [*动手实践机器学习：Scikit-Learn、Keras 和 TensorFlow*](https://amzn.to/433F4Nm) 作者：Aurelien Géron

+   **NLP：** [*文本作为数据：机器学习与社会科学的新框架*](https://amzn.to/3zvH43j) 作者：Justin Grimmer

+   **Sklearn / PyTorch：** [*使用 PyTorch 和 Scikit-Learn 进行机器学习：用 Python 开发机器学习和深度学习模型*](https://amzn.to/3Gcavve) 由 Sebastian Raschka 著

+   **数据可视化：** [*用数据讲故事：商业专业人士的数据可视化指南*](https://amzn.to/3HUtGtB) 由 Cole Knaflic 著

# 有用的链接（由我编写）

+   **学习如何在 Python 中进行顶级的探索性数据分析：** *Python 中的探索性数据分析 — 一步一步的过程*

+   **学习 TensorFlow 基础知识：** [*开始使用 TensorFlow 2.0 — 深度学习简介*](https://medium.com/towards-data-science/a-comprehensive-introduction-to-tensorflows-sequential-api-and-model-for-deep-learning-c5e31aee49fa)

+   **使用 TF-IDF 在 Python 中进行文本聚类：** [*Python 中的 TF-IDF 文本聚类*](https://medium.com/mlearning-ai/text-clustering-with-tf-idf-in-python-c94cd26a31e7)
