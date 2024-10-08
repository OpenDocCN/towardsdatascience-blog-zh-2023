# 《垄断游戏的数据驱动策略模拟》

> 原文：[`towardsdatascience.com/a-data-driven-tactics-simulation-for-monopoly-864e7cffe508`](https://towardsdatascience.com/a-data-driven-tactics-simulation-for-monopoly-864e7cffe508)

## 使用数千次 MATLAB 模拟的视觉指南，帮助你在下次《垄断》游戏中占据优势

[](https://medium.com/@Jake_Mitchell?source=post_page-----864e7cffe508--------------------------------)![Jake Mitchell](https://medium.com/@Jake_Mitchell?source=post_page-----864e7cffe508--------------------------------)[](https://towardsdatascience.com/?source=post_page-----864e7cffe508--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----864e7cffe508--------------------------------) [Jake Mitchell](https://medium.com/@Jake_Mitchell?source=post_page-----864e7cffe508--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----864e7cffe508--------------------------------) ·阅读时间 7 分钟·2023 年 1 月 9 日

--

![](img/4cb410d22e0ec5c6fcc90cbdb4c9c39d.png)

图片由[Joshua Hoehne](https://unsplash.com/@mrthetrain?utm_source=medium&utm_medium=referral)拍摄，来自[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 介绍：

在我上一篇文章的续集中，[模拟垄断](https://medium.com/towards-data-science/simulating-monopoly-finding-the-best-properties-using-matlab-130fe557b1ae)，我将探讨玩家的购买策略以及这些策略如何影响游戏结果。我将通过在 MATLAB 中建模游戏，然后模拟数千场游戏来寻找玩家选择中的模式。

## 编码游戏：

要模拟垄断游戏，你只需要模拟掷骰子的过程，并根据骰子的点数移动玩家。然后根据玩家落在什么类型的格子上应用一系列条件语句。如果玩家落在一处地产上，要么决定购买该地产，要么支付租金给所有者。如果玩家落在“前往监狱”上，将玩家的位置更改为“监狱”。游戏的这一部分在代码中非常直接，详细内容可以参考我上面链接的上一篇文章。

对我来说最令人兴奋的代码部分是涉及购买地产、建造房屋/酒店以及交易的决策。当玩家决定购买地产时，需要考虑两个因素：

+   **价格比率** — 玩家资金与地产价格的比例。比例越小，对玩家财务健康的影响越大。

+   **购买系数** — 游戏开始时分配的一个随机值。购买系数越低，玩家的购买习惯越冒险。

这两个值被输入到**决策因子公式**中。这个公式是一个修改过的 S 型函数，其结果在 0 和 1 之间。结果接近 0 意味着玩家在这种情况下绝不会购买该物业，而结果接近 1 则表示对玩家来说是个好交易。然后，随机数生成器产生一个在 0 和 1 之间的数字。如果这个数字小于决策因子，玩家就会购买该物业（见下方代码）。

```py
price_ratio = player_data(turn,1)/property_data(i,2); % calculates price ratio
decision_factor = 1/(1 + exp(-1 * price_ratio + player_data(turn,3))); % calculates decision factor
if change == 1 % if buying property would result in a monopoly
  player_data(turn,3) = player_data(turn,3) + mon_weight; % increases chance of getting property
end
if rand(1) < decision_factor % if player decided to buy property
  property_data(i,11) = turn; % assignes property to new owner
  player_data(turn,1) = player_data(turn,1) - property_data(i,2); % subtracts price from owner's money
end
```

这两个值和相关函数在游戏中的每个决策点都适用于所有玩家。甚至还为那些可以为玩家创造垄断的物业添加了额外的激励。

游戏被模拟直到除 1 名玩家外所有玩家破产，此时游戏结束，统计数据被记录。然后一切被重置，新游戏开始。重复 1,000 次。

## 结果：

以下结果和分析是基于 6 人玩游戏的假设进行的。

*你应该更加冒险，还是在用钱时更保守？*

下图显示了你的购买策略如何影响你在游戏过程中的平均财富。随着你从 x 轴的左侧移动到右侧，购买策略从冒险转为保守。

![](img/5013beee48f8a79098efc7b7e0249823.png)

图像由作者提供。

你可以争辩说，基于左下角较暗的点簇，冒险策略略微有优势，但差别不大。这张图不幸地显示了**购买策略与成功率之间没有相关性**，这让人非常失望。为了证明这与玩家数量无关，这里是 4 名玩家和 8 名玩家的相同结果：

![](img/b3f28a9812d67e9ce4b3adc0c7125f56.png)![](img/9d705dea91b56fdf4cab542515eb1593.png)

4 名玩家（左）和 8 名玩家（右）。图像由作者提供。

再次根据这些图表，几乎没有证据表明更冒险的策略可以提高你的平均财富。此外，接近 0 的点积累是因为在几乎所有人都以 0 美元结束的游戏中，你会期望平均值偏向那个方向。

*所以我没有理想的购买策略可以使用？*

事实证明，有一种购买策略可能对玩家有利，但实际上它与对手的玩法关系更大。

下图与之前的图几乎相同，但现在 x 轴是**购买系数比率**，即你的系数与对手平均系数的比率。低于 1 的值表示你比对手更冒险。高于 1 的值表示你更保守。

![](img/7055ccc03c202b68c671affd625a63d7.png)

图像由作者提供。

基于这个图表，我们可以更确定地说，**相比于你的对手，更加冒险的购买策略会增加你预期的平均财富。** 这是因为在 x 轴值为 0.5 到 1 之间，y 轴上的点显著增加。

我们也可以说，**相比于你的对手，更保守会减少你预期的平均财富。** 图表的右侧显示，较为保守的玩家几乎没有大量剩余金钱的情况。

*如果我应该在购买物业时比对手更冒险，那么我应该关注哪些物业？*

在 1000 场模拟游戏中，我查看了最终获胜者在第一个对手破产时所拥有的物业：

![](img/afe626b22e549a1966b83de73b05c3f4.png)![](img/9f59ea982b8cf69fc6d8accbc67b537a.png)

图片由作者提供。

x 轴仅仅是棋盘上的瓷砖位置。就像你把棋盘的 4 个边拆开并排成一行一样。颜色与标准大富翁棋盘上的物业相匹配，除了铁路用黑点表示，公共事业用灰点表示。

从这个图表来看，很明显**获胜的玩家瞄准了 3 个特定的物业组。他们瞄准了浅蓝色、粉色和橙色的物业组**（参见上方棋盘上的红色圆圈）。所有这些物业的拥有率都远超过 35%，其中浅蓝色物业的拥有率接近 45%。

*为什么赢得比赛的玩家通常拥有这些物业？*

理想的物业是那些投资回报率优良的物业。你不希望拥有那些需要大约 80 回合才能收回投资的物业。下图对此进行了完美的描述：

![](img/3b66c668d3568ce6c1eb489066ea7318.png)

图片由作者提供。

回本回合数是通过计算物业的平均支出，包括初始购买和任何后续增加的住房，然后将其除以每回合的平均租金收入，以找到收回投资所需的平均回合数。

赢得比赛的玩家最有可能拥有的物业通常位于图表的底部，这意味着收回投资所需的时间较少，因此赚取利润的时间更多。下图展示了这些赢家如何真正重视具有优良投资回报率的物业。

![](img/1ab27138409fb80068b56ee0a979c442.png)

图片由作者提供。

浅蓝色、粉色和橙色的物业显然是棋盘上的最佳选择。这一点很重要，因为**你不仅要比对手更冒险，还要将这些风险集中在这 3 组物业上。**

*可以给这种风险水平分配什么量化值？*

我追踪了模拟游戏中所有 1,000 名赢家的财富，并将每次掷骰子的金额平均化，制作了这个图示：

![](img/3ef892dd8482e0bc1008cbf658a1685b.png)

图片由作者提供。

平均赢家在 6 人游戏中**花钱购买属性直到手中剩下 750$到 800$之间。** 这应该在大约 60–70 次掷骰子的过程中完成，或者在所有玩家之间大约 10 个完整周期。

**一旦平均赢家的金额达到了起始金额的近 3 倍，他们就开始重新投资于那些玩家破产后变得可用的属性。** 他们此时也开始专注于购买房屋和酒店。

## **结论：**

1.  仅仅以冒险的方式玩是不够的，你还应该尝试比你的对手更冒险。至少，要匹配对手的游戏风格。

1.  你的购买策略应该围绕尝试获取浅蓝色、粉色和橙色属性。

1.  目标是游戏早期花费大约一半的起始金额用于购买属性。

感谢你花时间阅读我的文章！如果你对这种内容感兴趣，我推荐你阅读我的其他棋盘游戏模拟文章，包括[教机器玩四连棋](https://medium.com/towards-data-science/i-taught-a-machine-how-to-play-connect-4-df261da4e23f)或[模拟曼卡拉](https://medium.com/towards-data-science/simulating-mancala-what-happens-when-i-push-this-game-to-its-limits-28d9c0a58616)。
