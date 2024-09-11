# 使用Facebook的Prophet进行时间序列预测，10分钟 — 第1部分

> 原文：[https://towardsdatascience.com/time-series-forecasting-with-facebooks-prophet-in-10-minutes-958bd1caff3f?source=collection_archive---------2-----------------------#2023-03-23](https://towardsdatascience.com/time-series-forecasting-with-facebooks-prophet-in-10-minutes-958bd1caff3f?source=collection_archive---------2-----------------------#2023-03-23)

## 使用6行代码构建一个可用模型

[](https://guillaume-weingertner.medium.com/?source=post_page-----958bd1caff3f--------------------------------)[![Guillaume Weingertner](../Images/fbfb34af986a7788394b6033c6954d57.png)](https://guillaume-weingertner.medium.com/?source=post_page-----958bd1caff3f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----958bd1caff3f--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----958bd1caff3f--------------------------------) [Guillaume Weingertner](https://guillaume-weingertner.medium.com/?source=post_page-----958bd1caff3f--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4ebea49e580e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftime-series-forecasting-with-facebooks-prophet-in-10-minutes-958bd1caff3f&user=Guillaume+Weingertner&userId=4ebea49e580e&source=post_page-4ebea49e580e----958bd1caff3f---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----958bd1caff3f--------------------------------) · 6分钟阅读 · 2023年3月23日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F958bd1caff3f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftime-series-forecasting-with-facebooks-prophet-in-10-minutes-958bd1caff3f&user=Guillaume+Weingertner&userId=4ebea49e580e&source=-----958bd1caff3f---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F958bd1caff3f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftime-series-forecasting-with-facebooks-prophet-in-10-minutes-958bd1caff3f&source=-----958bd1caff3f---------------------bookmark_footer-----------)![](../Images/5229e96a7d81abdd74c0b1b14ccefc02.png)

Prophet的输出 — 作者提供的图像

# #1 动机

时间序列预测模型在商业决策过程中带来的附加价值是不可否认的。我最近构建了一个，其影响如此之大，我认为应该在这里分享这种方法。

在尝试预测目标变量的未来值时，主要有2类预测模型：

**传统时间序列模型** 和 **机器学习模型**。

![](../Images/a3d728784e3205eecb95c58c4347ae43.png)

时间序列模型 — 图片由作者提供

+   使用**单变量时间序列模型**时，其思想是仅基于目标变量（我们试图预测的变量）的过去数据的趋势和季节性来预测未来值，而不考虑其他因素。多变量模型是这一思想的扩展，其中输入可以是多个时间序列。

+   使用**（监督）机器学习模型**时，其思想是利用其他解释变量，并建立它们与目标变量的关系来进行预测。
