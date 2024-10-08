# 偏差和方差之间是否总有权衡？

> 原文：[`towardsdatascience.com/is-there-always-a-tradeoff-between-bias-and-variance-5ca44398a552`](https://towardsdatascience.com/is-there-always-a-tradeoff-between-bias-and-variance-5ca44398a552)

## 不拘一格的揭秘者

## 偏差-方差权衡，第一部分，共 3 部分

[](https://kozyrkov.medium.com/?source=post_page-----5ca44398a552--------------------------------)![Cassie Kozyrkov](https://kozyrkov.medium.com/?source=post_page-----5ca44398a552--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5ca44398a552--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----5ca44398a552--------------------------------) [Cassie Kozyrkov](https://kozyrkov.medium.com/?source=post_page-----5ca44398a552--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5ca44398a552--------------------------------) ·阅读时间 5 分钟·2023 年 2 月 15 日

--

你应该阅读这篇文章吗？如果你理解下一部分中的所有单词，那么不。如果你不在乎理解它们，那么也不。如果你想了解粗体部分的解释，那么是。

# 偏差-方差权衡

*“偏差-方差权衡”* 是一个在 [ML/AI](http://bit.ly/quaesita_emperor) 语境中常听到的流行词汇。如果你是 [统计学家](http://bit.ly/quaesita_statistics)，你可能认为它是对这个公式的总结：

**MSE = 偏差² + 方差**

不是的。

嗯，它有点相关，但这个短语实际上指的是如何选择模型的**复杂度甜点**的*实际方法*。当你在调优**正则化** **超参数**时，它最有用。

![](img/45a49487f48458ec80ce35b0cf1a3fd5.png)

作者插图。

***注意：*** *如果你从未听说过 MSE，你可能需要对一些术语有所了解。当你遇到新术语并想要更详细的解释时，你可以跟随链接到我的其他文章中，我会介绍我使用的词汇。*

# 理解基础知识

均方误差（[MSE](http://bit.ly/quaesita_babymse)）是模型的[损失函数](http://bit.ly/quaesita_emperorm)中最流行（且最基础）的选择，通常是你首先被教到的。你可能会上很多[统计](http://bit.ly/quaesita_statistics)课程，才会有人告诉你可以选择最小化其他损失函数。（但说实话：[抛物线很容易优化](http://bit.ly/quaesita_msefav)。记得 *d/dx* *x²* 吗？ 2*x*。这种便利足以让大多数人忠于 MSE。）

一旦你了解了 MSE，通常在几分钟内就会有人提到偏差和方差公式：

> MSE = 偏差² + 方差

[我也做到了](http://bit.ly/quaesita_lemur)，像一个普通的[数据科学](http://bit.ly/quaesita_datascim)狂人一样，把证明留给了[感兴趣的读者](https://twitter.com/kareem_carr/status/1535402776276217859?t=R60xEOxxTYkBq7mkj8CvnQ&s=09)。

让我们做些补救——如果你希望我在推导过程中做些讽刺评论，可以稍微绕道[这里](http://bit.ly/quaesita_mseformula)。如果你选择跳过数学内容，那么你只能接受我的手势，并相信我的话。

# 只有积极的氛围

想让我直言不讳地告诉你关键点吗？请注意，这个公式由两个**不能为负的**项组成。

你在调整你的预测[机器学习/人工智能模型](http://bit.ly/quaesita_emperor)时试图优化的数量（MSE）可以分解为**始终为正的项**，这些项只涉及偏差和方差。

> MSE = 偏差² + 方差 = (偏差)² + (标准差)²

更直接一点？好吧，当然可以。

更好的模型具有更低的 MSE。E 代表[误差](http://bit.ly/quaesita_msefav)，误差越少越好，所以*最佳*模型的 MSE 为零：它不会犯错。这也意味着它*没有偏差*和*没有方差*。

![](img/330c6cb9039af21d8e2b327f3fa087d2.png)

由[劳拉·克劳](https://unsplash.com/de/@laurarain?utm_source=medium&utm_medium=referral)拍摄于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

与其追求完美模型，不如看从好到更好的过程。**如果你真的能够改善你的模型（在 MSE 方面），就不需要在偏差和方差之间做权衡。** 如果你变得更好的弓箭手，你就变得更好的弓箭手了。没有权衡。（你可能需要更多的练习——[数据](http://bit.ly/quaesita_hist)!——才能做到这一点。）

> 正如托尔斯泰所说，所有完美的模型都是相似的，但每个不满意的模型都可以以自己的方式不满意。

# 所有完美的模型都是相似的

正如[托尔斯泰](https://en.wikipedia.org/wiki/Anna_Karenina_principle)所说，所有完美的模型都是相似的，但每个不满意的模型（在给定的 MSE 下）都可以以自己的方式不满意。你可以得到两个同样糟糕但不同的模型，MSE 相同：一个模型可能有很好的（低）偏差但高方差，而另一个模型可能有很好的（低）方差但高偏差，但两者的 MSE（整体得分）却相同。

![](img/62ffc025acf5afebcf290b9c1d6c3652.png)

如果我们用 MSE 来衡量弓箭手的表现，那就意味着减少弓箭手的标准差和减少偏差是等值的。我们说我们对这两者无所谓。（等等，如果*你*对它们并不无所谓呢？那么 MSE 可能不是你最好的选择。如果你不喜欢 MSE 的表现评分方式？没关系。自己制定一个[损失函数](http://bit.ly/quaesita_emperor)。）

现在我们已经铺好了桌子，前往[第二部分](http://bit.ly/quaesita_bivar2)，我们将深入探讨问题的核心：是否存在实际的权衡？（是的！但可能不是你想象的那样。）以及过拟合与此有何关系？（提示：一切都有关！）

# 感谢阅读！怎么样，来一门课程吧？

如果你在这里感到有趣，且正在寻找一个既能吸引人工智能初学者又能打动专家的领导力课程，[这里有一门我为你准备的小课程](https://bit.ly/funaicourse)：

![](img/300b5280620ea948fc3dbffb708084d4.png)

课程链接：[`bit.ly/funaicourse`](https://bit.ly/funaicourse)

想提升你的决策能力而不是仅仅增强你的人工智能技能？你可以通过这个[链接访问我的免费课程：](https://bit.ly/decisioncourse)

[## 你生活的方向盘——决策智能视频教程 | LinkedIn Learning…](https://bit.ly/decisioncourse?source=post_page-----5ca44398a552--------------------------------)

### 决策能力是你可以学习的最宝贵的技能。你的人生归结为两件事：你生活的质量和…

[决策课程](https://bit.ly/decisioncourse?source=post_page-----5ca44398a552--------------------------------)

*P.S. 你是否尝试过在 Medium 上多次点击鼓掌按钮看看会发生什么？* ❤️

# 喜欢作者吗？与 Cassie Kozyrkov 联系

让我们成为朋友吧！你可以在[Twitter](https://twitter.com/quaesita)、[YouTube](https://www.youtube.com/channel/UCbOX--VOebPe-MMRkatFRxw)、[Substack](http://decision.substack.com)和[LinkedIn](https://www.linkedin.com/in/kozyrkov/)上找到我。对让我在你的活动中发言感兴趣？请使用[这个表单](http://bit.ly/makecassietalk)与我联系。

[## 加入 Medium](https://kozyrkov.medium.com/membership?source=post_page-----5ca44398a552--------------------------------)

### 阅读 Cassie Kozyrkov 的每一个故事（以及 Medium 上成千上万的其他作家的故事）。你的会员费直接支持…

[kozyrkov.medium.com](https://kozyrkov.medium.com/membership?source=post_page-----5ca44398a552--------------------------------)
