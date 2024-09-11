# 10 个学习 Python JSON 模块的示例

> 原文：[`towardsdatascience.com/10-examples-to-learn-the-json-module-of-python-793e62309d64?source=collection_archive---------4-----------------------#2023-05-16`](https://towardsdatascience.com/10-examples-to-learn-the-json-module-of-python-793e62309d64?source=collection_archive---------4-----------------------#2023-05-16)

## 最常用的文件格式之一

[](https://sonery.medium.com/?source=post_page-----793e62309d64--------------------------------)![Soner Yıldırım](https://sonery.medium.com/?source=post_page-----793e62309d64--------------------------------)[](https://towardsdatascience.com/?source=post_page-----793e62309d64--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----793e62309d64--------------------------------) [Soner Yıldırım](https://sonery.medium.com/?source=post_page-----793e62309d64--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2cf6b549448&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F10-examples-to-learn-the-json-module-of-python-793e62309d64&user=Soner+Y%C4%B1ld%C4%B1r%C4%B1m&userId=2cf6b549448&source=post_page-2cf6b549448----793e62309d64---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----793e62309d64--------------------------------) ·6 min 阅读·2023 年 5 月 16 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F793e62309d64&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F10-examples-to-learn-the-json-module-of-python-793e62309d64&user=Soner+Y%C4%B1ld%C4%B1r%C4%B1m&userId=2cf6b549448&source=-----793e62309d64---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F793e62309d64&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F10-examples-to-learn-the-json-module-of-python-793e62309d64&source=-----793e62309d64---------------------bookmark_footer-----------)![](img/325096546b3bec115ac12184f691ca12.png)

图片来源于 [Joshua Hoehne](https://unsplash.com/@mrthetrain?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/photos/YPgTovTiUv4?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

JSON，即 JavaScript 对象表示法，是一种非常常用的文件（或数据）格式。虽然它来源于 JavaScript，但 JSON 是语言无关的，大多数编程语言都有可用于处理 JSON 文件的解析器。

JSON 可以被视为一组键值对，就像 Python 中的字典一样。

这是一个简单的 JSON 文件：

```py
{"name": "Jane", "age": 28, "city": "Houston"}
```

正如我们在上面的例子中所看到的，JSON 对人类友好，你可以自然地阅读 JSON 并理解其中包含的数据。它对机器解析和生成也很容易。

# 为什么 JSON 如此受欢迎？

众多因素促成了 JSON 在软件开发和编程中的广泛使用。让我们列出其中的一些：

1.  可读性：正如我们在上面的例子中所观察到的，JSON 易于阅读和编写，这使它成为存储你希望对人类友好的数据的绝佳选择。

1.  轻量级：JSON 不是很冗长，这使得它在网络上传输时更加轻松，解析和加载也更快。

1.  结构：它是结构化的……
