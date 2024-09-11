# Apache Spark MLlib 与 Scikit-learn：构建机器学习流水线

> 原文：[https://towardsdatascience.com/apache-spark-mllib-vs-scikit-learn-building-machine-learning-pipelines-be49ecc69a82?source=collection_archive---------2-----------------------#2023-03-09](https://towardsdatascience.com/apache-spark-mllib-vs-scikit-learn-building-machine-learning-pipelines-be49ecc69a82?source=collection_archive---------2-----------------------#2023-03-09)

## ML流水线的代码实现：从原始数据到预测

[](https://brunocaraffa.medium.com/?source=post_page-----be49ecc69a82--------------------------------)[![Bruno Caraffa](../Images/414f88c6974dba79ec9851c051eff49f.png)](https://brunocaraffa.medium.com/?source=post_page-----be49ecc69a82--------------------------------)[](https://towardsdatascience.com/?source=post_page-----be49ecc69a82--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----be49ecc69a82--------------------------------) [Bruno Caraffa](https://brunocaraffa.medium.com/?source=post_page-----be49ecc69a82--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F657289ec5532&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fapache-spark-mllib-vs-scikit-learn-building-machine-learning-pipelines-be49ecc69a82&user=Bruno+Caraffa&userId=657289ec5532&source=post_page-657289ec5532----be49ecc69a82---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----be49ecc69a82--------------------------------) · 8分钟阅读 · 2023年3月9日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fbe49ecc69a82&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fapache-spark-mllib-vs-scikit-learn-building-machine-learning-pipelines-be49ecc69a82&source=-----be49ecc69a82---------------------bookmark_footer-----------)![](../Images/f0e56fa05aee5a0b3ede725550076f2f.png)

图片由 [Rodion Kutsaiev](https://unsplash.com/@frostroomhead?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

真实的机器学习涉及一系列准备数据的任务，然后才是神奇的预测。填充缺失值、对分类特征进行独热编码、对数值特征进行标准化和缩放、特征提取和模型拟合只是机器学习项目中进行预测之前的一些阶段。在处理NLP应用时，过程更加深入，包括词干提取、词形还原、停用词移除、分词、向量化和词性标注（POS标注）等阶段。

完全可以使用像Pandas和NumPy或NLTK和SpaCy这样的库来执行这些步骤。问题在于，当模型投入生产时，团队成员之间的代码一致性可能会丧失，并且在没有操作特别设计的框架时，代码中的人为错误几率更高。这就是为什么ML团队倾向于使用称为**Pipelines**的高级API，这些API特别设计用于将所有阶段链接为一个对象。

本文将介绍如何使用两大主要库：Apache Spark的MLLib和Scikit-learn，实现ML Pipelines。由于智能的NumPy矢量化执行，Scikit-learn在仅使用CPU时表现出色，这些执行方式对CPU缓存行友好，并且高效利用了CPU的可用核心。尽管在大数据背景下，Apache Spark的MLLib由于其适合分布式计算，通常会优于scikit-learn，因为它设计用于在Spark上运行。

包含[10个欧洲城市的Airbnb房源数据集](https://zenodo.org/record/4446043#.ZAfeMh_MK3D)¹将被用于在scikit-learn和MLLib中创建相同的Pipeline。该Pipeline将在应用随机森林回归器生成房源价格预测之前，在预处理阶段操作数值和分类特征。这些特征及其各自的数据类型如下：

![](../Images/1f889bb45ceb3eee9de7136454ddfc6b.png)

图1 — 特征和数据类型。来源：作者。

首先，让我们加载数据集。由于价格在周末和工作日之间变化很大，因此有一个包含各城市工作日和周末房源数据的csv文件。因此，为了简化我们的工作，可以将所有数据集整合到一个数据框中，并创建“***city***”和“***weekday_or_weekend***”特征，这些特征对于模型来说肯定是必不可少的。

必须通过选择浮点数列并使用 pop 方法将第一个特征 realSum（即房源价格和我们的目标特征）排除，来将数值特征分组到一个列表中。multi 和 biz 特征采用 1/0 编码，scikit-learn 允许我们将它们作为布尔类型输入到模型中，因此我们将对它们进行转换。最后，我们通过选择布尔型和对象型数据列来创建分类特征列表。

然后我们可以导入 Scikit-learn 库中所有必要的类。有一个叫做 ColumnTransformer 的类，它会在数据框的列中应用变换器。数值变换器将是标准缩放器，而分类特征将使用传统的一热编码。在调用 Column Transformer 时，之前创建的列表将分配给每个变换器，可以将它们与随机森林回归器一起加入到管道中。就这样。当处理管道时，特征操作过程变得更加流畅和有序，如下面这段简单但操作密集的代码所示。

最后，我们可以将管道拟合到训练数据集，并预测测试数据集房源的价格，就像处理其他模型一样。管道的平均绝对误差为 67 欧元，考虑到我们没有应用任何异常值处理技术，只创建了两个新特征，并仅设置了随机森林回归器的最大深度和估算器数量参数，这还留有超参数调整的空间。

现在我们已经成功创建并应用了 scikit-learn 的管道，让我们用 Apache Spark 的 MLLib 库做同样的事情。为此，我使用了 [Databricks](https://www.databricks.com/)，这是一个由 Spark 的创始人创建的云数据平台，用于部署先进的机器学习项目。显然，它运行在 Apache Spark 上，这使其成为处理大数据环境的正确选择，因为 Spark 具有大规模分布式计算的属性。Databricks 提供了一个 [社区版](https://www.databricks.com/product/faq/community-edition)，托管在 AWS 上，免费提供给用户访问一个微型集群，并使用 Python 或 Scala 在 Spark 上编写代码。我强烈推荐它来提高 Spark 和 Databricks 技能，尽管免费微型集群只有 15 GB 的可用内存，这限制了现实生活中的大数据应用。

在 Databricks 上做的第一件事是创建一个集群，这是一组计算资源和配置，我们将在其上运行我们的管道。可以通过点击左上角菜单中的 create -> cluster 来完成。集群已经启动了 Spark，并安装了所有必需的机器学习库，甚至可以导入其他必要的库。唯一的限制是有限的内存、2 个 CPU 核心以及集群在两小时空闲后自动终止。

![](../Images/2c5756a5ad148f1708ee70ff930df3d6.png)

图 2— 启动 Databricks 集群。来源：作者。

Databricks 还具有一个表上传的用户界面（UI），可以通过在左上菜单中选择 create -> table 来访问。在数据加载 UI 中，可以轻松将 csv 文件加载到 Delta Lake 表中，并操作一些表的属性，如推断模式、选择数据类型以及将第一行提升为表头。因此，让我们加载我们在 scikit-learn 管道中创建的数据框的 csv 文件。

![](../Images/c22eadf3c79c46f86d0ca94edb0a9792.png)

图 3 — 使用 Databricks 数据加载 UI 加载 csv 文件。来源：作者。

现在集群已创建且数据已整理好，我们可以通过在用于集群和表设置的相同左上菜单中创建笔记本来开始使用。Spark 的 `read.csv` 函数还允许我们将第一行定义为表头并推断数据模式，我们只需使用表的目录来加载数据框并删除索引列。

现在是了解 MLLib 的时候了。为了重现第一部分中构建的管道，我们必须从 feature 类中加载 OneHotEncoder、StandardScaler、VectorAssembler 和 StringIndexer 函数，从回归类中加载 RandomForestRegressor，从同名类中加载 Pipeline。与 scikit-learn 不同，MLLib 不允许在操作中使用布尔列，因此我们必须先将其转换为整数。另一个不同之处是，对于类别列，我们必须在进行 one hot 编码之前添加字符串索引阶段，因为 MLLib 的 [OHE 函数](https://spark.apache.org/docs/3.1.1/api/python/reference/api/pyspark.ml.feature.OneHotEncoder.html) 仅使用类别索引作为输入，而不是字符串或布尔值。

在 MLLib 上构建管道时，一个好的实践是编写输入和输出列的名称，以跟踪特征，因为与 scikit-learn 的 Column Transformer 不同，变换不会发生在输入列本身，而是将新列添加到现有数据框中。

两个管道之间的最终差异是变换的顺序。在 scikit-learn 中，类别和数值变换在管道的预处理阶段并行进行，而在 MLLib 中，我们首先应用 one hot 编码器，然后使用 VectorAssembler 将所有特征合并到一个向量中，最后在随机森林回归之前应用 StandardScaler 函数。因此，与预处理阶段和回归阶段不同，所有的预处理步骤都按顺序添加到管道中，然后进行回归。

管道完成后，只需三行代码即可将数据拆分为训练集和测试集，将模型拟合到训练集上，并对测试集进行价格预测。在 MLlib 中，RegressionEvaluator 函数用于评估模型，并包含所有回归固有的评估指标（也有用于分类模型评估的函数）。与第一个管道一样，平均误差将被用作评估指标。

不出意料，平均误差与第一个管道几乎完全一致。

总结一下，我们展示了 ML 管道如何简化模型实现过程中的编码，使其更清晰、更快捷，通过在模型训练过程中改进代码的可读性，提供了逻辑流的变换。管道对于涉及大量数据和多位数据科学团队成员的 ML 项目至关重要。

在编码实现过程中，我们看到 Scikit-learn 在管道创建流程中相对于 Apache Spark 的 MLlib 有一些小优势。ColumnTransformer 函数能够在一行代码中转换分类和数值特征，这减少了与 MLlib 相比的 Pipeline 阶段。此外，有人可能会说，你无法在 MLlib 上复制一个复杂的 scikit-learn 管道，这可能是因为 scikit-learn 的函数相比 MLlib 更加深奥。

尽管如此，MLlib 通过使用 Spark 的分布式处理来处理大数据集，从而扩展项目的能力也非常宝贵。此外，选择哪个管道高度依赖于项目的上下文。例如，当在 Databricks 中工作时，可能通过平台内的 Delta Lake 访问数据，在这种情况下，使用 MLlib 运行管道可能是更好的选择。甚至可以将两个世界结合起来，仅将 MLlib 管道作为数据工程工具来准备数据，并考虑到大型数据集，这是明智的选择，然后使用 scikit-learn 进行建模、训练和评估。

希望你了解（或复习）机器学习项目中管道的重要性，以及 scikit-learn 和 MLlib 管道之间的差异。虽然构建它们可能需要一些时间，但从长远来看，良好的组织会带来多方面的回报。

[1] Gyódi, Kristóf, & Nawaro, Łukasz. (2021). 《欧洲城市 Airbnb 价格的决定因素：一种空间计量经济学方法》（补充材料）[数据集]。Zenodo. [https://doi.org/10.5281/zenodo.4446043](https://doi.org/10.5281/zenodo.4446043)。CC BY 4.0 许可证。
