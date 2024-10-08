# Google Foobar Challenge: Level 3

> 原文：[`towardsdatascience.com/google-foobar-challenge-level-3-f487fa3f204b`](https://towardsdatascience.com/google-foobar-challenge-level-3-f487fa3f204b)

## 探索二进制数字、动态编程和马尔可夫链

[](https://medium.com/@katyhagerty19?source=post_page-----f487fa3f204b--------------------------------)![凯蒂·哈格提](https://medium.com/@katyhagerty19?source=post_page-----f487fa3f204b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f487fa3f204b--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----f487fa3f204b--------------------------------) [凯蒂·哈格提](https://medium.com/@katyhagerty19?source=post_page-----f487fa3f204b--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f487fa3f204b--------------------------------) ·9 分钟阅读·2023 年 11 月 30 日

--

![](img/a0ec52c11538eee31caf7d77a54e6431.png)

图片来源于 [Rajeshwar Bachu](https://unsplash.com/@rajeshwerbatchu7?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# Foobar 挑战是什么？ 🧐

Foobar 挑战是由 Google 主办的编码挑战，可以用 Python 或 Java 完成。我用 Python 完成了挑战。这个挑战有自己专属的服务器，使用特定的终端风格命令。问题的难度各异，共分为 5 个级别。每个问题必须在一定时间限制内解决。较高级别的挑战会给予更多时间。

要了解更多关于 Foobar 挑战的信息，我推荐阅读我之前的文章，其中提供了对第 1 级问题的概述和分析。

[](/google-foobar-challenge-level-1-3487bb252780?source=post_page-----f487fa3f204b--------------------------------) ## Google Foobar Challenge: Level 1

### 对神秘编码挑战的介绍以及问题的分析

towardsdatascience.com

第 3 级开始变得严肃起来。第 1 级和第 2 级测试了基础知识，大约需要 15 分钟解决。第 3 级测试了问题解决技能，需要几个小时的研究。与之前的级别不同，我没有立即知道如何解决这些问题。我不得不多次阅读问题并在纸上进行测试。此外，我还需要研究和实践一些新的概念。

研究并不意味着仅仅通过 Google 搜索问题名称并查看其他人的解决方案。相反，我尝试重新表述问题或搜索那些看起来特别具体的短语，以找到相关的方程和模型。

起初，我有些犹豫。谷歌会跟踪我的搜索记录吗？他们会认为这是一种作弊吗？然而，随着我在这一关卡的进展，我意识到*这些问题很可能是为了迫使你查看外部材料*。我非常怀疑谷歌会期望开发者记住马尔可夫链公式。

当你解决这些问题时，我鼓励你研究不熟悉的概念，尤其是当你的解决方案变得冗长和不结构化时。这些问题的设计目的是为了找到优雅的解决方案。***如果你想不出一种优雅的解决方法，那说明可能存在某个公式或方法能简化它***。记住，编码的一部分是研究最佳的方法。

# 问题与概念 📚

# 警告 — 含有剧透 ⛔️

在下面，我将详细讲解这些问题并解释我的思考过程。我也提供了解决方案。不过，***我强烈建议你先尝试解决这个问题***。挑战中最好的部分就是解决一个难以捉摸的问题所带来的惊喜和满足感。

我曾考虑不发布解决方案，仅仅解释基础概念。然而，编码的一部分是学习如何排查故障和定位代码失败的地方。因此，我决定发布解决方案，以便当你卡住时，可以准确看到你的逻辑在哪里偏离了正确的路径。

# 第 3 关：问题 1

在第 3 关的所有问题中，这对我来说是最困难的。我在纸上做了很多示例。处理一个 309 位长的数字的要求似乎暗示着尝试用二进制数来解决这个问题。

问题的目标是用最少的操作达到一个弹珠。最有效的操作是将弹珠对半分，因为这样`n`会呈指数级衰减。

![](img/0105e2e38189cfdc0617a7e1d51cf47f.png)

图片来自作者。

不过，完美的指数衰减仅在`n`是 2 的幂时有效。如果不是，`n`最终将减少到一个奇数，并且不能被除以 2。在这种情况下，是加 1 还是减 1 更好？最佳的操作是使数字可以被最多次地除以 2 的那个选项。幸运的是，二进制形式提供了这些信息。一个二进制数中的尾随零的数量就是一个数字可以被 2 除以多少次。

例如，考虑`n = 15`，如下所示。这两个序列说明了第一步的两个选项：加 1（`n = 16`）和减 1（`n = 14`）。尽管 14 离期望的最终状态 1 更近，但 16 是更好的选择，因为它可以被更多次地除以 2。正如你在右侧图表上看到的，二进制形式中的尾随零的数量对应于一个数字可以被除以 2 的次数。

![](img/2af017fb2627430f0c3bd19d75f99eb2.png)

图片来自作者。

为了解决这个问题，我首先使用内置的`bin`函数将输入字符串(`n`)转换为二进制(`b`)。我使用了一个 while 循环将`b`减少到 1。如果`b`是偶数，我通过去掉尾部零来将其减半。尽可能多地将`b`减半将最小化总操作次数。如果`b`是奇数，我选择具有最多尾部零的最近邻整数。

如前所述，尾部零的数量表示一个给定的数字可以被二除的次数。尾部零更多的数字优先，因为它们允许更多的减半。换句话说，尾部零最多的数字提供了最有效的方法来达到 1。

# 级别 3：问题 2

开始时，我在纸上完成了大量的练习题。通过这样做，我意识到较大楼梯的解决方案包括了较小楼梯的解决方案。解决`n`涉及首先解决较小楼梯的子问题，这使得这是一个动态规划问题。

![](img/0f0cca9d56a7e675558c601a695cc62d.png)

图片来自作者。

我在解决这个问题的累积特性时真的很挣扎。虽然我理解较小的楼梯如何贡献于较大的楼梯，但我无法弄清楚如何计算两个步骤的楼梯。例如，当`n = 7`时，我看到`n = 3`是解决方案的一部分，但我不知道如何计算其他两个步骤的楼梯。

![](img/45e7af3ae94805b0eaeb3180cd373dfa.png)

图片来自作者。

作为一个练习，我简化了这个问题，并只是试图找出给定高度和`n`的楼梯数量。换句话说，我将上面的图表转为表格形式。

![](img/515b54d8c1fc066cdcfd53378cc8e4c1.png)

图片来自作者。

当我填充表格时，我找到了算法。对于每个单元格，我从`n`中减去楼梯高度。然后，我取余数并访问该列(`n = remainder`)。该列的总和等于解决方案的数量。在某些情况下，余数足够建造 1 步。为了计算这些情况，我在对角线上加了 1。请参见下图左侧的绿色数字。

![](img/42ce530ed34c458fd1a99d200d2c90a0.png)

图片来自作者。

例如，当`n = 8`且`staircase height = 5`时，余数为`n = 3`。3 列包含两个解决方案：`staircase height = 3`和`staircase height = 2`。因此，对于`n = 8`和`staircase height = 5`，共有两个解决方案。

这种方法有效，但可能更高效。与其访问列中的每个单元格，不如将表格结构化，使得行表示最大楼梯高度。例如，`max staircase height = 3`将是给定`n`的高度为 1、2 和 3 的解决方案之和。下方的紫色数字代表这种累积方法。现在，找到解决方案只需要访问一个单元格。

注意下图中包含了一个`n=0`列。这样，列索引号等于`n`值。这一小变化大大简化了代码。

![](img/fd221f092f44bca4d1be18bc0ded154e.png)

图片由作者提供。

# 第三级：问题 3

阅读问题后，我想到了几个短语。我搜索了诸如“固定概率状态”和“概率状态转移”等术语，最终找到了马尔可夫链。由于从每个状态到终端状态都有路径，这就是一个吸收马尔可夫链。终端状态，也称为吸收状态，意味着无法离开该状态（即，转移到其他状态的概率为零）。

我以前从未听说过马尔可夫链，所以在开始编写代码之前，我做了一些研究。[这里](https://brilliant.org/wiki/absorbing-markov-chains/) 是对马尔可夫链的很好的概述。

吸收马尔可夫链的转移矩阵 *P*，对于有 *t* 个过渡状态和 *s* 个吸收状态的情况如下：

![](img/a38ebf16ef400dd9ed2787c850fdc916.png)

图片由作者提供。

然后可以使用转移矩阵来找到基本矩阵：

![](img/b229b7b942d99f1c3f5bfa83eda8558f.png)

图片由作者提供。

基本矩阵提供了很多信息。但为了求解问题，还必须计算一个矩阵：

![](img/d8b1b0812169ed07620b7662db4f274d.png)

图片由作者提供。

在 *M* 中，行表示起始状态，列表示吸收状态。*M[i][j]* 的值是从过渡状态 *i* 开始被吸收到终端状态 *j* 的概率。因此，*M* 的行数由过渡状态的数量决定，而列数等于终端状态的数量。

首先，我需要找出过渡状态和吸收状态的数量及位置。然后，对于过渡状态，我需要将整数转换为分数。

给定以下输入：

![](img/3c703b0b5bff4f9aa3c96c8dc7c76cc3.png)

图片由作者提供。

上面的代码将其转换为：

![](img/4a765d1b4858a8fb6fc92165a05f90cb.png)

图片由作者提供。

接下来，我需要计算 *I* 和 *Q* 矩阵以求解 *N*。由于我不能使用库，我复习了像 点积 和 [矩阵求逆](https://integratedmlai.com/matrixinverse/) 这样的矩阵运算。

![](img/fa070364d577140a895d5f3a8d44619d.png)

图片由作者提供。

现在，我需要求解 *R*。

![](img/34c10516690a9288a4a65c325cfa378d.png)

图片由作者提供。

现在，*R* 和 *N* 都已经求解，我可以计算概率矩阵 *M*。

![](img/fcf1e5ae6ac0db4558656fc9c38631ae.png)

图片由作者提供。

现在，*M* 必须格式化以匹配输出规范。由于问题只要求从状态 0 开始的概率，因此只需要第一行。正如上面所示，*M* 是以分数形式表示的。为了将答案格式化为表示概率分子的一维数组，我需要找到最小公倍数。最小公倍数将是概率之间的公分母。

![](img/e4adc6f0c1648ecb3f9eb6464c896f8c.png)

图片来自作者。

这个问题中最棘手的部分是分数和重新学习矩阵操作。正如我在 Level 1 文章中提到的，这个挑战对接受的输出类型非常具体。

这是完整的解决方案。

# 结论

在级别结束时，我有机会提交我的联系信息给 Google 招聘人员。然而，那是超过一年前的事情了，Google 还没有联系我。

总体来说，我觉得这些问题的设计不仅仅是测试基础编码技能。解决这些类型的问题证明了你能够在处理复杂边缘情况和内存限制时开发解决方案。具有讽刺意味的是，我发现随着难度级别的提升，问题变得更容易了。然而，每解决一个新问题，我也开始*更加聪明地工作，而不是更努力地工作*。我重新阅读问题，暂停并在纸上绘制一些例子，而不是立即开始编码想到的第一个解决方案。最重要的是，我利用现有资源来学习新概念和巩固旧知识。

![](http://buymeacoffee.com/katyhagerty)

> *感谢你阅读我的文章。如果你喜欢我的内容，可以在* [*Medium*](https://medium.com/@katyhagerty19)*上关注我。*
> 
> *在* [*LinkedIn*](https://www.linkedin.com/in/katherine-katy-hagerty/)*、* [*Twitter*](https://twitter.com/HagertyKaty)* 或* [*Instagram*](https://www.instagram.com/katyhagerty/)*上与我联系。*
> 
> *所有反馈都是受欢迎的。我总是渴望学习新的或更好的做事方式。*
> 
> *随时可以留下评论或通过 katyhagerty19@gmail.com 联系我。*
