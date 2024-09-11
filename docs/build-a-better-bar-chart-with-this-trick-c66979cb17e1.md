# 使用这个技巧构建更好的条形图

> 原文：[https://towardsdatascience.com/build-a-better-bar-chart-with-this-trick-c66979cb17e1?source=collection_archive---------10-----------------------#2023-08-26](https://towardsdatascience.com/build-a-better-bar-chart-with-this-trick-c66979cb17e1?source=collection_archive---------10-----------------------#2023-08-26)

## （这确实是一个 seaborn 散点图！）

[](https://medium.com/@lee_vaughan?source=post_page-----c66979cb17e1--------------------------------)[![Lee Vaughan](../Images/9f6b90bb76102f438ab0b9a4a62ffa3f.png)](https://medium.com/@lee_vaughan?source=post_page-----c66979cb17e1--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c66979cb17e1--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----c66979cb17e1--------------------------------) [Lee Vaughan](https://medium.com/@lee_vaughan?source=post_page-----c66979cb17e1--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F5d604015c08b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuild-a-better-bar-chart-with-this-trick-c66979cb17e1&user=Lee+Vaughan&userId=5d604015c08b&source=post_page-5d604015c08b----c66979cb17e1---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c66979cb17e1--------------------------------) · 7 分钟阅读 · 2023年8月26日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc66979cb17e1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuild-a-better-bar-chart-with-this-trick-c66979cb17e1&user=Lee+Vaughan&userId=5d604015c08b&source=-----c66979cb17e1---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc66979cb17e1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuild-a-better-bar-chart-with-this-trick-c66979cb17e1&source=-----c66979cb17e1---------------------bookmark_footer-----------)![](../Images/aa181f1216654de580fb8ce4d78dbe26.png)

“国会年龄”散点图的一部分（所有图片由作者提供）

每当我需要寻找有效可视化的灵感时，我会浏览 [*The Economist*](https://www.economist.com/)、[*Visual Capitalist*](https://www.visualcapitalist.com/) 或 [*The Washington Post*](https://www.washingtonpost.com/)。在其中一次浏览中，我发现了一个有趣的信息图表——类似于上面展示的那个——它绘制了每位美国国会议员的年龄与其代际群体的关系。

我最初的印象是这是一种*水平条形图*，但经过仔细检查后发现每条条形图由多个*标记*组成，这使它成为了一种*散点图*。每个标记代表一位国会议员。

在这个 *快速成功数据科学* 项目中，我们将使用Python、pandas和seaborn重新创建这个吸引人的图表。在此过程中，我们将揭示一系列你可能不知道的标记类型。

# 数据集

由于美国有 [*候选年龄*](https://en.wikipedia.org/wiki/Age_of_candidacy_laws_in_the_United_States) 法律，国会议员的生日属于公开记录。你可以在多个地方找到它们，包括 [*美国国会传记目录*](https://bioguideretro.congress.gov/) 和 [维基百科](https://en.wikipedia.org/wiki/List_of_current_members_of_the_United_States_House_of_Representatives)。

为了方便，我已经编译了一个包含当前国会议员姓名、生日、政府部门和党派的CSV文件，并将其存储在这个 [Gist](https://gist.github.com/rlvaugh/35069885b74ca52a63aab217863440e0) 中。
