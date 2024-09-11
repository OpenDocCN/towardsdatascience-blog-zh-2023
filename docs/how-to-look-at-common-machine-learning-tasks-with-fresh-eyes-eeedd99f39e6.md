# 如何以全新的视角看待常见的机器学习任务

> 原文：[`towardsdatascience.com/how-to-look-at-common-machine-learning-tasks-with-fresh-eyes-eeedd99f39e6?source=collection_archive---------12-----------------------#2023-07-20`](https://towardsdatascience.com/how-to-look-at-common-machine-learning-tasks-with-fresh-eyes-eeedd99f39e6?source=collection_archive---------12-----------------------#2023-07-20)

[](https://towardsdatascience.medium.com/?source=post_page-----eeedd99f39e6--------------------------------)![TDS 编辑](https://towardsdatascience.medium.com/?source=post_page-----eeedd99f39e6--------------------------------)[](https://towardsdatascience.com/?source=post_page-----eeedd99f39e6--------------------------------)![数据科学前沿](https://towardsdatascience.com/?source=post_page-----eeedd99f39e6--------------------------------) [TDS 编辑](https://towardsdatascience.medium.com/?source=post_page-----eeedd99f39e6--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7e12c71dfa81&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-look-at-common-machine-learning-tasks-with-fresh-eyes-eeedd99f39e6&user=TDS+Editors&userId=7e12c71dfa81&source=post_page-7e12c71dfa81----eeedd99f39e6---------------------post_header-----------) 发表在 [数据科学前沿](https://towardsdatascience.com/?source=post_page-----eeedd99f39e6--------------------------------) · 发送为 通讯 · 3 分钟阅读 · 2023 年 7 月 20 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Feeedd99f39e6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-look-at-common-machine-learning-tasks-with-fresh-eyes-eeedd99f39e6&user=TDS+Editors&userId=7e12c71dfa81&source=-----eeedd99f39e6---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Feeedd99f39e6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-look-at-common-machine-learning-tasks-with-fresh-eyes-eeedd99f39e6&source=-----eeedd99f39e6---------------------bookmark_footer-----------)

我们绝不会推荐仅为了改变而改变稳定且表现良好的工作流程；“如果没坏，就别修” 是一种流行的俗语，原因很简单：这往往是正确的做法。

尽管如此，“非常频繁”和“总是”之间还有相当大的差距，而我们在工作中最令人沮丧的日子通常是当我们经过验证的方法未能产生预期结果或表现不佳时。这时扩展我们的知识基础真的会带来回报：我们不会陷入精神上的“死亡旋转轮”，而是尝试不同的方法，调整我们的过程，并（迟早）找到新的解决方案。

在拥抱新视角的精神下，我们整理了一系列出色的近期帖子，提供了对常见机器学习工作流的新颖解读。它们涵盖了漂移检测和模型训练等过程，以及从图像分割到命名实体识别的任务。为你的工具箱腾出空间——你会想要添加这些内容！

在深入之前，简单更新一下：如果你想寻找其他方式来跟上我们最新的优质文章，**我们刚刚推出了几个 Medium 列表**以帮助你发现更多精彩阅读。

+   算法推荐系统无处不在，从电子商务网站到流媒体服务，其输出有时可能显得重复和显而易见。正如[Christabelle Pabalan](https://medium.com/u/4200eb8e8b26?source=post_page-----eeedd99f39e6--------------------------------)所展示的，**我们不必满足于毫无创意的选择**—实际上，将推荐系统注入新奇和意外元素可以带来更好的用户留存率。

+   检测在非结构化数据上训练的模型的漂移，例如用于 LLM 驱动应用的嵌入，“是一个相当新的话题，目前没有‘最佳实践’方法，”[Elena Samuylova](https://medium.com/u/9621354b583a?source=post_page-----eeedd99f39e6--------------------------------)和 Olga Filippova 表示。**为了帮助你选择最有效的方法**，他们进行了若干实验，并根据他们的发现分享了清晰的建议。

+   许多数据科学家和机器学习从业者认为**合成数据选项在模型训练中的快速崛起**值得庆祝，但也认识到这带来了数据质量和长期表现方面的严重问题。[Vincent Vatter](https://medium.com/u/e9a7aeeab29f?source=post_page-----eeedd99f39e6--------------------------------)为我们介绍了微软的最新研究，指出了前进的有效路径。

+   模型校准是许多分类任务中的关键步骤，但以优化准确性的方式进行计算可能会很棘手。[Maja Pavlovic](https://medium.com/u/9b1766e00cb4?source=post_page-----eeedd99f39e6--------------------------------) 提供了**关于处理预期校准误差（ECE）的清晰实用教程**。

![](img/cc61ba6a7a4d37d6df89f6fc3240f411.png)

图片由 [Bonnie Kittle](https://unsplash.com/@bonniekdesign?utm_source=medium&utm_medium=referral) 拍摄，发布在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)。

+   如果你在最近使用卷积神经网络的图像分割项目中遇到了瓶颈，[Dhruv Matani](https://medium.com/u/63f5d5495279?source=post_page-----eeedd99f39e6--------------------------------) 和 [Naresh](https://medium.com/u/1e659a80cffd?source=post_page-----eeedd99f39e6--------------------------------) 提供了一个替代方案：不妨尝试**基于视觉变换器的模型**。

+   作为 NOS——荷兰公共广播基金会的数据科学家，[Felix van Deelen](https://medium.com/u/dfe1ea06bab8?source=post_page-----eeedd99f39e6--------------------------------) 可以访问丰富的新闻故事语料库；Felix 的首篇 TDS 文章**探讨了如何在命名实体识别项目中使用这些文本数据**。

+   检测数据中的异常没有一种通用的解决方案，这使得熟悉几种选项成为一个好主意。[Viyaleta Apgar](https://medium.com/u/ccae8864d5a4?source=post_page-----eeedd99f39e6--------------------------------) 向我们介绍了一种基于高斯分布的适合初学者的技术，并展示了**如何在多变量模型的背景下实现它**。

+   为了更有效地优化你的回归模型，[Erdogan Taskesen](https://medium.com/u/4e636e2ef813?source=post_page-----eeedd99f39e6--------------------------------) 提出了**在模型训练的超参数调整步骤中加入贝叶斯风格**的建议；该教程包括了一个全面的实现，依赖于 HGBoost 库的强大功能。

感谢你对我们作者的支持！如果你喜欢在 TDS 上阅读的文章，可以考虑[成为 Medium 会员](https://bit.ly/tds-membership)——这将解锁我们的全部档案（以及 Medium 上的其他每篇文章）。

直到下一个 Variable，

TDS 编辑部
