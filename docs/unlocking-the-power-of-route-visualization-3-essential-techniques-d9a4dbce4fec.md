# 解锁路线可视化的力量：3 种必备技巧

> 原文：[`towardsdatascience.com/unlocking-the-power-of-route-visualization-3-essential-techniques-d9a4dbce4fec?source=collection_archive---------5-----------------------#2023-01-29`](https://towardsdatascience.com/unlocking-the-power-of-route-visualization-3-essential-techniques-d9a4dbce4fec?source=collection_archive---------5-----------------------#2023-01-29)

## 分析路线时必须知道的技巧

[](https://pranay-dave9.medium.com/?source=post_page-----d9a4dbce4fec--------------------------------)![Pranay Dave](https://pranay-dave9.medium.com/?source=post_page-----d9a4dbce4fec--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d9a4dbce4fec--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----d9a4dbce4fec--------------------------------) [Pranay Dave](https://pranay-dave9.medium.com/?source=post_page-----d9a4dbce4fec--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F89d4bb7ead78&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funlocking-the-power-of-route-visualization-3-essential-techniques-d9a4dbce4fec&user=Pranay+Dave&userId=89d4bb7ead78&source=post_page-89d4bb7ead78----d9a4dbce4fec---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d9a4dbce4fec--------------------------------) ·4 分钟阅读·2023 年 1 月 29 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fd9a4dbce4fec&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funlocking-the-power-of-route-visualization-3-essential-techniques-d9a4dbce4fec&user=Pranay+Dave&userId=89d4bb7ead78&source=-----d9a4dbce4fec---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd9a4dbce4fec&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funlocking-the-power-of-route-visualization-3-essential-techniques-d9a4dbce4fec&source=-----d9a4dbce4fec---------------------bookmark_footer-----------)![](img/a2f0aa403f6de3b84c76701a0c28dccb.png)

照片由 [José Martín Ramírez Carrasco](https://unsplash.com/es/@martinirc?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/s/photos/route?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)。

路线可视化在交通规划、物流和供应链管理中至关重要。在人工智能和数据科学领域，它被应用于 ETA（预计到达时间）或车队优化等各种应用中。

在这篇博客中，我将介绍可视化路线的顶级技巧。为了说明博客内容，我将以波尔图市的出租车数据为例。出租车配备了每 15 秒钟发射一次位置的物联网设备。

为了说明，我将以一次出租车行程的数据为例。这些数据包含了出租车在每个位置的地点和速度。

![](img/be1bb5b2f8762aecbe035e0150df4735.png)

出租车轨迹数据（图片由作者提供）

现在，让我们使用不同的技巧来可视化这些数据。

# 1\. 路线折线图 — 基本但必不可少的技巧

路线折线图是创建一个通常宽度为两像素的红色线条，显示路径或轨迹。
