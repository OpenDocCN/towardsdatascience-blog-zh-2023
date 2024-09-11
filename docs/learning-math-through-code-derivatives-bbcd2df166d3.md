# 通过代码学习数学：导数

> 原文：[`towardsdatascience.com/learning-math-through-code-derivatives-bbcd2df166d3?source=collection_archive---------18-----------------------#2023-03-20`](https://towardsdatascience.com/learning-math-through-code-derivatives-bbcd2df166d3?source=collection_archive---------18-----------------------#2023-03-20)

## 使用 Python 深入理解导数

[](https://harrisonfhoffman.medium.com/?source=post_page-----bbcd2df166d3--------------------------------)![哈里森·霍夫曼](https://harrisonfhoffman.medium.com/?source=post_page-----bbcd2df166d3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----bbcd2df166d3--------------------------------)![数据科学的未来](https://towardsdatascience.com/?source=post_page-----bbcd2df166d3--------------------------------) [哈里森·霍夫曼](https://harrisonfhoffman.medium.com/?source=post_page-----bbcd2df166d3--------------------------------)

·

[查看](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F38889d0801d0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flearning-math-through-code-derivatives-bbcd2df166d3&user=Harrison+Hoffman&userId=38889d0801d0&source=post_page-38889d0801d0----bbcd2df166d3---------------------post_header-----------) 发布于 [数据科学的未来](https://towardsdatascience.com/?source=post_page-----bbcd2df166d3--------------------------------) ·5 分钟阅读·2023 年 3 月 20 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fbbcd2df166d3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flearning-math-through-code-derivatives-bbcd2df166d3&user=Harrison+Hoffman&userId=38889d0801d0&source=-----bbcd2df166d3---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fbbcd2df166d3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flearning-math-through-code-derivatives-bbcd2df166d3&source=-----bbcd2df166d3---------------------bookmark_footer-----------)

数学对许多人来说是一个 notoriously difficult 的学科。由于它的累积性和抽象性，学生可能会发现很难建立联系并理解数学的应用。在我的持续学习中，我发现通过在代码中实现数学概念来增强对数学概念的理解非常有帮助。

编程要求精确，因为计算机只能执行特定的一组指令。对准确性的需求要求采取逻辑性和系统性的问题解决方法，这对理解基础概念非常有益。通过编程，我们可以对实现的想法发展更深刻的直觉。此外，编程使我们能够以互动和动手的方式实验、可视化和自动化数学概念，这可以将理论概念变为现实，增强我们的学习体验。

在本文中，我们将尝试通过在 python 中实现“前向差分商”近似来更好地理解导数。虽然这是一个简单的实现，只需要少量代码，但它触及了导数所代表的核心内容。

# 什么是导数？

让我们从定义导数开始。由于有很多自由获取的导数资源，这里的解释将不会很全面。导数是……
