# 街道名称中的隐藏模式 [第二部分]

> 原文：[https://towardsdatascience.com/hidden-patterns-in-street-names-part-2-4ae9af5fdee3?source=collection_archive---------20-----------------------#2023-03-06](https://towardsdatascience.com/hidden-patterns-in-street-names-part-2-4ae9af5fdee3?source=collection_archive---------20-----------------------#2023-03-06)

## 使用数据科学分析阿尔巴尼亚地拉那的街道名称

[](https://deabardhoshi.medium.com/?source=post_page-----4ae9af5fdee3--------------------------------)[![Dea Bardhoshi](../Images/14ce0986fc2a4a192797a52ed9908d1e.png)](https://deabardhoshi.medium.com/?source=post_page-----4ae9af5fdee3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----4ae9af5fdee3--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----4ae9af5fdee3--------------------------------) [Dea Bardhoshi](https://deabardhoshi.medium.com/?source=post_page-----4ae9af5fdee3--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fd61c58ba988e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhidden-patterns-in-street-names-part-2-4ae9af5fdee3&user=Dea+Bardhoshi&userId=d61c58ba988e&source=post_page-d61c58ba988e----4ae9af5fdee3---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----4ae9af5fdee3--------------------------------) ·6 分钟阅读·2023年3月6日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F4ae9af5fdee3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhidden-patterns-in-street-names-part-2-4ae9af5fdee3&user=Dea+Bardhoshi&userId=d61c58ba988e&source=-----4ae9af5fdee3---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F4ae9af5fdee3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhidden-patterns-in-street-names-part-2-4ae9af5fdee3&source=-----4ae9af5fdee3---------------------bookmark_footer-----------)![](../Images/05158f80ab44e4cb803e0cf1773123d5.png)

图片来源：[Mario Beqollari](https://unsplash.com/es/@_lima_?utm_source=medium&utm_medium=referral) 于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

你好！

这是我关于地拉那街道名称的故事的第二部分（你可以在这里找到第一部分：[街道名称中的隐藏模式：数据科学故事 [第一部分]](/hidden-patterns-in-street-names-a-data-science-story-part-1-82c8dd130693)）。在第一部分中，我们探讨了整体性别分布以及它们如何根据社区和道路类型发生变化。在这篇后续文章中，我将重点关注职业以及这些人物生活的历史时期。让我们开始吧！

## 数据与工具

与第一篇博客一样，为了将人物与他们的出生和死亡日期及其职业匹配，我手动标记了从 OpenStreetMaps 获得的数据集的一部分（[许可：CC BY-SA 2.0](https://www.openstreetmap.org/copyright)）。具体来说，我使用了维基百科中列出的特定历史人物的主要职业，并依赖于那里列出的出生/死亡日期将这些细节添加到数据中。总共，我标记了**643**条独特的街道，并将其分为数据中的多个段落，其中相当一部分没有在线信息（更多内容见文末）。

在这第二部分中，我将使用 pandas、seaborn 和一些地理空间 Python 库来分析结果数据集并创建各种可视化图表。

## 职业和历史时期

在标记数据时，我根据每个人的维基百科页面将其分类为几个广泛的类别。结果发现街道名称中有62种独特的职业！以下是前20名及其各自计数的条形图：

![](../Images/00bee106f3d9c741e94abd37a8735985.png)

前20名职业（图片来源：作者）

请注意政治家、战士和作家的突出地位：这三个类别共同占据了数据中所有标记街道名称的约40%。艺术家紧随其后，完成了前10名的其余部分。我选择将党派人物（占标记总数的8%）与其他战士单独标记，因为他们与阿尔巴尼亚共产党政权时期（1946–1991）有直接关联，以及他们在此期间所参与的宣传。因此，我认为这在我们作为一个国家如何面对历史的更广泛讨论中值得单独考虑。

让我们更详细地了解一下**前三大类别**：政治家、作家和战士。具体来说，他们生活和工作的历史时期是什么？以下是出生和死亡年份的分布情况，以及它们的平均值：

政治家分布的代码片段

![](../Images/9480636c9397dc134563e6d2a652bf0a.png)

图片来源：作者

![](../Images/e7de3ba23d3d29d5f62e0ef30935be8c.png)

图片来源：作者

![](../Images/43c04af72316da7f60627fbffccb9daa.png)

图片来源：作者

+   **平均出生年份**：政治家（1882年）、作家（1858年）、战士（1853年）

+   **平均死亡年份**：政治家（1936年）、作家（1925年）、战士（1900年）

在上述图表和平均统计数据中可以注意到一些有趣的模式：首先，似乎大多数这些人物活跃在**1850年代**到**1930年代**。作家的情况则有所不同，他们也出现在17世纪和18世纪。此外，“战士”出生和死亡年份有一个高峰，这与奥斯曼帝国入侵期间斯坎德培领导的反抗时期相对应。斯坎德培（1405–1468）被认为是我们的民族英雄，至今在许多纪念碑、广场和机构中得到广泛尊敬。另一方面，19世纪到20世纪则对应于阿尔巴尼亚民族觉醒时期，这是一个旨在建立独立阿尔巴尼亚国的文化和政治运动。

## 以女性命名的街道

上一次，我们查看了以女性命名的街道大约占总街道名称的3%，但让我们也在历史背景和职业方面来看看这些街道。通过性别筛选，并查看相同的出生/死亡分布：

![](../Images/585d41e01240f54eb18efd31b8c9f79f.png)

图片由作者提供

+   **中位出生年：** 1912

+   **中位死亡年：** 1949

这里也有一些有趣的模式：首先，我选择了中位数而不是均值，因为均值会受到1400年代小高峰的显著影响。这导致了女性活动时代的时间移位，相较于整体分布。在第一部分中，我们看到这些女性中的许多人从事艺术或参与过各种战争，这可以解释中位死亡年份的情况。

## 绘制地图，邻里与道路类型

让我们从地理学的角度来看看这些内容。这是前三个类别的三张地图：

![](../Images/476303198ddbcae07ba43632b3c98e78.png)

图片由作者提供

![](../Images/bb03278d5fb89bac9a9a679ff8e0b0ee.png)

图片由作者提供

![](../Images/963ac9abcf06752df5881502866c30d1.png)

图片由作者提供

我会说，仅凭数据我无法立即找出任何明显的模式。为了开始识别这些模式，这里还展示了蒂拉纳14个行政区的最常见职业（与街道名称合并的行政区的多边形数据也来自OpenStreetMaps）：

![](../Images/dad03651c06c85b4c292f04963ddcf71.png)

图片由作者提供

不出所料，政治家和作家依然占据重要位置：我认为值得更多思考的是，某些地区以宗教或党派人物作为最常见的职业。这可能指向这些邻里的特定人物作为一种地方代表，或者是其他有趣的模式值得探讨。

## 互联网上没有信息的名字

正如我在开始时提到的，这些街道名称中很大一部分在网上没有 readily available 的信息。根据标记的数据，它们约占以人名命名街道的55%。我发现这是一个有趣的现象，因为这可能意味着这些名称是本地重要人物但不被广泛认知，或者（显而易见）互联网信息有限，需要查阅其他来源。这里是这些名称所在位置的可视化（请注意这些名称主要出现在城市的边缘，而不是城市的中心）：

![](../Images/aa102a58c69ae9efcfb9d75e389ad78a.png)

## 结论 + 代码

总体来说，这个故事从更具历史性的角度审视了地拉那的街道风貌。我们深入探讨了不同工作区域和历史时期的表现，并揭示了这两个方面的模式。此外，还有一些其他的方向可以推进这个项目：例如，如何随时间推移城市名称的变化，或是命名过程中的具体细节如何导致我们看到的分布。目前，这里是[Jupyter Notebook](https://github.com/DeaBardhoshi/AlbaniaExplorations/blob/main/Albania%20Street%20Analysis%20%5BPart%202%5D.ipynb)和[数据集](https://github.com/DeaBardhoshi/AlbaniaExplorations/blob/main/Work%20and%20Years.xlsx)。希望你喜欢！

如果你喜欢这些城市规划主题的帖子，你可能会喜欢我的新闻通讯，在其中我会更深入地讨论这些话题：[The Zoned Out Chronicles](https://deabardhoshi.substack.com)!
