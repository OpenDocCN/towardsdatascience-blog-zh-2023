# 旅游是否恢复到 COVID 危机前的水平？

> 原文：[`towardsdatascience.com/is-tourism-back-to-its-pre-covid-crisis-level-27ed32604fa6`](https://towardsdatascience.com/is-tourism-back-to-its-pre-covid-crisis-level-27ed32604fa6)

## 数据分析

## 进行全程分析，回答一个社会经济问题并附上漂亮的图表

[](https://marielefevre.medium.com/?source=post_page-----27ed32604fa6--------------------------------)![Marie Lefevre](https://marielefevre.medium.com/?source=post_page-----27ed32604fa6--------------------------------)[](https://towardsdatascience.com/?source=post_page-----27ed32604fa6--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----27ed32604fa6--------------------------------) [Marie Lefevre](https://marielefevre.medium.com/?source=post_page-----27ed32604fa6--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----27ed32604fa6--------------------------------) ·5 分钟阅读·2023 年 2 月 18 日

--

![](img/824a9eacac7633eaa9d9453fa46a6697.png)

图片由 [Alexandra Luniel](https://unsplash.com/@luniel?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来自 [Unsplash](https://unsplash.com/photos/86T5I7ZtjmM?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

你在圣诞节期间休假吗？你计划在三月底休息几天吗？或者你可能更喜欢在一年中的其他时间比如夏天利用机会环游世界？

无论你是什么类型的假期旅行者， chances are that you will take some holidays this year. 现在 COVID-19 危机的最糟糕阶段（祈祷不要再恶化）已经过去，**你有没有想过旅游是否已经恢复到危机前的水平？**

这是我最近问自己的一个问题，我想给出答案。我发现，对专业领域以外的话题进行分析是一种很好的练习，可以保持你“在数据分析的游戏中”。无论你是初级还是更高级的数据分析师，我认为**使用新的数据集进行训练**总是很有帮助的。

让我带你一起走过从最初构想到最终输出的全程分析旅程吧！

# 步骤 1：找出一个相关的问题进行处理

在这篇文章中，我想借此机会回顾一下今年的旅行，并调查旅游是否已经恢复到危机前的水平。这可以从全球范围来看，也可以缩小到特定的地理区域。由于我住在欧洲，我特别感兴趣的是**分析 COVID 危机对欧洲旅游的影响。**

我思考的另一个方面是：我如何衡量“旅游”？这一概念包含了各种领域，如交通（飞机、火车、汽车等）、旅游景点（博物馆、活动等）、住宿（酒店、露营地、家庭住宿等）。为了更精确地解决我在这里要分析的问题，让我们**专注于住宿**。

# 步骤 2：获取数据以进行分析

在这种情况下，我没有公司数据或预定义的数据集可用，所以让我们浏览互联网以寻找与我的主题相关的开放数据。

Eurostat 提供了可以在线可视化和以多种格式下载的开放数据集。Eurostat 的数据可以免费使用，只要提到 Eurostat 是数据来源即可。在这里，我将使用[这个关于在旅游住宿设施中度过的夜晚的数据集](https://ec.europa.eu/eurostat/databrowser/view/TOUR_OCC_NIM__custom_1210097/bookmark/table?lang=en&bookmarkId=0d6f821c-7e12-44ff-b1d5-b289659b3bc4)：TOUR_OCC_NIM，其来源是 Eurostat。

![](img/10221cc68e478b6453bfed0b633921e4.png)

原始数据集（由 Eurostat 提供，[查看版权声明](https://ec.europa.eu/eurostat/web/main/about-us/policies/copyright)）

# 步骤 3：绘制最终输出

为了回答我的初始问题，我想**将欧洲的全球旅游发展与各国的旅游发展进行比较**。通过这样做，我应该能够看到旅游是否、何时以及在哪个国家恢复到危机前的水平。

为此，我需要两个图表：一个显示每月在旅游住宿设施中度过的总夜晚数，第二个显示按国家划分的相同指标。

![](img/da0f8cbfe88416569a68ef88bc27c84e.png)

期望输出（使用 Excalidraw 绘制）

# 步骤 4：转换数据

手头有原始数据集（步骤 2）并且心中有目标输出（步骤 3），我现在已经准备好进行分析。你注意到实际上进入“数据分析模式”是在步骤 4，而不是更早吗？这是因为**数据分析更多的是关于你为何需要进行分析，而不是实际进行分析**。

要绘制第一个图表，我必须按月对值（在旅游住宿设施中度过的夜晚数量）进行分组。对于第二个图表，我必须按月和国家进行分组，如下所示：

```py
SELECT 
  TIME_PERIOD AS month,
  geo AS country,
  SUM(OBS_VALUE) AS nb_nights_spent,
FROM my_dataset.raw_data
GROUP BY 1,2
ORDER BY 1,2
```

# 步骤 5：可视化数据

代码片段的输出是数据表，而不是（还不是）图表。要将这些表格输出转换为漂亮的图表，让我们**使用数据可视化工具**。在这里，我使用了第 4 步的 Google BigQuery 和第 5 步的 Looker Studio（以前称为 DataStudio）的组合。

由于我们之前绘制了目标输出，我们已经知道最终图表应该是什么样的。这节省了很多时间，因为我只需配置工具，将正确的维度放在正确的位置。这将给我这些图表：

![](img/0dbfca4eee7fd048716655613fe97437.png)

输出图表（在 Looker Studio 中构建）

# 步骤 6：提取见解

那么现在该怎么办？制作图表很棒，但没有人脑来解释这些输出结果，这些图表就相当无用。让我们回到最初的问题：**根据住宿数据，欧洲的旅游是否已经恢复到危机前的水平？** 我们希望尽可能清晰地回答这个问题。

如果我们看全球的发展趋势，**欧洲旅游似乎正回到恢复到危机前水平的轨道上**。虽然 2022 年的夏季季节未能完全达到 2019 年的水平（2022 年 7 月至 8 月比 2019 年 7 月至 8 月减少了 15%），但与 2020 年和 2021 年相比，趋势表现出积极的变化。下一年 2023 年夏季结束时进行相同的分析将是很有趣的。

**如果我们仔细查看每个国家的发展情况**，这个普遍的评论并不总是适用。例如，2022 年西班牙的值与 2019 年非常接近（仅 7 月至 8 月减少了 4%），而其他国家 2022 年远低于 2019 年（捷克为-26%）。

# 结论

在解读结果时，另一个重要的因素是**偏差**。首先，数据收集方式可能存在偏差：当我们比较不同国家时，每个国家可能会采用不同的方法来计算在旅游住宿机构过夜的数量。

其次，仅基于一种指标分析就得出旅游几乎恢复到危机前水平的结论是片面的。要准确评估欧洲旅游的状态，应当**分析多个指标**，比较这些分析结果，并整合从中获得的经验教训。

简而言之：**进行分析时要遵循这 6 个步骤，并注意偏差。**

>> 如果你想获取这篇文章的视觉总结，[你可以在这里**免费下载**](https://marielefevre.gumroad.com/l/method-data-analysis) <<
