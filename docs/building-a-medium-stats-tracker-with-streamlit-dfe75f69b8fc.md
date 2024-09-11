# 使用 Streamlit 构建 Medium 统计追踪器

> 原文：[https://towardsdatascience.com/building-a-medium-stats-tracker-with-streamlit-dfe75f69b8fc?source=collection_archive---------12-----------------------#2023-02-27](https://towardsdatascience.com/building-a-medium-stats-tracker-with-streamlit-dfe75f69b8fc?source=collection_archive---------12-----------------------#2023-02-27)

## 使用 Python Streamlit 库追踪和监控 Medium 统计数据

[](https://andymcdonaldgeo.medium.com/?source=post_page-----dfe75f69b8fc--------------------------------)[![Andy McDonald](../Images/df11d647be032aeb3d31852affb33a64.png)](https://andymcdonaldgeo.medium.com/?source=post_page-----dfe75f69b8fc--------------------------------)[](https://towardsdatascience.com/?source=post_page-----dfe75f69b8fc--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----dfe75f69b8fc--------------------------------) [Andy McDonald](https://andymcdonaldgeo.medium.com/?source=post_page-----dfe75f69b8fc--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9c280f85f15c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-a-medium-stats-tracker-with-streamlit-dfe75f69b8fc&user=Andy+McDonald&userId=9c280f85f15c&source=post_page-9c280f85f15c----dfe75f69b8fc---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----dfe75f69b8fc--------------------------------) · 9 分钟阅读 · 2023 年 2 月 27 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fdfe75f69b8fc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-a-medium-stats-tracker-with-streamlit-dfe75f69b8fc&user=Andy+McDonald&userId=9c280f85f15c&source=-----dfe75f69b8fc---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fdfe75f69b8fc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-a-medium-stats-tracker-with-streamlit-dfe75f69b8fc&source=-----dfe75f69b8fc---------------------bookmark_footer-----------)![](../Images/d22a37957e1a52e038e3944f7b5719a4.png)

照片由 [Markus Winkler](https://unsplash.com/es/@markuswinkler?utm_source=medium&utm_medium=referral) 拍摄，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

我已经在 Medium 上写作了一段时间，并且最近开始使用 Excel 追踪我的统计数据。但是，最近我开始考虑使用 Streamlit 构建一个应用程序，以提供更好的体验。所以我想，为什么不试试呢？边构建边写一篇文章。

在本文中，我们将介绍构建一个简单的 Streamlit 仪表板的过程，并提供一种方法，让最终用户通过表单输入他们的最新统计数据。到文章结束时，我们将拥有一个基本的仪表板，我们可以在此基础上进行构建，并在后续使其更加强大。

![](../Images/de3327290d6f9e8d5748484e610df3db.png)

用于查看和分析 Medium 统计数据的 Streamlit 仪表板。图片由作者提供

如果你刚刚开始使用 Streamlit，你可能想查看我下面的文章。

[](/getting-started-with-streamlit-web-based-applications-626095135cb8?source=post_page-----dfe75f69b8fc--------------------------------) [## 开始使用 Streamlit Web 应用程序

### 轻松入门创建 Streamlit Web 应用程序

[towardsdatascience.com](/getting-started-with-streamlit-web-based-applications-626095135cb8?source=post_page-----dfe75f69b8fc--------------------------------)
