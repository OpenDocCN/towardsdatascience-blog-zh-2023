# 更高精度的浓缩咖啡篮成像

> 原文：[https://towardsdatascience.com/higher-precision-imaging-of-espresso-982270300b80?source=collection_archive---------12-----------------------#2023-02-28](https://towardsdatascience.com/higher-precision-imaging-of-espresso-982270300b80?source=collection_archive---------12-----------------------#2023-02-28)

## 咖啡数据科学

## 为了更好地测量每个孔的顶部和底部之间的差异

[](https://rmckeon.medium.com/?source=post_page-----982270300b80--------------------------------)[![Robert McKeon Aloe](../Images/ab747f7e39f9f4fdf10d92041d4dc37c.png)](https://rmckeon.medium.com/?source=post_page-----982270300b80--------------------------------)[](https://towardsdatascience.com/?source=post_page-----982270300b80--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----982270300b80--------------------------------) [Robert McKeon Aloe](https://rmckeon.medium.com/?source=post_page-----982270300b80--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fae592466d35f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhigher-precision-imaging-of-espresso-982270300b80&user=Robert+McKeon+Aloe&userId=ae592466d35f&source=post_page-ae592466d35f----982270300b80---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----982270300b80--------------------------------) ·4 min read·2023年2月28日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F982270300b80&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhigher-precision-imaging-of-espresso-982270300b80&user=Robert+McKeon+Aloe&userId=ae592466d35f&source=-----982270300b80---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F982270300b80&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhigher-precision-imaging-of-espresso-982270300b80&source=-----982270300b80---------------------bookmark_footer-----------)

多年来，摄像技术得到了极大的进步，使人们能够拍摄高质量的图像。虽然这对于计算机视觉应用很有用，但实验设计的简单改动往往可以大幅提高这些图像在特定应用中的质量。进入咖啡！

几年来，我一直在将我的图像处理技能应用于[意式咖啡滤篮](https://rmckeon.medium.com/espresso-baskets-and-related-topics-splash-page-ff10f690a738)。两年前，我尝试对滤篮的顶部和底部进行成像以测量每个孔的形状。然而，当我遇到自动对齐图像的问题时，这项调查暂停了。最近，我又开始重新进行这个工作，不过我使用了手动对齐来改进这个过程。

在收集一些数据时，我意识到我的成像设置也可以更好，所以我们在这里讨论一下。

# 挑战

成像意式咖啡滤篮面临几个挑战：

1.  金属滤篮和反射率

1.  孔很小

1.  相机镜头有曲线

我通过数据收集标准操作程序和后处理调整了许多这些问题。

# **数据收集**

我使用了一些标准化工具：

1.  使用平板屏幕照亮滤篮孔

1.  在一个黑暗的房间中隔离其他光源

1.  减少曝光以处理平板屏幕上的反射，使其从滤篮反射回相机。

# **后处理**

我有一个半自动化的过程来简化处理：

1.  使用蓝色圆圈标记滤篮

1.  手动阈值处理图像

1.  自动去除非孔区域

1.  将任何椭圆形孔调整为圆形。

1.  调整滤篮上的光照

# 如何改进这一过程？

首先，用于测量滤篮顶部的光量应与测量底部的光量匹配。

![](../Images/e80ca11b5181c23617acf88c457943c5.png)

所有图像由作者提供

所以我使用一个纸杯制作了一个领圈，将滤篮固定在上面，使得滤篮顶部（篮子内部）与翻转过来的滤篮底部处于同一高度。

![](../Images/91df198819c7223701afb755841f3398.png)

然后我还调整了光照。全屏幕是校准图像所需的（每毫米的像素数量），但任何不在滤篮下方的光线都会反射到相机（我的手机相机）上，再反射到滤篮上。

为了消除这个问题，我使用了全屏亮度进行校准，并拍摄了另一张中间有白色圆圈的图像。这消除了反射问题。

![](../Images/345baacfecad403ba4dd5421757809f3.png)

但接下来我需要校准！为了确保图像对齐，因为手机可能会移动（即使在支架上），我在应用程序Procreate中手动对齐图像。我以为这会更困难，但使用图层和50%透明度，这一过程非常简单。我还发现了一些有趣的事情，比如VST滤篮的对称性。

![](../Images/5dcf039bacfd2bcb8a857e8d521b841f.png)

我使用校准图像来对齐顶部和底部图像，以便一切都校准到相同的尺度。这涉及到线性缩放和物体旋转，直到孔的对齐最好或最对称。我必须镜像底部图像，以确保孔与顶部图像正确对齐。

![](../Images/4d939f6d6e4b7d941851c8f988f6511a.png)

然后我将这些图片通过我的算法处理。下面是Wafo Classic的假色图像的顶部和底部图像，以描绘孔径大小。图像的顶部（左侧）有一块蓝色斑点，这不是由于其他问题。这是多次拍摄中的一个持久特征。

![](../Images/09608664743c6b15e6c1758a401f50b7.png)![](../Images/e46fdd99356855c0b167e163b0cdcb32.png)

左：从过滤器顶部，右：从过滤器底部（镜像）

这些分布可用于更好地理解过滤篮。

![](../Images/6e288cf3dfd54abe2cd60566bfb131a4.png)

从根本上讲，这是一个帮助回答关于过滤篮和性能的问题的工具。将这些信息与实际的浓缩咖啡表现连接起来还存在其他挑战，但努力追求对性能关键属性的更深入理解仍然是令人愉快的。

如果你喜欢，可以在[Twitter](https://mobile.twitter.com/espressofun)、[YouTube](https://m.youtube.com/channel/UClgcmAtBMTmVVGANjtntXTw)以及[Instagram](https://www.instagram.com/espressofun/)关注我，在这里我会发布不同机器的浓缩咖啡镜头视频和与浓缩咖啡有关的内容。你也可以在[LinkedIn](https://www.linkedin.com/in/dr-robert-mckeon-aloe-01581595)上找到我。也可以在[Medium](https://towardsdatascience.com/@rmckeon/follow)上关注我和[订阅](https://rmckeon.medium.com/subscribe)。

# [我的其他阅读内容](https://rmckeon.medium.com/story-collection-splash-page-e15025710347)：

[我的书](https://www.kickstarter.com/projects/espressofun/engineering-better-espresso-data-driven-coffee)

[我的链接](https://rmckeon.medium.com/my-links-5de9eb69c26b)

[浓缩咖啡文章合集](https://rmckeon.medium.com/a-collection-of-espresso-articles-de8a3abf9917?postPublishedType=repub)

[工作和学校故事合集](https://rmckeon.medium.com/a-collection-of-work-and-school-stories-6b7ca5a58318)
