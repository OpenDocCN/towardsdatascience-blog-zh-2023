# Grover的量子搜索算法

> 原文：[https://towardsdatascience.com/grovers-quantum-search-algorithm-54c427315768?source=collection_archive---------12-----------------------#2023-05-23](https://towardsdatascience.com/grovers-quantum-search-algorithm-54c427315768?source=collection_archive---------12-----------------------#2023-05-23)

## 量子计算

## 对最早的量子算法之一的视觉解释

[](https://medium.com/@danjackho?source=post_page-----54c427315768--------------------------------)[![Dan Jackson](../Images/d7b923d6f0462d8226dd7ded7488ba9c.png)](https://medium.com/@danjackho?source=post_page-----54c427315768--------------------------------)[](https://towardsdatascience.com/?source=post_page-----54c427315768--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----54c427315768--------------------------------) [Dan Jackson](https://medium.com/@danjackho?source=post_page-----54c427315768--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F96ed28fe283&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgrovers-quantum-search-algorithm-54c427315768&user=Dan+Jackson&userId=96ed28fe283&source=post_page-96ed28fe283----54c427315768---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----54c427315768--------------------------------) ·16分钟阅读·2023年5月23日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F54c427315768&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgrovers-quantum-search-algorithm-54c427315768&user=Dan+Jackson&userId=96ed28fe283&source=-----54c427315768---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F54c427315768&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgrovers-quantum-search-algorithm-54c427315768&source=-----54c427315768---------------------bookmark_footer-----------)![](../Images/7d2e92f1b38559616e6d4b40772d403a.png)

IBM量子计算机的冷却系统特写图像。图片由 [IBM/Graham Carlow](https://newsroom.ibm.com/media-quantum-innovation?keywords=quantum&l=100) 提供。

**格罗弗算法** 是最早提出的量子算法之一，展示了量子优势，在这种情况下，相对于其经典对应物具有二次‘加速’。该算法由 ***洛夫·格罗弗*** [1] 在1996年开发，是量子计算领域的突破性进展，继承了类似的算法，如 [***肖尔算法***](https://en.wikipedia.org/wiki/Shor%27s_algorithm) 和 [***德意志-乔兹算法***](https://en.wikipedia.org/wiki/Deutsch%E2%80%93Jozsa_algorithm)。在本文中，我们将直观地解释格罗弗算法的工作原理，并数学上展示它如何相对于最佳经典搜索算法演示***量子‘加速’***。

# 未结构化搜索问题

但是，首先让我们介绍一下格罗弗算法所解决的问题。假设我们访问一个***未结构化数据库***，或列表，其中包含*N* ***元素***，每个元素由一个***唯一的 n 位字符串 ID 表示*** *x*。因此，该列表最多可以包含 *N* = 2*ⁿ* 个元素。我们的任务是从数据库中找到一个特定的***“标记”***元素，其位字符串为 *x₀*。
