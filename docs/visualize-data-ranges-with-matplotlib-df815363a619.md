# 使用 Matplotlib 可视化数据范围

> 原文：[`towardsdatascience.com/visualize-data-ranges-with-matplotlib-df815363a619?source=collection_archive---------10-----------------------#2023-09-26`](https://towardsdatascience.com/visualize-data-ranges-with-matplotlib-df815363a619?source=collection_archive---------10-----------------------#2023-09-26)

## 基准测试 NOAA 的飓风预报

[](https://medium.com/@lee_vaughan?source=post_page-----df815363a619--------------------------------)![Lee Vaughan](https://medium.com/@lee_vaughan?source=post_page-----df815363a619--------------------------------)[](https://towardsdatascience.com/?source=post_page-----df815363a619--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----df815363a619--------------------------------) [Lee Vaughan](https://medium.com/@lee_vaughan?source=post_page-----df815363a619--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F5d604015c08b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fvisualize-data-ranges-with-matplotlib-df815363a619&user=Lee+Vaughan&userId=5d604015c08b&source=post_page-5d604015c08b----df815363a619---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----df815363a619--------------------------------) ·10 min read·Sep 26, 2023[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fdf815363a619&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fvisualize-data-ranges-with-matplotlib-df815363a619&user=Lee+Vaughan&userId=5d604015c08b&source=-----df815363a619---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fdf815363a619&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fvisualize-data-ranges-with-matplotlib-df815363a619&source=-----df815363a619---------------------bookmark_footer-----------)![](img/60a1588871d13a5194b7b0c3e648a15e.png)

由 Leonardo AI DreamShaper_v7 模型生成的太空中的飓风

绘制*离散*数据很简单；表示*数据范围*则更复杂。幸运的是，Python 的 matplotlib 库提供了一个内置函数 `fill_between()`，使得可视化数据范围变得非常容易。在这个*快速成功的数据科学*项目中，我们将利用它来基准评估国家海洋和大气管理局的年度飓风预报。

# 数据集

每年五月，NOAA 发布其“亚特兰大飓风展望”报告，涵盖六月至十一月的飓风季节。这些展望包括命名风暴、飓风和强飓风（定义为三级及以上）的预测范围。你可以在[这里](https://www.cpc.ncep.noaa.gov/products/outlooks/hurricane2021/May/hurricane.shtml)找到 2021 年的示例报告[1]。NOAA/国家气象局的数据由美国政府提供，作为[开放数据](https://psl.noaa.gov/disclaimer/)，可以自由用于任何目的。

为了基准这些预测的准确性，我们将使用维基百科提供的年度飓风季节总结。这些总结提供了每年的*实际*风暴和飓风数量。你可以在[这里](https://en.wikipedia.org/wiki/2021_Atlantic_hurricane_season)找到 2021 年季节条目[2]。维基百科页面在[*CC BY-SA 4.0*](https://creativecommons.org/licenses/by-sa/4.0/)许可证下提供。

维基百科还包括了[*拉尼娜*](https://en.wikipedia.org/wiki/La_Ni%C3%B1a)和[*厄尔尼诺*](https://en.wikipedia.org/wiki/El_Ni%C3%B1o)事件的列表[3][4]。这些代表了每隔几年的太平洋天气模式……
