# 利用 GPT-3 优化文档编写

> 原文：[`towardsdatascience.com/streamline-your-documentation-with-gpt-3-5d9f2bbf217c?source=collection_archive---------10-----------------------#2023-01-16`](https://towardsdatascience.com/streamline-your-documentation-with-gpt-3-5d9f2bbf217c?source=collection_archive---------10-----------------------#2023-01-16)

## 使用人工智能生成 Python 文档字符串

[](https://mikhailklassen.medium.com/?source=post_page-----5d9f2bbf217c--------------------------------)![Mikhail Klassen](https://mikhailklassen.medium.com/?source=post_page-----5d9f2bbf217c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5d9f2bbf217c--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----5d9f2bbf217c--------------------------------) [Mikhail Klassen](https://mikhailklassen.medium.com/?source=post_page-----5d9f2bbf217c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fd9dfda9f9153&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fstreamline-your-documentation-with-gpt-3-5d9f2bbf217c&user=Mikhail+Klassen&userId=d9dfda9f9153&source=post_page-d9dfda9f9153----5d9f2bbf217c---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5d9f2bbf217c--------------------------------) · 6 分钟阅读 · 2023 年 1 月 16 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F5d9f2bbf217c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fstreamline-your-documentation-with-gpt-3-5d9f2bbf217c&user=Mikhail+Klassen&userId=d9dfda9f9153&source=-----5d9f2bbf217c---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5d9f2bbf217c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fstreamline-your-documentation-with-gpt-3-5d9f2bbf217c&source=-----5d9f2bbf217c---------------------bookmark_footer-----------)![](img/a6660b6f88943c3032a35dca7d0ba80a.png)

图像使用 DALL-E 生成，提示词为：“一个机器人在计算机终端检查代码。赛博朋克风格，现实主义。”

GPT-3 是由 OpenAI 开发的最新语言模型，能够生成类似人类的文本，使其成为各种自然语言处理任务的强大工具。

该模型也经过了编程语言的训练。其中一个应用场景是生成 Python 函数的文档字符串。

# 什么是文档字符串？

文档字符串（docstring）是出现在 Python 函数第一行的字符串。它提供了函数及其输入和输出的简要描述，使其他开发者更容易理解和使用代码。编写清晰且信息丰富的文档字符串可能非常耗时和乏味，特别是对于具有许多函数的大型项目。

```py
# A docstring example
def square(n):
    """Takes in a number n and returns the square of n"""
    return n**2
```

GPT-3 可以通过提供 Python 函数的源代码并使用一个有帮助的提示来生成文档字符串。

## 示例：计算数字列表的平均值

假设你写了一个用于计算数字列表平均值的函数…
