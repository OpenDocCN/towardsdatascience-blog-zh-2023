# 为什么你需要在 Python 中使用装饰器编写 DRY 代码

> 原文：[https://towardsdatascience.com/why-you-need-to-write-dry-code-with-decorators-in-python-3930ea23f569?source=collection_archive---------5-----------------------#2023-05-19](https://towardsdatascience.com/why-you-need-to-write-dry-code-with-decorators-in-python-3930ea23f569?source=collection_archive---------5-----------------------#2023-05-19)

## 数据

## 使用装饰器查看 Pandas 处理管道中数据发生了什么

[](https://byrondolon.medium.com/?source=post_page-----3930ea23f569--------------------------------)[![Byron Dolon](../Images/9ff32138c7b1913be24cc7ab971752b0.png)](https://byrondolon.medium.com/?source=post_page-----3930ea23f569--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3930ea23f569--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----3930ea23f569--------------------------------) [Byron Dolon](https://byrondolon.medium.com/?source=post_page-----3930ea23f569--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6b5d063df5dd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhy-you-need-to-write-dry-code-with-decorators-in-python-3930ea23f569&user=Byron+Dolon&userId=6b5d063df5dd&source=post_page-6b5d063df5dd----3930ea23f569---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----3930ea23f569--------------------------------) ·9 分钟阅读·2023年5月19日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F3930ea23f569&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhy-you-need-to-write-dry-code-with-decorators-in-python-3930ea23f569&user=Byron+Dolon&userId=6b5d063df5dd&source=-----3930ea23f569---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F3930ea23f569&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhy-you-need-to-write-dry-code-with-decorators-in-python-3930ea23f569&source=-----3930ea23f569---------------------bookmark_footer-----------)![](../Images/c0ac300ed86456b49bd3ab5a61fdcd44.png)

装饰器是关键 — 照片由[Adi Goldstein](https://unsplash.com/@adigold1?utm_source=medium&utm_medium=referral)拍摄，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

**DRY 代码是高效编程的关键之一。**

DRY 代表“不要重复自己”，这对于个人项目或大型代码库中的编程都非常重要。这意味着你应该接受 DRY 代码的概念，以创建模块化解决方案，而不是将逻辑从一个文件复制到另一个文件，甚至从一个函数复制到另一个函数。

使用 Python 中的装饰器可以帮助实现这一点。简而言之，装饰器允许你操作现有函数并扩展其功能。装饰器在许多不同场景中都很有用，但在这篇文章中，我们将探讨如何使用装饰器来改进 Pandas 数据处理管道。

装饰器可以帮助我们理解数据处理管道中每个阶段的情况，这在处理大规模数据时非常有用。你可以利用它们来跟踪数据的变化，这在对初始 DataFrame 应用不同转换时可能很难管理。

随意在你自己的笔记本中跟随记录。你可以下载数据集……
