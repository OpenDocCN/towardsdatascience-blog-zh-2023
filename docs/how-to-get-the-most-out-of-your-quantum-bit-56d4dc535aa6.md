# 如何最大化利用你的量子比特

> 原文：[https://towardsdatascience.com/how-to-get-the-most-out-of-your-quantum-bit-56d4dc535aa6?source=collection_archive---------13-----------------------#2023-01-11](https://towardsdatascience.com/how-to-get-the-most-out-of-your-quantum-bit-56d4dc535aa6?source=collection_archive---------13-----------------------#2023-01-11)

## 量子比特不仅仅是0和1

[](https://pyqml.medium.com/?source=post_page-----56d4dc535aa6--------------------------------)[![Frank Zickert | Quantum Machine Learning](../Images/ae361c0d68d13dac21bb86c7496d2917.png)](https://pyqml.medium.com/?source=post_page-----56d4dc535aa6--------------------------------)[](https://towardsdatascience.com/?source=post_page-----56d4dc535aa6--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----56d4dc535aa6--------------------------------) [Frank Zickert | Quantum Machine Learning](https://pyqml.medium.com/?source=post_page-----56d4dc535aa6--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Feebfab42a2c4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-get-the-most-out-of-your-quantum-bit-56d4dc535aa6&user=Frank+Zickert+%7C+Quantum+Machine+Learning&userId=eebfab42a2c4&source=post_page-eebfab42a2c4----56d4dc535aa6---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----56d4dc535aa6--------------------------------) ·7 min read·2023年1月11日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F56d4dc535aa6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-get-the-most-out-of-your-quantum-bit-56d4dc535aa6&user=Frank+Zickert+%7C+Quantum+Machine+Learning&userId=eebfab42a2c4&source=-----56d4dc535aa6---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F56d4dc535aa6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-get-the-most-out-of-your-quantum-bit-56d4dc535aa6&source=-----56d4dc535aa6---------------------bookmark_footer-----------)

想要开始学习量子机器学习吗？看看 [**《Python实战量子机器学习》**](https://www.pyqml.com/volume1?provider=medium&origin=mostofqubit)**。**

量子计算机是21世纪最有前途的技术之一。然而，我们仍处于早期研究阶段，试图找出如何大规模构建可靠的量子计算机。

但量子计算机已经不再是科幻小说了。IBM、谷歌、微软、亚马逊以及许多其他公司已经开始启动计划，旨在商业化利用现有量子计算机的潜力。

然而，主要挑战在于当前设备中量子比特（qubits）的数量极少。尽管我们不能简单地增加数量，但我们可以改变使用量子比特的方式。

本文提出了一种增加我们在量子比特中存储信息量的新方法。

量子比特处于叠加态。这是其两个基态之间复杂（指复数）的线性关系。

![](../Images/404e41a8830212ae0356bdc1219619d1.png)

图片来源于作者

它不是离散的，而是连续的。它具有两个维度，即实幅度和相位。理论上，凭借无限的精度，可以存储无限量的数据。
