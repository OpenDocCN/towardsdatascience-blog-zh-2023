# 最短路径（迪杰斯特拉）算法：逐步 Python 指南

> 原文：[https://towardsdatascience.com/shortest-path-dijkstras-algorithm-step-by-step-python-guide-896769522752?source=collection_archive---------2-----------------------#2023-10-04](https://towardsdatascience.com/shortest-path-dijkstras-algorithm-step-by-step-python-guide-896769522752?source=collection_archive---------2-----------------------#2023-10-04)

## 使用 OSMNX 1.6 和长途路径的更新

[](https://bryanvallejo16.medium.com/?source=post_page-----896769522752--------------------------------)[![Bryan R. Vallejo](../Images/fd92974f57c72875cc133a2c959d64ca.png)](https://bryanvallejo16.medium.com/?source=post_page-----896769522752--------------------------------)[](https://towardsdatascience.com/?source=post_page-----896769522752--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----896769522752--------------------------------) [Bryan R. Vallejo](https://bryanvallejo16.medium.com/?source=post_page-----896769522752--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fcbd681aaa725&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fshortest-path-dijkstras-algorithm-step-by-step-python-guide-896769522752&user=Bryan+R.+Vallejo&userId=cbd681aaa725&source=post_page-cbd681aaa725----896769522752---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----896769522752--------------------------------) ·6分钟阅读·2023年10月4日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F896769522752&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fshortest-path-dijkstras-algorithm-step-by-step-python-guide-896769522752&user=Bryan+R.+Vallejo&userId=cbd681aaa725&source=-----896769522752---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F896769522752&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fshortest-path-dijkstras-algorithm-step-by-step-python-guide-896769522752&source=-----896769522752---------------------bookmark_footer-----------)![](../Images/95cc11698ca37362e8ea9166f4856e39.png)

作者提供的图像。摩洛哥的最短路径（约350公里）

这个著名的算法在 Python 库 OSMNX 中得到了实现，可以用来找到两个位置之间按距离或时间加权的最短路径。该算法使用 OpenStreetMap (OSM) 网络，结合 Python 库 NETWORKX 在后台寻找驾驶、步行或骑行路线。

我写这次更新是因为函数的参数稍微有些变化，而且我收到了一些关于旧博客帖子中代码为何无法工作的询问，原因只是因为代码是用较旧版本的osmnx编写的。

旧教程包含了相当有价值的过程，但我也决定制作一个逐步指南，以便获取最短路径的过程更为精确，使用此指南的分析师可以真正理解这个过程。

如果你想查看，可以参考这些旧教程。

## 在赫尔辛基（芬兰），使用不同的网络
