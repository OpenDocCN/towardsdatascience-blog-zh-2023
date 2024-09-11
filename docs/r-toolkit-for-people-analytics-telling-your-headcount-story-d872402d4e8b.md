# 人力分析工具包：讲述你的员工数量故事

> 原文：[https://towardsdatascience.com/r-toolkit-for-people-analytics-telling-your-headcount-story-d872402d4e8b?source=collection_archive---------5-----------------------#2023-07-06](https://towardsdatascience.com/r-toolkit-for-people-analytics-telling-your-headcount-story-d872402d4e8b?source=collection_archive---------5-----------------------#2023-07-06)

## 使用 R 解决人力分析中的常见挑战

[](https://jeagleson.medium.com/?source=post_page-----d872402d4e8b--------------------------------)[![珍娜·伊格尔森](../Images/1f13d1104d9cb3d2c1d4376a6e124c55.png)](https://jeagleson.medium.com/?source=post_page-----d872402d4e8b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d872402d4e8b--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----d872402d4e8b--------------------------------) [珍娜·伊格尔森](https://jeagleson.medium.com/?source=post_page-----d872402d4e8b--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8300cae51c6c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fr-toolkit-for-people-analytics-telling-your-headcount-story-d872402d4e8b&user=Jenna+Eagleson&userId=8300cae51c6c&source=post_page-8300cae51c6c----d872402d4e8b---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d872402d4e8b--------------------------------) · 11 分钟阅读 · 2023年7月6日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fd872402d4e8b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fr-toolkit-for-people-analytics-telling-your-headcount-story-d872402d4e8b&user=Jenna+Eagleson&userId=8300cae51c6c&source=-----d872402d4e8b---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd872402d4e8b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fr-toolkit-for-people-analytics-telling-your-headcount-story-d872402d4e8b&source=-----d872402d4e8b---------------------bookmark_footer-----------)

在人力分析领域工作时，你经常被要求讲述公司员工数量的变化以及公司如何发展成今天的模样。我经常看到这种情况被呈现为 [瀑布图](https://www.storytellingwithdata.com/blog/2020/11/16/what-is-a-waterfall)，这虽然很不错，但在尝试展示年度变化和面对非技术观众时会变得模糊不清。

为了满足这一需求，我创建了迭代图，突出显示每一年的情况，并添加了一些额外的背景信息。然后，可以将这些图添加到 PowerPoint 中逐年展示，或将其合成一个 gif 动画。让我们一起动手制作吧！

![](../Images/8c8b00469215e4d4fbe68e1a89a64111.png)

用区域图的 gif 展示人员数量的变化。图片来源于作者。

挑战：讲述我们的人数如何逐年变化至今的故事。

步骤：

1\. 加载必要的包和数据

2\. 计算每月的人员数量

3\. 为每一年添加相关的背景信息

4\. 创建一个图表

5\. 设置自动为每一年创建图表

6\. 调整主题和图表格式

# 1\. 加载必要的包和数据
