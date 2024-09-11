# 使用 Python 构建美观折线图的 5 个步骤

> 原文：[`towardsdatascience.com/5-steps-to-build-beautiful-line-charts-with-python-655ac5477310?source=collection_archive---------3-----------------------#2023-10-27`](https://towardsdatascience.com/5-steps-to-build-beautiful-line-charts-with-python-655ac5477310?source=collection_archive---------3-----------------------#2023-10-27)

## 如何充分利用 Matplotlib 的功能来讲述更引人入胜的故事

[](https://guillaume-weingertner.medium.com/?source=post_page-----655ac5477310--------------------------------)![Guillaume Weingertner](https://guillaume-weingertner.medium.com/?source=post_page-----655ac5477310--------------------------------)[](https://towardsdatascience.com/?source=post_page-----655ac5477310--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----655ac5477310--------------------------------) [Guillaume Weingertner](https://guillaume-weingertner.medium.com/?source=post_page-----655ac5477310--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4ebea49e580e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F5-steps-to-build-beautiful-line-charts-with-python-655ac5477310&user=Guillaume+Weingertner&userId=4ebea49e580e&source=post_page-4ebea49e580e----655ac5477310---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----655ac5477310--------------------------------) ·7 分钟阅读·2023 年 10 月 27 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F655ac5477310&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F5-steps-to-build-beautiful-line-charts-with-python-655ac5477310&user=Guillaume+Weingertner&userId=4ebea49e580e&source=-----655ac5477310---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F655ac5477310&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F5-steps-to-build-beautiful-line-charts-with-python-655ac5477310&source=-----655ac5477310---------------------bookmark_footer-----------)![](img/3e969e64f1b6bb20d697426737e9786d.png)

5 个步骤来构建美观的折线图 — 图片由作者提供

# 动机

几个月前，我写了一篇关于柱状图的文章，讨论了如何使它们**清晰**、**自解释**和**视觉上令人愉悦**，以便更好地讲述故事（链接见下）。

[](/5-steps-to-build-beautiful-bar-charts-with-python-3691d434117a?source=post_page-----655ac5477310--------------------------------) ## 使用 Python 构建美观的柱状图的 5 个步骤

### 如何充分利用 Matplotlib 的功能来讲述更引人入胜的故事

[towardsdatascience.com

在这篇文章中，我将探讨**折线图**，这些图表具有其他值得探索的特性。

Matplotlib 使得使用现成的函数快速轻松地绘制数据成为可能，但精细调整步骤则需要更多的努力。

我花了相当多的时间研究最佳实践，以便你可以创建引人入胜的图表，而无需自己摸索。

这个想法是从这个开始……

![](img/2b1aa23ba1dd499314f82296ffd0e9de.png)

… 到那一点：
