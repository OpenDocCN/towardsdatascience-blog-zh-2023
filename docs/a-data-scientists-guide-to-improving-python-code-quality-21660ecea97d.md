# 数据科学家改进 Python 代码质量指南

> 原文：[`towardsdatascience.com/a-data-scientists-guide-to-improving-python-code-quality-21660ecea97d?source=collection_archive---------1-----------------------#2023-08-03`](https://towardsdatascience.com/a-data-scientists-guide-to-improving-python-code-quality-21660ecea97d?source=collection_archive---------1-----------------------#2023-08-03)

## 编写生产级 Python 代码的工具和包

[](https://medium.com/@egorhowell?source=post_page-----21660ecea97d--------------------------------)![Egor Howell](https://medium.com/@egorhowell?source=post_page-----21660ecea97d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----21660ecea97d--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----21660ecea97d--------------------------------) [Egor Howell](https://medium.com/@egorhowell?source=post_page-----21660ecea97d--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1cac491223b2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-data-scientists-guide-to-improving-python-code-quality-21660ecea97d&user=Egor+Howell&userId=1cac491223b2&source=post_page-1cac491223b2----21660ecea97d---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----21660ecea97d--------------------------------) ·6 分钟阅读·2023 年 8 月 3 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F21660ecea97d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-data-scientists-guide-to-improving-python-code-quality-21660ecea97d&user=Egor+Howell&userId=1cac491223b2&source=-----21660ecea97d---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F21660ecea97d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-data-scientists-guide-to-improving-python-code-quality-21660ecea97d&source=-----21660ecea97d---------------------bookmark_footer-----------)![](img/c47a9cda8d76910dc96373aa3ca5deb2.png)

图片由 [Christopher Gower](https://unsplash.com/@cgower?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 背景

如今，数据科学家越来越多地参与到机器学习模型的生产部署中。这意味着我们需要能够像其他软件工程师一样编写生产标准的 Python 代码。在这篇文章中，我想探讨一些关键工具和包，这些工具和包可以帮助你为下一个模型创建值得生产的代码。

# Linter 工具

## 概述

[**代码检查工具**](https://en.wikipedia.org/wiki/Lint_%28software%29) 是一种可以捕捉小错误、格式错误以及可能导致运行时问题和意外输出的奇怪设计模式的工具。

在 Python 中，我们有 [**PEP8**](https://pep8.org/)，它为我们的代码格式提供了一个全球统一的风格指南。Python 中有许多遵循 PEP8 的代码检查工具，但我个人偏好 [**flake8**](https://flake8.pycqa.org/en/latest/)。

## Flake8

Flake8 实际上是 [**Pyflakes**](https://github.com/PyCQA/pyflakes)、[**pycodestyle**](https://github.com/pycqa/pycodestyle) 和 [**McCabe**](https://github.com/PyCQA/mccabe) 这些代码检查包的组合。它检查错误、[**代码异味**](https://en.wikipedia.org/wiki/Code_smell) 并强制执行 PEP8 标准。

要安装 flake8，请使用 `pip install flake8`，然后通过 `flake8 <file_name.py>` 使用它。确实就是这么简单！
