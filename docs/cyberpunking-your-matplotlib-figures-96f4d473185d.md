# 让你的 matplotlib 图表变得有趣的黑客技术

> 原文：[https://towardsdatascience.com/cyberpunking-your-matplotlib-figures-96f4d473185d?source=collection_archive---------5-----------------------#2023-05-21](https://towardsdatascience.com/cyberpunking-your-matplotlib-figures-96f4d473185d?source=collection_archive---------5-----------------------#2023-05-21)

## 通过几行代码将你的 matplotlib 图表从乏味变得有趣

[](https://andymcdonaldgeo.medium.com/?source=post_page-----96f4d473185d--------------------------------)[![Andy McDonald](../Images/df11d647be032aeb3d31852affb33a64.png)](https://andymcdonaldgeo.medium.com/?source=post_page-----96f4d473185d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----96f4d473185d--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----96f4d473185d--------------------------------) [安迪·麦克唐纳](https://andymcdonaldgeo.medium.com/?source=post_page-----96f4d473185d--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9c280f85f15c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcyberpunking-your-matplotlib-figures-96f4d473185d&user=Andy+McDonald&userId=9c280f85f15c&source=post_page-9c280f85f15c----96f4d473185d---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----96f4d473185d--------------------------------) ·8 min read·2023年5月21日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F96f4d473185d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcyberpunking-your-matplotlib-figures-96f4d473185d&user=Andy+McDonald&userId=9c280f85f15c&source=-----96f4d473185d---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F96f4d473185d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcyberpunking-your-matplotlib-figures-96f4d473185d&source=-----96f4d473185d---------------------bookmark_footer-----------)![](../Images/fea8b724924c77103e370ccfe95d5961.png)

一个包含多个子图并应用了 mplcyberpunk 主题的 matplotlib 图表。图片由作者提供。

当我们创建包含数据的信息图或海报时，我们希望引起读者的注意，并使其在视觉上令人愉悦，同时讲述一个令人信服的故事。

在 Python 中，我们有许多绘图库可以用来创建图表，其中一个就是著名的 matplotlib 库。然而，默认情况下，matplotlib 生成的图表通常被认为乏味，要将其变得视觉上吸引人可能需要耗费大量时间。

这就是 matplotlib 主题库发挥作用的地方。我最喜欢的一个库是 [**CyberPunk 主题**](https://github.com/dhaitz/mplcyberpunk)。

[](https://github.com/dhaitz/mplcyberpunk?source=post_page-----96f4d473185d--------------------------------) [## GitHub - dhaitz/mplcyberpunk: “赛博朋克风格”用于 matplotlib 图表

### 一个基于 matplotlib 的 Python 包，用于创建 '赛博朋克' 风格的图表，只需额外添加 3 行代码。

github.com](https://github.com/dhaitz/mplcyberpunk?source=post_page-----96f4d473185d--------------------------------)

赛博朋克已成为一个广受欢迎的科幻子类型，其特征是描绘反乌托邦社会、高度先进的技术以及反文化主题。这些背景通常以未来感十足的外观呈现，突出表现为霓虹灯和鲜艳、大胆的色彩。
