# 4 个解决特定任务高效的 Pandas 单行代码

> 原文：[`towardsdatascience.com/4-pandas-one-liners-that-surprised-me-in-a-good-way-b67955211f81?source=collection_archive---------1-----------------------#2023-11-23`](https://towardsdatascience.com/4-pandas-one-liners-that-surprised-me-in-a-good-way-b67955211f81?source=collection_archive---------1-----------------------#2023-11-23)

## 以快速简便的方式完成复杂任务

[](https://sonery.medium.com/?source=post_page-----b67955211f81--------------------------------)![Soner Yıldırım](https://sonery.medium.com/?source=post_page-----b67955211f81--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b67955211f81--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----b67955211f81--------------------------------) [Soner Yıldırım](https://sonery.medium.com/?source=post_page-----b67955211f81--------------------------------)

·

[跟随](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2cf6b549448&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F4-pandas-one-liners-that-surprised-me-in-a-good-way-b67955211f81&user=Soner+Y%C4%B1ld%C4%B1r%C4%B1m&userId=2cf6b549448&source=post_page-2cf6b549448----b67955211f81---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b67955211f81--------------------------------) ·5 min read·Nov 23, 2023[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb67955211f81&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F4-pandas-one-liners-that-surprised-me-in-a-good-way-b67955211f81&user=Soner+Y%C4%B1ld%C4%B1r%C4%B1m&userId=2cf6b549448&source=-----b67955211f81---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb67955211f81&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F4-pandas-one-liners-that-surprised-me-in-a-good-way-b67955211f81&source=-----b67955211f81---------------------bookmark_footer-----------)![](img/5569fc0f11be8364c66846bfb0609968.png)

照片由[Tom Bradley](https://unsplash.com/@tomrootstudio?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)拍摄，来源于[Unsplash](https://unsplash.com/photos/brown-squirrel-on-green-grass-during-daytime-Z-ns9qAcvl4?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)

第三方库是为了满足需求而创建和发展的。没有人会坐下来说，“我要创建一个工具，等着别人需要它的时候再用”。相反，他们意识到问题，并想出解决方案来帮助解决。这就是工具产生的方式。

同样的情况适用于向现有工具添加新功能。新功能添加的速度和效果取决于工具的受欢迎程度和背后的团队。

Pandas 确实拥有一个高度活跃的社区，这使得 Pandas 成为数据科学生态系统中最受欢迎的数据分析和清洗库之一。

Pandas 具有解决非常具体的问题和用例的功能。这些功能肯定是社区中活跃用户的需求。

在这篇文章中，我将分享 4 个可以用 Pandas 一行代码完成的操作。这些操作帮助我高效地解决了特定任务，并让我感到惊喜。

## 1\. 从列表中创建一个字典

我有一个项目列表，我想查看这些项目的分布。更具体来说，我想查看唯一值及其数量……
