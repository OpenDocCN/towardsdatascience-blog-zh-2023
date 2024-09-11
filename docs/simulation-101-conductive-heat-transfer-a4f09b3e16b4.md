# 模拟 101：导热

> 原文：[https://towardsdatascience.com/simulation-101-conductive-heat-transfer-a4f09b3e16b4?source=collection_archive---------16-----------------------#2023-07-25](https://towardsdatascience.com/simulation-101-conductive-heat-transfer-a4f09b3e16b4?source=collection_archive---------16-----------------------#2023-07-25)

## 计算物理学的温和介绍

[](https://medium.com/@ln8378?source=post_page-----a4f09b3e16b4--------------------------------)[![Le Nguyen](../Images/05289b40bb528d5ba2a0ee00d1a75990.png)](https://medium.com/@ln8378?source=post_page-----a4f09b3e16b4--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a4f09b3e16b4--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----a4f09b3e16b4--------------------------------) [Le Nguyen](https://medium.com/@ln8378?source=post_page-----a4f09b3e16b4--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fb34fcbf59198&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsimulation-101-conductive-heat-transfer-a4f09b3e16b4&user=Le+Nguyen&userId=b34fcbf59198&source=post_page-b34fcbf59198----a4f09b3e16b4---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a4f09b3e16b4--------------------------------) ·11分钟阅读·2023年7月25日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa4f09b3e16b4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsimulation-101-conductive-heat-transfer-a4f09b3e16b4&user=Le+Nguyen&userId=b34fcbf59198&source=-----a4f09b3e16b4---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa4f09b3e16b4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsimulation-101-conductive-heat-transfer-a4f09b3e16b4&source=-----a4f09b3e16b4---------------------bookmark_footer-----------)

导热，即物体之间的热传递，是我们每天都会经历的现象。将锅放在炉子上或坐在热的公园长椅上给了我们对导热的直观感受，但在这里我们将对这一过程进行规范化，并建立一个基本的计算框架来模拟它。导热是一个优秀的首个模拟问题，因为它使用了许多计算物理问题中找到的基本工具。

![](../Images/56a8db8885ed2a6a492829258bf71be3.png)

在本文中，我们将：

+   创建一个网格以表示材料

+   学习基本的热传递方程及其计算等效形式

+   根据基础物理学更新网格中的值

+   模拟导热

# 创建一个网格

网格是一种用于离散化连续空间的计算工具。也就是说，我们不能在问题中的所有时间和空间上进行计算，因此我们选择一个代表性的点子集，通常是以规律的间隔进行查看。

在下面的图1中，我们可以看到一个网格示例。这里一个空间被划分成均匀间隔的单元，这在物理仿真中是常见的做法……
