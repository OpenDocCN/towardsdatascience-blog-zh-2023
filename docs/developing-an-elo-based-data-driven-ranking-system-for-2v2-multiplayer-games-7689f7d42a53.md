# 如何为2v2游戏创建基于数据的Elo评级系统

> 原文：[https://towardsdatascience.com/developing-an-elo-based-data-driven-ranking-system-for-2v2-multiplayer-games-7689f7d42a53?source=collection_archive---------0-----------------------#2023-09-06](https://towardsdatascience.com/developing-an-elo-based-data-driven-ranking-system-for-2v2-multiplayer-games-7689f7d42a53?source=collection_archive---------0-----------------------#2023-09-06)

## 将数学放在桌面上：从算法到桌上足球的疯狂，寻找终极办公室冠军。

[](https://medium.com/@lazarekolebka?source=post_page-----7689f7d42a53--------------------------------)[![Lazare Kolebka](../Images/c3697b77f6275d40d65e488faf9c01dd.png)](https://medium.com/@lazarekolebka?source=post_page-----7689f7d42a53--------------------------------)[](https://towardsdatascience.com/?source=post_page-----7689f7d42a53--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----7689f7d42a53--------------------------------) [Lazare Kolebka](https://medium.com/@lazarekolebka?source=post_page-----7689f7d42a53--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff1c1df53dff1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdeveloping-an-elo-based-data-driven-ranking-system-for-2v2-multiplayer-games-7689f7d42a53&user=Lazare+Kolebka&userId=f1c1df53dff1&source=post_page-f1c1df53dff1----7689f7d42a53---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----7689f7d42a53--------------------------------) ·19 分钟阅读·2023年9月6日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F7689f7d42a53&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdeveloping-an-elo-based-data-driven-ranking-system-for-2v2-multiplayer-games-7689f7d42a53&user=Lazare+Kolebka&userId=f1c1df53dff1&source=-----7689f7d42a53---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F7689f7d42a53&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdeveloping-an-elo-based-data-driven-ranking-system-for-2v2-multiplayer-games-7689f7d42a53&source=-----7689f7d42a53---------------------bookmark_footer-----------)![](../Images/2300eec4a055e10f90e34464ff111ace.png)

图片由 [Pascal Swier](https://unsplash.com/@pascalswier?utm_source=medium&utm_medium=referral) 提供，发布在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

*你好，欢迎！*

*我的名字是 Lazare，我刚完成了我的第二个学士学位，专业是商业数据分析。本文基于我为学士论文所做的工作。*

从友谊赛到激烈的竞争，桌上足球在企业文化中找到了自己的位置，为团队提供了一种独特的连接和竞争方式。

本文探讨了基于Elo的2v2评分系统的数学原理，该系统可以应用于桌上足球或任何其他2v2游戏。它还考察了支持数据处理的架构，并展示了创建一个实时排名和数据分析的网络应用程序，使用Python编写。

# Elo排名

Elo评级系统是一种用于确定玩家在[零和游戏](https://en.wikipedia.org/wiki/Zero-sum_game)中相对技能水平的方法。最初为国际象棋开发，但现在已被应用于各种其他体育项目，如棒球、篮球、各种棋盘游戏和电子竞技。

*这个系统的一个著名例子是国际象棋，其中Elo评级系统用于排名全球玩家。Magnus Carlsen，亦称为“国际象棋的莫扎特”，在2023年以2,853的评级保持全球最高Elo评级，展示了他在比赛中的非凡技能。*

Elo评级公式分为两部分：首先，它计算给定玩家组的**预期结果**，然后根据比赛结果和预期结果确定**评级调整**。

## 预期结果计算

考虑以下国际象棋中的例子，其中玩家A和玩家B的评级分别为R𝖠和*R*𝖡。玩家A对阵玩家B的预期得分方程如下：

![](../Images/a2395428427ef51203c05b739866b740.png)

Elo算法使用一个变量，可以调整以控制胜利概率如何受到玩家评级的影响。在这个例子中，设定为400，这在大多数体育项目中，包括国际象棋，是典型的。

现在让我们来看一个更实际的例子，其中玩家A的评级为1,500，而玩家B的评级为1,200。

上述相同的方程可以计算玩家A对阵玩家B的期望得分：

![](../Images/b09dece89f1cc0c9afca9bf759b0bacc.png)

通过这个计算，我们知道玩家A对阵玩家B的胜率为84.9%。

要找到玩家B对阵玩家A的估计胜率，使用相同的公式，但评级顺序会被颠倒：

![](../Images/1513d50bc356fa331e79e359b7746508.png)

玩家A获胜的概率与玩家B获胜的概率之和等于1（0.849 + 0.151 = 1）。在这种情况下，玩家A的胜率为84.9%，而玩家B的胜率仅为15.1%。

## *评级计算*

胜者和败者之间的评级差异决定了每场比赛后赢得或失去的总积分。

+   如果一名具有更高Elo评级的玩家获胜，他们将为胜利获得更少的积分，而对手将因失败而损失少量积分。

+   相比之下，如果低排名玩家获胜，这一成就被认为更为重要，因此奖励也更大，而高排名对手会因此受到相应的惩罚。

计算玩家A与玩家B对战的新评级的公式如下：

![](../Images/854d6ef9f2d62f5f132505e09fab6bc8.png)

在这个公式中，（*S*𝖠 — *E*𝖠）代表玩家A实际得分与预期得分之间的差异。附加变量*K*大致确定了单场比赛后玩家评级可以变化的程度。在象棋中，这个变量设为32。

如果玩家A获胜，则实际得分为1，在这种情况下，将高于预期得分0.849，从而产生正差异。

这表明玩家A的表现比最初预期的要好。因此，Elo评级系统会重新校准两位玩家的评级：

+   玩家A的评级将因胜利而增加

+   玩家B的评级将因失败而减少

再次地，这个相同的公式可以计算玩家A和玩家B的新评级：

![](../Images/45beac5691a17bfb831c6fbb863b675c.png)

总之，Elo评级系统提供了一种强大且高效的方法来动态而公平地评估和比较玩家的技能。它在每场比赛后持续更新玩家的评级，考虑到两位对手之间的技能差异。

这种方法奖励冒险，因为与高评级玩家对战获胜会显著提高玩家的评级，如下表所示：

![](../Images/07f27d4e9eee766fe835c73a1b7f71bd.png)

图 I ：象棋中的Elo系统示例 | 作者提供的表格

另一方面，如果一名高评级玩家违背了他们的胜率，与低评级玩家对战并输掉比赛，他们的评级将受到显著影响：他们将失去更多积分，而对手将获得更多积分。

*总之，当一名玩家赢得比赛时，他们的获胜概率越低，他们能够赢得的积分就越高。*

在当前状态下，这个最初为象棋设计的评级公式尚未完全适应桌上足球。

实际上，桌上足球比象棋有更多变量，例如：

+   它是一个**四人游戏**，由两个队伍组成（2v2）

+   每个队员**可以对他们的队友产生积极或消极的影响**

+   与棋类游戏中的二元结果不同，桌上足球中胜利或失败的规模可以根据队伍的得分有显著变化。

# 评级算法探索

*这里的重点是将Elo评级系统调整为满足桌上足球比赛的独特要求，这涉及四名玩家分成两个队伍。*

## 胜率

要开始计算新的玩家评级，需要建立一个精细的公式来确定涉及四名玩家分成两队的比赛的预期结果。

为了说明这一点，考虑一个假设的四人桌上足球比赛情境：玩家1、玩家2、玩家3和玩家4，每个人的评分代表了他们的技能水平。

![](../Images/6ad3b7d6fe009230c610207152d47238.png)

图II：四名玩家进行桌上足球比赛的情境 | 表格由作者提供

在修订后的Elo评分系统中，计算团队1对抗团队2的预期得分，需要确定游戏中每位玩家的预期得分。

玩家1的预期评分，记作*E*𝖯𝟣，可以通过使用Elo评分公式计算每个对手评分的平均值来得出：

![](../Images/4b831135973e20979eff19759b3216ad.png)

经过广泛测试后，决定将预期得分公式中用于除以评分差异的变量设为500，而不是传统的棋类使用的400。这一增加的值意味着玩家的评分对其预期得分的影响会更小。

*这一调整的主要原因在于，与棋类不同，桌上足球中有少许运气成分。通过使用500的值，比赛结果可以更准确地预测，并且可以开发出一个可靠的评分系统。*

计算玩家2的预期得分，记作*E*𝖯𝟤，对抗玩家3和玩家4，可以采用与玩家1相同的方法。

然后，团队的预期得分记作*E*𝖳𝟣，可以通过计算*E*𝖯𝟣和*E*𝖯𝟤的平均值得出：

![](../Images/55147bbaa288a35cbf6ef7a946e7fa49.png)

一旦计算出每位玩家的预期得分，就可以用这些得分来计算比赛结果。预期得分最高的团队更有可能获胜。通过平均每个团队成员的预期得分，**团队内部技能差异的问题可以得到解决！**

下表显示了玩家1和玩家2对抗玩家3和玩家4的预期得分。

+   P1对P3和P4的预期得分分别为0.091和0.201，对应14.6%的胜率。

+   P2对P3和P4的预期得分分别为0.201和0.387，总体获胜概率为29.4%。

+   对于P1，与像P2这样更强的玩家搭档可以提高他们的整体获胜机会，如22%所示。

![](../Images/6637be8017385f184d1e7f091834010e.png)

图III：基于图II所示情境的预期得分 | 表格由作者提供

如果P1和P2的团队获胜，P1获得的积分将低于其个人预期得分，因为排名较高的P2也参与了胜利，降低了P1的总体获胜概率。

另一方面，P2由于有一名排名较低的队友而获得更多积分。如果获胜，P2会因冒险而获得奖励，而P1则获得较少的积分，因为假设P2对胜利的贡献更大，反之亦然，如果他们失败。

## 评分参数

现在已经确定了四人比赛的预期结果，这些信息可以纳入一个新的公式，该公式考虑了影响比赛和球员评分的多个变量。

如前所述，K值可以调整以更好地适应评分系统的需要。这个新公式考虑了每个球员参加的比赛数量，反映了他们的资历以及比赛结果。

例如，在2014年世界杯半决赛中，德国以7–1战胜了巴西。这是世界杯历史上最令人震惊和羞辱的结果之一，因为巴西是东道主，自1975年以来在主场从未输过比赛。

如果将评分系统应用于这场比赛，我们预计德国会获得大量积分，而巴西会损失大量积分，反映出两队在表现和技能水平上的差异。

***K值*** 在这种情况下，*K*𝟣表示球员1的K评分，决定了球员在一场比赛后评分变化的幅度。这个修正后的K值考虑了球员参与的比赛数量，以平衡每场比赛对其评分的影响。经过多次测试，制定了计算每个球员K值的公式。

对于球员1，这可以表示为：

![](../Images/2962b144443f39fc46e071a01f749d90.png)

这个K值的公式旨在对新球员的评分产生更大的影响，同时为经验丰富的球员提供稳定性，减少评分波动。具体来说，在进行300场比赛后，球员的评分将更具代表性地反映其技能水平。

![](../Images/54909432ac4c61ad2a7b03f04e055d04.png)

图 IV：K值随时间变化 | 作者绘制的图表

图 IV 显示了比赛场数对K值的影响。从50场开始，该图显示随着比赛场数的增加，K值逐渐降低，在300场比赛后降至一半的25。这确保了随着经验的增加，每场比赛对球员评分的影响会减少。

***点因子*** 为了考虑每支球队得分的情况，方程中引入了一个新的变量，称为“点因子”。该因子将每个球员的*K*参数相乘，并基于两队得分的绝对差值。比赛的影响必须更大，当一队以大比分获胜，即取得压倒性胜利时。

计算点因子的公式如下：

![](../Images/bc213518762f7252f5c8accaf5a272a7.png)

该公式取两队得分的绝对差值，加1，并计算结果的以10为底的对数。然后将该值立方，并加上2，得到点因子的最终值。

![](../Images/b85648906b08641f9541561cf6cfc549.png)

图 V：点因子 | 作者绘制的图表

## 最终评分计算

在调整了所有必要的变量之后，开发了一种改进的公式来计算每个参与游戏的玩家的新排名。

现在，每个玩家的评级考虑了他们的先前评级、对手的评级、队友的影响、他们的比赛历史和比赛的得分。这个公式确保每个玩家根据他们的真实表现获得奖励，同时考虑了每场比赛的公平性。

以之前的例子为基础，玩家 A 的新排名公式如下：

![](../Images/49486b022ecd37996347ca9c017b86e9.png)

这个改进的公式根据玩家的实际表现给予奖励，鼓励冒险，并为新玩家和经验丰富的玩家提供了一个更平衡的评级系统。

现在我们有了 Elo 算法，我们可以继续进行数据库建模。

# 数据库设计与建模

[提议的数据库](https://github.com/lkolebka/baby-foot-python/blob/7aab1661e9f84e24746c26302fa03818b4834712/create_uppload%20_match/createDB.sql) 模型采用了关系型方法，通过使用主键（PKs）和外键（FKs）将数据组织成相互关联的表。这种结构化的组织有助于数据管理和分析，使 PostgreSQL 成为合适的数据库管理系统选择。PKs 和 FKs 帮助维护数据一致性并减少数据库中的冗余。

![](../Images/0a0200e85067fd74458d2a6ee309ffd5.png)

图 VI：数据库的模型图 | 作者提供的图像

在这个数据库模型中，表之间存在两种关系：*一对多* 和 *多对多*。

‘**Player**’ 表和‘**Match**’ 表之间的关系是多对多的，因为一个玩家可以参与多场比赛，多个玩家可以参与一场比赛。一个名为‘**PlayerMatch**’的连接表桥接了这一关系，包含两个外键：‘**player_id**’（引用参与的玩家）和‘**match_id**’（引用对应的比赛）。

这种结构确保了玩家和比赛的准确关联，如下面的代码所示：

```py
CREATE TABLE PlayerMatch (
player_match_id serial PRIMARY KEY,
player_id INT NOT NULL REFERENCES Player(player_id),
match_id INT NOT NULL REFERENCES Match(match_id)
);
```

类似的逻辑适用于‘**TeamMatch**’表，该表作为‘**Match**’和‘**Team**’表之间的连接点，允许多个队伍参加一场比赛，并且一场比赛涉及多个队伍。

为了简化长期的排名分析，设计了‘**PlayerRating**’和‘**TeamRating**’两个表。这些表分别通过‘**player_match_id**’和‘**team_match_id**’与‘**PlayerMatch**’和‘**TeamMatch**’表连接。

## ***数据完整性***

除了使用 PKs 和 FKs 之外，这个数据库模型还使用了适当的数据类型和 CHECK 约束以保证数据完整性：

+   ‘**winning_team_score**’ 和 ‘**losing_team_score**’ 列在‘**Match**’表中是整数，防止非数字输入。

+   CHECK 约束确保‘**winning_team_score**’恰好为 11。

+   CHECK 约束确保 ‘**losing_team_score**’ 在 0 到 10 之间，符合游戏规则。

如下面的代码块所示，为每个主键使用序列已在数据库创建中实现，以便于数据录入。这种自动化简化了后续使用 Python 循环进行数据录入过程的整体流程。

```py
CREATE SEQUENCE player_id_seq START 1;
CREATE SEQUENCE team_id_seq START 1;
CREATE SEQUENCE match_id_seq START 1;
CREATE SEQUENCE player_match_id_seq START 1;
CREATE SEQUENCE player_rating_id_seq START 1;
CREATE SEQUENCE team_match_id_seq START 1;
CREATE SEQUENCE team_rating_id_seq START 1;
```

## 数据处理

*主要挑战在于找到一种处理比赛数据的顺序，以便从正在处理并插入数据库的初始数据中检索 ID。*

这些特定的 ID 随后可以作为外键来管理其余的数据，在过程中创建必要的关系。换句话说，第一步是从原始数据中识别并存储特定数据（ID），然后利用这些 ID 作为桥梁来连接和处理其余的数据。

数据按步骤处理，使用日益复杂的 Python 循环。每个新条目都被分配一个由表的序列生成的唯一主键。

1.  第一步是处理单个玩家并获取他们的 ID。

1.  接下来，使用玩家 ID 处理团队。对于比赛中的每对独特的玩家，‘**Team**’ 表中创建了一个条目（FK players）。

1.  随后，比赛使用胜利和失败的团队 ID 进行处理。处理完比赛后，通过检索相应的比赛、玩家和团队 ID 处理了 ‘**PlayerMatch**’ 和 ‘**TeamMatch**’ 表。

1.  一旦所有必要的数据处理完成，‘**PlayerMatch**’ 和 ‘**TeamMatch**’ 的 ID，以及 ‘**match**’ 时间戳被用于 ‘**PlayerRating**’ 和 ‘**TeamRating**’ 表中，以跟踪评级随时间的变化。

# Web 应用程序开发

Web 应用程序的目标是允许用户输入比赛结果、验证数据，并直接与数据库进行交互。这确保了数据**是最新的并且实时提供**，使用户始终能够访问排名或可视化他们的指标。

此外，我还希望使 Web 应用程序适应移动设备，因为谁愿意拖着一台笔记本电脑来玩桌上足球？那既不实用也不有趣。

## 技术栈

***后台*** 在比较了 [Django 和 Flask](https://testdriven.io/blog/django-vs-flask/) 这两个流行的 Python Web 框架后，选择了 Flask，因为它对初学者更友好。Flask Web 框架用于处理用户请求、处理数据和与 PostgreSQL 数据库交互。

***前端*** 前端由静态 HTML 和 CSS 文件组成，这些文件定义了 Web 应用程序的结构和样式。JavaScript 用于表单验证和处理用户交互。这确保了用户提交的数据在发送到后台之前是一致和准确的。

***数据可视化*** 在数据可视化方面，最大的挑战是拥有最新的数据。为克服这一限制，数据可视化层使用了[Plotly](https://plotly.com)，这是一个Python库，用于生成互动图表和图形，以可视化玩家的评分变化。此组件从后端接收数据，处理数据，并以用户友好的格式呈现给用户。

***数据库*** PostgreSQL 被用于本地开发环境以及通过 Heroku 在 AWS 上的生产环境。Heroku 提供自动数据库备份，确保数据得到保护，并且在必要时可以轻松恢复。

## UI/UX 研究

对于UI/UX设计，灵感来自于Spotify的现代网页设计和新的Bing搜索引擎。目标是创造一个熟悉且直观的用户体验。

![](../Images/a6c22777108a8f96b727afe4612f297d.png)

图 VII: 应用程序的模拟图 | 作者提供的图像

# 应用程序功能

让我们通过一个具体的场景来深入了解应用程序的功能。团队1（Matthieu和Gabriel）想与团队2（Wissam和Malik）对战。所有玩家都有不同的评分，代表他们的技能水平，如下所示。

![](../Images/718fe6e281fd8428954e40a344e564c6.png)

## 计算概率

玩家在任何比赛之前最想做的第一件事是计算他们的获胜概率。

为此，“计算概率”视图允许用户通过下拉菜单选择四名玩家，并生成所选团队的获胜概率。

![](../Images/1c411eec63a24dfd4a73462a2d982d4f.png)

图 VIII: 计算概率 | 作者提供的图像

这一功能主要在游戏之前使用，以验证比赛是否平衡，并告知玩家他们的获胜概率。例如，团队1的获胜概率为64.19%，而团队2的获胜概率为35.81%。这一视图告知每个玩家赌注和承担的风险。

一旦表单提交，应用程序仅计算算法的第一部分，即在给定四个选定玩家的情况下计算游戏的预期结果。

## 上传游戏

“上传游戏”视图作为应用程序的主页。它旨在为用户提供方便，使他们在打开应用程序时能够立即上传游戏。

![](../Images/512127c1d46a84ea9eb0d7bca7392acf.png)

图 IX: 上传游戏 & 匹配上传 | 作者提供的图像

在表单提交之前，应用程序使用JavaScript进行数据验证，以确保：

+   选择了四名不同的玩家

+   分数为非负整数

+   只有一个得分恰好为11的获胜团队，不允许平局

当验证成功时，应用程序使用完整算法处理数据，更新数据库中的相应表，并向用户确认他们的上传。

“比赛上传”视图旨在向用户展示每场比赛对其个人评分的影响。它计算了比赛上传前后玩家评分之间的差异。

*如上所示，每场游戏对每个玩家评分的影响并不相同。这是因为算法对每个玩家的个体参数不同：他们的预期得分、游戏数量、队友和对方队伍。*

## Elo 排名

“玩家排名”视图允许用户访问实时的月度排名，并与其他玩家进行比较。用户可以看到他们的评分、他们在整个月份中参加的游戏数量以及他们参加的最后一场游戏，展示他们的最新评分。

![](../Images/d36343a515eb0a860855f525ecd53483.png)

图 X: 玩家排名 | 作者提供的图片

一旦访问“玩家排名”视图或提交新时间段，应用程序会使用 CTE 方法查询数据库。

这涉及到连接所有必要的表格并显示最新的排名更新，使用时间段选择器来筛选查询：

```py
def get_latest_player_ratings(month=None, year=None):
    now = datetime.now()
    default_month = now.month
    default_year = now.year
    selected_year = int(year) if year else default_year
    selected_month = int(month) if month else default_month
    start_date = f'{selected_year}-{selected_month:02d}-01 00:00:00'
    end_date = f'{selected_year}-{selected_month:02d}-{get_last_day_of_month(selected_month, selected_year):02d} 23:59:59'

    query = '''
        WITH max_player_rating_timestamp AS (
            SELECT 
                pm.player_id,
                MAX(pr.player_rating_timestamp) as max_timestamp
            FROM PlayerMatch pm
            JOIN PlayerRating pr ON pm.player_match_id = pr.player_match_id
            WHERE pr.player_rating_timestamp BETWEEN %s AND %s
            GROUP BY pm.player_id
        ),
        filtered_player_match AS (
            SELECT 
                pm.player_id,
                pm.match_id
            FROM PlayerMatch pm
            JOIN max_player_rating_timestamp mprt ON pm.player_id = mprt.player_id
        ),
        filtered_matches AS (
            SELECT match_id
            FROM Match
            WHERE match_timestamp BETWEEN %s AND %s
        )
        SELECT 
            CONCAT(p.first_name, '.', SUBSTRING(p.last_name FROM 1 FOR 1)) as player_name, 
            pr.rating, 
            COUNT(DISTINCT fpm.match_id) as num_matches,
            pr.player_rating_timestamp
        FROM Player p
        JOIN max_player_rating_timestamp mprt ON p.player_id = mprt.player_id
        JOIN PlayerMatch pm ON p.player_id = pm.player_id
        JOIN PlayerRating pr ON pm.player_match_id = pr.player_match_id
            AND pr.player_rating_timestamp = mprt.max_timestamp
        JOIN filtered_player_match fpm ON p.player_id = fpm.player_id
        JOIN filtered_matches fm ON fpm.match_id = fm.match_id
        GROUP BY p.player_id, pr.rating, pr.player_rating_timestamp
        ORDER BY pr.rating DESC;
    '''
```

# 数据可视化

开发这个综合解决方案的主要目标是为用户提供一个实时排名系统，作为每位玩家表现的可视化表示。

尽管像 PowerBI 和 Qlik 这样强大的数据可视化工具可供使用，但选择了一个完全兼容移动设备的解决方案，使用户能够在其设备上实时获取洞察，而无需支付许可费用。

实现这一点采用了两种方法：

+   首先，使用了 Dash Plotly，一个 Python 框架，允许开发者在 Flask 应用程序的基础上构建互动的、数据驱动的应用程序。

+   其次，使用了各种 SQL 查询和静态 HTML 页面来从数据库中提取信息并显示，确保用户始终可以访问实时数据。

## 排名演变

这种可视化方式让玩家能够观察每场比赛对其排名的影响，并识别出更广泛的趋势。例如，他们可以准确地看到何时被其他人超越，或者连续的胜利或失败的影响。

![](../Images/dcd44b67119ab453525c58d4122c9543.png)

图 XI: 排名演变 | 作者提供的图片

当访问“排名演变”视图时，应用程序会对每个选定的玩家在数据库中进行查询，检索每场比赛日的最新排名更新：

```py
SELECT DISTINCT ON (DATE_TRUNC('day', m.match_timestamp))
    DATE_TRUNC('day', m.match_timestamp) AS day_start,
    CASE WHEN p.first_name = '{player}' THEN pr.rating ELSE NULL END AS rating
FROM PlayerMatch pm
JOIN Player p ON pm.player_id = p.player_id
JOIN PlayerRating pr ON pm.player_match_id = pr.player_match_id
JOIN Match m ON pm.match_id = m.match_id
WHERE p.first_name = '{player}'
ORDER BY DATE_TRUNC('day', m.match_timestamp) DESC, m.match_timestamp DESC
```

检索到的数据表会被转换成折线图，列会被转换为轴，使用 Dash 进行展示。

为了减少数据库负载并简化图表中的数据展示，每天只显示最新的评分更新。

## 玩家指标

受到 Spotify Wrapped 的启发，目的是提供来自持续数据收集的洞察。虽然有巨大的潜力可视化玩家洞察，但重点是突出个体表现和玩家之间的联系的指标。

![](../Images/b6886075a078b6595eaaf377ace3928a.png)

图 XII：玩家指标 | 图片来源于作者

这些指标分为三类颜色编码的类别：搭档、比赛和对手，每个指标都附有标题、值和详细的子测量。

***游戏指标*** 这些指标集中在屏幕中央，以蓝色显示以保持中立。它们包括自数据收集开始以来的总比赛数量。

***搭档指标*** 搭档指标显示在屏幕左侧。由于其积极的含义，它们以绿色显示。

+   顶部的框突出了所选玩家最常一起打比赛的主要搭档。

+   第二个指标确定了玩家的最佳搭档。这是由最高的胜率定义的。

+   此类别中的第三个指标是所选玩家最差的搭档。这是根据最低的胜率（或最高的败率）计算的。

***对手指标*** 对手指标以红色显示以表示对抗。对手指标代表了玩家之间的竞争关系。

+   顶部的框显示了最常见的对手，附有一个子指标，指示一起打过的比赛数量，类似于搭档指标。

+   第二个指标，“最容易的对手”，表示玩家胜率最高的对手。这表明对手较弱。

+   最终指标是所选玩家胜率最低的对手。此指标表示最困难的对手。

# 结论

在写这篇文章时，应用程序已经使用了 6 个月，这些是迄今为止的结果：

1.  这个基于 Elo 系统的排名系统可以预测比赛结果，并根据玩家的实际表现准确地排名。

1.  玩家变得更加竞争激烈，因为他们通过数据可视化越来越了解自己的表现。

1.  由于改进的公式奖励冒险的玩家，玩家变得更加包容。那些通常不会一起打球的玩家现在有了配对的动力。

通过采用数据驱动的策略，本项目突显了数据的深远影响和重要性。

超越简单的玩家表现分析，本项目发起了玩家在踢足球游戏中与其他玩家及新手互动方式的变革。数据的力量真正培养了一个更具包容性和竞争性的环境。

感谢你读到这里！希望你觉得这篇文章有用。如果你有兴趣阅读完整的论文，可以在 [这里](https://www.academia.edu/104818976/CREATING_CONNECTION_THROUGH_COMPETITION_THE_DEVELOPMENT_OF_AN_ELO_BASED_DATA_DRIVEN_RANKING_SYSTEM_FOR_BABY_FOOT) 找到。

此外，所有代码都可以在 [Github](https://github.com/lkolebka/baby-foot-python.git) 上找到。

欢迎在评论中分享你的想法 :)
