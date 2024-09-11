# 新的 SHAP 图表：小提琴图和热图

> 原文：[https://towardsdatascience.com/new-shap-plots-violin-and-heatmap-20f647313b64?source=collection_archive---------3-----------------------#2023-08-14](https://towardsdatascience.com/new-shap-plots-violin-and-heatmap-20f647313b64?source=collection_archive---------3-----------------------#2023-08-14)

## SHAP 版本 0.42.1 中的图表可以告诉你关于模型的哪些信息

[](https://conorosullyds.medium.com/?source=post_page-----20f647313b64--------------------------------)[![Conor O'Sullivan](../Images/2dc50a24edb12e843651d01ed48a3c3f.png)](https://conorosullyds.medium.com/?source=post_page-----20f647313b64--------------------------------)[](https://towardsdatascience.com/?source=post_page-----20f647313b64--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----20f647313b64--------------------------------) [Conor O'Sullivan](https://conorosullyds.medium.com/?source=post_page-----20f647313b64--------------------------------)

·

[查看](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4ae48256fb37&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnew-shap-plots-violin-and-heatmap-20f647313b64&user=Conor+O%27Sullivan&userId=4ae48256fb37&source=post_page-4ae48256fb37----20f647313b64---------------------post_header-----------) 发布在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----20f647313b64--------------------------------) ·6 分钟阅读·2023年8月14日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F20f647313b64&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnew-shap-plots-violin-and-heatmap-20f647313b64&user=Conor+O%27Sullivan&userId=4ae48256fb37&source=-----20f647313b64---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F20f647313b64&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnew-shap-plots-violin-and-heatmap-20f647313b64&source=-----20f647313b64---------------------bookmark_footer-----------)![](../Images/8defb22358b723a94b013acfe5de076f.png)

(来源：作者)

对于 SHAP 的最大关注点之一是软件包本身。它已经有一段时间没有更新了，GitHub 问题不断增加。令许多用户感到欣慰的是，贡献者变得更加活跃。实际上，他们给我们提供了新的图表——小提琴图和热图。我们将：

+   提供这些图表的代码

+   讨论我们可以从中获得哪些新的见解

你还可以观看关于该主题的介绍：

# 现有的 SHAP 图表

我们继续之前的 SHAP 教程。你可以在下面的文章中找到这个教程。你也可以在 [GitHub](https://github.com/conorosully/SHAP-tutorial) 上找到完整的项目。要使用新的图表，你需要更新 SHAP 包。我正在使用版本 **0.42.1**。
