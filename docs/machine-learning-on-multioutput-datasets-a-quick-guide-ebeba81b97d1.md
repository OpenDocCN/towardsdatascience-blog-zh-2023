# 多输出数据集上的机器学习：快速指南

> 原文：[https://towardsdatascience.com/machine-learning-on-multioutput-datasets-a-quick-guide-ebeba81b97d1?source=collection_archive---------2-----------------------#2023-03-10](https://towardsdatascience.com/machine-learning-on-multioutput-datasets-a-quick-guide-ebeba81b97d1?source=collection_archive---------2-----------------------#2023-03-10)

## 如何在多输出数据集上以最小编码工作量训练和验证机器学习模型

[](https://tvdboom.medium.com/?source=post_page-----ebeba81b97d1--------------------------------)[![Marco vd Boom](../Images/3fc053efda1c23dd84a6418ded2603ca.png)](https://tvdboom.medium.com/?source=post_page-----ebeba81b97d1--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ebeba81b97d1--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----ebeba81b97d1--------------------------------) [Marco vd Boom](https://tvdboom.medium.com/?source=post_page-----ebeba81b97d1--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe2091b627921&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmachine-learning-on-multioutput-datasets-a-quick-guide-ebeba81b97d1&user=Marco+vd+Boom&userId=e2091b627921&source=post_page-e2091b627921----ebeba81b97d1---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ebeba81b97d1--------------------------------) ·6 min read·2023年3月10日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Febeba81b97d1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmachine-learning-on-multioutput-datasets-a-quick-guide-ebeba81b97d1&user=Marco+vd+Boom&userId=e2091b627921&source=-----ebeba81b97d1---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Febeba81b97d1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmachine-learning-on-multioutput-datasets-a-quick-guide-ebeba81b97d1&source=-----ebeba81b97d1---------------------bookmark_footer-----------)![](../Images/c406c9a0eb9fcb1e0cbb132ad550c8e4.png)

图片由 [Victor Barrios](https://unsplash.com/@thevictorbarrios?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 介绍

大家熟悉的标准机器学习任务包括分类（二分类和多分类）和回归。在这些情况下，我们尝试预测一个目标列。在多输出情况下，有多个目标列，我们希望训练一个能够同时预测所有目标列的模型。我们识别出三种多输出任务：

+   多标签：多标签是一个分类任务，将每个样本标记为*m*个标签，标签来自*n_classes*个可能的类别，其中*m*的范围是0到*n_classes*。这可以被视为预测样本的非互斥属性。例如，对文本文件相关话题的预测。该文件可能涉及宗教、政治、金融或教育中的一个、多个或所有话题类别。

+   多类-多输出：多类-多输出（也称为多任务分类）是一个分类任务，其中每个样本都有一组非二元属性标签。属性的数量和每个属性的类别数量都大于2。这不仅是对多标签分类任务的泛化（多标签分类任务只考虑二元属性），也是对多类分类任务的泛化（多类分类任务只考虑一个属性）。例如，对一组水果图像的“水果类型”和“颜色”属性进行分类。属性“水果类型”可能的类别有：苹果、梨和橙子。属性“颜色”可能的类别有：绿色、红色、黄色和橙色。每个样本是一张水果的图像，对两个属性都会输出一个标签，每个标签都是对应属性的可能类别之一。

+   多输出回归：多输出回归预测每个样本的多个数值属性。每个属性是一个数值变量，每个样本需要预测的属性数量>=2。例如，使用在某个位置获得的数据预测风速和风向（以度为单位）。每个样本是从一个位置获得的数据，对每个样本会输出风速和风向。

在这个故事中，我们将解释[ATOM](https://github.com/tvdboom/ATOM)库如何帮助你加快多输出数据集的管道。从数据预处理到模型训练、验证和结果分析。ATOM是一个开源Python包，旨在帮助数据科学家探索机器学习管道。

**注意**：这个故事专注于使用ATOM处理多输出数据集。库的基础知识教学不在本故事范围之内。如果你想了解库的简单介绍，请阅读[这篇文章](/atom-a-python-package-for-fast-exploration-of-machine-learning-pipelines-653956a16e7b)。

## 数据预处理

在*atom*中初始化多输出数据集的过程与其他任务非常相似，但有一点需要注意：你**必须**使用关键字参数`y`指定目标列。

```py
atom = ATOMClassifier(X, y=y, verbose=2, random_state=1)
```

如果不提供`y=`，atom会认为第二个参数是测试集，就像你用`atom = ATOMClassifier(train, test)`进行初始化一样，这会导致列不匹配异常。

你还可以提供一系列列名或位置来指定 `X` 中的目标列。例如，要指定最后 3 列作为目标，请使用：

```py
atom = ATOMClassifier(X, y=(-3, -2, -1), verbose=2, random_state=1)
```

在所有情况下，打印 `self.y` 现在返回的是 `DataFrame` 类型的目标，而不是 `Series` 类型。

对于多标签任务，目标列可能看起来像这样。

```py
0                        [politics]
1               [religion, finance]
2    [politics, finance, education]
3                                []
4                         [finance]
5               [finance, religion]
6                         [finance]
7               [religion, finance]
8                       [education]
9     [finance, religion, politics]

Name: target, dtype: object
```

模型不能直接处理变量数量的目标类。使用

使用 [*clean*](https://tvdboom.github.io/ATOM/latest/API/ATOM/atomclassifier/#atomclassifier-clean) 方法为每个样本的每个类分配一个二进制输出。正类用 1 表示，负类用 0 表示。因此，它可与运行 *n_classes* 个二元分类任务相比较。

```py
atom.clean()
```

在我们的示例中，目标（`atom.y`）被转换为：

```py
 education  finance  politics  religion
0          0        0         1         0
1          0        1         0         1
2          1        1         1         0
3          0        0         0         0
4          0        1         0         0
5          0        1         0         1
6          0        1         0         0
7          0        1         0         1
8          1        0         0         0
9          0        1         1         1
```

## 模型训练和验证

一些模型对多输出任务有原生支持。这意味着

原始估计器用于直接对所有

目标列。

然而，大多数模型没有对多输出任务的集成支持。ATOM 仍然使使用它们成为可能，通过将估计器包装在一个能够处理多个目标列的元估计器中。这是自动完成的，无需额外的代码或用户的先验知识。

对于多标签任务，默认使用的元估计器是：

+   [ClassifierChain](https://scikit-learn.org/stable/modules/generated/sklearn.multioutput.ClassifierChain.html#sklearn.multioutput.ClassifierChain)

对于多类多输出和多输出回归任务，

默认的元估计器分别是：

+   [MultioutputClassifier](https://scikit-learn.org/stable/modules/generated/sklearn.multioutput.MultiOutputClassifier.html#sklearn.multioutput.MultiOutputClassifier)

+   [MultioutputRegressor](https://scikit-learn.org/stable/modules/generated/sklearn.multioutput.MultiOutputRegressor.html#sklearn.multioutput.MultiOutputRegressor)

`multioutput` 属性包含了元估计器对象。更改该属性：

属性的值使用自定义对象。无论是类还是实例，只要

原估计器是第一个参数的情况。比如，要更改回归模型的元估计器，使用：

```py
from sklearn.multioutput import RegressorChain

atom.multioutput = RegressorChain
```

要检查哪些模型对多输出数据集有原生支持，哪些没有，使用：

```py
atom.available_model()[["acronym", "model", "native_multioutput"]]
```

![](../Images/fea209a08b54666c72874ae0a8d8bc55.png)

现在，你可以正常训练模型。

```py
atom.run(models=["LDA", "RF"], metric="f1")
```

并检查估计器。

![](../Images/846e5e36b68b1deea8a8369dbe73c3ac.png)

一些模型，如 [MultiLayer Perceptron](https://tvdboom.github.io/ATOM/latest/API/models/mlp/)，对多标签任务有原生支持，但对多类多输出任务没有。因此，它们的 `native_multioutput` 标记为 False，但如果你有一个多标签任务，这些模型不一定需要一个多输出元估计器。在这种情况下，使用 *atom* 的 `multioutput` 属性告诉 atom 不使用任何多输出包装器。

```py
atom.multioutput = None

# MLP won't use a meta-estimator wrapper now
atom.run(models=["MLP"])
```

![](../Images/bd1b477e3db5d9cd05bf7da82849e8e3.png)

**注意：** sklearn度量不支持多类-多输出分类任务。ATOM计算这种任务的度量方法是对每个目标列的得分取平均值。

## 结果分析

具有多输出估算器的模型可以正常调用分析方法和绘图。在绘图中使用`target`参数以指定使用哪个目标列。

```py
atom.plot_roc(target=2)
```

当`target`参数还指定了类别时，使用格式（列，类别）。

```py
atom.plot_probabilities(models="MLP", target=(2, 1))
```

```py
with atom.canvas(figsize=(900, 600)):
    atom.plot_calibration(target=0)
    atom.plot_calibration(target=1)
```

## 结论

我们已经展示了如何轻松使用ATOM包，以便快速探索多输出数据集上的机器学习管道。点击[这里](https://tvdboom.github.io/ATOM/latest/examples/multioutput_regression/)查看多输出回归任务的完整示例，点击[这里](https://tvdboom.github.io/ATOM/latest/examples/multilabel_classification/)查看多标签分类示例。

关于ATOM的更多信息，请查看该包的[文档](https://tvdboom.github.io/ATOM/)。如有错误或功能请求，请随时在[GitHub](https://github.com/tvdboom/ATOM)上提交问题或发送电子邮件给我。

相关故事：

+   [https://towardsdatascience.com/atom-a-python-package-for-fast-exploration-of-machine-learning-pipelines-653956a16e7b](/atom-a-python-package-for-fast-exploration-of-machine-learning-pipelines-653956a16e7b)

+   [https://towardsdatascience.com/how-to-test-multiple-machine-learning-pipelines-with-just-a-few-lines-of-python-1a16cb4686d](/how-to-test-multiple-machine-learning-pipelines-with-just-a-few-lines-of-python-1a16cb4686d)

+   [https://towardsdatascience.com/from-raw-data-to-web-app-deployment-with-atom-and-streamlit-d8df381aa19f](/from-raw-data-to-web-app-deployment-with-atom-and-streamlit-d8df381aa19f)

+   [https://towardsdatascience.com/exploration-of-deep-learning-pipelines-made-easy-e1cf649892bc](/exploration-of-deep-learning-pipelines-made-easy-e1cf649892bc)

+   [https://towardsdatascience.com/deep-feature-synthesis-vs-genetic-feature-generation-6ba4d05a6ca5](/deep-feature-synthesis-vs-genetic-feature-generation-6ba4d05a6ca5)

+   [https://towardsdatascience.com/from-raw-text-to-model-prediction-in-under-30-lines-of-python-32133d853407](/from-raw-text-to-model-prediction-in-under-30-lines-of-python-32133d853407)

+   [https://towardsdatascience.com/how-to-make-40-interactive-plots-to-analyze-your-machine-learning-pipeline-ee718afd7bc2](/how-to-make-40-interactive-plots-to-analyze-your-machine-learning-pipeline-ee718afd7bc2)

参考文献：

+   本故事中的所有绘图均由作者创建。
