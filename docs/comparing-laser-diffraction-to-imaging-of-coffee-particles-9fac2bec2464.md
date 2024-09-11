# 比较激光衍射与咖啡颗粒成像

> 原文：[https://towardsdatascience.com/comparing-laser-diffraction-to-imaging-of-coffee-particles-9fac2bec2464?source=collection_archive---------21-----------------------#2023-04-11](https://towardsdatascience.com/comparing-laser-diffraction-to-imaging-of-coffee-particles-9fac2bec2464?source=collection_archive---------21-----------------------#2023-04-11)

## 咖啡数据科学

## 分裂豆子

[](https://rmckeon.medium.com/?source=post_page-----9fac2bec2464--------------------------------)[![Robert McKeon Aloe](../Images/ab747f7e39f9f4fdf10d92041d4dc37c.png)](https://rmckeon.medium.com/?source=post_page-----9fac2bec2464--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9fac2bec2464--------------------------------)[![数据科学前沿](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----9fac2bec2464--------------------------------) [Robert McKeon Aloe](https://rmckeon.medium.com/?source=post_page-----9fac2bec2464--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fae592466d35f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcomparing-laser-diffraction-to-imaging-of-coffee-particles-9fac2bec2464&user=Robert+McKeon+Aloe&userId=ae592466d35f&source=post_page-ae592466d35f----9fac2bec2464---------------------post_header-----------) 发表在 [数据科学前沿](https://towardsdatascience.com/?source=post_page-----9fac2bec2464--------------------------------) ·4分钟阅读·2023年4月11日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F9fac2bec2464&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcomparing-laser-diffraction-to-imaging-of-coffee-particles-9fac2bec2464&user=Robert+McKeon+Aloe&userId=ae592466d35f&source=-----9fac2bec2464---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F9fac2bec2464&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcomparing-laser-diffraction-to-imaging-of-coffee-particles-9fac2bec2464&source=-----9fac2bec2464---------------------bookmark_footer-----------)

在过去两年里，我一直在 [使用成像测量咖啡粉分布](https://medium.com/nerd-for-tech/measuring-coffee-grind-distribution-d37a39ffc215)。这对于深入了解咖啡研磨机的工作情况非常有用，我也非常喜欢图像处理的挑战。然而，我一直好奇这些测量与激光衍射的比较。激光衍射通常非常昂贵，但我有机会运行一些样本并与我的方法进行比较。

[**激光衍射**](https://en.wikipedia.org/wiki/Laser_diffraction_analysis)用于粒子分布，利用激光和衍射光栅更准确地测量小颗粒。它输出粒子的平均直径，典型的激光粒子分析仪使用进料管顺序测量颗粒。然后基于颗粒体积制作概率分布。这些机器的价格约为10万美元，这对于咖啡爱好者的探索来说并不经济。

咖啡颗粒也可以通过**成像**进行测量。这涉及到将一份咖啡样本放在纸上，展开/解聚咖啡渣，然后拍摄校准图像。校准图像意味着图像平面可以转换为真实的尺寸测量。

![](../Images/daf6781bc73d517ba52de0975c559ad8.png)

所有图片均由作者提供

我非常喜欢进行形状分析，我认为这比粒子直径更具信息性。然而，我希望能同时拥有激光成像的高精度和相机成像的粒子形状信息。

# 数据

让我们看看一些数据。我有磨碎的咖啡和用过的咖啡。用过的咖啡团聚得更少，但它并不总是代表研磨机的情况。

![](../Images/0701ca124b2cec5cf617ddde4a0ff90c.png)![](../Images/db7c31389eed92b4740f635cd3af4960.png)

所有使用的箱子

在查看所有箱子时，很明显，累积分布不能在不去除激光技术的较低箱子的情况下进行查看。这样可以有更多的对齐，但更好的测试也可以筛选咖啡渣，以确保激光和成像之间的测量是相同的颗粒类型。

![](../Images/019e902b2344dababf0a255e862654fc.png)![](../Images/2d1fe9fa012cd20affc3c56a2c371f69.png)

在我通常的成像测量中，我使用最小直径，因为这是筛选器测量的内容。然而，为了更接近激光测量，我考虑了最小直径和平均直径。对于大于100微米的大颗粒，平均直径似乎更合适，但对于小于100微米的颗粒，最小直径更为适合。

![](../Images/6e1fa88d98840df82c2b43dc8cee0980.png)

我将坚持使用平均直径，因为这更接近激光测量提供的结果。我可以查看拍摄前后的研磨。成像数据表明拍摄后的颗粒变得更细，而激光测量则显示相反的结果。我想知道这是否与较低读数的准确性有关。

![](../Images/6105abdcc27dbc86800f5edafdf0974f.png)![](../Images/b09c58319e833a766278d57ce2b7f726.png)

我绘制了粒子的累积百分比，并将它们相互比较，如果它们相同，这将是一条平坦的线。

![](../Images/c7f4a6903527ca141a6b3aa7c00e085a.png)

这些结果仍然显示出性能差距，但成像似乎在激光数据范围内。我通常不会将这些分布与激光衍射测量进行比较，因此在相同的方法下，变量控制得比与其他测量类型比较时更好。

我仍然更喜欢激光测量提供的3D形状精度以及实际的3D形状，以更好地理解磨豆机，如果有人制作一个桌面激光衍射粒子分析仪，这或许有一天会实现。

如果你喜欢，可以关注我在 [Twitter](https://mobile.twitter.com/espressofun)、[YouTube](https://m.youtube.com/channel/UClgcmAtBMTmVVGANjtntXTw) 和 [Instagram](https://www.instagram.com/espressofun/) 上，我会发布关于不同机器的浓缩咖啡镜头和相关内容的视频。你还可以在 [LinkedIn](https://www.linkedin.com/in/dr-robert-mckeon-aloe-01581595) 找到我。你还可以在 [Medium](https://towardsdatascience.com/@rmckeon/follow) 和 [Subscribe](https://rmckeon.medium.com/subscribe) 上关注我。

# [我的进一步阅读](https://rmckeon.medium.com/story-collection-splash-page-e15025710347):

[我的书](https://www.indiegogo.com/projects/engineering-better-espresso-data-driven-coffee)

[我的链接](https://rmckeon.medium.com/my-links-5de9eb69c26b)

[浓缩咖啡文章合集](https://rmckeon.medium.com/a-collection-of-espresso-articles-de8a3abf9917?postPublishedType=repub)

[工作和学校故事合集](https://rmckeon.medium.com/a-collection-of-work-and-school-stories-6b7ca5a58318)
