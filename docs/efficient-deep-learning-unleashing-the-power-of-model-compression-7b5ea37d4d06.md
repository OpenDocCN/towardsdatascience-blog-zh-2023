# 高效深度学习：释放模型压缩的力量

> 原文：[`towardsdatascience.com/efficient-deep-learning-unleashing-the-power-of-model-compression-7b5ea37d4d06?source=collection_archive---------3-----------------------#2023-09-03`](https://towardsdatascience.com/efficient-deep-learning-unleashing-the-power-of-model-compression-7b5ea37d4d06?source=collection_archive---------3-----------------------#2023-09-03)

![](img/e3f104a88962263b5a88baa0f644362f.png)

图片由作者提供

## 加速生产环境中的模型推理速度

[](https://medium.com/@marcellopoliti?source=post_page-----7b5ea37d4d06--------------------------------)![Marcello Politi](https://medium.com/@marcellopoliti?source=post_page-----7b5ea37d4d06--------------------------------)[](https://towardsdatascience.com/?source=post_page-----7b5ea37d4d06--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----7b5ea37d4d06--------------------------------) [Marcello Politi](https://medium.com/@marcellopoliti?source=post_page-----7b5ea37d4d06--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7390355d40fe&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fefficient-deep-learning-unleashing-the-power-of-model-compression-7b5ea37d4d06&user=Marcello+Politi&userId=7390355d40fe&source=post_page-7390355d40fe----7b5ea37d4d06---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----7b5ea37d4d06--------------------------------) ·9 分钟阅读·2023 年 9 月 3 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F7b5ea37d4d06&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fefficient-deep-learning-unleashing-the-power-of-model-compression-7b5ea37d4d06&user=Marcello+Politi&userId=7390355d40fe&source=-----7b5ea37d4d06---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F7b5ea37d4d06&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fefficient-deep-learning-unleashing-the-power-of-model-compression-7b5ea37d4d06&source=-----7b5ea37d4d06---------------------bookmark_footer-----------)

## 介绍

当机器学习模型被部署到生产环境时，通常会有一些原型阶段未考虑到的需求。例如，生产中的模型必须处理来自不同用户的大量请求。因此，你需要优化延迟和/或吞吐量等方面。

+   **延迟**：是指完成任务所需的时间，例如点击链接后加载网页的时间。这是开始某事和看到结果之间的等待时间。

+   **吞吐量**：是指系统在一定时间内可以处理的请求数量。

这意味着机器学习模型在进行预测时必须非常快速，为此有各种技术可以提高模型推断的速度，本文将介绍其中最重要的几种。

# 模型压缩

有一些技术旨在使**模型更小**，因此被称为**模型压缩**技术，而其他专注于使模型**在推断时更快**的技术则属于**模型**领域。
