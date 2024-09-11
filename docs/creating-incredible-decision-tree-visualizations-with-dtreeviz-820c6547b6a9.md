# 使用 dtreeviz 创建令人难以置信的决策树可视化

> 原文：[`towardsdatascience.com/creating-incredible-decision-tree-visualizations-with-dtreeviz-820c6547b6a9?source=collection_archive---------2-----------------------#2023-06-21`](https://towardsdatascience.com/creating-incredible-decision-tree-visualizations-with-dtreeviz-820c6547b6a9?source=collection_archive---------2-----------------------#2023-06-21)

## 如何使用这个有用的库来可视化决策树模型

[](https://amolmavuduru.medium.com/?source=post_page-----820c6547b6a9--------------------------------)![Amol Mavuduru](https://amolmavuduru.medium.com/?source=post_page-----820c6547b6a9--------------------------------)[](https://towardsdatascience.com/?source=post_page-----820c6547b6a9--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----820c6547b6a9--------------------------------) [Amol Mavuduru](https://amolmavuduru.medium.com/?source=post_page-----820c6547b6a9--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F511816e5976d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreating-incredible-decision-tree-visualizations-with-dtreeviz-820c6547b6a9&user=Amol+Mavuduru&userId=511816e5976d&source=post_page-511816e5976d----820c6547b6a9---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----820c6547b6a9--------------------------------) · 6 分钟阅读 · 2023 年 6 月 21 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F820c6547b6a9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreating-incredible-decision-tree-visualizations-with-dtreeviz-820c6547b6a9&user=Amol+Mavuduru&userId=511816e5976d&source=-----820c6547b6a9---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F820c6547b6a9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreating-incredible-decision-tree-visualizations-with-dtreeviz-820c6547b6a9&source=-----820c6547b6a9---------------------bookmark_footer-----------)![](img/18a9ad0cc89c82cf3b9dbf37c38220eb.png)

作者提供的图片，使用 dtreeviz 创建。

当涉及到模型可解释性时，决策树是一些最直观且可解释的模型。每个决策树模型都可以解释为一组人类可理解的规则。能够可视化决策树模型对于模型的可解释性很重要，并且可以帮助利益相关者和业务经理对这些模型建立信任。

幸运的是，我们可以使用 dtreeviz 库轻松地可视化和解释决策树。**在本文中，我将演示如何使用 dtreeviz 来可视化回归和分类的树模型。**

# 安装 dtreeviz

您可以使用以下命令通过 pip 轻松安装 dtreeviz：

```py
pip install dtreeviz
```

对于详细的依赖项列表和可能需要根据操作系统安装的额外库，请参考[这个 GitHub 仓库](https://github.com/parrt/dtreeviz)。

# 可视化回归树

在这一部分，我们将使用[糖尿病数据集](https://archive.ics.uci.edu/dataset/34/diabetes)训练一个决策树回归模型。请注意，您可以在这个 GitHub 仓库中找到本教程的所有代码。保持…
