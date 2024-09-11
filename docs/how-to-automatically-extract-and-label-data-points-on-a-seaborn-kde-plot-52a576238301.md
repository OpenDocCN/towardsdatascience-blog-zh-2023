# 如何自动提取和标记 Seaborn KDE 图上的数据点

> 原文：[https://towardsdatascience.com/how-to-automatically-extract-and-label-data-points-on-a-seaborn-kde-plot-52a576238301?source=collection_archive---------3-----------------------#2023-09-05](https://towardsdatascience.com/how-to-automatically-extract-and-label-data-points-on-a-seaborn-kde-plot-52a576238301?source=collection_archive---------3-----------------------#2023-09-05)

[](https://medium.com/@lee_vaughan?source=post_page-----52a576238301--------------------------------)[![李·沃恩](../Images/9f6b90bb76102f438ab0b9a4a62ffa3f.png)](https://medium.com/@lee_vaughan?source=post_page-----52a576238301--------------------------------)[](https://towardsdatascience.com/?source=post_page-----52a576238301--------------------------------)[![数据科学的前沿](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----52a576238301--------------------------------) [李·沃恩](https://medium.com/@lee_vaughan?source=post_page-----52a576238301--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F5d604015c08b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-automatically-extract-and-label-data-points-on-a-seaborn-kde-plot-52a576238301&user=Lee+Vaughan&userId=5d604015c08b&source=post_page-5d604015c08b----52a576238301---------------------post_header-----------) 发表在 [数据科学的前沿](https://towardsdatascience.com/?source=post_page-----52a576238301--------------------------------) ·8分钟阅读·2023年9月5日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F52a576238301&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-automatically-extract-and-label-data-points-on-a-seaborn-kde-plot-52a576238301&user=Lee+Vaughan&userId=5d604015c08b&source=-----52a576238301---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F52a576238301&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-automatically-extract-and-label-data-points-on-a-seaborn-kde-plot-52a576238301&source=-----52a576238301---------------------bookmark_footer-----------)![](../Images/cdbbc4c1c1002109c7b24d9cd28ea6c6.png)

DALL·E 2023——一幅表现山脉起伏的印象派画作，山脊线上点缀着鲜艳的圆圈（所有剩余的图片由作者提供）。

*核密度估计图（Kernel Density Estimate plot）* 是一种类似于直方图的方法，用于可视化数据点的分布。尽管直方图 *将观察值分箱并计数*，但 KDE 图则 *使用高斯核对观察值进行平滑*。作为直方图的替代方案，KDE 图在某种程度上更具吸引力，更易于在同一图形中进行比较，并且更能突出数据分布中的模式。

![](../Images/eb84d573c206ec87f791d6343da9abf9.png)

直方图与KDE图

在KDE图上注释统计量如均值、中位数或众数使其更有意义。虽然添加这些度量的线条是*简单*的，但让它们看起来干净整洁则不容易。

![](../Images/365149b556f136d9ae8847a26388cdf9.png)

使用简单方法添加的标记线（左）与使用更复杂但更吸引人的方法（右）

在这个*快速成功的数据科学*项目中，我们将使用美国人口普查和国会数据集程序化地注释多个KDE图，并标注*中位数*值。这种方法将确保图表注释*自动*调整数据集更新。

关于KDE图的更多细节，请参见我之前的文章 [这里](https://medium.com/towards-data-science/when-are-songwriters-most-successful-9fdf90708e77)。

# 数据集
