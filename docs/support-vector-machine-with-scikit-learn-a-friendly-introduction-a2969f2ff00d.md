# 使用 Scikit-Learn 的支持向量机：友好的介绍

> 原文：[https://towardsdatascience.com/support-vector-machine-with-scikit-learn-a-friendly-introduction-a2969f2ff00d?source=collection_archive---------3-----------------------#2023-10-11](https://towardsdatascience.com/support-vector-machine-with-scikit-learn-a-friendly-introduction-a2969f2ff00d?source=collection_archive---------3-----------------------#2023-10-11)

## 每个数据科学家都应该在他们的工具箱中拥有支持向量机。了解如何通过实际操作掌握这个多功能模型。

[](https://medium.com/@riccardo.andreoni?source=post_page-----a2969f2ff00d--------------------------------)[![Riccardo Andreoni](../Images/5e22581e419639b373019a809d6e65c1.png)](https://medium.com/@riccardo.andreoni?source=post_page-----a2969f2ff00d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a2969f2ff00d--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----a2969f2ff00d--------------------------------) [Riccardo Andreoni](https://medium.com/@riccardo.andreoni?source=post_page-----a2969f2ff00d--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F76784541161c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsupport-vector-machine-with-scikit-learn-a-friendly-introduction-a2969f2ff00d&user=Riccardo+Andreoni&userId=76784541161c&source=post_page-76784541161c----a2969f2ff00d---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a2969f2ff00d--------------------------------) ·9 分钟阅读·2023年10月11日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa2969f2ff00d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsupport-vector-machine-with-scikit-learn-a-friendly-introduction-a2969f2ff00d&user=Riccardo+Andreoni&userId=76784541161c&source=-----a2969f2ff00d---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa2969f2ff00d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsupport-vector-machine-with-scikit-learn-a-friendly-introduction-a2969f2ff00d&source=-----a2969f2ff00d---------------------bookmark_footer-----------)![](../Images/ca089d89ca86dd60ffd60783581574e6.png)

图片来源：[unsplash.com](https://unsplash.com/photos/3DkouQeZjp4)。

在现有的机器学习模型中，有一种模型因其多功能性而成为**每个数据科学家工具箱**中的必备工具：[**支持向量机**](https://en.wikipedia.org/wiki/Support_vector_machine)（[SVM](https://scikit-learn.org/stable/modules/generated/sklearn.svm.SVC.html)）。

SVM 是一个强大而多功能的算法，其核心可以在高维空间中描绘**最优超平面**，有效地**分隔数据集中的不同类别**。但它的应用并不止于此！其有效性不仅限于**分类**任务：SVM 同样适用于**回归**和**异常检测**任务。

一个特性使得 SVM 方法特别有效。与 [KNN](https://levelup.gitconnected.com/optical-character-recognition-with-knn-classifier-a-step-by-step-guide-b6d7b90e1122) 需要处理整个数据集不同，SVM 战略性地只关注位于决策边界附近的数据点。这些点被称为 [*支持向量*](https://www.analyticsvidhya.com/blog/2021/10/support-vector-machinessvm-a-complete-guide-for-beginners/#:~:text=Support%20Vectors%3A%20These%20are%20the,is%20considered%20a%20good%20margin.)，而这一独特思想背后的数学将在接下来的章节中简单解释。

通过这样做，SVM 算法是**计算上保守的**，非常适合处理中等甚至中大型数据集的任务。
