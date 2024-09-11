# 在Panel中构建互动式ML仪表盘

> 原文：[https://towardsdatascience.com/building-an-interactive-ml-dashboard-in-panel-d3e344ea7126?source=collection_archive---------15-----------------------#2023-06-06](https://towardsdatascience.com/building-an-interactive-ml-dashboard-in-panel-d3e344ea7126?source=collection_archive---------15-----------------------#2023-06-06)

[](https://sophiamyang.medium.com/?source=post_page-----d3e344ea7126--------------------------------)[![Sophia Yang, Ph.D.](../Images/c133f918245ea4857dc46df3a07fc2b1.png)](https://sophiamyang.medium.com/?source=post_page-----d3e344ea7126--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d3e344ea7126--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----d3e344ea7126--------------------------------) [Sophia Yang, Ph.D.](https://sophiamyang.medium.com/?source=post_page-----d3e344ea7126--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fae9cae9cbcd2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-an-interactive-ml-dashboard-in-panel-d3e344ea7126&user=Sophia+Yang%2C+Ph.D.&userId=ae9cae9cbcd2&source=post_page-ae9cae9cbcd2----d3e344ea7126---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d3e344ea7126--------------------------------) ·6分钟阅读·2023年6月6日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fd3e344ea7126&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-an-interactive-ml-dashboard-in-panel-d3e344ea7126&user=Sophia+Yang%2C+Ph.D.&userId=ae9cae9cbcd2&source=-----d3e344ea7126---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd3e344ea7126&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-an-interactive-ml-dashboard-in-panel-d3e344ea7126&source=-----d3e344ea7126---------------------bookmark_footer-----------)

作者：[Andrew Huang](https://www.linkedin.com/in/huangandrew12/)、[Sophia Yang](https://www.linkedin.com/in/sophiamyang/)、[Philipp Rudiger](https://www.linkedin.com/in/philippjfr/)

![](../Images/785daab09c0004e8911ae91fc0d18f44.png)

图像分类应用的演示。

HoloViz Panel 是一个多功能的 Python 库，能够使开发者和数据科学家轻松构建交互式可视化。无论你是在进行机器学习项目、开发 web 应用程序，还是设计数据仪表板，Panel 提供了一套强大的工具和功能，以提升你的数据探索和呈现能力。在这篇博客文章中，我们将深入探讨 HoloViz Panel 的激动人心的功能，探索它如何彻底改变你的数据可视化工作流程，并演示如何用大约 100 行代码构建类似的应用程序。

尝试一下这个应用程序，并查看代码：

+   [Hugging Face 空间](https://huggingface.co/spaces/Panel-Org/panel-template)

+   [Anaconda 上的应用程序](https://heroic-milk-snake.anacondaapps.cloud/test-image-classification)

+   [Hugging Face 上的应用程序](https://panel-org-panel-template.hf.space/app)

+   [Anaconda Notebook 上的代码](https://anaconda.cloud/api/nbserve/launch_notebook?nb_url=https%3A%2F%2Fanaconda.cloud%2Fapi%2Fprojects%2F7c7966e4-0696-4900-b148-7bbd13543c15%2Fversions%2F81e7ac6b-cd37-4f0d-9626-19c05845d3f3%2Ffiles%2Ftest-image-classification.ipynb)

+   [Hugging Face 上的代码](https://huggingface.co/spaces/Panel-Org/panel-template/blob/main/app.py)

# 充分利用 ML/AI 的力量

ML/AI 已成为数据分析和决策过程中的不可或缺的一部分。通过 Panel，你可以将 ML 模型和结果无缝集成到你的可视化中。在这篇博客文章中，我们将探讨如何使用 OpenAI CLIP 模型进行图像分类任务。
