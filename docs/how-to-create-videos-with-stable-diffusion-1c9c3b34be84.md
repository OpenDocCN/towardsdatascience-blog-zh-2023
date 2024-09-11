# 如何使用 Stable Diffusion 和 Deform 创建插值视频

> 原文：[https://towardsdatascience.com/how-to-create-videos-with-stable-diffusion-1c9c3b34be84?source=collection_archive---------1-----------------------#2023-02-17](https://towardsdatascience.com/how-to-create-videos-with-stable-diffusion-1c9c3b34be84?source=collection_archive---------1-----------------------#2023-02-17)

## 生成式 AI

## 使用帧插值技术通过 Stable Diffusion 和 Deforum 创建视频

[](https://medium.com/@oscarleo?source=post_page-----1c9c3b34be84--------------------------------)[![Oscar Leo](../Images/7733c9147bad2875a35155fca3903aa8.png)](https://medium.com/@oscarleo?source=post_page-----1c9c3b34be84--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1c9c3b34be84--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----1c9c3b34be84--------------------------------) [Oscar Leo](https://medium.com/@oscarleo?source=post_page-----1c9c3b34be84--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fd7e5c1ca65b7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-create-videos-with-stable-diffusion-1c9c3b34be84&user=Oscar+Leo&userId=d7e5c1ca65b7&source=post_page-d7e5c1ca65b7----1c9c3b34be84---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1c9c3b34be84--------------------------------) ·6分钟阅读·2023年2月17日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F1c9c3b34be84&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-create-videos-with-stable-diffusion-1c9c3b34be84&source=-----1c9c3b34be84---------------------bookmark_footer-----------)![](../Images/10a84d5f8ec2a71a26f5a620cabc9241.png)

图片由作者使用 [DeForum](https://github.com/deforum-art/deforum-stable-diffusion) 生成

# 介绍

无论你在哪里看，都会看到由算法如 Stable Diffusion 和 Midjourney 生成的图像。然而，视频则是一个更具挑战性的前景。

我在一家媒体制作公司工作，AI 视频在成为行业内广泛使用之前，还有很长的路要走。质量差距仍然很大。

## 挑战

存在多个挑战，例如帧之间的闪烁、分辨率和计算等。

我还应该提到关于从互联网内容中训练算法的版权问题的持续辩论。很明显，当前的方法过于宽松。我将对此发展保持极大的兴趣。

## 当前的使用案例

然而，媒体制作的核心在于创建引人入胜且独特的内容，而人工智能无疑可以成为其中的一个组成部分。

一个使用案例是给原始视频添加不同的风格，就像他们在[《乌鸦》](https://www.youtube.com/watch?v=5dvxY6vXHsA)中做的那样，他们首先录制了一个……
