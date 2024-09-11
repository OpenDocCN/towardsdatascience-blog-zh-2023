# 构建可部署的机器学习管道

> 原文：[`towardsdatascience.com/build-deployable-machine-learning-pipelines-a6d7035816a6?source=collection_archive---------8-----------------------#2023-06-30`](https://towardsdatascience.com/build-deployable-machine-learning-pipelines-a6d7035816a6?source=collection_archive---------8-----------------------#2023-06-30)

## 利用 Kedro 构建生产就绪的机器学习管道

[](https://johnadeojo.medium.com/?source=post_page-----a6d7035816a6--------------------------------)![John Adeojo](https://johnadeojo.medium.com/?source=post_page-----a6d7035816a6--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a6d7035816a6--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----a6d7035816a6--------------------------------) [John Adeojo](https://johnadeojo.medium.com/?source=post_page-----a6d7035816a6--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff933e1637e40&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuild-deployable-machine-learning-pipelines-a6d7035816a6&user=John+Adeojo&userId=f933e1637e40&source=post_page-f933e1637e40----a6d7035816a6---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a6d7035816a6--------------------------------) · 8 min read · 2023 年 6 月 30 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa6d7035816a6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuild-deployable-machine-learning-pipelines-a6d7035816a6&user=John+Adeojo&userId=f933e1637e40&source=-----a6d7035816a6---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa6d7035816a6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuild-deployable-machine-learning-pipelines-a6d7035816a6&source=-----a6d7035816a6---------------------bookmark_footer-----------)![](img/ec434ef42ac9cdfcc488fd5ed7688ab9.png)

作者提供的图像：由 Midjourney 生成

# 背景 — 笔记本无法“部署”

许多数据科学家最初接触编码的方式是通过笔记本风格的用户界面。笔记本在探索中不可或缺——这是我们工作流程中的一个关键环节。然而，它们并不是为生产环境设计的。这是我在许多客户中观察到的一个关键问题，他们中的一些人询问如何将他们的笔记本投入生产。与其将笔记本投入生产，不如选择构建模块化、可维护且可重现的代码，这才是实现生产就绪的最佳途径。

在本文中，我展示了一个用于训练模型以分类欺诈信用卡交易的模块化 ML 管道示例。通过本文的阅读，我希望你能：

1.  了解并欣赏模块化 ML 管道。

1.  感到受到启发，自己动手构建一个。

如果你想要充分利用部署机器学习模型的好处，编写模块化代码是一个重要的步骤。

首先简单定义一下[模块化](https://en.wikipedia.org/wiki/Modular_programming)代码。模块化代码是一种强调软件设计的范式……
