# 我在几乎所有数据科学项目中使用的 5 个 Python 装饰器

> 原文：[https://towardsdatascience.com/python-decorators-for-data-science-6913f717669a?source=collection_archive---------0-----------------------#2023-03-13](https://towardsdatascience.com/python-decorators-for-data-science-6913f717669a?source=collection_archive---------0-----------------------#2023-03-13)

## 装饰器提供了一种新的便捷方式，从缓存到发送通知。

[](https://thuwarakesh.medium.com/?source=post_page-----6913f717669a--------------------------------)[![Thuwarakesh Murallie](../Images/44f1a14a899426592bbd8c7f73ce169d.png)](https://thuwarakesh.medium.com/?source=post_page-----6913f717669a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6913f717669a--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----6913f717669a--------------------------------) [Thuwarakesh Murallie](https://thuwarakesh.medium.com/?source=post_page-----6913f717669a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F93ce19993bef&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-decorators-for-data-science-6913f717669a&user=Thuwarakesh+Murallie&userId=93ce19993bef&source=post_page-93ce19993bef----6913f717669a---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6913f717669a--------------------------------) ·6 分钟阅读·2023年3月13日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F6913f717669a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-decorators-for-data-science-6913f717669a&user=Thuwarakesh+Murallie&userId=93ce19993bef&source=-----6913f717669a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F6913f717669a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-decorators-for-data-science-6913f717669a&source=-----6913f717669a---------------------bookmark_footer-----------)![](../Images/fc0bc39e9f13f04a39005e4e7610a8fe.png)

图片由 [Elena Mozhvilo](https://unsplash.com/@miracleday?utm_source=medium&utm_medium=referral) 拍摄，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

起初，每个开发者的目标是让事情运作起来。渐渐地，我们开始关注代码的可读性和可扩展性。这时我们才会开始考虑使用装饰器。

装饰器是为函数添加额外行为的绝佳方式。而且，作为数据科学家，我们常常需要在函数定义中注入一些小东西。

使用装饰器，你会惊讶于代码重复和可读性的改善。我确实感到惊讶。

[使用 Streamlit 在几分钟内创建 GPT3 驱动的应用程序](https://levelup.gitconnected.com/streamlit-openai-gpt3-example-app-b333da955ceb?source=post_page-----6913f717669a--------------------------------)

### 学会构建智能应用程序，而无需过多担心软件开发。

[如何使用 GitHub Actions 构建简单的 ETL 流水线](https://levelup.gitconnected.com/streamlit-openai-gpt3-example-app-b333da955ceb?source=post_page-----6913f717669a--------------------------------)

### ETL 不必复杂。如果是这样，使用 GitHub Actions。

[GitHub Actions 和 ETL 的定时操作](https://levelup.gitconnected.com/streamlit-openai-gpt3-example-app-b333da955ceb?source=post_page-----6913f717669a--------------------------------)

以下是我在几乎每个数据密集型项目中使用的五种最常见的方法。
