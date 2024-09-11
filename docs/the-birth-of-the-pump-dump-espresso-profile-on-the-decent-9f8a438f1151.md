# Decent机器上的Pump & Dump浓缩咖啡配置的诞生

> 原文：[https://towardsdatascience.com/the-birth-of-the-pump-dump-espresso-profile-on-the-decent-9f8a438f1151?source=collection_archive---------22-----------------------#2023-01-24](https://towardsdatascience.com/the-birth-of-the-pump-dump-espresso-profile-on-the-decent-9f8a438f1151?source=collection_archive---------22-----------------------#2023-01-24)

## 咖啡数据科学

## 低流量的挑战

[](https://rmckeon.medium.com/?source=post_page-----9f8a438f1151--------------------------------)[![Robert McKeon Aloe](../Images/ab747f7e39f9f4fdf10d92041d4dc37c.png)](https://rmckeon.medium.com/?source=post_page-----9f8a438f1151--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9f8a438f1151--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----9f8a438f1151--------------------------------) [Robert McKeon Aloe](https://rmckeon.medium.com/?source=post_page-----9f8a438f1151--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fae592466d35f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-birth-of-the-pump-dump-espresso-profile-on-the-decent-9f8a438f1151&user=Robert+McKeon+Aloe&userId=ae592466d35f&source=post_page-ae592466d35f----9f8a438f1151---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----9f8a438f1151--------------------------------) ·5分钟阅读·2023年1月24日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F9f8a438f1151&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-birth-of-the-pump-dump-espresso-profile-on-the-decent-9f8a438f1151&user=Robert+McKeon+Aloe&userId=ae592466d35f&source=-----9f8a438f1151---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F9f8a438f1151&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-birth-of-the-pump-dump-espresso-profile-on-the-decent-9f8a438f1151&source=-----9f8a438f1151---------------------bookmark_footer-----------)

我在使用Decent Espresso Machine时的一个挫折是[水分配器](https://medium.com/@rmckeon/water-distribution-for-espresso-cd361bc0734)。它导致低流量、高温度情况下的萃取偏向左侧，这对[staccato](/staccato-espresso-is-fundamentally-better-than-regular-espresso-e73e88337127)浓缩咖啡特别具有挑战性。因此，我决定改进我的配置，以便再次制作staccato咖啡。

# 使用过的咖啡

我不想浪费大量咖啡，所以我的调查从使用过的咖啡开始。

> 我能否将水均匀地推送到staccato咖啡的底层？

我用Fellow Shimy筛选了干的、用过的咖啡渣，并用底部6克细磨和顶部13克粗磨制作了staccato shots。

![](../Images/f20e3ed435ef3ddb886470186cd94cc2.png)

第一次测试运行了我的[Vaporexpress](/debugging-vaporexpress-profile-espresso-shots-bfe050dcca0a)配置文件4秒。咖啡被蒸了，但触摸时不湿。

![](../Images/15dbc943ddd3587e69d1679f38d98fcc.png)

我拿着这咖啡重新制作了相同的 shot，但我运行了9秒。然而，它速度很快，咖啡出来时非常湿。我怀疑是因为咖啡已经被蒸过。

![](../Images/3f86f9fa9e2e55a8a04a086ac4605740.png)

我用干的、用过的咖啡渣重新进行了6.5秒的测试，但结果与第一次测试相同。

我接着进行了10秒，但有一侧出现了通道。

![](../Images/9d40fa7968ca1b8f125d924fda68b972.png)![](../Images/8cc9b1b12a872afa912c41c244efd478.png)

然后我尝试了一个[双重双重](/designing-the-double-double-sudo-staccato-espresso-shot-5c435c292672)和一些延长的预浸泡。

# 无流量

我查看了蒸汽PI然后没有流量（0 ml/s流量）。我考虑将其作为脉冲动作进行。结果很有趣，似乎更均匀。

![](../Images/cb108db477a74fc4a86db67a23f0067b.png)

压力没有下降，我询问了DE Diaspora。我被告知尝试将压力设置为0 bar，这样可以排出水。

![](../Images/c5769bcbf43abe046fb1ce087f2cb650.png)

我用真正的咖啡迭代了几次，并将排水时间缩短至5秒。我不知道在蒸汽预浸泡期间的最佳泵送和排水次数。

![](../Images/5455e7e84401243dfa2253f427e4fe94.png)

然后我将结束的斜坡改为两个斜坡，中间有一个排水。

![](../Images/31e0156e4cd0112771ecbf6797fbf782.png)

我还明确测量了废水，以了解用了多少水。我惊讶地发现水的TDS只有0.05%，这意味着废水中咖啡含量非常少。

![](../Images/7216fd8b4a324c0583d81a6056aa7b81.png)

我使用了DE的输入估算和这些测量数据来估算因蒸汽损失了多少水。

![](../Images/a821d55bcd224ae02329b31bf9faf8bb.png)

# 设备/技术

[浓缩咖啡机](/taxonomy-of-lever-espresso-machines-f32d111688f1): [优质浓缩咖啡机](/developing-a-decent-profile-for-espresso-c2750bed053f)

[咖啡研磨机](/rok-beats-niche-zero-part-1-7957ec49840d): [Niche Zero](https://youtu.be/2F_0bPW7ZPw)

咖啡: [自家烘焙咖啡](https://rmckeon.medium.com/coffee-roasting-splash-page-780b0c3242ea), 中度（第一次爆裂+1分钟）

备料: [Staccato 压实](/staccato-tamping-improving-espresso-without-a-sifter-b22de5db28f6)

[过滤篮](https://rmckeon.medium.com/espresso-baskets-and-related-topics-splash-page-ff10f690a738): Wafo Classic

其他设备: [Atago TDS 计](/affordable-coffee-solubility-tools-tds-for-espresso-brix-vs-atago-f8367efb5aa4), [Acaia Pyxis 秤](/data-review-acaia-scale-pyxis-for-espresso-457782bafa5d)

# 性能指标

我使用了两组[指标](/metrics-of-performance-espresso-1ef8af75ce9a)来评估技术之间的差异：最终评分和咖啡提取。

[**最终评分**](https://towardsdatascience.com/@rmckeon/coffee-data-sheet-d95fd241e7f6)是基于7个指标（Sharp、Rich、Syrup、Sweet、Sour、Bitter和Aftertaste）的评分卡的平均值。这些评分当然是主观的，但它们是根据我的口味进行校准的，并帮助我改进我的咖啡制作。这些评分有一些变化。我的目标是每个指标的一致性，但有时颗粒度很难把握。

[**溶解固体总量（TDS）**](/coffee-solubility-in-espresso-an-initial-study-88f78a432e2c)是使用折射计测量的，这个数字与咖啡的输出重量和输入重量结合，用于确定杯中提取的咖啡百分比，称为**提取率（EY）**。

[**强度半径（IR）**](/improving-coffee-extraction-metrics-intensity-radius-bb31e266ca2a)被定义为TDS与EY的控制图上原点的半径，因此IR = sqrt( TDS² + EY²)。这个指标有助于在输出产量或冲泡比例之间标准化咖啡制作表现。

# 可行性数据

在这些拍摄中，味道有了显著的提升，TDS/EY/IR 也有所改进。这一档案还需要更多的工作，但这些结果给了我很大的希望。

![](../Images/24ca6d8fcc9fc7630c1b75fcde824ba0.png)![](../Images/e398bf560d1c33e6178e94a3b2d85b50.png)

# 理论

我对蒸汽的理论是，蒸汽将可溶物推向咖啡饼底部。因此，当咖啡饼顶部的水排出时，水中几乎没有可溶物。由于蒸汽的作用，可溶物会卡在咖啡饼的更深处。这就是为什么多次泵与倾倒不会将可溶物从咖啡中去除到废水中的原因。

我完全意识到这个档案看起来非常疯狂，特别是因为它与典型的9 bar浓缩咖啡非常不同。这个档案的压力甚至没有达到2 bar，但提取效率简直疯狂。这个档案让我对达到 [Kim Express](/designing-the-double-double-sudo-staccato-espresso-shot-5c435c292672) 的表现甚至更高的表现充满希望。

> 未来充满希望，泵与倾倒！

如果你愿意，可以关注我的 [Twitter](https://mobile.twitter.com/espressofun)、[YouTube](https://m.youtube.com/channel/UClgcmAtBMTmVVGANjtntXTw) 和 [Instagram](https://www.instagram.com/espressofun/)，我会在这些平台上发布不同机器的意式浓缩咖啡制作视频和咖啡相关的内容。你也可以在 [LinkedIn](https://www.linkedin.com/in/dr-robert-mckeon-aloe-01581595) 找到我。你还可以关注我的 [Medium](https://towardsdatascience.com/@rmckeon/follow) 和 [订阅](https://rmckeon.medium.com/subscribe)。

# [我的进一步阅读](https://rmckeon.medium.com/story-collection-splash-page-e15025710347)：

[我的书](https://www.kickstarter.com/projects/espressofun/engineering-better-espresso-data-driven-coffee)

[我的链接](https://rmckeon.medium.com/my-links-5de9eb69c26b)

[浓缩咖啡文章合集](https://rmckeon.medium.com/a-collection-of-espresso-articles-de8a3abf9917?postPublishedType=repub)

[工作和学校故事合集](https://rmckeon.medium.com/a-collection-of-work-and-school-stories-6b7ca5a58318)
