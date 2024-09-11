# 从头开始的模拟退火局部搜索

> 原文：[`towardsdatascience.com/local-search-with-simulated-annealing-from-scratch-9f8dcb6c2e06?source=collection_archive---------9-----------------------#2023-04-12`](https://towardsdatascience.com/local-search-with-simulated-annealing-from-scratch-9f8dcb6c2e06?source=collection_archive---------9-----------------------#2023-04-12)

![](img/99361b24f6101d25e65a3ea1a9ea55ed.png)

温度是模拟退火中的一个重要部分。图片来源：Dall-E 2。

## 带有 3 个示例的通用 Python 代码

[](https://hennie-de-harder.medium.com/?source=post_page-----9f8dcb6c2e06--------------------------------)![Hennie de Harder](https://hennie-de-harder.medium.com/?source=post_page-----9f8dcb6c2e06--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9f8dcb6c2e06--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----9f8dcb6c2e06--------------------------------) [Hennie de Harder](https://hennie-de-harder.medium.com/?source=post_page-----9f8dcb6c2e06--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ffb96be98b7b9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flocal-search-with-simulated-annealing-from-scratch-9f8dcb6c2e06&user=Hennie+de+Harder&userId=fb96be98b7b9&source=post_page-fb96be98b7b9----9f8dcb6c2e06---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----9f8dcb6c2e06--------------------------------) ·11 分钟阅读·2023 年 4 月 12 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F9f8dcb6c2e06&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flocal-search-with-simulated-annealing-from-scratch-9f8dcb6c2e06&user=Hennie+de+Harder&userId=fb96be98b7b9&source=-----9f8dcb6c2e06---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F9f8dcb6c2e06&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flocal-search-with-simulated-annealing-from-scratch-9f8dcb6c2e06&source=-----9f8dcb6c2e06---------------------bookmark_footer-----------)

**在我之前的一些文章中，我解释了启发式算法以及如何使用它们来找到数学优化问题的优质解决方案。在这篇文章中，我将提供用于局部搜索的通用 Python 代码以及模拟退火。除了通用代码之外，还有三个经典示例问题的实现：旅行商问题、背包问题和 Rastrigin 函数。**

简要回顾：局部搜索是一种启发式方法，通过查看邻域来尝试改进给定的解决方案。如果一个邻域的目标值优于当前目标值，则接受该邻域解，并继续搜索。模拟退火允许接受更差的解，这使得它能够逃脱局部最小值。

## 模拟退火通用代码

代码的工作流程如下：我们将创建四个代码文件。最重要的是`sasolver.py`，这个文件包含了模拟退火的通用代码。问题目录包含了三个优化问题的示例，我们可以运行这些示例来测试 SA 求解器。

这是文件夹结构：
