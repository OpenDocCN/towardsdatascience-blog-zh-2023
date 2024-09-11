# 进化算法 — 突变解释

> 原文：[https://towardsdatascience.com/evolutionary-algorithm-mutations-explained-4a3b5c2d49de?source=collection_archive---------15-----------------------#2023-11-07](https://towardsdatascience.com/evolutionary-algorithm-mutations-explained-4a3b5c2d49de?source=collection_archive---------15-----------------------#2023-11-07)

## 使用代码实现的示例，应用于TSP

[](https://medium.com/@byjameskoh?source=post_page-----4a3b5c2d49de--------------------------------)[![James Koh, PhD](../Images/8e7af8b567cdcf24805754801683b426.png)](https://medium.com/@byjameskoh?source=post_page-----4a3b5c2d49de--------------------------------)[](https://towardsdatascience.com/?source=post_page-----4a3b5c2d49de--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----4a3b5c2d49de--------------------------------) [James Koh, PhD](https://medium.com/@byjameskoh?source=post_page-----4a3b5c2d49de--------------------------------)

·

[跟进](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F780706b02d58&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fevolutionary-algorithm-mutations-explained-4a3b5c2d49de&user=James+Koh%2C+PhD&userId=780706b02d58&source=post_page-780706b02d58----4a3b5c2d49de---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----4a3b5c2d49de--------------------------------) ·8 分钟阅读·2023年11月7日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F4a3b5c2d49de&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fevolutionary-algorithm-mutations-explained-4a3b5c2d49de&source=-----4a3b5c2d49de---------------------bookmark_footer-----------)![](../Images/1f3a72402d15dee37703769569019834.png)

由DALL·E 3基于提示“绘制一幅看起来像科幻的图像，描述突变。左边显示‘之前’，右边显示‘之后’”创作的图像。

这是[进化算法 — 选择解释](/evolutionary-algorithm-selections-explained-2515fb8d4287?sk=a4cd1504b6098f82f004db32567c8832)的延续。

如果你阅读以获得对基于前置表达的重组和突变的高层理解，本文将作为一个独立的自给自足的文章。

但是，要全面理解每个步骤的详细信息，建议先阅读上述链接的文章，然后再继续阅读这里。

配合上一篇文章中的代码片段，你将能够在个人电脑上解决著名的旅行商问题（TSP）。更重要的是，你将理解所有幕后发生的事情。

# 达成共识

在第一部分，我概述了进化算法框架，如下所示：

![](../Images/dbe66fc1a36b486b957647fe9fcaf5b8.png)

作者提供的图像

在了解了一些进化算法中使用的术语后，我们深入探讨了初始化合适基因型的细节（在第3.1节，对于<1>）以及轮盘赌等……
