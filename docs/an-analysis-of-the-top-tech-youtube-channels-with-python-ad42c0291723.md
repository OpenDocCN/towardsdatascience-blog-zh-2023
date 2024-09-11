# 用 Python 分析顶级科技 YouTube 频道

> 原文：[`towardsdatascience.com/an-analysis-of-the-top-tech-youtube-channels-with-python-ad42c0291723?source=collection_archive---------5-----------------------#2023-09-08`](https://towardsdatascience.com/an-analysis-of-the-top-tech-youtube-channels-with-python-ad42c0291723?source=collection_archive---------5-----------------------#2023-09-08)

## 使用 YouTube API 理解顶级 YouTube 科技频道的表现

[](https://guillaume-weingertner.medium.com/?source=post_page-----ad42c0291723--------------------------------)![Guillaume Weingertner](https://guillaume-weingertner.medium.com/?source=post_page-----ad42c0291723--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ad42c0291723--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----ad42c0291723--------------------------------) [Guillaume Weingertner](https://guillaume-weingertner.medium.com/?source=post_page-----ad42c0291723--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4ebea49e580e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fan-analysis-of-the-top-tech-youtube-channels-with-python-ad42c0291723&user=Guillaume+Weingertner&userId=4ebea49e580e&source=post_page-4ebea49e580e----ad42c0291723---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ad42c0291723--------------------------------) · 9 分钟阅读 · 2023 年 9 月 8 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fad42c0291723&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fan-analysis-of-the-top-tech-youtube-channels-with-python-ad42c0291723&user=Guillaume+Weingertner&userId=4ebea49e580e&source=-----ad42c0291723---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fad42c0291723&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fan-analysis-of-the-top-tech-youtube-channels-with-python-ad42c0291723&source=-----ad42c0291723---------------------bookmark_footer-----------)![](img/3c9e03b46832b007aa1cde6d70ac1d43.png)

[Szabo Viktor](https://unsplash.com/fr/@vmxhu?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 的照片来自 [Unsplash](https://unsplash.com/fr/photos/UfseYCHvIH0?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# #0 YouTube API

你知道 YouTube 有 API 吗？你知道这个 API 可以用来获取你需要的所有数据以进行一个酷炫的数据科学项目吗？好吧，现在你知道了。

在本文中，我们将演示如何使用它来获取丰富的数据集，以便分析和比较顶级科技频道。

为了能够向 YouTube API 发出请求，我们需要遵循以下步骤：

1.  在你的 [Google 开发者控制台](https://console.developers.google.com/) 创建一个新项目——你只需要一个 Google 账户即可完成此操作

1.  在“凭据”选项卡中点击“创建凭据”以请求 API 密钥

1.  通过点击仪表板上的“启用 API 和服务”，然后搜索并勾选“YouTube Data API v3**”**来启用 YouTube API 服务

现在我们已经准备好调用 API 来获取数据。

如果需要，可以在 Google 文档中找到这些解释的更详细版本：
