# 面向视觉的语义占据预测用于自动驾驶

> 原文：[https://towardsdatascience.com/vision-centric-semantic-occupancy-prediction-for-autonomous-driving-16a46dbd6f65?source=collection_archive---------7-----------------------#2023-05-29](https://towardsdatascience.com/vision-centric-semantic-occupancy-prediction-for-autonomous-driving-16a46dbd6f65?source=collection_archive---------7-----------------------#2023-05-29)

## 2023年上半年学术“Occupancy Networks”综述

[](https://medium.com/@patrickllgc?source=post_page-----16a46dbd6f65--------------------------------)[![Patrick Langechuan Liu](../Images/fecbf85146a9bde21e6b2251538ddd65.png)](https://medium.com/@patrickllgc?source=post_page-----16a46dbd6f65--------------------------------)[](https://towardsdatascience.com/?source=post_page-----16a46dbd6f65--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----16a46dbd6f65--------------------------------) [Patrick Langechuan Liu](https://medium.com/@patrickllgc?source=post_page-----16a46dbd6f65--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fd875946648f7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fvision-centric-semantic-occupancy-prediction-for-autonomous-driving-16a46dbd6f65&user=Patrick+Langechuan+Liu&userId=d875946648f7&source=post_page-d875946648f7----16a46dbd6f65---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----16a46dbd6f65--------------------------------) ·11分钟阅读·2023年5月29日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F16a46dbd6f65&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fvision-centric-semantic-occupancy-prediction-for-autonomous-driving-16a46dbd6f65&user=Patrick+Langechuan+Liu&userId=d875946648f7&source=-----16a46dbd6f65---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F16a46dbd6f65&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fvision-centric-semantic-occupancy-prediction-for-autonomous-driving-16a46dbd6f65&source=-----16a46dbd6f65---------------------bookmark_footer-----------)

现有的 3D 目标检测方法在自动驾驶中的一个关键痛点是，它们通常输出简洁的 3D 边界框，忽略了更精细的几何细节，并且难以处理一般的、超出词汇表的物体。这个痛点在[单目 3D 目标检测](/monocular-3d-object-detection-in-autonomous-driving-2476a3c7f57e)和[BEV 多摄像头目标检测](/monocular-bev-perception-with-transformers-in-autonomous-driving-c41e4a893944)中都存在。为了解决这个问题，**Occupancy Network**，一种以视觉为中心的通用障碍物检测解决方案，首先在[特斯拉在 CVPR 2022 的主题演讲](https://www.youtube.com/watch?v=jPCV4GKX9Dw)中介绍，随后在[AI Day 2022](https://www.youtube.com/watch?v=ODSJsviD_SU)上得到推广。有关更多详细信息，请参阅之前关于可驾驶空间的博客系列。

[](https://medium.com/@patrickllgc/drivable-space-in-autonomous-driving-the-industry-7a4624b94d41?source=post_page-----16a46dbd6f65--------------------------------) [## 自动驾驶中的可驾驶空间 — 行业

### 截至 2023 年，行业应用的最新趋势

medium.com](https://medium.com/@patrickllgc/drivable-space-in-autonomous-driving-the-industry-7a4624b94d41?source=post_page-----16a46dbd6f65--------------------------------)

在学术界，与 Occupancy Network 对应的感知轨迹被称为 **语义占据预测 (SOP)**，有时也被称为 **语义场景补全 (SSC)**，两者之间的细微差别将在下文解释。语义占据预测为场景中的每个体素分配占据状态和语义标签。这是一种通用且富有表现力的表示方式，可以描述已知类别的物体，但适用于形状不规则的物体或超出已知范围的物体。
