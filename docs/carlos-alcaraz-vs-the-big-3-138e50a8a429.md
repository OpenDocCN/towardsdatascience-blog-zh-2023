# 卡洛斯·阿尔卡拉斯与三大巨头

> 原文：[https://towardsdatascience.com/carlos-alcaraz-vs-the-big-3-138e50a8a429?source=collection_archive---------1-----------------------#2023-07-29](https://towardsdatascience.com/carlos-alcaraz-vs-the-big-3-138e50a8a429?source=collection_archive---------1-----------------------#2023-07-29)

## 一张视觉数据对比图，展示了新兴网球明星与罗杰、纳达尔和德约科维奇的对比

[](https://medium.com/@asawhney27?source=post_page-----138e50a8a429--------------------------------)[![Abhi Sawhney](../Images/21f712d1a325a71c58fe456e93e4c746.png)](https://medium.com/@asawhney27?source=post_page-----138e50a8a429--------------------------------)[](https://towardsdatascience.com/?source=post_page-----138e50a8a429--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----138e50a8a429--------------------------------) [Abhi Sawhney](https://medium.com/@asawhney27?source=post_page-----138e50a8a429--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F42ce11c2a627&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcarlos-alcaraz-vs-the-big-3-138e50a8a429&user=Abhi+Sawhney&userId=42ce11c2a627&source=post_page-42ce11c2a627----138e50a8a429---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----138e50a8a429--------------------------------) ·6分钟阅读·2023年7月29日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F138e50a8a429&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcarlos-alcaraz-vs-the-big-3-138e50a8a429&user=Abhi+Sawhney&userId=42ce11c2a627&source=-----138e50a8a429---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F138e50a8a429&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcarlos-alcaraz-vs-the-big-3-138e50a8a429&source=-----138e50a8a429---------------------bookmark_footer-----------)![](../Images/8f9d4e01fbb6c34b982ca7cbe8d62b7d.png)

照片由[Filip Mroz](https://unsplash.com/@mroz?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)拍摄，发布在[Unsplash](https://unsplash.com/photos/people-at-the-tennis-court-stadium-during-sunset--WgTWXb4nh4?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)

# 背景

几十年来，男子网球与其主宰的三大巨头——**罗杰·费德勒**、**拉斐尔·纳达尔**和**诺瓦克·德约科维奇**几乎是同义词。这是一个独特的网球时代，如同在其他任何运动中一样，三位有史以来最成功的球员几乎都在同一代里竞技。在他们之间，这三大巨头（如下面的绿色区域所示）自2003年以来赢得了82场大满贯赛事中的65场。

![](../Images/48d5711a48536174fda9b8e93f7bffcf.png)

图表 1

然而，随着 Roger 于 2022 年退役、Rafa 由于伤病目前缺席，以及 Novak 最近在一场史诗般的温网决赛中输给 Carlos，似乎传递接力棒的过程已经开始。

随着网球开始设想没有大三巨头的世界，球迷和专家们一直在想是否有球员能够再次拥有像 Roger、Rafa 或 Novak 那样辉煌的职业生涯，赢得超过 20 个大满贯。一两年前，甚至思考这样的问题都是愚蠢的，更不用说大声问出来了。

随着 20 岁的世界第一 Carlos Alcaraz 的惊人崛起，网球界不得不暂停并重新考虑。如今，考虑这个想法已不那么具争议性。作为网球迷，你可能会看到大量比较 Carlos 与大三巨头的评论和媒体报道。然而，很少有这些讨论将 Carlos 到目前为止的表现与大三巨头在职业生涯早期的表现进行比较。将 Carlos 在第十个大满贯正赛出场的水平与 Novak 在第七十一场的水平进行比较，虽然令人兴奋，但并未提供完整的画面。

## **数据来源与分析范围**

作为一个网球迷和数据专业人士，我一直期待看到更多的同类比较，比较球员在职业生涯相似阶段的表现。幸运的是，得益于 Jeff Sackmann 在通过他的 [tennis_atp](https://github.com/JeffSackmann/tennis_atp) GitHub 仓库提供巡回赛 ATP 比赛数据的伟大工作，这样的分析成为可能。这个仓库，更广泛地说，他的 [GitHub](https://github.com/JeffSackmann) 是任何对网球分析感兴趣的人的极好资源。另一方面，你可以在我的 [GitHub](https://github.com/asawhney27/Tennis-Analytics) 上找到本文中用于分析和视觉呈现的所有 Python 代码。

拥有这些数据并在 Pandas 和 Matplotlib 中进行一些分析，我们可以更细致地了解 Carlos 的早期职业生涯与大三巨头的比较。

我们将特别关注大满贯和大师赛1000赛事这两类最受追捧的赛事，并将比较范围扩大到包括 [前十名球员](https://www.atptour.com/en/rankings/singles?rankRange=0-100&rankDate=2023-07-17)，而不仅仅局限于 Carlos。这将有助于为表现提供更多背景和视角。

首先，这里是大三巨头与今天排名前十的球员（截至 2023 年 7 月 17 日）在大满贯和大师赛1000赛事中获胜数量的当前状态比较。

![](../Images/c9d995f088507433c5c69aa9379f5ef7.png)

图表 2

我们可以明确说的是，目前大三巨头与前十名之间的差距相当大。了解 Roger、Rafa 和 Novak 到达现有地位的过程将是有帮助的。

# 比较历程

为了做到这一点，让我们对比一下上面显示的两大主要赛事类别中的球员历程。我们将从大师赛1000赛事开始，然后转到至关重要的大满贯赛事。

## **大师赛1000**

下图绘制了三巨头及七位其他前十名球员的历程，这些球员至少赢得过一个大师赛1000赛事。x轴代表大师赛1000赛事的参与次数（从正赛开始），y轴代表赢得的大师赛1000赛事的数量。在某一点上，球员趋势线越高，说明到那一点的表现越好。

![](../Images/54682c9eb1ef35271366f7247d4c2d31.png)

图表 3

三巨头的长期性和持续成功显而易见，每位三巨头成员参加了超过120个大师赛1000赛事，并赢得了20多个冠军。然而，为了讨论方便，让我们聚焦于上面突出显示的左下角象限，比较两位杰出球员与三巨头的表现。

![](../Images/8384047321e10d60e9ea6ca60f008f7c.png)

图表 4

**达尼尔·梅德韦杰夫**的六个大师赛1000冠军是当前前十名球员中最多的（不包括诺瓦克）。他起步较晚，在第21次出战时赢得了他的第一个大师赛1000冠军。以42次赛事赢得六个冠军，他目前略微领先于诺瓦克在此阶段的表现，并与罗杰持平。

然而，**卡洛斯·阿尔卡拉斯**是最引人注目的。卡洛斯在仅仅第7次出战时便获得了他的第一个大师赛1000冠军，他的起步比三巨头中的任何一位都要快。在参加了16个大师赛1000赛事后，他与拉法在四个冠军上并列领先，而在类似阶段，诺瓦克（2）和罗杰（0）则远远落后。

## **大满贯赛事**

让我们看看这些趋势在最大舞台——大满贯赛事上是否也成立。卡洛斯和达尼尔是前十名中唯一在大满贯赛事中赢得过冠军的球员。让我们与三巨头对比他们的大满贯历程。

![](../Images/d42e13775a41d13f32fe0930802436ec.png)

图表 5

达尼尔在25次出战中仅赢得一个大满贯冠军，目前稍微落后于诺瓦克（2），远远落后于罗杰（5）和拉法（8）。与此同时，卡洛斯则一飞冲天，在诺瓦克和罗杰获得首个大满贯之前就赢得了两个大满贯冠军。

由于每年只有四个大满贯赛事，因此对球员早期职业生涯的大满贯赛事数据集相对有限。为了进一步了解大满贯赛事的表现，我们将数据的粒度从赛事层面提高到比赛层面。

在下图中，x轴代表大满贯比赛的数量（从正赛开始），y轴代表赢得的大满贯比赛数量。

![](../Images/3f5b23023a9c7b17fcc12102409ed2a0.png)

图表 6

这个视图让我们感受到每位球员在淘汰赛中的深度。在44场大满贯比赛后，卡洛斯与拉法在36场胜利上并列第一，如果他在今年晚些时候的美国公开赛首轮比赛中获胜，他将独占榜首。

# 结论

从上述数据可以明显看出，卡洛斯·阿尔卡拉斯是一位绝对的天才。如果我们考虑年龄因素，他的表现更为引人注目。让我们看看这些球员在25岁之前赢得的各项大师1000赛和大满贯赛事的数量。

![](../Images/dc038995a5da66cffa025217166a5dc4.png)

图表 7

作为19岁时最年轻的男子世界第一，且在仅20岁时已经拥有两个大满贯和四个大师1000赛，卡洛斯前途广阔。

但正如大多数历代伟大球员一样，真正特别的是这三位伟大球员能够保持如此长时间的卓越。如果卡洛斯能保持健康，并继续像今天这样主导比赛，而这些图表中的x轴年复一年地延展，大三巨头可能需要腾出一些位置。

*感谢阅读！*

数据来源：*Sackmann, Jeff. GitHub 仓库：* [https://github.com/JeffSackmann/tennis_atp](https://github.com/JeffSackmann/tennis_atp)

分析与视觉笔记本：[https://github.com/asawhney27/Tennis-Analytics](https://github.com/asawhney27/Tennis-Analytics)
