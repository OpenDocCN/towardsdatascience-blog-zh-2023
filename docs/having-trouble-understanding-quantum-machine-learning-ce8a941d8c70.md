# 对量子机器学习感到困惑？

> 原文：[`towardsdatascience.com/having-trouble-understanding-quantum-machine-learning-ce8a941d8c70?source=collection_archive---------4-----------------------#2023-03-02`](https://towardsdatascience.com/having-trouble-understanding-quantum-machine-learning-ce8a941d8c70?source=collection_archive---------4-----------------------#2023-03-02)

## 使用函数式编程实现量子近似优化算法

[](https://pyqml.medium.com/?source=post_page-----ce8a941d8c70--------------------------------)![Frank Zickert | Quantum Machine Learning](https://pyqml.medium.com/?source=post_page-----ce8a941d8c70--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ce8a941d8c70--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----ce8a941d8c70--------------------------------) [Frank Zickert | Quantum Machine Learning](https://pyqml.medium.com/?source=post_page-----ce8a941d8c70--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Feebfab42a2c4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhaving-trouble-understanding-quantum-machine-learning-ce8a941d8c70&user=Frank+Zickert+%7C+Quantum+Machine+Learning&userId=eebfab42a2c4&source=post_page-eebfab42a2c4----ce8a941d8c70---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ce8a941d8c70--------------------------------) ·7 min read·Mar 2, 2023[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fce8a941d8c70&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhaving-trouble-understanding-quantum-machine-learning-ce8a941d8c70&user=Frank+Zickert+%7C+Quantum+Machine+Learning&userId=eebfab42a2c4&source=-----ce8a941d8c70---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fce8a941d8c70&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhaving-trouble-understanding-quantum-machine-learning-ce8a941d8c70&source=-----ce8a941d8c70---------------------bookmark_footer-----------)

想要开始量子机器学习吗？请查看[**Python 实战量子机器学习**](https://www.pyqml.com/volume1?provider=medium&origin=troubleunderstanding)**。**

本文将详细讲解量子近似优化算法（QAOA）的最重要部分。QAOA 是一种机器学习算法，可以用来解决组合优化问题。

特别之处在于，这种算法适应了量子计算机的特性——一种新型计算机，承诺在问题解决中实现指数级的加速。

尽管量子机器学习（QML）——即利用量子计算解决机器学习算法——是最有前途的技术之一，**它也是极具挑战性的！**

因此，本文旨在以易于理解的方式解释 QAOA 的基本概念。

量子计算、优化和机器学习严重依赖数学。除非你是数学家，否则这将是一项艰巨的工作。

幸运的是，一些 QML 库，如 IBM Qiskit，解决了这个问题。它们提供易于使用的接口，并将所有复杂性隐藏在背后。

正如我在[上一篇文章](https://medium.com/@pyqml/quantum-wildfire-fighting-e828497b7b89)中所展示的，它们甚至处理了问题的表述。
