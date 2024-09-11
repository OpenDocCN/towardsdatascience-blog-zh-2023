# 利用这个 Python 库的强大功能弥合数据与人类之间的差距

> 原文：[https://towardsdatascience.com/bridge-the-gap-between-data-and-humanity-with-the-power-of-this-python-library-553a9f55908b?source=collection_archive---------15-----------------------#2023-02-22](https://towardsdatascience.com/bridge-the-gap-between-data-and-humanity-with-the-power-of-this-python-library-553a9f55908b?source=collection_archive---------15-----------------------#2023-02-22)

## 使您的 Python 输出对人类更易于理解

[](https://zoumanakeita.medium.com/?source=post_page-----553a9f55908b--------------------------------)[![Zoumana Keita](../Images/34a15c1d03687816dbdbc065f5719f80.png)](https://zoumanakeita.medium.com/?source=post_page-----553a9f55908b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----553a9f55908b--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----553a9f55908b--------------------------------) [Zoumana Keita](https://zoumanakeita.medium.com/?source=post_page-----553a9f55908b--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe6ae785a30d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbridge-the-gap-between-data-and-humanity-with-the-power-of-this-python-library-553a9f55908b&user=Zoumana+Keita&userId=e6ae785a30d&source=post_page-e6ae785a30d----553a9f55908b---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----553a9f55908b--------------------------------) · 4 分钟阅读 · 2023 年 2 月 22 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F553a9f55908b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbridge-the-gap-between-data-and-humanity-with-the-power-of-this-python-library-553a9f55908b&user=Zoumana+Keita&userId=e6ae785a30d&source=-----553a9f55908b---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F553a9f55908b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbridge-the-gap-between-data-and-humanity-with-the-power-of-this-python-library-553a9f55908b&source=-----553a9f55908b---------------------bookmark_footer-----------)

# 介绍

我们不需要依赖任何统计数据来意识到 Python 是软件开发人员、数据科学家等使用最多的编程语言之一。这不仅因为其灵活性和易用性，还因为众多的库让我们的日常任务变得更加轻松。

本文介绍了另一个强大的库：`humanize`。它通过使 Python 输出更易于理解，帮助弥合人类与 Python 输出之间的差距。让我们看看一些示例。

# 入门

要使用`humanize`，第一步是通过Python包管理器`pip`进行安装，如下所示：

```py
!pip3 install humanize
```

接下来，你需要导入以下相关库以成功完成教程。

+   `getsize()` 来自`os`库，用于获取给定文件的大小。

+   `datetime` 用于处理时间。

+   最后是`humanize`库，它…
