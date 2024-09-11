# 如何使用参数解析来提高机器学习工作流的效率

> 原文：[https://towardsdatascience.com/how-to-use-argument-parsing-for-greater-efficiency-in-machine-learning-workflows-2f637eaf5f6a?source=collection_archive---------14-----------------------#2023-03-29](https://towardsdatascience.com/how-to-use-argument-parsing-for-greater-efficiency-in-machine-learning-workflows-2f637eaf5f6a?source=collection_archive---------14-----------------------#2023-03-29)

## 关于如何使用`argparse`来处理命令行应用程序的完整指南，以及它如何在机器学习项目中得到应用。

[](https://thomasdorfer.medium.com/?source=post_page-----2f637eaf5f6a--------------------------------)[![Thomas A Dorfer](../Images/9258a1735cee805f1d9b02e2adf01096.png)](https://thomasdorfer.medium.com/?source=post_page-----2f637eaf5f6a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2f637eaf5f6a--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----2f637eaf5f6a--------------------------------) [Thomas A Dorfer](https://thomasdorfer.medium.com/?source=post_page-----2f637eaf5f6a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7c54f9b62b90&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-use-argument-parsing-for-greater-efficiency-in-machine-learning-workflows-2f637eaf5f6a&user=Thomas+A+Dorfer&userId=7c54f9b62b90&source=post_page-7c54f9b62b90----2f637eaf5f6a---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----2f637eaf5f6a--------------------------------) · 6分钟阅读 · 2023年3月29日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F2f637eaf5f6a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-use-argument-parsing-for-greater-efficiency-in-machine-learning-workflows-2f637eaf5f6a&user=Thomas+A+Dorfer&userId=7c54f9b62b90&source=-----2f637eaf5f6a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F2f637eaf5f6a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-use-argument-parsing-for-greater-efficiency-in-machine-learning-workflows-2f637eaf5f6a&source=-----2f637eaf5f6a---------------------bookmark_footer-----------)![](../Images/69122e7dab880b86b587b9c89ca6fe94.png)

作者提供的图片。

如果你曾在数据科学或软件工程领域游历一段时间，你很可能遇到过需要使用命令行界面（CLI）的应用程序。常见的例子包括用于管理Azure资源的[Azure CLI](https://learn.microsoft.com/en-us/cli/azure/)或用于版本控制和源代码管理的[Git](https://git-scm.com/book/en/v2/Getting-Started-The-Command-Line)。

你自己的自定义Python应用程序也可以实现相同类型的功能和程序交互性。命令行参数是一个很好的工具，可以为你的应用程序提供必要的灵活性，使你和你的用户能够无缝地配置和自定义程序的行为。

一个流行且（或许）最常用的Python库用于解析命令行参数的是[**argparse**](https://docs.python.org/3/library/argparse.html)。在本文中，我们将深入探讨其核心功能，并通过具体示例详细了解如何高效地在Python应用程序中利用它们。

# argparse简介
