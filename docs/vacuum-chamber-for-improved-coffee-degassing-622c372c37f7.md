# 改善咖啡脱气的真空腔

> 原文：[`towardsdatascience.com/vacuum-chamber-for-improved-coffee-degassing-622c372c37f7?source=collection_archive---------13-----------------------#2023-06-27`](https://towardsdatascience.com/vacuum-chamber-for-improved-coffee-degassing-622c372c37f7?source=collection_archive---------13-----------------------#2023-06-27)

## 咖啡数据科学

## 再探脱气

[](https://rmckeon.medium.com/?source=post_page-----622c372c37f7--------------------------------)![Robert McKeon Aloe](https://rmckeon.medium.com/?source=post_page-----622c372c37f7--------------------------------)[](https://towardsdatascience.com/?source=post_page-----622c372c37f7--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----622c372c37f7--------------------------------) [Robert McKeon Aloe](https://rmckeon.medium.com/?source=post_page-----622c372c37f7--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fae592466d35f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fvacuum-chamber-for-improved-coffee-degassing-622c372c37f7&user=Robert+McKeon+Aloe&userId=ae592466d35f&source=post_page-ae592466d35f----622c372c37f7---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----622c372c37f7--------------------------------) ·5 分钟阅读·2023 年 6 月 27 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F622c372c37f7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fvacuum-chamber-for-improved-coffee-degassing-622c372c37f7&user=Robert+McKeon+Aloe&userId=ae592466d35f&source=-----622c372c37f7---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F622c372c37f7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fvacuum-chamber-for-improved-coffee-degassing-622c372c37f7&source=-----622c372c37f7---------------------bookmark_footer-----------)

家用烘焙已经是我一段时间以来的热情所在，但我在等待咖啡休息时总是很不耐烦。我曾发现等待的最佳时间是 3 到 5 周，这让我感到挑战。所以我开发了一种 使用水或湿度更快脱气的技术。这将最佳使用时间缩短到了 1 到 2 周，但咖啡在 3 到 4 周时常常会变味，而没有这种处理的情况下则是 6 到 7 周。

# 引入真空腔

我最初购买了一个真空室，以查看是否可以在没有水的情况下更快地脱气。然而，气体的内部压力很高（根据 Samo Smrke 的说法接近 6 巴），将外部压力从 1 巴减少到 0 巴仅会改变气体内部与外部压力之间的差值，从 5 巴变为 6 巴。

![](img/bea7fd79041036cfd0b327d85818489d.png)

所有图片由作者提供

让我们将两者合并：

1.  烘焙咖啡

1.  通过在豆子中添加和混合水分，增加 4%的湿度

1.  静置 12 到 24 小时以便吸收

1.  放入真空室

![](img/8065a8d224658e283335919e7effab32.png)

主要挑战在于脱气会导致真空室压力增加，所以我每天需要开启两次以保持良好的真空状态。我希望水分引起的氧化作用能被真空减小。

首先，我观察了咖啡在真空罐与真空室中的重量损失。真空罐配有手动泵，但其真空效果不如真空室。果然，真空室比罐子减少了更多的重量，我怀疑重量损失的主要成分是二氧化碳。通常咖啡中有 6 mg/g 的二氧化碳，因此 100 克咖啡中应有 0.6 克二氧化碳。

![](img/93d674d42022658cb5007f0ea77ce218.png)

# 设备/技术

意式咖啡机: 优质意式咖啡机, 泵与倒废配置

咖啡磨豆机: [Niche Zero](https://youtu.be/2F_0bPW7ZPw)

咖啡: [家庭烘焙咖啡](https://rmckeon.medium.com/coffee-roasting-splash-page-780b0c3242ea), 中度烘焙（第一次裂纹 + 1 分钟）

冲泡准备: Staccato 压粉

预注水: 长时间，大约 25 秒

[滤篮](https://rmckeon.medium.com/espresso-baskets-and-related-topics-splash-page-ff10f690a738): 20 Wafo Spirit

其他设备: Acaia Pyxis 称, DiFluid R2 TDS 仪

# 性能指标

我使用了两组指标来评估技术之间的差异：最终评分和咖啡提取。

[**最终评分**](https://towardsdatascience.com/@rmckeon/coffee-data-sheet-d95fd241e7f6)是 7 个指标（锐度、丰富度、糖浆感、甜度、酸度、苦味和回味）的评分卡平均值。这些评分当然是主观的，但它们经过了对我口味的校准，并帮助我提高了制作咖啡的水平。评分存在一定的变动。我希望每个指标的评分都能保持一致，但有时细微差别很难把握。

**总溶解固体 (TDS)**是通过折射仪测量的，这个数字结合了咖啡萃取的输出重量和输入重量，用于确定杯中提取的咖啡百分比，称为**提取率 (EY)**。

**强度半径 (IR)**定义为 TDS 与 EY 的控制图上从原点到半径的距离，因此 IR = sqrt(TDS² + EY²)。这个指标有助于在输出产量或冲泡比例之间规范化样本表现。

# 数据

我在 4 次烘焙中抽取了 23 组配对样本，并发现大多数情况下口味有所改善。

编辑：发布后，我在查看数据时发现一些样本对没有对齐。我旨在使样本拍摄时间相近，以便烘焙时间相似。这一变化没有影响口味评分，但由于样本对 DE 与 Kim 的错位，丢失了一个样本对。这一变化确实影响了 EY 和 IR 结果，我已相应修改了文本。

![](img/4aa37571fa5cb6f8b3104e5eac03cea4.png)

我在 EY 和 TDS 上看到了一些改善，但从图表上看不太明显。

![](img/5146f43e0cfcfaab7038616c09f2c5e2.png)

细分味道成分时，发现有积极效果。酸味和苦味没有看到统计上的显著影响。

![](img/81d34a2737da16c267fc51f8e0dcddaf.png)

从更高层次来看，TDS/EY/IR 在统计上没有显著变化。如果收集更多数据，这可能会改变，我对此实验感兴趣，想尝试用热预浸的方法。

除了酸味和苦味之外，所有的味道指标都在统计上有显著改善。

![](img/07a148b054c42847ced00320c3fb95c1.png)

我继续使用这种方法进行脱气，我并不建议他人使用这种技术，除非他们真的想要突破咖啡的界限。我怀疑烘焙师可以有效地利用这种方法来处理他们在吧台上使用的咖啡。像所有建议一样，改善口味的脱气技术也有改进的空间。

如果你愿意，可以关注我在[Twitter](https://mobile.twitter.com/espressofun)、[YouTube](https://m.youtube.com/channel/UClgcmAtBMTmVVGANjtntXTw)和[Instagram](https://www.instagram.com/espressofun/)上的账号，我会发布不同机器上的浓缩咖啡镜头和咖啡相关的内容。你还可以在[LinkedIn](https://www.linkedin.com/in/dr-robert-mckeon-aloe-01581595)找到我。你还可以关注我在[Medium](https://towardsdatascience.com/@rmckeon/follow)上的账号，并[订阅](https://rmckeon.medium.com/subscribe)。

# [我的进一步阅读](https://rmckeon.medium.com/story-collection-splash-page-e15025710347)：

[我的书](https://www.kickstarter.com/projects/espressofun/engineering-better-espresso-data-driven-coffee)

[我的链接](https://rmckeon.medium.com/my-links-5de9eb69c26b)

[意式浓缩文章合集](https://rmckeon.medium.com/a-collection-of-espresso-articles-de8a3abf9917?postPublishedType=repub)

[工作与学校故事合集](https://rmckeon.medium.com/a-collection-of-work-and-school-stories-6b7ca5a58318)
