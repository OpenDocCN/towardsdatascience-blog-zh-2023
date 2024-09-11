# 如何在 R 中使用 DiagrammeR 包绘制图形

> 原文：[https://towardsdatascience.com/how-to-plot-graphs-using-the-diagrammer-package-in-r-3fe4642091cc?source=collection_archive---------13-----------------------#2023-02-15](https://towardsdatascience.com/how-to-plot-graphs-using-the-diagrammer-package-in-r-3fe4642091cc?source=collection_archive---------13-----------------------#2023-02-15)

## DiagrammeR, grViz()

[](https://indhumathychelliah.medium.com/?source=post_page-----3fe4642091cc--------------------------------)![Indhumathy Chelliah](../Images/5c8a238adb411a43f854953caf7d3e3a.png)](https://indhumathychelliah.medium.com/?source=post_page-----3fe4642091cc--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3fe4642091cc--------------------------------)![关注 Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----3fe4642091cc--------------------------------) [Indhumathy Chelliah](https://indhumathychelliah.medium.com/?source=post_page-----3fe4642091cc--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F720e3a4ac60c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-plot-graphs-using-the-diagrammer-package-in-r-3fe4642091cc&user=Indhumathy+Chelliah&userId=720e3a4ac60c&source=post_page-720e3a4ac60c----3fe4642091cc---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----3fe4642091cc--------------------------------) ·6 min read·Feb 15, 2023[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F3fe4642091cc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-plot-graphs-using-the-diagrammer-package-in-r-3fe4642091cc&user=Indhumathy+Chelliah&userId=720e3a4ac60c&source=-----3fe4642091cc---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F3fe4642091cc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-plot-graphs-using-the-diagrammer-package-in-r-3fe4642091cc&source=-----3fe4642091cc---------------------bookmark_footer-----------)![有关如何在 R 中使用 DiagrammeR 包绘制图形的文章图](../Images/80964a39441eb55c198a4dd56ff83581.png)

Lukas 拍摄的照片：[https://www.pexels.com/photo/person-holding-blue-pen-590014/](https://www.pexels.com/photo/person-holding-blue-pen-590014/)

DiagrammeR 是 R 中的一个包，它用于使用 graphviz 和 mermaid 创建图表和流程图。

在本文中，让我们看看如何使用 DiagrammeR 包中的 grViz() 创建图形。

1.  什么是 DiagrammeR?

1.  什么是 grViz()?

1.  如何使用 grViz() 创建节点、边缘、标签以及它们的替换和连接

1.  如何编织成 HTML

# 什么是 DiagrammeR

DiagrammeR 是 R 中 htmlwidgets 的一个包。它用于使用 graphviz 和 mermaid 库生成图形。

## 创建一个RMarkdown文档。

在RStudio中 -> 进入文件 → 新建文件 → R Markdown，然后输入标题并选择输出格式为HTML。

![](../Images/c3c92c437d98aab11c22d91b85c9e646.png)

R Markdown文档将在顶部显示文档标题和其他信息。
