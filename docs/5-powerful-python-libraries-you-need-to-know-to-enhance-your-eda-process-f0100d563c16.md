# 5 个强大的 Python 库，助力你的探索性数据分析（EDA）

> 原文：[`towardsdatascience.com/5-powerful-python-libraries-you-need-to-know-to-enhance-your-eda-process-f0100d563c16?source=collection_archive---------3-----------------------#2023-02-15`](https://towardsdatascience.com/5-powerful-python-libraries-you-need-to-know-to-enhance-your-eda-process-f0100d563c16?source=collection_archive---------3-----------------------#2023-02-15)

## 利用 Python 的强大功能探索和理解你的数据

[](https://andymcdonaldgeo.medium.com/?source=post_page-----f0100d563c16--------------------------------)![Andy McDonald](https://andymcdonaldgeo.medium.com/?source=post_page-----f0100d563c16--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f0100d563c16--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----f0100d563c16--------------------------------) [Andy McDonald](https://andymcdonaldgeo.medium.com/?source=post_page-----f0100d563c16--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9c280f85f15c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F5-powerful-python-libraries-you-need-to-know-to-enhance-your-eda-process-f0100d563c16&user=Andy+McDonald&userId=9c280f85f15c&source=post_page-9c280f85f15c----f0100d563c16---------------------post_header-----------) 发表在 [数据科学前沿](https://towardsdatascience.com/?source=post_page-----f0100d563c16--------------------------------) · 10 分钟阅读 · 2023 年 2 月 15 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff0100d563c16&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F5-powerful-python-libraries-you-need-to-know-to-enhance-your-eda-process-f0100d563c16&user=Andy+McDonald&userId=9c280f85f15c&source=-----f0100d563c16---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff0100d563c16&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F5-powerful-python-libraries-you-need-to-know-to-enhance-your-eda-process-f0100d563c16&source=-----f0100d563c16---------------------bookmark_footer-----------)![](img/db53c469cf3830c20cea698f30724e15.png)

图片由 [Gerd Altmann](https://pixabay.com/users/geralt-9301/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=6511448) 提供，来源于 [Pixabay](https://pixabay.com//?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=6511448)

确保数据在运行机器学习模型之前的质量至关重要。如果我们将低质量的数据输入到这些模型中，可能会导致意想不到或不希望的结果。然而，对数据进行准备工作并尝试理解你拥有或缺少的内容是非常耗时的。[这个过程往往会消耗项目可用时间的高达 90%。](https://www.researchgate.net/publication/357867454_Data_Quality_Considerations_for_Petrophysical_Machine-Learning_Models)

如果你在 Python 中进行探索性数据分析（EDA），你会了解到常见的库，如 pandas、matplotlib 和 seaborn。它们都是很棒的库，但每个库都有自己的细微差别，这可能需要时间去学习或记住。

近年来，出现了几个强大的低代码 python 库，使得数据探索和分析阶段的项目变得更快更简单。

在本文中，我将介绍这 5 个 python 库，它们将提升你的数据分析工作流程。所有这些库都可以在 Jupyter notebook 环境中运行。

# 1\. YData Profiling（以前称为 Pandas Profiling）
