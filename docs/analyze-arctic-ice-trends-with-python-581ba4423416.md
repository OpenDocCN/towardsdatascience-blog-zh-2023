# 使用 Python 分析北极冰层趋势

> 原文：[`towardsdatascience.com/analyze-arctic-ice-trends-with-python-581ba4423416?source=collection_archive---------12-----------------------#2023-06-27`](https://towardsdatascience.com/analyze-arctic-ice-trends-with-python-581ba4423416?source=collection_archive---------12-----------------------#2023-06-27)

## 探索过去的预测

[](https://medium.com/@lee_vaughan?source=post_page-----581ba4423416--------------------------------)![李·沃恩](https://medium.com/@lee_vaughan?source=post_page-----581ba4423416--------------------------------)[](https://towardsdatascience.com/?source=post_page-----581ba4423416--------------------------------)![数据科学前沿](https://towardsdatascience.com/?source=post_page-----581ba4423416--------------------------------) [李·沃恩](https://medium.com/@lee_vaughan?source=post_page-----581ba4423416--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F5d604015c08b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fanalyze-arctic-ice-trends-with-python-581ba4423416&user=Lee+Vaughan&userId=5d604015c08b&source=post_page-5d604015c08b----581ba4423416---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----581ba4423416--------------------------------) ·7 分钟阅读·2023 年 6 月 27 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F581ba4423416&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fanalyze-arctic-ice-trends-with-python-581ba4423416&user=Lee+Vaughan&userId=5d604015c08b&source=-----581ba4423416---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F581ba4423416&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fanalyze-arctic-ice-trends-with-python-581ba4423416&source=-----581ba4423416---------------------bookmark_footer-----------)![](img/ac287aa16b720f0990be1654a45158b7.png)

倾斜的冰岛冰山（作者提供的图片）

*测量* 是 *所有* 科学的基石。没有它，我们如何测试我们的假设？

Python，作为数据科学的杰出编程语言，使得收集、清理和理解测量数据变得容易。通过 Python，我们可以对预测进行回测，验证模型，并追究预测者的责任。

去年，一个[过时的迷因](https://www.dailymail.co.uk/sciencetech/article-2738653/Stunning-satellite-images-summer-ice-cap-thicker-covers-1-7million-square-kilometres-MORE-2-years-ago-despite-Al-Gore-s-prediction-ICE-FREE-now.html)嘲笑阿尔·戈尔的帖子出现在我的 LinkedIn 动态中，并标记了“#灾难夸张”标签。内容涉及他在 2007 年和 2009 年的评论，称北极海将在七年内夏季没有冰。几个事实核查[网站](https://www.politifact.com/factchecks/2021/mar/02/facebook-posts/fact-checking-claims-al-gore-said-all-arctic-ice-w/)验证了这一声明为“基本真实”，并引用了以下引用：

> “一些模型向（维斯拉夫）马斯洛夫博士建议，整个北极冰盖在接下来的五到七年内，某些夏季月份可能完全无冰的概率为 75%。”
> 
> - 阿尔·戈尔，2009 年 12 月

虽然许多人将迷因视为事实，但数据科学家具备深入数据并得出自己结论的能力。在…
