# Human-Learn: 作为机器学习替代方案的基于规则的学习

> 原文：[`towardsdatascience.com/human-learn-rule-based-learning-as-an-alternative-to-machine-learning-baf1899ecb3a`](https://towardsdatascience.com/human-learn-rule-based-learning-as-an-alternative-to-machine-learning-baf1899ecb3a)

## 将领域知识融入你的模型中，使用基于规则的学习

[](https://khuyentran1476.medium.com/?source=post_page-----baf1899ecb3a--------------------------------)![Khuyen Tran](https://khuyentran1476.medium.com/?source=post_page-----baf1899ecb3a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----baf1899ecb3a--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----baf1899ecb3a--------------------------------) [Khuyen Tran](https://khuyentran1476.medium.com/?source=post_page-----baf1899ecb3a--------------------------------)

·发布在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----baf1899ecb3a--------------------------------) ·阅读时间 7 分钟·2023 年 1 月 1 日

--

# 动机

你会得到一个标记的数据集，并被要求预测一个新的数据集。你会怎么做？

你可能尝试的第一种方法是训练一个机器学习模型来寻找标记新数据的规则。

![](img/1044eec31dae2c0e7f68aa7188b489cc.png)

作者提供的图片

这很方便，但很难了解机器学习模型为何得出特定的预测。你也无法将你的领域知识融入模型中。

与其依赖机器学习模型来进行预测，不如根据你的知识设定数据标记的规则。

![](img/cfa0724b6d540aa558c2d888cdb6878e.png)

作者提供的图片

这时，human-learn 就派上用场了。

# 什么是 human-learn？

human-learn 是一个 Python 包，用于创建易于构建且与 scikit-learn 兼容的基于规则的系统

要安装 human-learn，请输入：

```py
pip install human-learn
```

在上一篇文章中，我谈到了如何通过绘制创建人类学习模型：

[](/human-learn-create-rules-by-drawing-on-the-dataset-bcbca229f00?source=post_page-----baf1899ecb3a--------------------------------) ## human-learn: 通过绘制创建人类学习模型

### 使用你的领域知识来标记数据

towardsdatascience.com

在本文中，我们将学习如何使用一个简单的函数创建模型。

随意访问并分叉本文的源代码：

[](https://github.com/khuyentran1401/Data-science/blob/master/machine-learning/human_learn_examples/rule_based_model.ipynb?source=post_page-----baf1899ecb3a--------------------------------) [## Data-science/rule_based_model.ipynb at master · khuyentran1401/Data-science

### 收集有用的数据科学主题，包括代码和文章——Data-science/rule_based_model.ipynb at master ·…

[github.com](https://github.com/khuyentran1401/Data-science/blob/master/machine-learning/human_learn_examples/rule_based_model.ipynb?source=post_page-----baf1899ecb3a--------------------------------)

要评估基于规则的模型的性能，我们先用机器学习模型预测一个数据集。

# 机器学习模型

我们将使用[*占用检测数据集*](https://archive.ics.uci.edu/ml/machine-learning-databases/00357/)作为本教程的示例。

我们的任务是根据温度、湿度、光线和 CO2 预测房间占用情况。如果`Occupancy=0`则房间未被占用，如果`Occupancy=1`则房间被占用。

下载数据集后，解压并读取数据：

```py
import pandas as pd
from sklearn.ensemble import RandomForestClassifier
from sklearn.metrics import classification_report

# Get train and test data
train = pd.read_csv("occupancy_data/datatraining.txt").drop(columns="date")
test = pd.read_csv("occupancy_data/datatest.txt").drop(columns="date")

# Get X and y
target = "Occupancy"
train_X, train_y = train.drop(columns=target), train[target]
val_X, val_y = test.drop(columns=target), test[target]
```

查看`train`数据集的前十条记录：

```py
train.head(10)
```

![](img/b9b0f6b71bad5cf8a64842ba8df73043.png)

图片作者

在训练数据集上训练 scikit-learn 的`RandomForestClassifier`模型，并用它来预测测试数据集：

```py
# Train
forest_model = RandomForestClassifier(random_state=1)

# Preduct
forest_model.fit(train_X, train_y)
machine_preds = forest_model.predict(val_X)

# Evalute
print(classification_report(val_y, machine_preds))
```

![](img/a8582d20ad9c1a25cc9a26b9777dbc58.png)

图片作者

分数相当不错。然而，我们不确定模型如何得出这些预测。

让我们看看能否用简单规则标记新数据。

# 基于规则的模型

创建标记数据规则的步骤有四个：

1.  生成假设

1.  观察数据以验证假设

1.  从基于观察的简单规则开始

1.  改进规则

## 生成假设

房间的光线是判断房间是否被占用的一个好指标。因此，我们可以假设房间越亮，它被占用的可能性就越大。

让我们通过查看数据来验证这一点。

## 观察数据

为了验证我们的猜测，让我们使用箱线图找出占用房间（`Occupancy=1`）和空房间（`Occupancy=0`）之间光线量的差异。

```py
import plotly.express as px
import plotly.graph_objects as go

feature = "Light"
px.box(data_frame=train, x=target, y=feature)
```

![](img/085bcdf2cab8d01a1c3eb0d56592f75f.png)

图片作者

我们可以看到占用房间和空房间之间的中位数存在显著差异。

## 从简单规则开始

现在，我们将根据房间内的光线创建是否占用的规则。具体来说，如果光线量超过某个阈值，则`Occupancy=1`，否则`Occupancy=0`。

![](img/d424677a5204dae2f8b17353596754d9.png)

图片作者

但那个阈值应该是多少呢？我们从选择`100`作为阈值开始，看看结果如何。

![](img/838ac0be44c442e05785c036f4f12ba0.png)

图片作者

要用 human-learn 创建基于规则的模型，我们将：

+   编写一个简单的 Python 函数来指定规则

+   使用`FunctionClassifier`将函数转化为 scikit-learn 模型

```py
import numpy as np
from hulearn.classification import FunctionClassifier

def create_rule(data: pd.DataFrame, col: str, threshold: float=100):
    return np.array(data[col] > threshold).astype(int)

mod = FunctionClassifier(create_rule, col='Light')
```

预测测试集并评估预测结果：

```py
mod.fit(train_X, train_y)
preds = mod.predict(val_X)
print(classification_report(val_y, preds))
```

![](img/017a7959f333721390155797542c738d.png)

图片作者

准确率比之前使用`RandomForestClassifier`时更好！

## 改进规则

让我们通过实验几个阈值来看看是否能得到更好的结果。我们将使用平行坐标图来分析特定光值与房间占用之间的关系。

```py
from hulearn.experimental.interactive import parallel_coordinates

parallel_coordinates(train, label=target, height=200)
```

![](img/836ada2674bd7b4913173fda8d64e37a.png)

图片来源：作者

从平行坐标图中，我们可以看到光线超过 250 Lux 的房间有很高的被占用的概率。将占用的房间与空房间分开的最佳阈值似乎在 250 Lux 和 750 Lux 之间。

让我们使用 scikit-learn 的`GridSearch`在这个范围内找到最佳阈值。

```py
from sklearn.model_selection import GridSearchCV

grid = GridSearchCV(mod, cv=2, param_grid={"threshold": np.linspace(250, 750, 1000)})
grid.fit(train_X, train_y)
```

找到最佳阈值：

```py
best_threshold = grid.best_params_["threshold"]
best_threshold
> 364.61461461461465
```

在箱线图上绘制阈值。

![](img/0cf82b8aa8b48292ba75a93d7c648abd.png)

图片来源：作者

使用最佳阈值的模型来预测测试集：

```py
human_preds = grid.predict(val_X)
print(classification_report(val_y, human_preds))
```

![](img/28090f529d202cd18a041aa9740f0d4e.png)

图片来源：作者

阈值`365`比阈值`100`给出更好的结果。

# 结合机器学习模型和规则基础模型

使用领域知识来创建规则的规则基础模型很好，但也有一些缺点：

+   它对未见数据的泛化能力差

+   为复杂数据制定规则是困难的

+   模型没有反馈循环来改进

因此，将规则基础模型与机器学习模型结合将帮助数据科学家扩展和改进模型，同时仍能结合他们的领域专长。

一种简单的方法来结合这两种模型是决定是减少假阴性还是假阳性。

## 减少假阴性

在预测患者是否有癌症的场景中，你可能会想要减少假阴性（与其错把健康的患者说成有癌症，不如错过发现癌症的机会）。

为了减少假阴性，当两个模型不一致时，选择**正标签**。

![](img/2165e7bcb6d2b4fb5d0a2437bdf98d85.png)

图片来源：作者

## 减少假阳性

在推荐可能对孩子有害的视频等场景中，你可能会想要减少假阳性（与其错过推荐适合孩子的视频，不如错推荐成人视频给孩子）。

为了减少假阳性，当两个模型不一致时，选择**负标签**。

![](img/3671651d544719f677c8781af5bbecae.png)

图片来源：作者

你也可以使用其他更复杂的策略层来决定选择哪个预测。

对于深入了解如何结合机器学习模型和规则基础模型，我推荐查看[Jeremy Jordan 的这段精彩视频](https://youtu.be/gXe9iXNTuDc)。

# 结论

恭喜！你刚刚学会了什么是规则基础模型以及如何将其与机器学习模型结合。希望这篇文章能为你开发自己的规则基础模型提供所需的知识。

我喜欢写关于数据科学概念的文章并玩各种数据科学工具。你可以通过[LinkedIn](https://www.linkedin.com/in/khuyen-tran-1401/)和[Twitter](https://twitter.com/KhuyenTran16)与我联系。

如果你想查看我写的文章中的代码，请给[这个仓库](https://github.com/khuyentran1401/Data-science)加星。关注我在 Medium 上的文章，获取我最新的数据科学文章：

## 使用基于用户的协同过滤预测电影评分

### Python 中协同过滤的综合介绍

towardsdatascience.com ## SHAP：在 Python 中解释任何机器学习模型

### SHAP 和 Shapley 值的综合指南

[towardsdatascience.com ## 使用 TextBlob 提升你的 Python 字符串

### 在一行代码中从你的文本中获得更多洞察！

[towardsdatascience.com ## 使用 dirty_cat 对脏类别进行相似度编码

[towardsdatascience.com

# 参考文献

数据引用：

> 准确检测办公室房间的占用情况，通过光线、温度、湿度和 CO2 测量，使用统计学习模型。Luis M. Candanedo，Véronique Feldheim。《能源与建筑》。第 112 卷，2016 年 1 月 15 日，第 28–39 页。
