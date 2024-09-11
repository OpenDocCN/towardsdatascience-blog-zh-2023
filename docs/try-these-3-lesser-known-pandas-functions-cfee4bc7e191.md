# 尝试这三种鲜为人知的 Pandas 函数

> 原文：[https://towardsdatascience.com/try-these-3-lesser-known-pandas-functions-cfee4bc7e191?source=collection_archive---------3-----------------------#2023-08-28](https://towardsdatascience.com/try-these-3-lesser-known-pandas-functions-cfee4bc7e191?source=collection_archive---------3-----------------------#2023-08-28)

## 提升你的数据处理技能

[](https://yongcui01.medium.com/?source=post_page-----cfee4bc7e191--------------------------------)[![Yong Cui](../Images/475918ba9ca0ecd923abe2e7843582a9.png)](https://yongcui01.medium.com/?source=post_page-----cfee4bc7e191--------------------------------)[](https://towardsdatascience.com/?source=post_page-----cfee4bc7e191--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----cfee4bc7e191--------------------------------) [Yong Cui](https://yongcui01.medium.com/?source=post_page-----cfee4bc7e191--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F88ff1e2545d0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftry-these-3-lesser-known-pandas-functions-cfee4bc7e191&user=Yong+Cui&userId=88ff1e2545d0&source=post_page-88ff1e2545d0----cfee4bc7e191---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----cfee4bc7e191--------------------------------) ·6 min read·2023年8月28日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fcfee4bc7e191&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftry-these-3-lesser-known-pandas-functions-cfee4bc7e191&user=Yong+Cui&userId=88ff1e2545d0&source=-----cfee4bc7e191---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fcfee4bc7e191&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftry-these-3-lesser-known-pandas-functions-cfee4bc7e191&source=-----cfee4bc7e191---------------------bookmark_footer-----------)![](../Images/bf3727e8ae6553e9f1b0037f4efe9f40.png)

图片由 [Balázs Kétyi](https://unsplash.com/@balazsketyi?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

如果你问任何经验丰富的数据科学家和机器学习工程师，他们工作中耗费时间最多的是什么？我想很多人会说：数据预处理——这是一个清理数据并为后续数据分析做好准备的步骤。原因很简单——垃圾进，垃圾出。也就是说，如果你不正确地准备数据，你对数据的“洞察”几乎不会有意义。

尽管数据预处理步骤可能相当繁琐，但 Pandas 提供了所有必需的功能，使我们能够相对轻松地完成数据清理工作。然而，由于其多功能性，并不是每个用户都知道 pandas 库提供的所有功能。在这篇文章中，我想分享3个鲜为人知但非常有用的函数，你可以在数据科学项目中尝试。

不再废话，让我们直接开始吧。

*注意：为了提供背景，假设你负责一个服装店的数据管理和分析。下面展示的示例基于这一假设。*

# 1\. explode

我想提到的第一个函数是`explode`。当你处理包含列表的列中的数据时，这个函数非常有用。当你使用…
