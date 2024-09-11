# 增强 Python 文档：链接源代码的逐步指南

> 原文：[`towardsdatascience.com/enhancing-python-documentation-a-step-by-step-guide-to-linking-source-code-9da102b2bb2a?source=collection_archive---------8-----------------------#2023-12-07`](https://towardsdatascience.com/enhancing-python-documentation-a-step-by-step-guide-to-linking-source-code-9da102b2bb2a?source=collection_archive---------8-----------------------#2023-12-07)

[](https://piskunow.medium.com/?source=post_page-----9da102b2bb2a--------------------------------)![Pablo Piskunow](https://piskunow.medium.com/?source=post_page-----9da102b2bb2a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9da102b2bb2a--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----9da102b2bb2a--------------------------------) [Pablo Piskunow](https://piskunow.medium.com/?source=post_page-----9da102b2bb2a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2860e02af738&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fenhancing-python-documentation-a-step-by-step-guide-to-linking-source-code-9da102b2bb2a&user=Pablo+Piskunow&userId=2860e02af738&source=post_page-2860e02af738----9da102b2bb2a---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----9da102b2bb2a--------------------------------) ·4 分钟阅读·2023 年 12 月 7 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F9da102b2bb2a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fenhancing-python-documentation-a-step-by-step-guide-to-linking-source-code-9da102b2bb2a&user=Pablo+Piskunow&userId=2860e02af738&source=-----9da102b2bb2a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F9da102b2bb2a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fenhancing-python-documentation-a-step-by-step-guide-to-linking-source-code-9da102b2bb2a&source=-----9da102b2bb2a---------------------bookmark_footer-----------)

> 你已经阅读了这个类方法的描述，但仍然不理解发生了什么。要是你能快速阅读源代码就好了…

# 缩小文档与代码之间的差距：简化 Python 学习

Python 的强大不仅在于它的简单性和效率，还在于它庞大的社区和丰富的文档。但如果你能让这些文档变得更加互动和信息丰富，会怎么样呢？今天，我将指导你如何通过直接链接到 GitHub 上对应的源代码来增强你由 Sphinx 生成的 Python 文档。

![](img/f1ac9810e77866bfc357b8a03fa99355.png)

作者使用 Dalle-3 创建的图片。提示：“抽象图像，浅奶油色画布上的黑色水彩，展示了与机器内部工作相关的手写片段，并用细箭头连接。”

# 步骤 1：使用 Sphinx 进行文档编写

当我们在 Python 代码中编写恰当的 docstring 时，我们为生成全面的 API 文档奠定了基础。像 Sphinx 的 `autodoc` 和 `automodule` 这样的工具非常适合从我们的模块、类和函数中提取这些 docstring。但是，它们通常在提供直接链接到源代码方面有所不足。

如果你需要开始使用 Sphinx，请查看这些教程：
