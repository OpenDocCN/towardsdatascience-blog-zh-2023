# 让你的 Python 代码快速恢复活力

> 原文：[`towardsdatascience.com/performance-fix-for-slow-python-lru-cache-f9a454776716?source=collection_archive---------7-----------------------#2023-06-06`](https://towardsdatascience.com/performance-fix-for-slow-python-lru-cache-f9a454776716?source=collection_archive---------7-----------------------#2023-06-06)

## 显著加速可重复使用的计算密集型任务。

[](https://thuwarakesh.medium.com/?source=post_page-----f9a454776716--------------------------------)![Thuwarakesh Murallie](https://thuwarakesh.medium.com/?source=post_page-----f9a454776716--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f9a454776716--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----f9a454776716--------------------------------) [Thuwarakesh Murallie](https://thuwarakesh.medium.com/?source=post_page-----f9a454776716--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F93ce19993bef&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fperformance-fix-for-slow-python-lru-cache-f9a454776716&user=Thuwarakesh+Murallie&userId=93ce19993bef&source=post_page-93ce19993bef----f9a454776716---------------------post_header-----------) 发布在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f9a454776716--------------------------------) ·6 分钟阅读·2023 年 6 月 6 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff9a454776716&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fperformance-fix-for-slow-python-lru-cache-f9a454776716&user=Thuwarakesh+Murallie&userId=93ce19993bef&source=-----f9a454776716---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff9a454776716&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fperformance-fix-for-slow-python-lru-cache-f9a454776716&source=-----f9a454776716---------------------bookmark_footer-----------)![](img/9e339d712f7ddce36ca1ce48ec399b29.png)

图片由 [Ryan Johnston](https://unsplash.com/@ryanjohnstonco?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

计算密集型任务现在无处不在。

我们现在经常使用资源密集型技术，如 LLMs 和生成性 AI。

使用珍贵资源的人都会知道，即使我们知道结果会相同，重复执行同样的任务有多么令人畏惧。你会责怪自己没有存储上一次运行的结果。

这就是 LRU 缓存可以帮助我们的地方。LRU 代表最近最少使用（Least Recently Used）。它是众多缓存策略之一。让我们首先了解它是如何工作的。

[](/python-decorators-for-data-science-6913f717669a?source=post_page-----f9a454776716--------------------------------) ## 我在几乎所有数据科学项目中使用的 5 个 Python 装饰器

### 装饰器提供了一种新颖且方便的方法，用于缓存到发送通知的各种功能。

towardsdatascience.com

## @lru_cache 在 Python 中是如何工作的？

想象一下你的大脑是一个小玩具箱。它只能容纳五个玩具。你的朋友们不断询问你关于不同玩具的事情，你用你那超强的记忆力来记住并讲述这些玩具的故事。

有些玩具很容易找到，因为你的朋友们经常问这些玩具，所以它们在最上面。有些玩具…
