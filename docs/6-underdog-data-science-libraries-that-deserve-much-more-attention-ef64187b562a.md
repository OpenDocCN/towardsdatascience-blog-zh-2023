# 6 个值得更多关注的数据科学库

> 原文：[`towardsdatascience.com/6-underdog-data-science-libraries-that-deserve-much-more-attention-ef64187b562a`](https://towardsdatascience.com/6-underdog-data-science-libraries-that-deserve-much-more-attention-ef64187b562a)

## 该是走出阴影的时候了

[](https://ibexorigin.medium.com/?source=post_page-----ef64187b562a--------------------------------)![Bex T.](https://ibexorigin.medium.com/?source=post_page-----ef64187b562a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ef64187b562a--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----ef64187b562a--------------------------------) [Bex T.](https://ibexorigin.medium.com/?source=post_page-----ef64187b562a--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ef64187b562a--------------------------------) ·阅读时间 9 分钟·2023 年 4 月 13 日

--

![](img/d6f92a6487bca58fd3fd8f54669eef08.png)

图片由我通过 Midjourney 制作。

当像 Pandas、Scikit-learn、NumPy、Matplotlib、TensorFlow 等大牌库占据你所有注意力时，很容易忽视一些接地气且令人惊叹的库。

他们可能不是 GitHub 的明星开发者，也没有在昂贵的 Coursera 专业课程中教授，但成千上万的开源开发者却将他们的心血和汗水倾注于编写这些库。它们在默默无闻中填补了流行库留下的空白。

本文的目的是让大家关注这些库，并一起惊叹开源社区的强大。

让我们开始吧！

## 0\. Manim

![](img/123157ca03fc7bd4ba55e2bfa1898e1a.png)

图片来源于 [Manim GitHub 页面](https://github.com/3b1b/manim)。MIT 许可证。

我们都对 [3Blue1Brown](https://www.youtube.com/c/3blue1brown) 视频的美丽感到惊叹。但我们大多数人不知道，所有的动画都是由 Grant Sanderson 本人编写的 Mathematical Animation Engine（Manim）库创建的。（我们对 Grant Sanderson 的贡献常常*视为理所当然*。）

每个 3b1b 视频都由 Manim 中数千行代码提供支持。例如，传奇系列“Calculus 的本质”花费了 Grant Sanderson 超过 22,000 行代码。

在 Manim 中，每个动画由一个场景类表示，如下所示（如果你不理解也不用担心）：

```py
import numpy as np
from manim import *

class FunctionExample(Scene):
    def construct(self):
        axes = Axes(...)
        axes_labels=axes.get_axis_labels()

        # Get the graph of a simple functions
        graph = axes.get_graph(lambda x: np.sin(1/x), color=RED)
        # Set up its label
        graph_label = axes.get_graph_label(
            graph, x_val=1, direction=2 * UP + RIGHT,
           label=r'f(x) = \sin(\frac{1}{x})', color=DARK_BLUE
        )

        # Graph the axes components together
        axes_group = VGroup(axes, axes_labels)

        # Animate
        self.play(Create(axes_group), run_time=2)
        self.wait(0.25)
        self.play(Create(graph), run_time=3)
        self.play(Write(graph_label), run_time=2)
```

这会生成 `sin(1/x)` 函数的以下动画：

![](img/458f18a74810de176aca592163642977.png)

GIF 由作者使用 Manim 制作。

不幸的是，Manim 的维护和文档并不好，理解也可以理解，因为 Grant Sanderson 大部分精力都投入到制作精彩的视频中。

不过，Manim Community 社区对该库进行了分叉，提供了更好的支持、文档和学习资源。

如果你已经非常兴奋（你这个数学爱好者！），这里是我对 Manim API 的温和而全面的介绍：

[](/how-to-create-slick-math-animations-like-3blue1brown-in-python-457f74701f68?source=post_page-----ef64187b562a--------------------------------) ## 如何在 Python 中创建像 3Blue1Brown 一样的光滑数学动画

### 编辑描述

towardsdatascience.com

统计数据和链接：

+   [GitHub: 3b1b Manim (50.6k ⭐)](https://github.com/3b1b/manim)

+   [GitHub: Manim 社区版 (13.8k ⭐)](https://github.com/ManimCommunity/manim)

+   [上个月 Manim 社区的下载量：14k](https://pypistats.org/packages/manim)

由于其陡峭的学习曲线和复杂的安装过程，Manim 每月下载量很少。它值得更多的关注。

## 1\. PyTorch Lightning

![](img/c40fcf701b53afb489eeef86ecb1ad58.png)

[PyTorch Lightning GitHub 页面。](https://github.com/Lightning-AI/lightning) 的截图 Apache-2.0 许可证。

当我在 TensorFlow 之后开始学习 PyTorch 时，我变得非常不高兴。很明显 PyTorch 很强大，但我忍不住说“TensorFlow 做得更好”，或者“在 TF 中会更简洁”，甚至更糟的是，“我几乎希望我从未学习过 PyTorch”。

这因为 PyTorch 是一个低级库。是的，这意味着 PyTorch 让你对模型训练过程有完全的控制，但这也需要大量的样板代码。如果我没记错的话，它就像 TensorFlow 但年轻了五年。

结果发现，有很多人都有这样的感觉。更具体地说，几乎 830 名 Lightning AI 的贡献者开发了 PyTorch Lightning。

![](img/69bc5157748dbf4ce418333512aed4f9.png)

GIF 来源于 [PyTorch Lightning GitHub 页面。](https://github.com/Lightning-AI/lightning) Apache-2.0 许可证。

PyTorch Lightning 是一个围绕 PyTorch 构建的高级封装库，它抽象了大部分样板代码，并且*缓解*了所有的痛点：

+   硬件无关的模型

+   代码高度可读，因为工程代码由 Lightning 模块处理

+   灵活性保持不变（所有 Lightning 模块仍然是 PyTorch 模块）

+   多 GPU、多节点、TPU 支持

+   16 位精度

+   实验跟踪

+   早期停止和模型检查点（终于来了！）

以及其他接近 [40 个高级功能](https://lightning.ai/docs/pytorch/stable/common/trainer.html#trainer-flags)，所有这些功能旨在让 AI 研究人员感到愉悦，而不是让他们恼火。

统计数据和链接：

+   [GitHub: PyTorch Lightning (22.3k ⭐)](https://github.com/Lightning-AI/lightning)

+   [上个月下载量：4.5M](https://pypistats.org/packages/lightning)

从官方教程中学习：

[](https://www.pytorchlightning.ai/tutorials?source=post_page-----ef64187b562a--------------------------------) [## PyTorch Lightning 教程

### Lightning 速度的视频，从零基础到 Lightning 英雄。

www.pytorchlightning.ai](https://www.pytorchlightning.ai/tutorials?source=post_page-----ef64187b562a--------------------------------)

## 2\. Optuna

是的，使用 GridSearch 进行超参数调优很简单、舒适，只需一个导入语句。但你必须承认，它比醉酒的蜗牛还慢，效率极低。

![](img/be37ca61bfd673c1b461272ca3d06247.png)

图片由我通过 Midjourney 制作。

暂时把超参数调优想象成购物。使用 GridSearch 意味着要走遍超市的每一个过道并检查每一件商品。这是一种系统性和有序的方法，但你浪费了很多时间。

另一方面，如果你有一个具有贝叶斯根源的智能个人购物助手，你将准确知道你需要什么以及去哪儿。这是一种更高效、更有针对性的方法。

如果你喜欢那个助手，它的名字是 Optuna。它是一个贝叶斯超参数优化框架，用于高效地搜索给定的超参数空间，并找到能提供最佳模型性能的黄金超参数组合。

这里是它的一些最佳功能：

+   与框架无关：调整你能想到的任何机器学习模型

+   Pythonic API 来定义搜索空间：Optuna 让你从给定范围内线性、随机或对数地采样超参数值，而不是手动列出可能的值

+   可视化：支持超参数重要性（平行坐标）图、历史图和切片图

+   控制迭代次数或持续时间：设置精确的迭代次数或调优过程持续的最长时间

+   暂停并恢复搜索

+   剪枝：在试验开始之前停止那些不太可能的试验

所有这些功能都旨在节省时间和资源。如果你想看到它们的实际应用，可以查看我的 Optuna 教程（这是我在 150 篇文章中表现最好的之一）：

[](/why-is-everyone-at-kaggle-obsessed-with-optuna-for-hyperparameter-tuning-7608fdca337c?source=post_page-----ef64187b562a--------------------------------) ## 为什么 Kaggle 上的每个人都对 Optuna 用于超参数调优情有独钟？

### 编辑描述

towardsdatascience.com

统计信息和链接：

+   [GitHub: Optuna (7.9k⭐)](https://github.com/optuna/optuna)

+   [上个月下载量：1.6M](https://pypistats.org/packages/optuna)

## 3\. PyCaret

![](img/f3a9ce394e5b33ccd2d38b02b02e3500.png)

[PyCaret GitHub 页面](https://github.com/pycaret/pycaret) 的截图。MIT 许可证。

我对 Moez Ali 充满敬意，他从零开始独自创建了这个库。目前，PyCaret 是现有的最佳低代码机器学习库。

如果 PyCaret 在电视上做广告，它的广告词会是这样的：

“你是否厌倦了在机器学习工作流中花费几个小时编写几乎相同的代码？那么，PyCaret 就是答案！

我们的一体化机器学习库帮助你用尽可能少的代码行构建和部署机器学习模型。可以把它看作是包含你所有喜欢的机器学习库代码的鸡尾酒，如 Scikit-learn、XGBoost、CatBoost、LightGBM、Optuna 等。

然后，广告将展示这段代码，并伴有戏剧性的声音效果来显示每一行：

```py
# Classification OOP API Example

# loading sample dataset
from pycaret.datasets import get_data
data = get_data('juice')

# init setup
from pycaret.classification import ClassificationExperiment
s = ClassificationExperiment()
s.setup(data, target = 'Purchase', session_id = 123)

# model training and selection
best = s.compare_models()

# evaluate trained model
s.evaluate_model(best)

# predict on hold-out/test set
pred_holdout = s.predict_model(best)

# predict on new data
new_data = data.copy().drop('Purchase', axis = 1)
predictions = s.predict_model(best, data = new_data)

# save model
s.save_model(best, 'best_pipeline')
```

讲述者会在代码显示时进行旁白：

“只需几行代码，你就可以从不同框架中的几十个模型中训练和选择最佳模型，评估它们并保存以供部署。使用起来非常简单，任何人都可以做到！

赶紧从 GitHub 或通过 PIP 获取我们的软件，之后再感谢我们！

统计数据和链接：

+   [GitHub: PyCaret (7.2k ⭐)](https://github.com/pycaret/pycaret)

+   [上个月下载量：278k](https://pypistats.org/packages/pycaret)

+   [PyCaret 教程](https://pycaret.org/tutorial/)

## 4\. BentoML

Web 开发人员像宠物一样喜欢 FastAPI。它是最受欢迎的 GitHub 项目之一，确实使 API 开发变得极其简单和直观。

因为这种受欢迎程度，它也进入了机器学习领域。工程师们常常使用 FastAPI 将他们的模型部署为 API，以为整个过程不会更好或更简单。

但大多数人都抱有幻想。仅仅因为 FastAPI 比它的前身（Flask）好得多，并不意味着它是最适合这个工作的工具。

那么，*最适合*这个工作的工具是什么？我很高兴你问了——就是 BentoML！

BentoML 尽管相对年轻，但它是一个端到端的框架，用于将任何机器学习库的模型打包并部署到任何云平台。

![](img/8af7bd219326c5d0c0550bdbddd899ac.png)

来自 [BentoML 首页](https://www.bentoml.com/) 的图像，已获许可。

FastAPI 是为 Web 开发人员设计的，因此在部署 ML 模型方面有许多明显的不足。BentoML 解决了这些问题：

+   **标准 API** 用于保存/加载模型

+   **模型存储** 用于版本控制和跟踪模型

+   通过一行终端代码实现模型的 **Docker 化**

+   在 **GPU** 上服务模型

+   通过一段简短的脚本和几个终端命令将模型作为 **API** 部署到任何云服务提供商

我已经写了一些关于 BentoML 的教程。这里是其中之一：

[](/the-easiest-way-to-deploy-your-ml-dl-models-in-2022-streamlit-bentoml-dagshub-ccf29c901dac?source=post_page-----ef64187b562a--------------------------------) ## 2022 年最简单的机器学习模型部署方法：Streamlit + BentoML + DagsHub

### 编辑描述

towardsdatascience.com

统计数据和链接：

+   [GitHub: BentoML (4.1 ⭐)](https://github.com/bentoml/BentoML)

+   [上个月下载量：36k](https://pypistats.org/packages/bentoml)

## 5\. PyOD

![](img/1fe4a1a50eed205504f56e3c3f36c289.png)

图片由我通过 Midjourney 制作。

这个库被视为黑马，因为它解决的问题，异常检测，也同样是不被看好的。

几乎所有你参加的机器学习课程只教 z 分数来进行异常检测，然后转向更高级的概念和工具，如 R（讽刺）。

但异常检测远不止普通的 z 分数。它包括修改的 z 分数、隔离森林（酷名字）、KNN 异常检测、局部异常因子，以及 30 多种最先进的异常检测算法，这些都打包在 Python 异常检测工具包（PyOD）中。

当没有被正确检测和处理时，异常值会扭曲特征的均值和标准差，并在训练数据中产生噪声——这是你绝对不希望发生的情况。

这就是 PyOD 的生活目标——提供工具来帮助发现异常。除了其广泛的算法，它还与 Scikit-learn 完全兼容，使其易于在现有的机器学习管道中使用。

如果你仍然没有被异常检测的重要性和 PyOD 在其中的作用所说服，我强烈建议你阅读这篇文章（由我撰写）：

[](/how-to-perform-outlier-detection-in-python-in-easy-steps-for-machine-learning-1-8f9a3e6c88b5?source=post_page-----ef64187b562a--------------------------------) [## 如何在 Python 中进行异常检测以用于机器学习：第一部分]

### 编辑描述

towardsdatascience.com](/how-to-perform-outlier-detection-in-python-in-easy-steps-for-machine-learning-1-8f9a3e6c88b5?source=post_page-----ef64187b562a--------------------------------)

统计和链接：

+   [GitHub: PyOD (6.9k⭐)](https://github.com/yzhao062/pyod)

+   [上个月下载量：65 万](https://pypistats.org/packages/pyod)

## 6\. Sktime

![](img/aceb7517d10204cc59c0d6c1e29a50c0.png)

图片来自 [Sktime GitHub 页面](https://github.com/sktime/sktime)。BSD-3 条款许可。

时间机器不再是科幻小说中的东西。它以 Sktime 的形式成为现实。

与其在时间段之间跳跃，Sktime 执行的是*稍微不那么酷*的时间序列分析任务。

它借用了其“大哥” Scikit-learn 的最佳工具，来执行以下时间序列任务：

+   分类

+   回归

+   聚类（这个挺有趣的！）

+   注释

+   预测

它提供了超过 30 种最先进的算法，具有熟悉的 Scikit-learn 语法，还支持管道化、集成和模型调优，适用于单变量和多变量时间序列数据。

它也得到了很好的维护——Sktime 的贡献者们像蜜蜂一样忙碌。

这里有一个关于它的教程（不是我的，遗憾的是）：

[](/build-complex-time-series-regression-pipelines-with-sktime-910bc25c96b6?source=post_page-----ef64187b562a--------------------------------) [## 使用 sktime 构建复杂的时间序列回归管道]

### 编辑描述

towardsdatascience.com](/build-complex-time-series-regression-pipelines-with-sktime-910bc25c96b6?source=post_page-----ef64187b562a--------------------------------)

统计数据和链接：

+   [GitHub: Sktime (6.3k⭐)](https://github.com/sktime/sktime)

+   [上个月下载次数: 788k](https://pypistats.org/packages/sktime)

## 包装

虽然我们的日常工作流程主要由像 Scikit-learn、TensorFlow 或 PyTorch 这样的流行工具主导，但重要的是不要忽视那些鲜为人知的库。

它们可能没有相同的认可度或支持，但在合适的使用者手中，它们能提供优雅的解决方案，解决那些流行对手未能解决的问题。

这篇文章仅关注了 *六* 个，但你可以肯定还有数百个其他库。你只需做些探索！

喜欢这篇文章以及，它那奇特的写作风格？想象一下，拥有更多类似的文章，全部由一位才华横溢、迷人、机智的作者（顺便说一下，就是我 :)）撰写。

只需 4.99$ 会员资格，你不仅可以访问我的故事，还能获得 Medium 上最优秀、最聪明的思想的丰富知识宝库。如果你使用[我的推荐链接](https://ibexorigin.medium.com/membership)，你将获得我的超级感激和对支持我工作的虚拟击掌。

[](https://ibexorigin.medium.com/membership?source=post_page-----ef64187b562a--------------------------------) [## 通过我的推荐链接加入 Medium — Bex T.

### 独享我所有的 ⚡高级⚡ 内容和 Medium 上的所有内容，无限制地支持我的工作，购买…

[ibexorigin.medium.com](https://ibexorigin.medium.com/membership?source=post_page-----ef64187b562a--------------------------------) ![](img/a01b5e4fb641db5f35b8172a4388e821.png)

图片由我通过 Midjourney 制作。
