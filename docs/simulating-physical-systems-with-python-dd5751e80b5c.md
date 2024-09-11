# 使用 Python 模拟物理系统

> 原文：[https://towardsdatascience.com/simulating-physical-systems-with-python-dd5751e80b5c?source=collection_archive---------4-----------------------#2023-03-07](https://towardsdatascience.com/simulating-physical-systems-with-python-dd5751e80b5c?source=collection_archive---------4-----------------------#2023-03-07)

## 每位工程师或科学家必备的技能

[](https://medium.com/@nhemenway2013?source=post_page-----dd5751e80b5c--------------------------------)[![Nick Hemenway](../Images/a38cade9fe4f81c7e43e5d3155a21d78.png)](https://medium.com/@nhemenway2013?source=post_page-----dd5751e80b5c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----dd5751e80b5c--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----dd5751e80b5c--------------------------------) [Nick Hemenway](https://medium.com/@nhemenway2013?source=post_page-----dd5751e80b5c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7db1695b5485&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsimulating-physical-systems-with-python-dd5751e80b5c&user=Nick+Hemenway&userId=7db1695b5485&source=post_page-7db1695b5485----dd5751e80b5c---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----dd5751e80b5c--------------------------------) ·20分钟阅读·2023年3月7日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fdd5751e80b5c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsimulating-physical-systems-with-python-dd5751e80b5c&user=Nick+Hemenway&userId=7db1695b5485&source=-----dd5751e80b5c---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fdd5751e80b5c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsimulating-physical-systems-with-python-dd5751e80b5c&source=-----dd5751e80b5c---------------------bookmark_footer-----------)![](../Images/379eaf6912584be658e3fa37c4f517b0.png)

图片来源：[NASA](https://unsplash.com/@nasa?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

模拟物理系统的行为在几乎所有科学和工程领域都有着巨大的实用价值。模拟允许我们理解系统的时间演变，而且这种理解方式通常是无法通过物理测试来比拟的。虽然物理测试可能耗时、昂贵且有潜在危险，但模拟是快速、便宜的，并且不会造成设备损坏或人身伤害的风险。实验还受到限制，因为它们只能提供你实际能测量的数据——未测量的系统状态要么不可获得，要么必须在后处理过程中进行估算。另一方面，模拟则像窗口一样，让我们能够窥视系统的整个内部状态。这种本质上观察系统内部运作的能力在工程应用中可以提供宝贵的设计见解。

本文旨在提供一个快速入门指南，帮助你开始使用 Python 编程语言来模拟物理系统。为此，我们将通过一个全面的示例来演示如何模拟一个弹跳的球。我特别选择了这个系统，因为它直观且不太复杂，但仍然有一些细微的差别……
