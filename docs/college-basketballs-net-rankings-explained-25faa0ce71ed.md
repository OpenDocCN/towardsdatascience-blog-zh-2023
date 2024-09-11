# 大学篮球的NET排名解析

> 原文：[https://towardsdatascience.com/college-basketballs-net-rankings-explained-25faa0ce71ed?source=collection_archive---------2-----------------------#2023-03-08](https://towardsdatascience.com/college-basketballs-net-rankings-explained-25faa0ce71ed?source=collection_archive---------2-----------------------#2023-03-08)

## 数据科学如何驱动“疯狂三月”

[](https://medium.com/@gspmalloy?source=post_page-----25faa0ce71ed--------------------------------)[![Giovanni Malloy](../Images/e94218e244fab1af845c68e63e5706a1.png)](https://medium.com/@gspmalloy?source=post_page-----25faa0ce71ed--------------------------------)[](https://towardsdatascience.com/?source=post_page-----25faa0ce71ed--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----25faa0ce71ed--------------------------------) [Giovanni Malloy](https://medium.com/@gspmalloy?source=post_page-----25faa0ce71ed--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fa0442a984e63&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcollege-basketballs-net-rankings-explained-25faa0ce71ed&user=Giovanni+Malloy&userId=a0442a984e63&source=post_page-a0442a984e63----25faa0ce71ed---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----25faa0ce71ed--------------------------------) ·12分钟阅读·2023年3月8日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F25faa0ce71ed&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcollege-basketballs-net-rankings-explained-25faa0ce71ed&source=-----25faa0ce71ed---------------------bookmark_footer-----------)![](../Images/2de95da4970838f84c68f13bc9ea1517.png)

图片由 [Jacob Rice](https://unsplash.com/@jrice_photography?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

如果你是大学篮球迷，你一定会迫不及待地等待即将到来的“疯狂三月”。如果你是大学篮球新手，“疯狂三月”是指NCAA锦标赛，旨在为男子篮球一级联赛选出冠军。不论你是新晋粉丝还是资深观众，数据科学在你体验比赛的方式上正发挥着比以往更大的作用。参赛队伍的选择很大程度上依赖于一种数据驱动的算法——NET排名。

作为数据科学家或机器学习工程师，了解该领域如何影响不同的行业，包括体育和娱乐是非常重要的。虽然大学篮球在这一不断增长的趋势中起步较晚，但 NET 排名是一个很好的例子，说明我们如何设计算法会影响结果并激励行为。如果你从事体育分析工作，理解 NET 排名是绝对必要的，但无论你所处的行业如何，大学篮球世界为利用数据科学改进产品和增长收入提供了一个重要的案例研究。

**疯狂三月简要介绍**

对于那些从未听说过疯狂三月的人，这个博客需要一些额外的背景信息：疯狂三月是一个68支球队参加的男子大学篮球锦标赛，每年从三月中旬持续到四月初。锦标赛的获胜者被冠以国家冠军的头衔。锦标赛开始时，有四场叫做“首场四强”的附加赛。在这四场比赛之后，剩下的64支球队被分为四个区域，每个区域包含16支球队，排名从1到16。每个区域的冠军进入被称为“最终四强”的半决赛。

在整个赛季中，讨论的主要内容往往围绕疯狂三月展开。该锦标赛广受关注，通常是朋友间或在拉斯维加斯下注的绝佳借口。在68支参赛球队中，有31支是会议冠军，37支获得了“外卡”资格 [3]。关于这些球队如何在锦标赛中组织的研究和讨论被称为“赛程分析”。然而，赛程分析更多的是艺术而非科学。决定谁获得“外卡”资格是一个持续争论的话题。这正是 NET 排名发挥作用的地方。

**NET 排名介绍**

早在2018年，NCAA首次发布了一种新的排名系统，称为 NCAA 评估工具或 NET [1]。该排名系统是与 Google Cloud 专业服务合作的，旨在提供一个数据驱动的指标来衡量给定大学篮球队的质量。当排名首次发布时，它依赖于五个不同的指标：球队价值指数、净效率、胜率、调整后的胜率和得分差 [2]。然而，从那时起，排名已被调整为仅包括球队价值指数和净效率 [1]。

关于这是否是确定球队质量的最佳系统，体育记者和篮球迷之间确实存在争议。尽管对 NET 排名有各种不同的看法，但 NCAA 选择委员会将其作为决策的基础，用于确定哪些球队获得“外卡”资格以及如何在一个区域内分配排名（这些排名称为种子）。所有这些决策都会影响锦标赛的结果。因此，你可以开始看到数据科学如何构成疯狂三月的基础。

**计算 NET 排名**

NET排名由数据科学驱动。NCAA在2018年推特上发布了这张图表来解释这一指标：

正如你所见，团队价值指数是游戏结果、对手和地点的函数。计算团队价值指数的算法没有公开，因此是一个黑箱，但我们可以确定的是，团队价值指数的重要组成部分与对手的质量相关。NET排名将对手质量细分为四个象限，分别命名为Quad 1、Quad 2、Quad 3和Quad 4。根据[4]，象限的定义如下：

+   Quad 1: “主场对阵NET排名在1–30之间的对手，中立场对阵NET排名在1–50之间的对手，客场对阵NET排名在1–75之间的对手” [4]

+   Quad 2: “主场对阵NET排名在31–75之间的对手，中立场对阵NET排名在51–100之间的对手，客场对阵NET排名在76–135之间的对手” [4]

+   Quad 3: “主场对阵NET排名在76–160之间的对手，中立场对阵NET排名在101–200之间的对手，客场对阵NET排名在135–240之间的对手” [4]

+   Quad 4: “主场对阵NET排名在161–363之间的对手，中立场对阵NET排名在201–363之间的对手，客场对阵NET排名在241–363之间的对手” [4]

Quad系统固有地捕捉了对手实力和比赛地点的特征。因此，无论团队价值指数的结果如何，选拔委员会在分配“外卡”名额和锦标赛种子时，都极为重视Quad 1的胜利和Quad 4的失利。

另一方面，净效率是极为透明的。净效率是进攻效率和防守效率的函数 [2]。进攻效率的计算公式如下：

O = PF/(FGA — OREB+TO+.475*FTA)

其中 O 为进攻效率，PF 为得分（总得分），FGA 为投篮尝试次数（投篮数量），OREB 为进攻篮板，TO 为失误，FTA 为罚球尝试次数 [2]。

防守效率计算公式如下：

D = PA/(Opp_FGA — Opp_OREB+Opp_TO+.475*Opp_FTA)

其中 D 为防守效率，PA 为被得分，Opp_FGA 为对手的投篮尝试次数，Opp_OREB 为对手的进攻篮板，Opp_TO 为对手的失误，Opp_FTA 为对手的罚球尝试次数 [2]。

净效率就是进攻效率和防守效率之间的差值，即 NE = O — D [2]。净效率是一个密集的指标，从整体上反映了球队相对于对手的表现。

**示例NET球队表**

那么，这一切如何汇聚到NCAA选拔委员会呢？答案并不完全明确。显然，他们将能访问NET排名。此外，他们还将获得每支球队的NET表格报告。每支球队的NET表格被分成几个部分。表格的顶部包含NET排名、球队记录信息、赛程强度、对手的平均NET排名、其他基于结果和预测的排名，以及按对手象限和比赛地点细分的胜负记录。表格的下半部分是逐场比赛的球队表现分析，按对手NET排名/象限分为几个部分。我鼓励你查看一些示例NET团队表格，例如我下面提供的[[5](https://www.warrennolan.com/basketball/2023/net-teamsheets-plus)]。

![](../Images/c3f7c9ffcedca4677934969035aed3b8.png)

图片由[NET — NCAA 男子大学篮球的细节报告和团队表格 | WarrenNolan.com](https://www.warrennolan.com/basketball/2023/net-teamsheets-plus)提供，已获许可。

虽然这不是最美观的数据可视化，但信息量很大，紧凑地集中在一个空间里。在团队表格中，Quad 1和Quad 2的比赛进一步分为上半部分和下半部分。此外，注意到非会议比赛用蓝色突出显示，并在上述指标中加以标注。损失用红色突出显示，以便轻松指出糟糕的（Quad 4）损失或出色的（Quad 1）胜利。如你所见，象限系统在数据呈现中发挥了关键作用。

**NET排名的局限性**

我知道有许多篮球迷对NET排名系统持批评态度。没有任何模型是完美的，NET也不例外。不过，我会尝试从数据科学家的角度（结合大学篮球迷的视角）突出一些我看到的NET排名系统的局限性。

我看到的NET排名系统最大的局限性是没有考虑最近的表现[1]。虽然整个赛季的一致性是有价值和值得称赞的，但在赛季中的最佳状态也有其重要性。无论是体能、化学反应还是信心，一切都需要完美对接，才能在三月疯狂期间取得成功。在篮球术语中，这些被称为“无形因素”。这些因素不容易衡量（[虽然有人尝试过](https://www.bruinsportsanalytics.com/post/nba_intangibles)），但它们会随时间变化并影响结果。在计量经济学术语中，这就是模型固有的“[异质性](https://en.wikipedia.org/wiki/Heterogeneity_in_economics)”。

另一个我将其归类为限制的 NET 排名的好奇之处是其初次发布的延迟。NET 排名每天更新，但直到 12 月初才更新——在大多数球队打完 5 到 10 场比赛后。我认为这可能意味着 NET 排名有一个高度不确定的初始化状态。如果我们能看到赛季开始前 NET 排名的初始状态，我认为我们可以获得一些关于算法如何工作的非常有价值的见解。它是否完全是幼稚的，还是有某种[迁移学习](https://en.wikipedia.org/wiki/Transfer_learning)的元素，来自于以往的赛季或其他投票的季前排名？

为了汇总最终的 NET 排名，我假设有某种方式将团队价值指数转换为数值，并与净效率结合，计算出团队质量的加权指标。我承认 NET 排名很可能确实是启发式或其他非 AI 算法。当然，Net Efficiency 统计数据的计算方式表明 NCAA 可能会接受启发式方法。此外，我在将数据科学洞察提供给非技术领域（如健康政策）的经验表明，有时候少即是多。更易于理解的模型有时对决策者更具吸引力。

尽管如此，我的第三个也是最后一个限制依赖于假设这是一个监督学习算法。如果 NET 排名源于一个监督学习算法，那么我想知道训练数据可能来自哪里。基准真相是什么？准确性如何衡量？究竟是什么真正区分了第232队和第233队？即使在将同一队伍与其自身逐年比较时，你也可能在比较大相径庭的阵容。在像均方根误差这样的误差指标中很难找到意义。

**假设基础算法**

那么，NET 排名系统是如何形成的呢？也许我们应该尝试重新创建它？我们确实知道一些确定的事情：

1.  前黄金标准的大学篮球排名统计模型，[RPI 排名系统](https://en.wikipedia.org/wiki/Rating_percentage_index)，是一个优雅但简单的启发式算法。像 NCAA 这样的机构通常不以创新著称，我怀疑大学篮球界是否希望它的皇冠赛事由不可解释的 AI 算法驱动。因此，我的最佳猜测是，机器学习的应用非常有限，如果有的话。回到我之前提到的第三个限制，一个监督学习方法可能会更多麻烦而不值得。

1.  从某种意义上说，NET 排名是[递归的](https://www.geeksforgeeks.org/recursion-practice-problems-solutions/)。一个团队的 NET 排名依赖于其对手的 NET 排名，而这些对手的 NET 排名又依赖于其他对手的 NET 排名，依此类推。NET 排名可能由一种[贝叶斯方法](https://en.wikipedia.org/wiki/Bayesian_inference)驱动，其中对每个团队有一个初始的朴素分布，并在每场比赛后更新该分布。

1.  Google Cloud 专业服务参与其中。这可能是认知偏见或巧妙营销的一个很好的例子，但我希望相信 Google 所涉及的都是最前沿的方法。虽然不一定真实，但与 Google 合作使 NCAA 获得了巨大的计算资源和开发超越传统体育分析领域的方法的能力。即使算法是可解释的，也许结构是复杂的，甚至可能是反直觉的。

1.  历史上的 NET 排名很难找到。经过大约一个小时的网络搜索，很难找到每天发布 NET 排名的来源。这让我对我在第3点中的假设产生了怀疑。也许，该算法足够简单，可以通过访问一个赛季的数据和 NET 排名来轻松逆向工程。也许，我们可以拟合一个简单的线性回归来为每支球队生成一个分数，而 NET 排名则是这些分数的排序列表。

鉴于产生最终 NET 排名的方法有很多可能性，我认为最可能的情况是 NCAA 使用了[集成学习](https://en.wikipedia.org/wiki/Ensemble_learning)，比如投票。这意味着他们可能会采用多种方法来生成一个基于团队价值指数和净效率的 NET 排名。然后，他们将这些方法的结果结合起来，制定出每周发布的最终 NET 排名。

**追随金钱**

![](../Images/42505030cd2d47ad04e62602c62b22b9.png)

图片由 [Giorgio Trovato](https://unsplash.com/@giorgiotrovato?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

尽管试图推测生成 NET 排名的可能方法是一项有趣的练习，但我认为结果并不是 NET 排名最重要的结果。当然，NCAA 希望识别出最佳的球队，使三月疯狂尽可能竞争激烈和有价值。因此，排名需要看起来合理，并可能与 AP 和教练投票紧密对齐。

*然而，* NET 排名为大学篮球的商业运作提供了其他排名系统显式没有的一个重要功能：**NET 排名激励高质量的非会议比赛**。

这一点不容忽视。这是数据力量的另一面。通过公布 NET 排名的组成部分，NCAA 公开宣示了所有球队应当达到的指标。从商业战略的角度来看，团队价值指数（Team Value Index）非常有创意。像净效率指标（Net Efficiency metric）一样，它旨在以易于理解的方式量化大学篮球队的质量。然而，与净效率指标不同的是，团队价值指数推动体育总监、教练和所有参与排赛的人员确保非会议赛程充满了四分之一（Quad 1）比赛。与会议赛程相比，非会议赛程更加灵活和多样化。虽然非会议竞争的重要性在 NET 排名推出之前就已经在增加，但团队价值指数和 NET 排名中的四分之一系统的强调使这一重要性得到了正式化，并奖励了赛程更艰难的球队。

在一个流媒体变得越来越重要的体育媒体和娱乐环境中，内容为王。NCAA 篮球赛季中能产生的高质量（四分之一（Quad 1））比赛越多，大学篮球的媒体内容价值就越高。随着更多流媒体服务开始涉足大学体育，赛季中改善的比赛意味着更多的球迷。更多的球迷意味着在常规赛和季后赛期间都会有更多的兴趣。更多的兴趣意味着整个赛季的收入增加，以及对已经非常盈利的三月疯狂的收入大幅提升。

如果你怀疑 NET 排名是否旨在激励更高质量的非会议比赛，请回顾一下 NET 球队表的图像。非会议赛程的强度和记录在顶部有自己的行，而非会议比赛以明亮的青色突出显示。我不能保证这是选拔委员会看到的版本，但这仍然支持了我的观点。

**结论**

如果你像我一样，到这篇博客结束时，你可能会有更多的问题而不是答案。NET 排名的组成部分非常简单，但将这些组成部分整合成一个连贯排名的算法却笼罩在神秘之中。可能有许多方法和模型可以用来生成 NET 排名，但无论方法是什么，它都由数据支持。

数据正在从两个方面推动三月疯狂（March Madness）。NET 排名向选拔委员会提供了如何构建锦标赛的建议，同时明确激励球队提高其非会议赛程的强度。总体而言，我认为 NET 排名对篮球运动和锦标赛是有益的。它们有助于减少选择和排名球队时的偏见，并提高常规赛期间的运动质量。因此，无论你是从未错过过第一次四强赛（First Four）还是从未听说过最终四强赛（Final Four），现在你知道数据科学如何在其中发挥作用了。

**参考文献**

[大学篮球词典：51个术语定义 | NCAA.com](https://www.ncaa.com/news/basketball-men/article/2020-11-15/college-basketball-dictionary-51-terms-defined?utm_campaign=mbk-rr-links)

[大学篮球NET排名解析 | NCAA.com](https://www.ncaa.com/news/basketball-men/article/2022-12-05/college-basketballs-net-rankings-explained)

[前四场NCAA比赛的解析 | NCAA.com](https://www.ncaa.com/news/basketball-men/bracketiq/2022-03-15/first-four-ncaa-tournament-ultimate-guide)

[大学篮球NET排名解析：Quad 1胜利如何影响NCAA比赛球队 | Sporting News](https://www.sportingnews.com/us/ncaa-basketball/news/net-rankings-explained-quad-1-ncaa-tournament/kokjb6zynkotpe3hjwoo0d3b)

[NET — NCAA男子篮球球队的Nitty Gritty报告 | WarrenNolan.com](https://www.warrennolan.com/basketball/2023/net-teamsheets-plus)

对我的内容感兴趣？请考虑在Medium上关注我。

在Twitter上关注我：@malloy_giovanni

你认为NET排名背后使用了什么算法？你是否更喜欢其他系统？请在下方评论中分享你的想法或经历，保持讨论的热度！
