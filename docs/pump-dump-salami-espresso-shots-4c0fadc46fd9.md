# Pump & Dump 萨拉米浓缩咖啡 shot

> 原文：[https://towardsdatascience.com/pump-dump-salami-espresso-shots-4c0fadc46fd9?source=collection_archive---------12-----------------------#2023-03-17](https://towardsdatascience.com/pump-dump-salami-espresso-shots-4c0fadc46fd9?source=collection_archive---------12-----------------------#2023-03-17)

## 咖啡数据科学

## 切分 shot 以了解新的东西

[](https://rmckeon.medium.com/?source=post_page-----4c0fadc46fd9--------------------------------)[![Robert McKeon Aloe](../Images/ab747f7e39f9f4fdf10d92041d4dc37c.png)](https://rmckeon.medium.com/?source=post_page-----4c0fadc46fd9--------------------------------)[](https://towardsdatascience.com/?source=post_page-----4c0fadc46fd9--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----4c0fadc46fd9--------------------------------) [Robert McKeon Aloe](https://rmckeon.medium.com/?source=post_page-----4c0fadc46fd9--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fae592466d35f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpump-dump-salami-espresso-shots-4c0fadc46fd9&user=Robert+McKeon+Aloe&userId=ae592466d35f&source=post_page-ae592466d35f----4c0fadc46fd9---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----4c0fadc46fd9--------------------------------) · 3 分钟阅读 · 2023年3月17日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F4c0fadc46fd9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpump-dump-salami-espresso-shots-4c0fadc46fd9&user=Robert+McKeon+Aloe&userId=ae592466d35f&source=-----4c0fadc46fd9---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F4c0fadc46fd9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpump-dump-salami-espresso-shots-4c0fadc46fd9&source=-----4c0fadc46fd9---------------------bookmark_footer-----------)

咖啡的萃取过程随着时间的推移而发生变化，这一变化贯穿整个浓缩咖啡的萃取过程。为了更好地理解这一点，人们使用了“萨拉米 shot”。制作“萨拉米 shot”时，需要在浓缩咖啡过程中使用多个杯子。我想看看我的新[Pump & Dump](/the-birth-of-the-pump-dump-espresso-profile-on-the-decent-9f8a438f1151) 配方与单个颗粒大小的萃取效果相比如何。

萨拉米 shot 可以用来测量萃取效果，并更好地理解何时不同的味道成分被萃取到杯中。

[**总溶解固体 (TDS)**](/coffee-solubility-in-espresso-an-initial-study-88f78a432e2c) 是使用折射仪测量的，该数字与 shot 的输出重量和咖啡的输入重量结合使用，以确定提取到杯中的咖啡百分比，称为 **提取率 (EY)**。

# 数据

首先，我萃取了一次，并将其与我在 [之前的数据](/the-coffee-bean-is-not-homogenous-sifted-salami-espresso-5b861bfbfbb7) 中的筛过咖啡进行了比较。筛过的咖啡使用了 4 ml/s 流速控制配置，因此能够减少变量。我还分别处理了内部细度和外部细度，并且发现内部细度提取速度极快。

![](../Images/40c3a232228bae7d52337d0d2caac9f7.png)

Pump & Dump 配置提取的细度范围在 <300 微米的内部细度中，这意味着即使是较粗的颗粒也被提取得比之前的数据快得多。

![](../Images/3330b688e84ea5bce0589de8acb2dae1.png)

然后我筛选了咖啡粉，进行了与之前类似的操作，使用了 8 克筛过的咖啡粉和 13 克使用后的咖啡粉。然而，我没有分开内部和外部细度。只有粒径大于 500 微米的颗粒提取速度比整个资料要慢。

![](../Images/0b0ac93c94448278653af6fc36a0fe90.png)

当我们将 Pump & Dump 的细度与 4 ml/s 流速实验中的内部细度进行比较时，前者的萃取速度要高得多。

![](../Images/0222e4e067e47ef665c24c322d91952e.png)

其他粒度的情况也是如此。

![](../Images/7d91aad0546b0f044bcc929a7f485b1f.png)![](../Images/31eaac7ce6235ffd84dedaa15de4e650.png)

在观察 EY 发生的百分比时，Pump & Dump 配置在 1:1 比例中提取了大部分咖啡。

![](../Images/2a38b812acfbf72ee29addb5b9fae17b.png)![](../Images/290f3b7454ed9c85edf39c58f99d542d.png)

这次 salami 咖啡测试进一步展示了该配置在咖啡萃取中的效率。这种配置还表明，如果我们更好地理解咖啡萃取的动态，我们可以显著改善萃取效果。我特别感兴趣的是如何观察热能或热量在咖啡饼中的传递，因为该配置也旨在通过蒸汽加热咖啡粉。

如果你喜欢，可以在 [Twitter](https://mobile.twitter.com/espressofun)、[YouTube](https://m.youtube.com/channel/UClgcmAtBMTmVVGANjtntXTw) 和 [Instagram](https://www.instagram.com/espressofun/) 上关注我，我会在这些平台上发布不同机器上的意式浓缩咖啡镜头和相关内容。你还可以在 [LinkedIn](https://www.linkedin.com/in/dr-robert-mckeon-aloe-01581595) 上找到我，也可以在 [Medium](https://towardsdatascience.com/@rmckeon/follow) 和 [Subscribe](https://rmckeon.medium.com/subscribe) 上关注我。

# [我的进一步阅读](https://rmckeon.medium.com/story-collection-splash-page-e15025710347)：

[我的未来书籍](https://www.kickstarter.com/projects/espressofun/engineering-better-espresso-data-driven-coffee)

[我的链接](https://rmckeon.medium.com/my-links-5de9eb69c26b)

[浓缩文章集](https://rmckeon.medium.com/a-collection-of-espresso-articles-de8a3abf9917?postPublishedType=repub)

[工作与学校故事集](https://rmckeon.medium.com/a-collection-of-work-and-school-stories-6b7ca5a58318)
