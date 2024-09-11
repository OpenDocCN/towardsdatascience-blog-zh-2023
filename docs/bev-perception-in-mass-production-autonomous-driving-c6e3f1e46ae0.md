# 大规模生产自动驾驶中的 BEV 感知

> 原文：[`towardsdatascience.com/bev-perception-in-mass-production-autonomous-driving-c6e3f1e46ae0?source=collection_archive---------5-----------------------#2023-06-19`](https://towardsdatascience.com/bev-perception-in-mass-production-autonomous-driving-c6e3f1e46ae0?source=collection_archive---------5-----------------------#2023-06-19)

## 小鹏汽车的 XNet 配方

[](https://medium.com/@patrickllgc?source=post_page-----c6e3f1e46ae0--------------------------------)![Patrick Langechuan Liu](https://medium.com/@patrickllgc?source=post_page-----c6e3f1e46ae0--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c6e3f1e46ae0--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----c6e3f1e46ae0--------------------------------) [Patrick Langechuan Liu](https://medium.com/@patrickllgc?source=post_page-----c6e3f1e46ae0--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fd875946648f7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbev-perception-in-mass-production-autonomous-driving-c6e3f1e46ae0&user=Patrick+Langechuan+Liu&userId=d875946648f7&source=post_page-d875946648f7----c6e3f1e46ae0---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c6e3f1e46ae0--------------------------------) ·9 min 阅读·2023 年 6 月 19 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc6e3f1e46ae0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbev-perception-in-mass-production-autonomous-driving-c6e3f1e46ae0&user=Patrick+Langechuan+Liu&userId=d875946648f7&source=-----c6e3f1e46ae0---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc6e3f1e46ae0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbev-perception-in-mass-production-autonomous-driving-c6e3f1e46ae0&source=-----c6e3f1e46ae0---------------------bookmark_footer-----------)

> 本博客文章基于在 2023 年 CVPR 大会于温哥华举行的*端到端自动驾驶工作坊*上邀请演讲的内容，题为“中国的大规模生产自动驾驶实践”。主旨演讲的录音可以在 [这里](https://www.youtube.com/watch?v=d6ucRgDDUWQ&t=162s) 找到。

BEV（鸟瞰视角）感知在过去几年中取得了巨大进展。它直接感知自动驾驶车辆周围的环境。**BEV 感知**可以被视为一个**端到端感知**系统，是迈向端到端自动驾驶系统的重要一步。我们在这里将端到端自动驾驶系统定义为完全可微分的管道，这些管道以原始传感器数据作为输入，并产生高级驾驶计划或低级控制动作作为输出。

[](https://opendrivelab.com/e2ead/cvpr23.html?source=post_page-----c6e3f1e46ae0--------------------------------) [## CVPR 2023 自动驾驶研讨会 | OpenDriveLab

### 我们很高兴地宣布，今年将与我们的合作伙伴共同推出四项全新的挑战——以视觉为中心的…

opendrivelab.com](https://opendrivelab.com/e2ead/cvpr23.html?source=post_page-----c6e3f1e46ae0--------------------------------)

自动驾驶社区见证了采纳端到端算法框架的方法的快速增长。我们将从第一原则讨论端到端方法的必要性。然后，我们将回顾将 BEV 感知算法部署到量产车辆上的努力，以 Xpeng 的 BEV 感知架构 XNet 的发展为例。最后，我们将进行头脑风暴……
