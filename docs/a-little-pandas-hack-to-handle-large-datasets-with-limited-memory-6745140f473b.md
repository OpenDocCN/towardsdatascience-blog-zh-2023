# 一个小巧的 Pandas 黑客，用于在内存有限的情况下处理大型数据集

> 原文：[`towardsdatascience.com/a-little-pandas-hack-to-handle-large-datasets-with-limited-memory-6745140f473b?source=collection_archive---------3-----------------------#2023-01-19`](https://towardsdatascience.com/a-little-pandas-hack-to-handle-large-datasets-with-limited-memory-6745140f473b?source=collection_archive---------3-----------------------#2023-01-19)

## Pandas 的默认设置并不理想。一个小的配置可以将你的数据框压缩到适合你的内存中。

[](https://thuwarakesh.medium.com/?source=post_page-----6745140f473b--------------------------------)![Thuwarakesh Murallie](https://thuwarakesh.medium.com/?source=post_page-----6745140f473b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6745140f473b--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----6745140f473b--------------------------------) [Thuwarakesh Murallie](https://thuwarakesh.medium.com/?source=post_page-----6745140f473b--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F93ce19993bef&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-little-pandas-hack-to-handle-large-datasets-with-limited-memory-6745140f473b&user=Thuwarakesh+Murallie&userId=93ce19993bef&source=post_page-93ce19993bef----6745140f473b---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6745140f473b--------------------------------) ·8 分钟阅读·2023 年 1 月 19 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F6745140f473b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-little-pandas-hack-to-handle-large-datasets-with-limited-memory-6745140f473b&user=Thuwarakesh+Murallie&userId=93ce19993bef&source=-----6745140f473b---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F6745140f473b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-little-pandas-hack-to-handle-large-datasets-with-limited-memory-6745140f473b&source=-----6745140f473b---------------------bookmark_footer-----------)![](img/c4b1299e06f03069b94110ff87456e73.png)

你可以在不丧失属性的情况下压缩一个巨大的 Pandas 数据框，就像挤压汉堡一样。通过这个小技巧节省内存，提高工作效率。 — 图片来自 [Leonardo Luz](https://www.pexels.com/photo/photo-of-a-burger-between-a-person-s-hands-14001304/)

我从未认为我的代码需要改进。我总是抱怨内存不足或数据集太大而无法处理。

我通常的解决方案是将它们放在 Postgres 数据库中并编写 SQL 查询。毕竟，这是一种处理大规模数据集的可接受方式。每当我得到一个大型数据集时，我就会这样做。

但我无法获得在 Python 中得到的完全灵活性。出于这个原因，我不得不将它们结合起来并交替使用。例如，我将数据集加载到 SQL 数据库中，并编写 Python 脚本来运行 SQL 查询，通常使用 Sqlalchemy。

尽管这种方法让我兼具两方面的灵活性，但我在与其他团队成员共享代码时遇到了问题。其他成员需要具备关系数据库的知识和设置。

这个问题的经典解决方案是增加内存并并行运行任务。在基础设施方面，迁移到云端是我最喜欢的解决方案。我可以按使用量为高性能资源付费。在并行执行方面，像[Dask](https://www.dask.org/)这样的技术可以满足我的需求。
