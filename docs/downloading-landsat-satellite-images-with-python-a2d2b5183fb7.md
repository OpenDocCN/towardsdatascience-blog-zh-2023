# 使用Python下载Landsat卫星图像

> 原文：[https://towardsdatascience.com/downloading-landsat-satellite-images-with-python-a2d2b5183fb7?source=collection_archive---------3-----------------------#2023-05-09](https://towardsdatascience.com/downloading-landsat-satellite-images-with-python-a2d2b5183fb7?source=collection_archive---------3-----------------------#2023-05-09)

## 使用landsatxplore Python包简化Landsat场景下载

[](https://conorosullyds.medium.com/?source=post_page-----a2d2b5183fb7--------------------------------)[![Conor O'Sullivan](../Images/2dc50a24edb12e843651d01ed48a3c3f.png)](https://conorosullyds.medium.com/?source=post_page-----a2d2b5183fb7--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a2d2b5183fb7--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----a2d2b5183fb7--------------------------------) [Conor O'Sullivan](https://conorosullyds.medium.com/?source=post_page-----a2d2b5183fb7--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4ae48256fb37&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdownloading-landsat-satellite-images-with-python-a2d2b5183fb7&user=Conor+O%27Sullivan&userId=4ae48256fb37&source=post_page-4ae48256fb37----a2d2b5183fb7---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----a2d2b5183fb7--------------------------------) ·6分钟阅读·2023年5月9日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa2d2b5183fb7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdownloading-landsat-satellite-images-with-python-a2d2b5183fb7&user=Conor+O%27Sullivan&userId=4ae48256fb37&source=-----a2d2b5183fb7---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa2d2b5183fb7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdownloading-landsat-satellite-images-with-python-a2d2b5183fb7&source=-----a2d2b5183fb7---------------------bookmark_footer-----------)![](../Images/9b6d132a1cbec04005db3935f410aec1.png)

图片由[NASA](https://unsplash.com/@nasa?utm_source=medium&utm_medium=referral)提供，发布在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

Landsat卫星是最常用的地球观测数据来源之一。它们已经提供了超过四十年的高质量地球表面图像。然而，手动下载这些图像可能非常繁琐！幸运的是，通过[landsatxplore](https://github.com/yannforget/landsatxplore)包，你可以轻松地用几行代码下载和处理Landsat场景。

我们将深入了解landsatxplore包，并逐步演示如何使用Python下载Landsat卫星图像。这包括：

+   使用USGS账户设置API访问

+   搜索和过滤Landsat场景

+   在Python中下载和处理场景

告别手动下载，迎接自动化、高效的工作流程！

# 设置landsatxplore

## 第一步：注册USGS

首先，你需要[设置一个USGS账户](https://ers.cr.usgs.gov/)。这是你用来通过[EarthExplorer](http://earthexplorer.usgs.gov)下载场景的同一个账户。请记住你的**用户名**和**密码**，因为稍后我们将需要它们。
