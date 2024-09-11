# Makefile 教程

> 原文：[https://towardsdatascience.com/a-data-scientists-guide-to-make-and-makefiles-1595f39e0704?source=collection_archive---------6-----------------------#2023-08-11](https://towardsdatascience.com/a-data-scientists-guide-to-make-and-makefiles-1595f39e0704?source=collection_archive---------6-----------------------#2023-08-11)

## 如何使用 Make 和 Makefiles 来优化你的机器学习管道

[](https://medium.com/@egorhowell?source=post_page-----1595f39e0704--------------------------------)[![Egor Howell](../Images/1f796e828f1625440467d01dcc3e40cd.png)](https://medium.com/@egorhowell?source=post_page-----1595f39e0704--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1595f39e0704--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----1595f39e0704--------------------------------) [Egor Howell](https://medium.com/@egorhowell?source=post_page-----1595f39e0704--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1cac491223b2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-data-scientists-guide-to-make-and-makefiles-1595f39e0704&user=Egor+Howell&userId=1cac491223b2&source=post_page-1cac491223b2----1595f39e0704---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1595f39e0704--------------------------------) · 5 分钟阅读 · 2023 年 8 月 11 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F1595f39e0704&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-data-scientists-guide-to-make-and-makefiles-1595f39e0704&user=Egor+Howell&userId=1cac491223b2&source=-----1595f39e0704---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F1595f39e0704&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-data-scientists-guide-to-make-and-makefiles-1595f39e0704&source=-----1595f39e0704---------------------bookmark_footer-----------)![](../Images/df1db4763ccc19619b1d790059ed64f7.png)

照片由 [Nubelson Fernandes](https://unsplash.com/@nublson?utm_source=medium&utm_medium=referral) 提供，发布在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 背景

现在数据科学家需要编写生产代码以部署他们的机器学习算法。因此，我们需要了解软件工程标准和方法，以确保我们的模型能够稳健且有效地部署。在开发者社区中，有一个非常知名的工具是`make`。这是一个强大的 Linux 命令，开发者们已经使用了很长时间。在这篇文章中，我想展示如何使用它来构建高效的机器学习管道。

# 什么是 Make？

`make` 是一个终端命令/可执行文件，就像 `ls` 或 `cd` 一样，存在于大多数类 UNIX 操作系统中，如 MacOS 和 Linux。

`make` 的使用是为了简化并将你的工作流程分解为一组逻辑上的 shell 命令。

它被开发人员广泛使用，并且数据科学家也在采用，因为它简化了机器学习流程并支持更可靠的生产部署。

# 为什么数据科学要使用 Make？

`make` 是一个强大的工具，数据科学家应当利用它，原因如下：

+   *自动化机器设置*…
