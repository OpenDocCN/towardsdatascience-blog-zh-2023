# 离散化量子比特在实际中有效吗？

> 原文：[`towardsdatascience.com/do-discretized-qubits-work-in-practice-60acf19fc68c?source=collection_archive---------18-----------------------#2023-01-18`](https://towardsdatascience.com/do-discretized-qubits-work-in-practice-60acf19fc68c?source=collection_archive---------18-----------------------#2023-01-18)

## 如果量子比特不仅仅是 0 或 1 呢？

[](https://pyqml.medium.com/?source=post_page-----60acf19fc68c--------------------------------)![弗兰克·齐克特 | 量子机器学习](https://pyqml.medium.com/?source=post_page-----60acf19fc68c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----60acf19fc68c--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----60acf19fc68c--------------------------------) [弗兰克·齐克特 | 量子机器学习](https://pyqml.medium.com/?source=post_page-----60acf19fc68c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Feebfab42a2c4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdo-discretized-qubits-work-in-practice-60acf19fc68c&user=Frank+Zickert+%7C+Quantum+Machine+Learning&userId=eebfab42a2c4&source=post_page-eebfab42a2c4----60acf19fc68c---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----60acf19fc68c--------------------------------) ·9 min read·2023 年 1 月 18 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F60acf19fc68c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdo-discretized-qubits-work-in-practice-60acf19fc68c&user=Frank+Zickert+%7C+Quantum+Machine+Learning&userId=eebfab42a2c4&source=-----60acf19fc68c---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F60acf19fc68c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdo-discretized-qubits-work-in-practice-60acf19fc68c&source=-----60acf19fc68c---------------------bookmark_footer-----------)

想开始了解量子机器学习吗？看看 [**Python 实战量子机器学习**](https://www.pyqml.com/volume1?provider=medium&origin=discretizedqubit)**。**

机器学习模型变得越来越复杂，因此也越来越难以训练。例如 ChatGPT。在一块 GPU 上，它的训练需要 355 年。

量子计算是一项有前景的技术，可能加速此类模型的训练。然而，它也面临着一系列挑战。

量子比特（qubits）是我们在量子计算机内部使用的基本单位。与经典比特（经典比特要么是 0，要么是 1）不同，量子比特处于其两个基态 |0⟩ 和 |1⟩ 之间的复杂线性关系中，这种状态称为叠加态。

![](img/d4ac85b337301a61c12c7e25e0d8faca.png)

作者提供的图片

这使得它们极其强大。首先，这种关系不是离散的而是连续的，这意味着量子比特可以在两个基态之间取任何值。其次，这种关系建立在复数的基础上——这些是二维结构——超出了我们习惯使用的一维数的能力。

但问题是，这里总是有问题——不幸的是。

一旦我们测量量子比特，它就会塌缩到其基态之一。不可避免地，我们看到的只有 0 或…
