# 扩展时间

> 原文：[https://towardsdatascience.com/expanding-time-8a1c41e101c2?source=collection_archive---------8-----------------------#2023-06-02](https://towardsdatascience.com/expanding-time-8a1c41e101c2?source=collection_archive---------8-----------------------#2023-06-02)

## 如何通过提取时间特征来增加低维数据的价值

[](https://medium.com/@kurt.klingensmith?source=post_page-----8a1c41e101c2--------------------------------)[![Kurt Klingensmith](../Images/2249e99f12d10f81598c754b1aaf76cc.png)](https://medium.com/@kurt.klingensmith?source=post_page-----8a1c41e101c2--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8a1c41e101c2--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----8a1c41e101c2--------------------------------) [Kurt Klingensmith](https://medium.com/@kurt.klingensmith?source=post_page-----8a1c41e101c2--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fbaf16815de65&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fexpanding-time-8a1c41e101c2&user=Kurt+Klingensmith&userId=baf16815de65&source=post_page-baf16815de65----8a1c41e101c2---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----8a1c41e101c2--------------------------------) ·8分钟阅读·2023年6月2日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F8a1c41e101c2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fexpanding-time-8a1c41e101c2&user=Kurt+Klingensmith&userId=baf16815de65&source=-----8a1c41e101c2---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F8a1c41e101c2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fexpanding-time-8a1c41e101c2&source=-----8a1c41e101c2---------------------bookmark_footer-----------)![](../Images/8fc618f72a8909656efa287fc09d4fff.png)

照片由 [Guillaume de Germain](https://unsplash.com/@guillaumedegermain?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/AtzIa-yrAN4?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)。

尽管低维数据集看起来用途有限，但往往有方法从中提取更多特征——尤其是当数据集中包含时间数据时。通过“解包”日期和时间的值来提取额外特征，可以提供在基本数据集中无法直接获得的额外见解。本文将介绍如何使用 Python 将低维天气数据的价值提升到原始特征所无法显现的水平。

## 数据

这些数据是公共领域的天气数据，已获得蒙大拿气候办公室的许可，网址为 [climate.umt.edu](http://climate.umt.edu/) [1]。蒙大拿的天气数据可以通过以下链接访问：[https://shiny.cfc.umt.edu/mesonet-download/](https://shiny.cfc.umt.edu/mesonet-download/) [2]。本文中使用的数据包括两个气象站的每日气温记录：白鱼北站（Whitefish North, MT）和哈丁分叉站（Harding Cutoff, SD）。请注意，为了提高本文的可读性，气象站名称和气温列经过了略微的重新格式化。

## 代码和数据 CSV：

带有额外可视化效果的完整笔记本和数据 CSV 可在**链接的 GitHub 页面**上找到：[**从 Git 克隆或下载以跟随教程**](https://github.com/kurtklingensmith/ExpandingTime)**。**
