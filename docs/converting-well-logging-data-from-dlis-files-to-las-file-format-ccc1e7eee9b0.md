# 将井下测录数据从 DLIS 文件格式转换为 LAS 文件格式

> 原文：[`towardsdatascience.com/converting-well-logging-data-from-dlis-files-to-las-file-format-ccc1e7eee9b0?source=collection_archive---------19-----------------------#2023-07-25`](https://towardsdatascience.com/converting-well-logging-data-from-dlis-files-to-las-file-format-ccc1e7eee9b0?source=collection_archive---------19-----------------------#2023-07-25)

## 与地球科学和岩石物理数据文件格式的工作

[](https://andymcdonaldgeo.medium.com/?source=post_page-----ccc1e7eee9b0--------------------------------)![Andy McDonald](https://andymcdonaldgeo.medium.com/?source=post_page-----ccc1e7eee9b0--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ccc1e7eee9b0--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----ccc1e7eee9b0--------------------------------) [Andy McDonald](https://andymcdonaldgeo.medium.com/?source=post_page-----ccc1e7eee9b0--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9c280f85f15c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fconverting-well-logging-data-from-dlis-files-to-las-file-format-ccc1e7eee9b0&user=Andy+McDonald&userId=9c280f85f15c&source=post_page-9c280f85f15c----ccc1e7eee9b0---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ccc1e7eee9b0--------------------------------) ·8 分钟阅读·2023 年 7 月 25 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fccc1e7eee9b0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fconverting-well-logging-data-from-dlis-files-to-las-file-format-ccc1e7eee9b0&user=Andy+McDonald&userId=9c280f85f15c&source=-----ccc1e7eee9b0---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fccc1e7eee9b0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fconverting-well-logging-data-from-dlis-files-to-las-file-format-ccc1e7eee9b0&source=-----ccc1e7eee9b0---------------------bookmark_footer-----------)![](img/69d4e3da04b4d0c32395107fa95d681e.png)

图片由 [Mika Baumeister](https://unsplash.com/pt-br/@mbaumi?utm_source=medium&utm_medium=referral) 提供，刊登于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在石油和天然气行业的地球科学领域，使用多种格式来存储[井下测录](https://en.wikipedia.org/wiki/Well_logging)和[岩石物理](https://en.wikipedia.org/wiki/Petrophysics)数据。最常见的两种格式是 LAS 文件和 DLIS 文件。

[LAS](https://en.wikipedia.org/wiki/Log_ASCII_standard) 文件是平面 ASCII 文件，可以使用任何文本编辑器轻松读取，而 DLIS 文件是结构化的二进制文件，包含有关测井环境及测井数据的表格信息。DLIS 文件处理起来要困难得多，不能轻易在文本编辑器中打开，这可能会妨碍理解文件中的内容。

幸运的是，已经开发了一些很棒的 Python 库，使得从 LAS 和 DLIS 文件中访问数据变得更加容易。

[lasio](https://github.com/kinverarity1/lasio) 是一个旨在轻松读取和处理 LAS 文件的库，即使这些文件存在由于格式不正确或其他错误导致的问题，也能有效处理。

[dlsio](https://github.com/equinor/dlisio) 是 Equinor ASA 开发的一个 Python 库，用于读取 DLIS 文件和日志信息标准 79 (LIS79) 文件。该库旨在减少在没有详细了解 DLIS 结构的情况下探索这些文件的负担和工作量。
