# 介绍 Python 的 `parse`：**正则表达式的终极替代方案**

> 原文：[`towardsdatascience.com/introducing-pythons-parse-the-ultimate-alternative-to-regular-expressions-3ae07e51b753?source=collection_archive---------1-----------------------#2023-06-19`](https://towardsdatascience.com/introducing-pythons-parse-the-ultimate-alternative-to-regular-expressions-3ae07e51b753?source=collection_archive---------1-----------------------#2023-06-19)

## [PYTHON TOOLBOX](https://medium.com/@qtalen/list/python-toolbox-4289824c6407)

## 使用最佳实践和实际案例展示强大的文本解析库

[](https://qtalen.medium.com/?source=post_page-----3ae07e51b753--------------------------------)![Peng Qian](https://qtalen.medium.com/?source=post_page-----3ae07e51b753--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3ae07e51b753--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----3ae07e51b753--------------------------------) [Peng Qian](https://qtalen.medium.com/?source=post_page-----3ae07e51b753--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8e2fe735546d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroducing-pythons-parse-the-ultimate-alternative-to-regular-expressions-3ae07e51b753&user=Peng+Qian&userId=8e2fe735546d&source=post_page-8e2fe735546d----3ae07e51b753---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----3ae07e51b753--------------------------------) · 7 分钟阅读 · 2023 年 6 月 19 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F3ae07e51b753&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroducing-pythons-parse-the-ultimate-alternative-to-regular-expressions-3ae07e51b753&user=Peng+Qian&userId=8e2fe735546d&source=-----3ae07e51b753---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F3ae07e51b753&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroducing-pythons-parse-the-ultimate-alternative-to-regular-expressions-3ae07e51b753&source=-----3ae07e51b753---------------------bookmark_footer-----------)![](img/42ed2c770be414428f5dcde02d4033ad.png)

[parse](https://pypi.org/project/parse/) 库非常易于使用。照片由 [Amanda Jones](https://unsplash.com/@amandagraphc?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

这篇文章介绍了一个名为 `parse` 的 Python 库，用于快速便捷地解析和提取文本数据，是 Python 正则表达式的一个极佳替代方案。

其中涵盖了使用`parse`库的最佳实践以及解析 nginx 日志文本的实际示例。

# 介绍

我有一个同事叫王。有一天，他带着忧虑的表情来到我面前，说他遇到了一个复杂的问题：他的老板希望他分析过去一个月的服务器日志，并提供访客流量的统计数据。

我告诉他这很简单。只需使用正则表达式。例如，要分析 nginx 日志，请使用以下正则表达式，这很基础。

```py
content:
192.168.0.2 - - [04/Jan/2019:16:06:38 +0800] "GET http://example.aliyundoc.com/_astats?application=&inf.name=eth0 HTTP/1.1" 200 273932
regular expression:
(?<ip>\d+\.\d+\.\d+\.\d+)( - - \[)(?<datetime>[\s\S]+)(?<t1>\][\s"]+)(?<request>[A-Z]+) (?<url>[\S]*) (?<protocol>[\S]+)["] (?<code>\d+)…
```
