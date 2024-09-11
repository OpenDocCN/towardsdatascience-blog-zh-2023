# 中国大规模生产自动驾驶的挑战

> 原文：[https://towardsdatascience.com/challenges-of-mass-production-autonomous-driving-in-china-407c7e2dc5d8?source=collection_archive---------9-----------------------#2023-06-19](https://towardsdatascience.com/challenges-of-mass-production-autonomous-driving-in-china-407c7e2dc5d8?source=collection_archive---------9-----------------------#2023-06-19)

## 以及小鹏汽车在2023年的最新进展

[](https://medium.com/@patrickllgc?source=post_page-----407c7e2dc5d8--------------------------------)[![Patrick Langechuan Liu](../Images/fecbf85146a9bde21e6b2251538ddd65.png)](https://medium.com/@patrickllgc?source=post_page-----407c7e2dc5d8--------------------------------)[](https://towardsdatascience.com/?source=post_page-----407c7e2dc5d8--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----407c7e2dc5d8--------------------------------) [Patrick Langechuan Liu](https://medium.com/@patrickllgc?source=post_page-----407c7e2dc5d8--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fd875946648f7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fchallenges-of-mass-production-autonomous-driving-in-china-407c7e2dc5d8&user=Patrick+Langechuan+Liu&userId=d875946648f7&source=post_page-d875946648f7----407c7e2dc5d8---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----407c7e2dc5d8--------------------------------) · 7分钟阅读 · 2023年6月19日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F407c7e2dc5d8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fchallenges-of-mass-production-autonomous-driving-in-china-407c7e2dc5d8&user=Patrick+Langechuan+Liu&userId=d875946648f7&source=-----407c7e2dc5d8---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F407c7e2dc5d8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fchallenges-of-mass-production-autonomous-driving-in-china-407c7e2dc5d8&source=-----407c7e2dc5d8---------------------bookmark_footer-----------)

> 本博客文章基于在2023年于温哥华举行的*端到端自动驾驶研讨会*上的主题演讲，题为“中国大规模生产自动驾驶的实践”。主题演讲的录音可以在[这里](https://www.youtube.com/watch?v=d6ucRgDDUWQ&t=162s)找到。

自动驾驶是一项艰巨的挑战，特别是在中国，这里的人工驾驶已经是世界上最具挑战性的之一。主要有三个因素：动态交通参与者、静态道路结构和交通信号。特别是交通信号控制信号，作为**几何上静态但语义上动态**的信号，带来了独特的挑战。在接下来的环节中，我们将简要回顾动态对象和静态环境，并对交通信号这一有趣且特殊的主题进行深入探讨。

[## CVPR 2023 自动驾驶研讨会 | OpenDriveLab](https://opendrivelab.com/e2ead/cvpr23.html?source=post_page-----407c7e2dc5d8--------------------------------)

### 我们很自豪地宣布，今年将与我们的合作伙伴 -Vision-Centric… 一起推出四个全新的挑战。

[OpenDriveLab](https://opendrivelab.com/e2ead/cvpr23.html?source=post_page-----407c7e2dc5d8--------------------------------)

# 动态与静态挑战

动态交通参与者，如易受伤害的道路使用者（VRUs），给中国的自动驾驶车辆带来了重大挑战。VRUs 常常不可预测，可能采取不同的姿态并出现在司机最意想不到的地方。大型动物可能会突然出现…
