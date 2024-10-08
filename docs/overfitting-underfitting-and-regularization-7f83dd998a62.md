# 过拟合、欠拟合与正则化

> 原文：[`towardsdatascience.com/overfitting-underfitting-and-regularization-7f83dd998a62`](https://towardsdatascience.com/overfitting-underfitting-and-regularization-7f83dd998a62)

## 与 AI 交朋友

## 偏差-方差权衡，第三部分中的第二部分

[](https://kozyrkov.medium.com/?source=post_page-----7f83dd998a62--------------------------------)![Cassie Kozyrkov](https://kozyrkov.medium.com/?source=post_page-----7f83dd998a62--------------------------------)[](https://towardsdatascience.com/?source=post_page-----7f83dd998a62--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----7f83dd998a62--------------------------------) [Cassie Kozyrkov](https://kozyrkov.medium.com/?source=post_page-----7f83dd998a62--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----7f83dd998a62--------------------------------) ·阅读时长 4 分钟·2023 年 2 月 15 日

--

在[第一部分](http://bit.ly/quaesita_bivar1)中，我们覆盖了大部分基本术语以及关于偏差-方差公式的几个关键见解（**MSE = 偏差² + 方差**），包括来自[安娜·卡列尼娜](https://www.goodreads.com/work/quotes/2507928)的这段释义：

> 所有完美的模型都是相似的，但每个不完美的模型都有其独特的不完美方式。

为了充分利用*这篇*文章，我建议你先阅读[第一部分](http://bit.ly/quaesita_bivar1)，确保你为吸收这篇文章做好了准备。

![](img/6073e1755019bc63fad47ad5d1bed129.png)

欠拟合与过拟合…… 图片由作者提供。

# 过拟合/欠拟合与此有何关系？

假设你有一个[模型](http://bit.ly/quaesita_emperorm)，这是你能从现有信息中得到的最好模型。

要获得更好的模型，你需要更好的[数据](http://bit.ly/quaesita_hist)。换句话说，就是更多的数据（数量）或*更相关*的数据（质量）。

当我说你能得到的*最好*时，我指的是在模型未见过的数据上的[MSE](http://bit.ly/quaesita_msefav)性能。（它应该是[*预测*](http://bit.ly/quaesita_parrot)，而不是*后验*。）你已经从现有信息中尽力而为——剩下的误差是你无法通过信息解决的。

> 现实 = 最佳模型 + 不可避免的误差

但问题是……我们已经提前了；**你还没有这个模型。**

你只有一堆旧数据来学习这个模型。如果你足够聪明，你会在模型未见过的数据上[验证这个模型](http://bit.ly/quaesita_idiot)，但首先你必须通过发现数据中的有用模式，尽量接近目标：一个尽可能低的 MSE。

不幸的是，在[学习过程中](http://bit.ly/quaesita_mrbean)，你无法观察到你期望的 MSE（即来自现实的那个）。你只能计算出来自当前训练数据集的粗略版本。

![](img/c20873c3f1bfa1837ca701dc6174fbfd.png)

[Jason Leung](https://unsplash.com/@ninjason?utm_source=medium&utm_medium=referral) 的照片，来自[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

哦，还有，在这个例子中，“你”不是一个人，你是一个[优化算法](http://bit.ly/mfml_045)，被你的人类老板指示调整模型设置中的旋钮，直到 MSE 降到最低。

你说，*“太好了！我能做到这一点！！老板，如果你给我一个具有许多设置的极其灵活的模型（*[*神经网络*](http://bit.ly/quaesita_emperor)*，怎么样？），我可以给你一个完美的训练 MSE。没有偏差也没有方差。”*

要获得比真实模型的测试 MSE 更好的训练 MSE，你需要将所有噪声（你无法进行预测的错误）与信号一起拟合。如何实现这个小奇迹？通过使模型更复杂。本质上就是连接点。

这被称为[过拟合](http://bit.ly/quaesita_sydd)。这样的模型在训练 MSE 上表现优异，但在实际应用中会有巨大的方差。这就是试图通过创建一个[比信息支持的复杂度更高的解决方案](http://bit.ly/mfml_049)来作弊的结果。

老板太聪明了，识破了你的把戏。知道一个[灵活的](http://bit.ly/quaesita_emperor)复杂模型会使你在[训练集](http://bit.ly/quaesita_mrbean)上得分过高，老板更改了评分函数，以惩罚复杂性。这被称为[正则化](http://bit.ly/quaesita_059)。 （坦率地说，我希望我们能更多地正则化工程师的恶作剧，阻止他们为了复杂性而做复杂的事情。）

正则化本质上是说，*“每增加一点复杂度都要付出代价，所以除非它至少能改善拟合，否则不要这么做…”*

如果老板过度正则化——对简化过于严格——你的表现评估会很糟糕，除非你过度简化模型，否则你会这样做。

这被称为[欠拟合](http://bit.ly/mfml_050)。这样的模型在训练分数上表现优异（主要是因为获得了所有简化奖励），但在现实中却有巨大的偏差。这就是坚持认为解决方案应比问题要求更简单的结果。

这样，我们就准备好了[第三部分](http://bit.ly/quaesita_bivar3)，在这一部分中，我们将所有内容整合在一起，把偏差-方差权衡压缩成一个方便的 nutshell。

# 谢谢阅读！怎么考虑一下课程？

如果你在这里玩得很开心，并且你正在寻找一个旨在取悦 AI 初学者和专家的领导力导向课程，[这是我为你准备的一些东西](https://bit.ly/funaicourse)：

![](img/300b5280620ea948fc3dbffb708084d4.png)

课程链接：[`bit.ly/funaicourse`](https://bit.ly/funaicourse)

# 寻找实操性的机器学习/人工智能教程？

这里是我喜欢的一些 10 分钟快速演练：

+   [AutoML](https://console.cloud.google.com/?walkthrough_id=automl_quickstart)

+   [Vertex AI](https://bit.ly/kozvertex)

+   [AI 笔记本](https://bit.ly/kozvertexnotebooks)

+   [表格数据的机器学习](https://bit.ly/kozvertextables)

+   [文本分类](https://bit.ly/kozvertextext)

+   [图像分类](https://bit.ly/kozverteximage)

+   [视频分类](https://bit.ly/kozvertexvideo)
