# Scikit-Learn 中的管道：一种打包转换的绝妙方法

> 原文：[`towardsdatascience.com/pipelines-in-scikit-learn-an-amazing-way-to-bundle-transformations-9ef0594000ac`](https://towardsdatascience.com/pipelines-in-scikit-learn-an-amazing-way-to-bundle-transformations-9ef0594000ac)

## 管道如何帮助您编写更好的机器学习和数据科学代码 😍

[](https://medium.com/@ebbeberge?source=post_page-----9ef0594000ac--------------------------------)![Eirik Berge, PhD](https://medium.com/@ebbeberge?source=post_page-----9ef0594000ac--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9ef0594000ac--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----9ef0594000ac--------------------------------) [Eirik Berge, PhD](https://medium.com/@ebbeberge?source=post_page-----9ef0594000ac--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----9ef0594000ac--------------------------------) ·5 分钟阅读·2023 年 4 月 5 日

--

![](img/af74d4aa798e6d0abd86bb7284cd5dc3.png)

图片来源：[Rodion Kutsaiev](https://unsplash.com/fr/@frostroomhead?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 您的旅程概述

1.  介绍

1.  没有管道的问题？

1.  管道来救援！

1.  有用的属性和实用函数

1.  总结

# 1 — 介绍

处理机器学习任务的最受欢迎的 Python 库之一是 [scikit-learn](https://scikit-learn.org/stable/index.html)。它于 2010 年发布，自那时起，它对于实现流行的监督学习算法，如 [逻辑回归](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LogisticRegression.html#sklearn.linear_model.LogisticRegression)、 [随机森林](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestClassifier.html#sklearn.ensemble.RandomForestClassifier) 和 [支持向量机](https://scikit-learn.org/stable/modules/generated/sklearn.svm.LinearSVR.html#sklearn.svm.LinearSVR) 已经成为必不可少的工具。

在使用 scikit-learn 编写代码时，您可以使用一个叫做 **管道** 的功能。这个功能允许您将机器学习过程中的几个步骤打包成一个单一的组件。管道的使用是判断 scikit-learn 代码是否易于操作的*最关键因素之一*。许多人在创建 scikit-learn 机器学习模型时忽视了管道，这真的让人沮丧 😞

在这篇博客文章中，您将学习到 scikit-learn 管道的优点。阅读完毕后，您应该能够自信地将管道应用到自己的机器学习项目中。让我们开始吧 👍

# 2 — 没有管道的问题？

管道的目标是将机器学习项目中的多个步骤封装成一个**可管理的整体**。为了说明这一点，我们从以下设置代码开始：

```py
from sklearn.datasets import make_classification
from sklearn.preprocessing import MinMaxScaler
from sklearn.ensemble import RandomForestClassifier
from sklearn.model_selection import train_test_split

# Create data
X, y = make_classification(random_state=42)

# Split the data into training and testing
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.1)
```

如上代码块中的导入所示，我们将对数据进行缩放，然后使用**随机森林模型**进行分类。如果没有使用管道，这可能看起来是这样的：

```py
# Scale the data
scaler = MinMaxScaler(feature_range=(0, 1))
scaler.fit(X_train)
X_train_scaled = scaler.transform(X_train)
X_test_scaled = scaler.transform(X_test)

# Train the random forest
random_forest = RandomForestClassifier(n_estimators=10)
random_forest.fit(X_train_scaled, y_train)

# Predict with the random forest
random_forest.predict(X_test_scaled)
```

上面的代码存在一些严重和轻微的问题！让我们提及其中的一些：

+   **数据泄漏：** 如果仔细查看上面的代码，你会发现它实际上将训练数据的信息泄露给了测试数据。具体来说，当`scaler`转换`X_test`时，它使用了`X_train`的最小值和最大值。因此，训练集的信息在测试集中被揭示。这可能使随机森林在测试集上的准确性有些乐观！

+   **中间变量名称：** 我们创建了中间变量名称`X_train_scaled`和`X_test_scaled`。这仅仅是因为我们将缩放和训练作为完全独立的过程。

+   **较少的显式函数调用：** 在上面的代码中，几行是中间的`.fit()`和`.transform()`函数调用。这使得代码变得混乱且不易读。

+   **超参数搜索：** 我们希望对`MinMaxScaler`中的`feature_range`参数和`RandomForestClassifier`中的`n_estimators`参数进行超参数搜索。我们需要完全分开进行这些搜索！这不仅麻烦，而且我们会分别贪婪地优化每个参数，而不是寻找一个组合的最佳解。这可能使我们错过最佳解决方案。

# 3 — 管道来拯救！

![](img/bef3e9da02e6c480b931e6a8f1b2ceb6.png)

照片由[Erlend Ekseth](https://unsplash.com/@er1end?utm_source=medium&utm_medium=referral)提供，来自[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

不足为奇的是，管道将拯救这个问题。将上述代码与以下代码片段进行比较：

```py
from sklearn.pipeline import Pipeline

# Create a pipeline that combines the scaling and training
pipe = Pipeline([
  ('scaler', MinMaxScaler(feature_range=(0, 1))), 
  ('forest', RandomForestClassifier(n_estimators=10))
])

# Fit the pipeline to the training data
pipe.fit(X_train, y_train)

# Predict with the random forest
pipe.score(X_test, y_test) 
```

使用管道的代码更简洁、更高效，并避免了上述所有问题：

+   **数据泄漏：** 管道自动确保没有数据泄漏发生！

+   **中间变量名称：** 你不再需要像`X_train_scaled`和`X_test_scaled`这样的中间变量名称了！

+   **较少的显式函数调用：** 使用管道，你只需要一个`.fit()`函数调用来执行整个序列。这使得代码更易读！

+   **超参数搜索：** 一旦使用了管道，你可以对所有组件同时进行超参数搜索。方法`.get_params()`对获取管道中所有变换器/估计器的参数名称非常有用。这个在博客文章将管道集成到 Scikit-Learn 的超参数搜索中中有很好的解释！

# 4 — 有用的属性和实用功能

在 scikit-learn 中拟合管道后，有一些属性可以让你的工作变得更轻松。我曾经忽视这些属性，付出了代价😅

第一个是属性`named_steps`：

```py
# Gives us the components of the pipeline
print(pipe.named_steps)

# Output:
{'scaler': MinMaxScaler(), 'forest': RandomForestClassifier(n_estimators=10)}

# Can now access each of them
print(pipe.named_steps["forest"])

# Output:
RandomForestClassifier(n_estimators=10)
```

每当你有一个复合对象（如 scikit-learn 中的管道）时，知道如何访问各个组件是很有用的。

另一个有用的属性是`.n_features_in_`。这将显示传递到管道第一个隐式`.fit()`方法中的特征数量（在我们的例子中，是`MinMaxScaler`的`.fit()`方法）：

```py
# Gives us the number of features passed into the pipeline
print(pipe.n_features_in_)

# Output:
20
```

最后，你还可以使用实用函数`make_pipeline()`来创建 scikit-learn 中的管道。不同之处在于，`make_pipeline` 会自动为不同的变换器/估计器命名：

```py
from sklearn.pipeline import make_pipeline

# Automatically assign names to the components
pipe = make_pipeline(MinMaxScaler(), RandomForestClassifier(n_estimators=10))
print(pipe)

# Output:
Pipeline(
  steps=[
    ('minmaxscaler', MinMaxScaler()),
    ('randomforestclassifier',RandomForestClassifier(n_estimators=10))
  ]
)
```

我个人更喜欢使用实用函数`make_pipeline`，这样我就不需要自己想出名称。如果有许多开发人员在处理不同的管道，那么`make_pipeline` **确保一致性**。

如果你想了解更多关于管道的嵌套参数和缓存的信息，请查看 [管道用户指南](https://scikit-learn.org/stable/modules/compose.html)。

# 5— 总结

![](img/4a06c775ef80f575c5d7cdfe36d7a151.png)

图片由 [Spencer Bergen](https://unsplash.com/@spencerbergen?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

希望你现在理解了在编写 Scikit-Learn 机器学习代码时如何以及为什么要使用管道。如果你对数据科学、编程或介于两者之间的任何事物感兴趣，可以在 [LinkedIn](https://www.linkedin.com/in/eirik-berge/) 上加我并打个招呼 ✋

**喜欢我的写作吗？** 查看我其他的一些文章获取更多 Python 内容：

+   用优美的类型提示现代化你罪恶的 Python 代码

+   在 Python 中可视化缺失值极其简单

+   在 Python 中使用 PyOD 介绍异常/离群值检测 🔥

+   5 个在紧急情况下拯救你的了不起的 NumPy 函数

+   5 个提升你 Python 词典技能的专家建议 🚀
