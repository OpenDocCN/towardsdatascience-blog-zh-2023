# 什么是 Python 中的 pyproject.toml

> 原文：[`towardsdatascience.com/pyproject-python-9df8cc092f61?source=collection_archive---------4-----------------------#2023-05-09`](https://towardsdatascience.com/pyproject-python-9df8cc092f61?source=collection_archive---------4-----------------------#2023-05-09)

## 在 pyproject.toml 文件中管理 Python 项目依赖

[](https://gmyrianthous.medium.com/?source=post_page-----9df8cc092f61--------------------------------)![Giorgos Myrianthous](https://gmyrianthous.medium.com/?source=post_page-----9df8cc092f61--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9df8cc092f61--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----9df8cc092f61--------------------------------) [Giorgos Myrianthous](https://gmyrianthous.medium.com/?source=post_page-----9df8cc092f61--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F76c21e75463a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpyproject-python-9df8cc092f61&user=Giorgos+Myrianthous&userId=76c21e75463a&source=post_page-76c21e75463a----9df8cc092f61---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----9df8cc092f61--------------------------------) ·5 分钟阅读·2023 年 5 月 9 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F9df8cc092f61&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpyproject-python-9df8cc092f61&user=Giorgos+Myrianthous&userId=76c21e75463a&source=-----9df8cc092f61---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F9df8cc092f61&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpyproject-python-9df8cc092f61&source=-----9df8cc092f61---------------------bookmark_footer-----------)![](img/f52d5d1a92753c16cb2f548c0ac9aecd.png)

[Fré Sonneveld](https://unsplash.com/@fresonneveld?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)拍摄的照片，来源于[Unsplash](https://unsplash.com/s/photos/chain?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在 Python 中进行依赖管理是复杂且有时令人沮丧的工作。新手通常倾向于安装他们认为有用的任何依赖（即包），即使是在一个虚拟环境中。因此，这种方法增加了包依赖冲突的可能性，从而陷入所谓的**依赖地狱**。

在我之前的几篇文章中，我们讨论了几种不同的方法来处理 Python 项目的依赖，使用了 `setup.py`、`setup.cfg` 和 `requirements.txt` 文件。然而，从 Python 3.6 开始，推出了一种新的标准配置文件 `pyproject.toml`，旨在简化用户管理依赖和元数据定义的方式。

在过去几年中，`pyproject.toml` 文件已成为管理 Python 项目依赖的标准（也是最流行）方法。在接下来的几节中，我们将深入探讨如何使用此文件实现依赖管理。此外，我们还将演示如何以可编辑模式安装带有 `pyproject.toml` 规范的项目。

[**订阅数据管道**](https://thedatapipeline.substack.com/welcome)**，这是一个专注于数据工程的新闻通讯**

## pyproject.toml 之前的依赖管理
