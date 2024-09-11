# 分水器和淋水屏对浓缩咖啡的影响

> 原文：[https://towardsdatascience.com/impact-of-diffuser-and-shower-screens-on-espresso-6df135a0c50a?source=collection_archive---------12-----------------------#2023-06-16](https://towardsdatascience.com/impact-of-diffuser-and-shower-screens-on-espresso-6df135a0c50a?source=collection_archive---------12-----------------------#2023-06-16)

## 咖啡数据科学

## 理解堆栈

[](https://rmckeon.medium.com/?source=post_page-----6df135a0c50a--------------------------------)[![Robert McKeon Aloe](../Images/ab747f7e39f9f4fdf10d92041d4dc37c.png)](https://rmckeon.medium.com/?source=post_page-----6df135a0c50a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6df135a0c50a--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----6df135a0c50a--------------------------------) [Robert McKeon Aloe](https://rmckeon.medium.com/?source=post_page-----6df135a0c50a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fae592466d35f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimpact-of-diffuser-and-shower-screens-on-espresso-6df135a0c50a&user=Robert+McKeon+Aloe&userId=ae592466d35f&source=post_page-ae592466d35f----6df135a0c50a---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6df135a0c50a--------------------------------) ·3分钟阅读·2023年6月16日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F6df135a0c50a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimpact-of-diffuser-and-shower-screens-on-espresso-6df135a0c50a&user=Robert+McKeon+Aloe&userId=ae592466d35f&source=-----6df135a0c50a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F6df135a0c50a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimpact-of-diffuser-and-shower-screens-on-espresso-6df135a0c50a&source=-----6df135a0c50a---------------------bookmark_footer-----------)

我对Decent Espresso机器的群头进行了大量测试，并对水分布进行了更仔细的观察。因此，我想要了解淋水屏、分水器和水分配器在水进入咖啡饼中的操作方式。当然，其中一些部件是为了保持群头的清洁，但我对几个问题进行了探索：

1.  淋水屏有助于萃取吗？

1.  分水器有助于萃取吗？

# 测试参数

我用6个月的咖啡进行了这项测试，这意味着咖啡几乎没有CO2或仅有很少的CO2。然后我做了腊肠咖啡，并测量了TDS。

[**总溶解固体 (TDS)**](/coffee-solubility-in-espresso-an-initial-study-88f78a432e2c) 是使用折射仪测量的，这个数值与 shot 的输出重量和咖啡的输入重量结合，用于确定杯中提取的咖啡百分比，这称为 **提取产率 (EY)**。

对于 shot 配方，我在 90°C 下进行了一次平坦的配方，流速为 2 ml/s。这个配方的结束有一个长时间的缓慢下降，以尽量避免将咖啡粉吸入组头。

对于剂量，我不得不增加剂量以减少头部空间。我也不知道应该增加多少，所以我调整了压粉。我先处理了一半的咖啡粉，然后处理第二半。我没有压第二半。

缺少压粉步骤使得头部空间很少，而不知道确切的最小值。因此可以多做一些研究并调整一些参数，但我没有。我希望获得一些数据指导下的直觉。

# 数据

没有淋浴屏幕我有点惊讶 shot 看起来如此正常。

![](../Images/b05ac5cdcc7f4544699ff2707617e6bb.png)

这个 shot 看起来相当典型。

![](../Images/0d8a9def3f099864045dc3e314edf161.png)

然后我移除了扩散器，我们孤注一掷了。

![](../Images/70a9561b4ffbc3faba313def1c6c7aa6.png)

这次的 shot 看起来也很正常。我原以为会有明显的侧向通道现象，但一旦加压，情况还是一样。

![](../Images/ac6e291a515836a4a109890b543fabb5.png)

在测量方面，提取产率没有显著差异。

![](../Images/d12748546b018435c18e51d38d1c6f08.png)

我确实期望淋浴屏幕和扩散器对 shot 质量有积极影响。这些结果并不具有决定性或适用于所有机器，但至少，它们指向了关于组头设计的公众知识缺口。设计不能仅仅基于单次 shot，而应该通过保持设备更长时间的清洁来适应多次 shot。

如果你喜欢，可以在 [Twitter](https://mobile.twitter.com/espressofun)、[YouTube](https://m.youtube.com/channel/UClgcmAtBMTmVVGANjtntXTw) 和 [Instagram](https://www.instagram.com/espressofun/) 上关注我，我会发布不同机器上制作的浓缩咖啡视频以及与浓缩咖啡相关的内容。你也可以在 [LinkedIn](https://www.linkedin.com/in/dr-robert-mckeon-aloe-01581595) 找到我。你还可以在 [Medium](https://towardsdatascience.com/@rmckeon/follow) 上关注我，并 [订阅](https://rmckeon.medium.com/subscribe)。

# [我的进一步阅读](https://rmckeon.medium.com/story-collection-splash-page-e15025710347):

[我的书](https://www.kickstarter.com/projects/espressofun/engineering-better-espresso-data-driven-coffee)

[我的链接](https://rmckeon.medium.com/my-links-5de9eb69c26b)

[浓缩咖啡文章汇集](https://rmckeon.medium.com/a-collection-of-espresso-articles-de8a3abf9917?postPublishedType=repub)

[工作与学校故事集](https://rmckeon.medium.com/a-collection-of-work-and-school-stories-6b7ca5a58318)
