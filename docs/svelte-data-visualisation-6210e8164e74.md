# Svelte 与数据可视化

> 原文：[https://towardsdatascience.com/svelte-data-visualisation-6210e8164e74?source=collection_archive---------15-----------------------#2023-03-20](https://towardsdatascience.com/svelte-data-visualisation-6210e8164e74?source=collection_archive---------15-----------------------#2023-03-20)

![](../Images/81954de2afaf7180320431e00696f500.png)

Svelte 条形图（来源：作者，2023年）

## 使用 Svelte 创建交互式条形图

[](https://sutan.co.uk/?source=post_page-----6210e8164e74--------------------------------)[![Sutan Mufti](../Images/0a7922168ff75a80b2ddb38d4a142f37.png)](https://sutan.co.uk/?source=post_page-----6210e8164e74--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6210e8164e74--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----6210e8164e74--------------------------------) [Sutan Mufti](https://sutan.co.uk/?source=post_page-----6210e8164e74--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6b3de0d6aa21&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsvelte-data-visualisation-6210e8164e74&user=Sutan+Mufti&userId=6b3de0d6aa21&source=post_page-6b3de0d6aa21----6210e8164e74---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6210e8164e74--------------------------------) ·6分钟阅读·2023年3月20日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F6210e8164e74&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsvelte-data-visualisation-6210e8164e74&user=Sutan+Mufti&userId=6b3de0d6aa21&source=-----6210e8164e74---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F6210e8164e74&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsvelte-data-visualisation-6210e8164e74&source=-----6210e8164e74---------------------bookmark_footer-----------)

# 介绍

数据可视化能够洞察我们数据中的信息。通常的方法是制作图表或地图。我们可以通过像 Microsoft Excel 或 Google Sheets 这样的电子表格软件来实现这一点。通常这已经足够了，但我们可以通过添加交互性来使其更有趣。添加交互性的一个简单示例是通过添加切片器来根据类别过滤数据。在 Microsoft Excel 中，我们可以启用“开发者”选项卡，并添加表单元素，如按钮、复选框等。

使用现代软件技术，创建这种互动数据体验的经典工具是使用 d3.js 和原生 JavaScript。它的工作方式简单却能产生美丽的结果，这一点令人惊叹。然而，与现代框架如 React、Angular、Vue 等相比，原生 JavaScript 感觉有些过时。人们对这些框架都很熟悉，但我觉得有一个数据可视化的 UI 框架被低估了；你在标题中已经猜到了，就是 Svelte。考虑到它年轻的生态系统，我认为这个惊人的框架需要被推广，以便让更多人使用或开发它，特别是在数据科学领域。

这篇文章展示了如何使用 Svelte 进行互动数据可视化（代码链接在以下的 GitHub 链接中）…
