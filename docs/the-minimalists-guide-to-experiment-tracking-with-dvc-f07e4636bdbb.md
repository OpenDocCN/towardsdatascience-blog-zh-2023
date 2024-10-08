# 《极简主义者的 DVC 实验跟踪指南》

> 原文：[`towardsdatascience.com/the-minimalists-guide-to-experiment-tracking-with-dvc-f07e4636bdbb`](https://towardsdatascience.com/the-minimalists-guide-to-experiment-tracking-with-dvc-f07e4636bdbb)

![](img/40caa436b9fce122c4b24ed80894039f.png)

图片由 Zarak Khan 提供，来源于 [pexels.com](https://www.pexels.com/photo/a-minimalist-workspace-4256211/)

## 开始进行实验跟踪的最低要求指南

[](https://eryk-lewinson.medium.com/?source=post_page-----f07e4636bdbb--------------------------------)![Eryk Lewinson](https://eryk-lewinson.medium.com/?source=post_page-----f07e4636bdbb--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f07e4636bdbb--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----f07e4636bdbb--------------------------------) [Eryk Lewinson](https://eryk-lewinson.medium.com/?source=post_page-----f07e4636bdbb--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f07e4636bdbb--------------------------------) ·阅读时间 6 分钟·2023 年 5 月 15 日

--

本文是一个系列文章的第三部分，展示了如何利用 DVC 及其 VS Code 插件进行机器学习实验。在 第一部分 中，我展示了一个机器学习项目的完整设置，并演示了如何在 VS Code 中跟踪和评估实验。在 第二部分 中，我展示了如何使用不同类型的图，包括实时图，用于实验评估。

阅读完这些文章后，你可能有兴趣在下一个项目中使用 DVC。然而，你可能会认为设置它需要大量工作，例如定义管道和数据版本控制。也许对于你下一个快速实验来说，这可能有些过于复杂，你决定不尝试。那将是很可惜的！

虽然这些步骤有充分的理由存在——你的项目将完全被跟踪并可重现——我理解有时我们会面临很大的压力，需要快速进行实验和迭代。因此，在本文中，我将向你展示开始使用 DVC 跟踪实验所需的最低要求。

# 从零开始，用几行代码进行实验跟踪

在我们开始编写代码之前，我想提供一些关于我们将使用的示例的背景信息。目标是构建一个模型，以识别欺诈信用卡交易。这个数据集（可以在 [Kaggle](https://www.kaggle.com/datasets/mlg-ulb/creditcardfraud) 上找到）被认为高度不平衡，只有 0.17% 的观察值属于正类。

正如我在引言中承诺的那样，我们将涵盖一个基本的场景，在这个场景中，你几乎可以立即开始跟踪你的实验。除了某些标准库，我们将使用`dvc`和`dvclive`库，以及[DVC VS Code 扩展](https://marketplace.visualstudio.com/items?itemName=Iterative.dvc)。最后一个不是硬性要求。我们可以从命令行检查跟踪的实验。然而，我更喜欢使用集成到 IDE 中的特殊标签。

让我们从创建一个基本的脚本开始。在这个简短的脚本中，我们加载数据，将其分为训练集和测试集，拟合模型，并评估其在测试集上的表现。你可以在下面的代码片段中看到整个脚本。

```py
import pandas as pd
from sklearn.model_selection import train_test_split
from sklearn.ensemble import RandomForestClassifier
from sklearn.metrics import f1_score, precision_score, recall_score

# set the params
train_params = {
    "n_estimators": 10,
    "max_depth": 10,
}

# load data
df = pd.read_csv("data/creditcard.csv")
X = df.drop(columns=["Time"]).copy()
y = X.pop("Class")

# train-test split
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42, stratify=y
)

# fit-predict
model = RandomForestClassifier(random_state=42, **train_params)
model.fit(X_train, y_train)
y_pred = model.predict(X_test)

# evaluate
print("recall", recall_score(y_test, y_pred))
print("precision", precision_score(y_test, y_pred))
print("f1_score", f1_score(y_test, y_pred))
```

运行脚本将返回以下输出：

```py
recall 0.7755102040816326
precision 0.926829268292683
f1_score 0.8444444444444446
```

我不认为我需要说服你将这些数字写在纸上或电子表格中并不是跟踪实验的最佳方式。这尤其如此，因为我们不仅需要跟踪输出，还需要知道哪些代码和潜在的超参数导致了这个分数。如果不知道这些，我们永远无法重现实验结果。

说到这里，让我们使用 DVC 来实现实验跟踪。首先，我们需要初始化 DVC。我们可以通过在终端中（在项目的根目录下）运行以下代码来完成。

```py
dvc init
git add -A
git commit -m "initialize DVC"
```

然后，我们需要稍微修改一下我们的代码，使用`dvclive`。

```py
import pandas as pd
from sklearn.model_selection import train_test_split
from sklearn.ensemble import RandomForestClassifier
from sklearn.metrics import f1_score, precision_score, recall_score
from dvclive import Live

# set the params
train_params = {
    "n_estimators": 10,
    "max_depth": 10,
}

# load data
df = pd.read_csv("data/creditcard.csv")
X = df.drop(columns=["Time"]).copy()
y = X.pop("Class")

# train-test split
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42, stratify=y
)

# fit-predict
model = RandomForestClassifier(random_state=42, **train_params)
model.fit(X_train, y_train)
y_pred = model.predict(X_test)

# evaluate
with Live(save_dvc_exp=True) as live:
    for param_name, param_value in train_params.items():
        live.log_param(param_name, param_value)
    live.log_metric("recall", recall_score(y_test, y_pred))
    live.log_metric("precision", precision_score(y_test, y_pred))
    live.log_metric("f1_score", f1_score(y_test, y_pred))
```

唯一改变的部分是评估。使用`Live`上下文，我们正在记录模型的参数（存储在`train_params`字典中）以及我们之前打印的相同分数。我们还可以跟踪[其他内容](https://dvc.org/doc/dvclive/how-it-works)，例如，图表或图像。为了帮助你更快上手，你可以在`dvclive`的[文档](https://dvc.org/doc/dvclive/live)中或在 DVC 扩展的设置屏幕上找到很多有用的代码片段。

在查看结果之前，值得一提的是`dvclive`期望每次运行都由 Git 跟踪。这意味着它将把每次运行保存到相同的路径，并每次覆盖结果。我们指定了`save_dvc_exp=True`以便自动跟踪为 DVC 实验。在后台，DVC 实验是 DVC 可以识别的 Git 提交，但同时，它们不会使我们的 Git 历史记录杂乱无章或创建额外的分支。

运行修改后的脚本后，我们可以在 DVC 扩展的*Experiments*面板中检查结果。正如我们所见，分数与我们手动打印到控制台中的分数相匹配。

![](img/8f7bc8a228daa2080b3b36615f44306d.png)

为了清楚地看到设置跟踪的好处，我们可以快速运行另一个实验。例如，假设我们认为应该将`max_depth`超参数减少到 5。为此，我们只需在`train_params`字典中更改超参数的值，并再次运行脚本。然后，我们可以立即在汇总表中看到新实验的结果。此外，我们可以查看哪种超参数组合导致了该得分。

![](img/18a3fad85e5ff978b53e3204205d021f.png)

简单明了！自然，我们展示的简化示例可以轻松扩展。例如，我们可以：

+   跟踪图表并比较实验结果，例如它们的 ROC 曲线。

+   添加 DVC 管道以确保我们项目每一步的可重复性（加载数据、处理、拆分等）。

+   使用 `params.yaml` 文件对我们管道中的所有步骤进行参数化，包括 ML 模型的训练。

+   使用 DVC 回调。在我们的例子中，我们手动存储了模型的超参数及其性能信息。对于像 XGBoost、LightGBM 或 Keras 这样的框架，我们可以使用回调自动存储所有这些信息。

# 总结

在这篇文章中，我们探讨了使用 DVC 的最简单实验跟踪设置。我知道开始一个项目时考虑数据版本控制、可重复管道等可能会让人感到望而生畏。然而，运用本文描述的方法，我们可以以最小的开销开始跟踪实验。虽然对于较大的项目，我仍然强烈建议使用所有确保可重复性的工具，但对于较小的临时实验，这种方法无疑更具吸引力。

一如既往，任何建设性的反馈都非常欢迎。你可以通过 [Twitter](https://twitter.com/erykml1) 或评论区联系我。你可以在 [这个仓库](https://github.com/erykml/dvc_minimalist) 中找到本文使用的所有代码。

*喜欢这篇文章？成为 Medium 会员，继续无限制地阅读学习。如果你使用* [*这个链接*](https://eryk-lewinson.medium.com/membership) *成为会员，你将以零额外成本支持我。提前感谢，期待与你见面！*

你可能也会对以下内容感兴趣：

[](/11-useful-pandas-functionalities-you-might-have-overlooked-ad080527c768?source=post_page-----f07e4636bdbb--------------------------------) ## 11 个你可能忽略的有用 Pandas 功能

### 本系列探索 Pandas 隐藏宝石的第三部分

towardsdatascience.com [](https://medium.com/geekculture/top-10-vs-code-extensions-for-data-science-ce3e24e24347?source=post_page-----f07e4636bdbb--------------------------------) [## 数据科学的十大 VS Code 扩展

### 提升生产力的必备工具！

[medium.com](https://medium.com/geekculture/top-10-vs-code-extensions-for-data-science-ce3e24e24347?source=post_page-----f07e4636bdbb--------------------------------) [](/turn-vs-code-into-a-one-stop-shop-for-ml-experiments-49c97c47db27?source=post_page-----f07e4636bdbb--------------------------------) ## 将 VS Code 转变为 ML 实验的一站式平台

### 如何在不离开 IDE 的情况下运行和评估实验

[towardsdatascience.com

除非另有说明，否则所有图片均为作者提供。
