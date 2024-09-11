# mypy介绍

> 原文：[https://towardsdatascience.com/introduction-to-mypy-3d32fc96db75?source=collection_archive---------9-----------------------#2023-04-06](https://towardsdatascience.com/introduction-to-mypy-3d32fc96db75?source=collection_archive---------9-----------------------#2023-04-06)

## Python的静态类型检查

[](https://medium.com/@hrmnmichaels?source=post_page-----3d32fc96db75--------------------------------)[![Oliver S](../Images/b5ee0fa2d5fb115f62e2e9dfcb92afdd.png)](https://medium.com/@hrmnmichaels?source=post_page-----3d32fc96db75--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3d32fc96db75--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----3d32fc96db75--------------------------------) [Oliver S](https://medium.com/@hrmnmichaels?source=post_page-----3d32fc96db75--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff2daf6260cca&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroduction-to-mypy-3d32fc96db75&user=Oliver+S&userId=f2daf6260cca&source=post_page-f2daf6260cca----3d32fc96db75---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----3d32fc96db75--------------------------------) ·8分钟阅读·2023年4月6日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F3d32fc96db75&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroduction-to-mypy-3d32fc96db75&user=Oliver+S&userId=f2daf6260cca&source=-----3d32fc96db75---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F3d32fc96db75&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroduction-to-mypy-3d32fc96db75&source=-----3d32fc96db75---------------------bookmark_footer-----------)![](../Images/1256da4cc8ec98ef8ebafde56c0e759c.png)

照片由[Agence Olloweb](https://unsplash.com/@olloweb?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)拍摄，发布于[Unsplash](https://unsplash.com/photos/d9ILr-dbEdg?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

我们在之前关于[Python最佳实践](https://medium.com/towards-data-science/best-practices-for-python-development-bf74c2880f87)的文章中提到了mypy作为一个必备工具——在这里，我们将更详细地介绍它。

[mypy](https://mypy.readthedocs.io/en/stable/)，如文档所述，是“Python 的静态类型检查器”。这意味着，它为设计上动态类型的 Python 语言添加类型注解和检查（与例如 C++ 相对，类型是在运行时推断的）。这样可以在编译时发现代码中的错误，这对任何半专业的 Python 项目都是极大的帮助——正如我在上一篇文章中解释的那样。

在这篇文章中，我们将通过几个示例介绍 mypy。免责声明：这篇文章不会介绍 mypy 的所有功能（甚至远远不及）。相反，我会尝试在足够的细节与让你几乎能够编写所有需要的代码之间找到一个好的平衡——同时避免从零开始到扎实理解 mypy 的陡峭学习曲线。有关更多细节，我建议参考官方文档或其他优秀教程。

# 安装

要安装 mypy，只需运行：`pip install mypy`

不过，我建议使用某种形式的依赖管理系统，例如 [poetry](https://medium.com/towards-data-science/dependency-management-with-poetry-f1d598591161)。如何在较大的软件项目中包含此工具和 mypy 的方法，请参见 [这里](https://medium.com/towards-data-science/best-practices-for-python-development-bf74c2880f87)。

# 第一步骤
