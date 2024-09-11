# 如何修复 TypeError: ObjectId 无法序列化为 JSON

> 原文：[https://towardsdatascience.com/pymongo-cursor-to-json-9f770740375a?source=collection_archive---------3-----------------------#2023-01-07](https://towardsdatascience.com/pymongo-cursor-to-json-9f770740375a?source=collection_archive---------3-----------------------#2023-01-07)

## 在 Python 中将 mongo 游标转换为 JSON 对象

[](https://gmyrianthous.medium.com/?source=post_page-----9f770740375a--------------------------------)[![Giorgos Myrianthous](../Images/ff4b116e4fb9a095ce45eb064fde5af3.png)](https://gmyrianthous.medium.com/?source=post_page-----9f770740375a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9f770740375a--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----9f770740375a--------------------------------) [Giorgos Myrianthous](https://gmyrianthous.medium.com/?source=post_page-----9f770740375a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F76c21e75463a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpymongo-cursor-to-json-9f770740375a&user=Giorgos+Myrianthous&userId=76c21e75463a&source=post_page-76c21e75463a----9f770740375a---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----9f770740375a--------------------------------) · 4 分钟阅读 · 2023年1月7日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F9f770740375a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpymongo-cursor-to-json-9f770740375a&user=Giorgos+Myrianthous&userId=76c21e75463a&source=-----9f770740375a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F9f770740375a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpymongo-cursor-to-json-9f770740375a&source=-----9f770740375a---------------------bookmark_footer-----------)![](../Images/eaab2de46fd1a4afdef1ebe34e4c49af.png)

图片由 [Ciprian Boiciuc](https://unsplash.com/@ciprian?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/TrNSWatUW5g?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

欢迎来到我们的 PyMongo 游标序列化为 JSON 的教程。在本文中，我们将介绍如何使用自定义的 `JSONEncoder` 正确处理 `ObjectId` 和 `datetime` 对象，以及其他任何对象。

在使用 PyMongo 时，一个常见的任务是将数据序列化以便存储或通过网络传输。在本教程中，我们将讨论如何将 PyMongo 游标（这是一个用于存储 MongoDB 查询结果的常见数据结构）序列化为 JSON 格式。

在这样做时，常见的错误是以下 `TypeError`：

```py
TypeError: ObjectId('') is not JSON serializable
```

我们还将深入探讨如何正确处理复杂数据类型，如 `ObjectId` 和日期时间对象，这些对象不能直接序列化为 JSON。我们将展示如何使用自定义 `JSONEncoder` 来正确处理这些对象以及你在 PyMongo 游标中可能遇到的其他自定义对象类型。

所以，如果你想学习如何将 PyMongo 游标序列化为 JSON 并处理复杂数据类型，请继续阅读！

## 创建一个自定义 `JSONEncoder`
