# 使用 Scikit-Learn 和 Matplotlib 进行异常值检测：实用指南

> 原文：[https://towardsdatascience.com/outlier-detection-with-scikit-learn-and-matplotlib-a-practical-guide-382d1411b8ec?source=collection_archive---------4-----------------------#2023-10-27](https://towardsdatascience.com/outlier-detection-with-scikit-learn-and-matplotlib-a-practical-guide-382d1411b8ec?source=collection_archive---------4-----------------------#2023-10-27)

## 了解如何利用可视化、算法和统计方法来识别机器学习任务中的异常值。

[](https://medium.com/@riccardo.andreoni?source=post_page-----382d1411b8ec--------------------------------)[![Riccardo Andreoni](../Images/5e22581e419639b373019a809d6e65c1.png)](https://medium.com/@riccardo.andreoni?source=post_page-----382d1411b8ec--------------------------------)[](https://towardsdatascience.com/?source=post_page-----382d1411b8ec--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----382d1411b8ec--------------------------------) [Riccardo Andreoni](https://medium.com/@riccardo.andreoni?source=post_page-----382d1411b8ec--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F76784541161c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Foutlier-detection-with-scikit-learn-and-matplotlib-a-practical-guide-382d1411b8ec&user=Riccardo+Andreoni&userId=76784541161c&source=post_page-76784541161c----382d1411b8ec---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----382d1411b8ec--------------------------------) ·10分钟阅读·2023年10月27日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F382d1411b8ec&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Foutlier-detection-with-scikit-learn-and-matplotlib-a-practical-guide-382d1411b8ec&user=Riccardo+Andreoni&userId=76784541161c&source=-----382d1411b8ec---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F382d1411b8ec&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Foutlier-detection-with-scikit-learn-and-matplotlib-a-practical-guide-382d1411b8ec&source=-----382d1411b8ec---------------------bookmark_footer-----------)![](../Images/b52c5234e67397f7a6f6010da838a994.png)

气球与异常值有什么关系？在引言中找到答案。图片来源：[pixabay.com](https://pixabay.com/illustrations/balloons-spring-nature-watercolor-1615032/)。

想象一间充满**色彩缤纷的气球**的房间，每个气球代表数据集中的一个数据点。由于它们具有不同的特征，气球在不同的高度飘浮。现在，想象一些**充氦气的气球**突然飞得比其他气球高得多。正如这些特别的气球打破了房间的均匀性一样，离群值打破了数据集的模式。

从这个丰富多彩的类比回到纯粹的统计学，**离群值**被定义为异常值，或者更好地说，是与数据集中其余数据点显著偏离的数据点。

考虑一个基于患者数据开发的**机器学习算法**用于诊断疾病。在这个现实世界的例子中，离群值可能是实验室结果或生理参数中的极高数值。它们的起源可能是**数据收集错误**、**测量不准确性**或真实的**罕见事件**，它们的存在可能导致算法做出错误的诊断。

这就是为什么我们，机器学习或数据科学从业者，必须始终**谨慎对待离群值**。
