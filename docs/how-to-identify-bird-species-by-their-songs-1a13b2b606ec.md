# 如何通过鸟类的歌曲识别鸟类种类？

> 原文：[https://towardsdatascience.com/how-to-identify-bird-species-by-their-songs-1a13b2b606ec?source=collection_archive---------7-----------------------#2023-05-22](https://towardsdatascience.com/how-to-identify-bird-species-by-their-songs-1a13b2b606ec?source=collection_archive---------7-----------------------#2023-05-22)

## 应用机器学习于声音的起步

[](https://medium.com/@jacky.kaub?source=post_page-----1a13b2b606ec--------------------------------)[![Jacky Kaub](../Images/e66c699ee5a9d5bbd58a1a72d688234a.png)](https://medium.com/@jacky.kaub?source=post_page-----1a13b2b606ec--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1a13b2b606ec--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----1a13b2b606ec--------------------------------) [Jacky Kaub](https://medium.com/@jacky.kaub?source=post_page-----1a13b2b606ec--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7ccb7065ef90&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-identify-bird-species-by-their-songs-1a13b2b606ec&user=Jacky+Kaub&userId=7ccb7065ef90&source=post_page-7ccb7065ef90----1a13b2b606ec---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1a13b2b606ec--------------------------------) ·10分钟阅读·2023年5月22日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F1a13b2b606ec&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-identify-bird-species-by-their-songs-1a13b2b606ec&user=Jacky+Kaub&userId=7ccb7065ef90&source=-----1a13b2b606ec---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F1a13b2b606ec&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-identify-bird-species-by-their-songs-1a13b2b606ec&source=-----1a13b2b606ec---------------------bookmark_footer-----------)![](../Images/ece658a10d750c0d4ee2d4b907341888.png)

图片由 [Jan Meeus](https://unsplash.com/@janmeeus?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

你知道声音可以被转换成图像，以供标准卷积网络在机器学习中识别声音是什么吗？

我们将讨论你需要的工具，以快速启动一个新的声音分类任务，并提供一个大致的视角，展示它们如何协同工作。

## 关于本文中使用的数据

为了说明声音分类，我们将使用从[https://xeno-canto.org/](https://xeno-canto.org/)录制的鸟鸣声音。Xeno-canto是一个由社区驱动的全球鸟鸣声音集合，包含了大量的鸟类录音。

在使用这些数据之前，你需要知道每个录音可能有不同的许可证，而在这篇文章中，我选择了**CC BY 4.0**许可证下的记录。

# 关于声音和信号处理的基本知识

声音是物体振动时空气中压力波的结果。这些压力波从源头传到我们这里。耳朵随后处理两个参数：压力波的频率和其振幅。

耳朵接收到的压力波频率被转化为音高：频率越高……
