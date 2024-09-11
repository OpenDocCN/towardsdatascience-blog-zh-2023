# D-Tale 用于快速和轻松的井日志数据探索性数据分析

> 原文：[https://towardsdatascience.com/d-tale-for-fast-and-easy-exploratory-data-analysis-of-well-log-data-a2ffca5295b6?source=collection_archive---------4-----------------------#2023-02-13](https://towardsdatascience.com/d-tale-for-fast-and-easy-exploratory-data-analysis-of-well-log-data-a2ffca5295b6?source=collection_archive---------4-----------------------#2023-02-13)

## 利用 D-Tale Python 库加速探索性数据分析工作流

[](https://andymcdonaldgeo.medium.com/?source=post_page-----a2ffca5295b6--------------------------------)[![安迪·麦克唐纳](../Images/df11d647be032aeb3d31852affb33a64.png)](https://andymcdonaldgeo.medium.com/?source=post_page-----a2ffca5295b6--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a2ffca5295b6--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----a2ffca5295b6--------------------------------) [安迪·麦克唐纳](https://andymcdonaldgeo.medium.com/?source=post_page-----a2ffca5295b6--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9c280f85f15c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fd-tale-for-fast-and-easy-exploratory-data-analysis-of-well-log-data-a2ffca5295b6&user=Andy+McDonald&userId=9c280f85f15c&source=post_page-9c280f85f15c----a2ffca5295b6---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a2ffca5295b6--------------------------------) ·9分钟阅读·2023年2月13日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa2ffca5295b6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fd-tale-for-fast-and-easy-exploratory-data-analysis-of-well-log-data-a2ffca5295b6&user=Andy+McDonald&userId=9c280f85f15c&source=-----a2ffca5295b6---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa2ffca5295b6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fd-tale-for-fast-and-easy-exploratory-data-analysis-of-well-log-data-a2ffca5295b6&source=-----a2ffca5295b6---------------------bookmark_footer-----------)![](../Images/69731dfd474498c1868f43295fad17d5.png)

图片来自 [Photo Mix](https://pixabay.com/users/photomix-company-1546875/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=1433427) [Pixabay](https://pixabay.com//?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=1433427)

探索性数据分析（EDA）可能是数据科学和机器学习工作流程中耗时但至关重要的部分。通过这一过程，我们熟悉数据集，了解其内容，概览数据的统计信息等等。许多项目中，我们在这一阶段花费了大量时间。[在某些情况下，这可能占用最多90%的项目时间](https://www.researchgate.net/publication/357867454_Data_Quality_Considerations_for_Petrophysical_Machine-Learning_Models)。

在Python中进行EDA时，我们通常依赖于pandas和matplotlib等库来探索数据。通常，这可能会导致编写大量代码来使图表按照我们想要的方式显示。例如，[使用matplotlib创建井位图需要时间来处理和显示数据。](https://medium.com/@andymcdonaldgeo/loading-and-displaying-well-log-data-b9568efd1d8)

目前有多个Python库可以显著加快项目的EDA阶段。其中之一是[D-Tale](https://pypi.org/project/dtale/)。

[D-Tale](https://pypi.org/project/dtale/)是一个强大的探索性数据分析Python库，使你能够互动地查看、分析和编辑pandas数据框中的数据。如果你想探索D-Tale的功能而不下载它……
