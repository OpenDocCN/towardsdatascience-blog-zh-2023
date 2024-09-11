# 7 种使用 Python 处理多变量分类数据的可视化方法

> 原文：[`towardsdatascience.com/7-visualizations-with-python-to-handle-multivariate-categorical-data-63158db0911d?source=collection_archive---------1-----------------------#2023-09-20`](https://towardsdatascience.com/7-visualizations-with-python-to-handle-multivariate-categorical-data-63158db0911d?source=collection_archive---------1-----------------------#2023-09-20)

## 展示复杂分类数据的简单方法的创意。

[](https://medium.com/@borih.k?source=post_page-----63158db0911d--------------------------------)![Boriharn K](https://medium.com/@borih.k?source=post_page-----63158db0911d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----63158db0911d--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----63158db0911d--------------------------------) [Boriharn K](https://medium.com/@borih.k?source=post_page-----63158db0911d--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe20a7f1ba78f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F7-visualizations-with-python-to-handle-multivariate-categorical-data-63158db0911d&user=Boriharn+K&userId=e20a7f1ba78f&source=post_page-e20a7f1ba78f----63158db0911d---------------------post_header-----------) 发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----63158db0911d--------------------------------) · 8 分钟阅读 · 2023 年 9 月 20 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F63158db0911d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F7-visualizations-with-python-to-handle-multivariate-categorical-data-63158db0911d&user=Boriharn+K&userId=e20a7f1ba78f&source=-----63158db0911d---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F63158db0911d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F7-visualizations-with-python-to-handle-multivariate-categorical-data-63158db0911d&source=-----63158db0911d---------------------bookmark_footer-----------)![](img/4247ee18957f3a558350b336da617fb1.png)

照片由[Kaizen Nguyễn](https://unsplash.com/@kaizennguyen)提供，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

常见数据，如著名的鸢尾花或企鹅[数据集](https://github.com/mwaskom/seaborn-data)，用于分析时相对简单，因为它们只有少数几类变量。顺便提一下，现实世界的数据可能更复杂，包含多个类别级别。

多变量分类数据是一种具有多个类别的数据。例如，考虑将人群分组。由于一个人的特点可能因类别（如性别、国籍、薪资范围或教育水平）而有所不同，我们可能会得到许多可能性。车辆也有各种分类变量，如品牌、原产国、燃料类型、细分市场等。

![](img/826de8dbbb848b7f4f7f07b806d6b23a.png)![](img/3ff6562ae6203d535d5eea5e2e297918.png)

本文中的多变量分类数据可视化示例。图片来源于作者。

建议使用数据可视化进行探索性数据分析（EDA），以帮助理解数据。柱状图或饼图等图表是绘制简单分类数据的基本选择。顺便提一下，显示多变量分类数据可能更复杂，因为有许多分类变量的级别。因此，本文将指导……
