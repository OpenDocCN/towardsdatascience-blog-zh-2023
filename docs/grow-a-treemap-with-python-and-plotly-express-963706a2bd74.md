# 使用 Python 和 Plotly Express 创建树形图

> 原文：[`towardsdatascience.com/grow-a-treemap-with-python-and-plotly-express-963706a2bd74?source=collection_archive---------5-----------------------#2023-04-07`](https://towardsdatascience.com/grow-a-treemap-with-python-and-plotly-express-963706a2bd74?source=collection_archive---------5-----------------------#2023-04-07)

## 将政府 PDF 转换为财务规划工具

[](https://medium.com/@lee_vaughan?source=post_page-----963706a2bd74--------------------------------)![Lee Vaughan](https://medium.com/@lee_vaughan?source=post_page-----963706a2bd74--------------------------------)[](https://towardsdatascience.com/?source=post_page-----963706a2bd74--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----963706a2bd74--------------------------------) [Lee Vaughan](https://medium.com/@lee_vaughan?source=post_page-----963706a2bd74--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F5d604015c08b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgrow-a-treemap-with-python-and-plotly-express-963706a2bd74&user=Lee+Vaughan&userId=5d604015c08b&source=post_page-5d604015c08b----963706a2bd74---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----963706a2bd74--------------------------------) · 8 分钟阅读 · 2023 年 4 月 7 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F963706a2bd74&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgrow-a-treemap-with-python-and-plotly-express-963706a2bd74&user=Lee+Vaughan&userId=5d604015c08b&source=-----963706a2bd74---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F963706a2bd74&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgrow-a-treemap-with-python-and-plotly-express-963706a2bd74&source=-----963706a2bd74---------------------bookmark_footer-----------)![](img/b3a0fa3113923d8d9c7a5a3b1e3e8827.png)

由 Robert Murray 拍摄，图片来源于 Unsplash！

*层级* 数据是一种数据模型，其中项目通过父子关系相互关联，形成树状结构。一些明显的例子是家谱和公司组织结构图。

*树形图* 是一种使用嵌套矩形来表示层级数据的图示。每个矩形的面积与其数值对应。树形图已经存在约 30 年了。早期应用之一是用于可视化硬盘使用情况，如下图所示。

![](img/5fbfa693cbf1cf386397aaca4297ed39.png)

硬盘空间的分配通过树图进行可视化（[Carnivore1973 via Wikimedia Commons](https://commons.wikimedia.org/w/index.php?search=Carnivore1973&title=Special%3AMediaSearch&go=Go&type=image)）。

树图让你能够同时捕捉到单个类别的*值*和层次结构的*结构*。它们适用于：

+   当类别数量超过条形图时，显示层次结构数据。

+   突出显示各个类别与整体之间的比例。

+   使用不同的大小和颜色来区分类别。

+   发现模式、主要贡献者和异常值。

+   为数据可视化带来新视角。
