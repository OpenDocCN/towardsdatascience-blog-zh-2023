# 使用Pandas可视化数据的3种快速简便方法

> 原文：[https://towardsdatascience.com/3-quick-and-easy-ways-to-visualize-your-data-using-pandas-4cac57fb4c82?source=collection_archive---------3-----------------------#2023-03-14](https://towardsdatascience.com/3-quick-and-easy-ways-to-visualize-your-data-using-pandas-4cac57fb4c82?source=collection_archive---------3-----------------------#2023-03-14)

## 数据科学

## 通过这些简单有效的Pandas可视化技巧，充分利用你的数据

[](https://medium.com/@17.rsuraj?source=post_page-----4cac57fb4c82--------------------------------)[![Suraj Gurav](../Images/f5dca32861f8c1c428e66fbe2174c04b.png)](https://medium.com/@17.rsuraj?source=post_page-----4cac57fb4c82--------------------------------)[](https://towardsdatascience.com/?source=post_page-----4cac57fb4c82--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----4cac57fb4c82--------------------------------) [Suraj Gurav](https://medium.com/@17.rsuraj?source=post_page-----4cac57fb4c82--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1fdda183cca2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F3-quick-and-easy-ways-to-visualize-your-data-using-pandas-4cac57fb4c82&user=Suraj+Gurav&userId=1fdda183cca2&source=post_page-1fdda183cca2----4cac57fb4c82---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----4cac57fb4c82--------------------------------) ·9分钟阅读·2023年3月14日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F4cac57fb4c82&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F3-quick-and-easy-ways-to-visualize-your-data-using-pandas-4cac57fb4c82&user=Suraj+Gurav&userId=1fdda183cca2&source=-----4cac57fb4c82---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F4cac57fb4c82&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F3-quick-and-easy-ways-to-visualize-your-data-using-pandas-4cac57fb4c82&source=-----4cac57fb4c82---------------------bookmark_footer-----------)![](../Images/204d202e04c5449832ff08d3f3fdaf59.png)

[照片由](https://dengxiang.pages.dev/) [Choong Deng Xiang](https://unsplash.com/@dengxiangs?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

数据可视化是探索性数据分析中的一个重要方面。它帮助你在短时间内识别数据中的趋势、模式和异常值。

同时，数据可视化有助于以更有效的方式展示你的分析结果，这将促进对这些发现的讨论，而不仅仅是展示数字。这就是为什么 Net Objectives 的创始人兼 CEO Al Shalloway 曾经引用过——

> “可视化就像篝火，我们围绕它讲故事。”

然而，在你的数据分析项目中，可视化数据并不总是需要像 PowerBI、Tableau 这样的数据可视化软件，也不需要安装任何独立的数据可视化库，只需使用 Python。

你可以利用**pandas** —— 一个常用的数据处理库 —— 来可视化数据，同时对数据进行转换。

虽然 pandas 不是一个数据可视化库，但它具有创建基本但有效的可视化的内置功能。它可以轻松地转换……
