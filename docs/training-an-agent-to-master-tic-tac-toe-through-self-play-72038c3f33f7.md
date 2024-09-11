# 训练一个智能体通过自我对弈掌握井字游戏

> 原文：[https://towardsdatascience.com/training-an-agent-to-master-tic-tac-toe-through-self-play-72038c3f33f7?source=collection_archive---------4-----------------------#2023-09-18](https://towardsdatascience.com/training-an-agent-to-master-tic-tac-toe-through-self-play-72038c3f33f7?source=collection_archive---------4-----------------------#2023-09-18)

## 令人惊讶的是，软件智能体永远不会对游戏感到厌倦。

[](https://sebastiengilbert.medium.com/?source=post_page-----72038c3f33f7--------------------------------)[![Sébastien Gilbert](../Images/380f6588c3ef718947bcf82061f190eb.png)](https://sebastiengilbert.medium.com/?source=post_page-----72038c3f33f7--------------------------------)[](https://towardsdatascience.com/?source=post_page-----72038c3f33f7--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----72038c3f33f7--------------------------------) [Sébastien Gilbert](https://sebastiengilbert.medium.com/?source=post_page-----72038c3f33f7--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F975aef8c496a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftraining-an-agent-to-master-tic-tac-toe-through-self-play-72038c3f33f7&user=S%C3%A9bastien+Gilbert&userId=975aef8c496a&source=post_page-975aef8c496a----72038c3f33f7---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----72038c3f33f7--------------------------------) ·7分钟阅读·2023年9月18日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F72038c3f33f7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftraining-an-agent-to-master-tic-tac-toe-through-self-play-72038c3f33f7&user=S%C3%A9bastien+Gilbert&userId=975aef8c496a&source=-----72038c3f33f7---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F72038c3f33f7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftraining-an-agent-to-master-tic-tac-toe-through-self-play-72038c3f33f7&source=-----72038c3f33f7---------------------bookmark_footer-----------)

啊！小学时光！那是我们学习宝贵技能的时候，比如识字、算术，以及如何最优地玩井字游戏。

![](../Images/4987ff3824a1167744cc7eac7a8a995e.png)

照片由[Solstice Hannan](https://unsplash.com/@darkersolstice?utm_source=medium&utm_medium=referral)提供，拍摄于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

和你的朋友玩井字棋比赛而不被老师发现是一门艺术。你必须在暗中将游戏纸悄悄传递到桌子下，同时表现出对课堂内容的关注。乐趣可能更多在于隐秘操作而不是游戏本身。

我们无法教给软件代理在课堂上避免被抓到的艺术，但我们能否训练一个代理掌握游戏？

在 [我之前的文章](/training-an-agent-to-master-a-simple-game-through-self-play-88bdd0d60928)中，我们研究了一个通过自我对战学习游戏*SumTo100*的代理。这是一个简单的游戏，允许我们展示状态值，这帮助我们建立了关于代理如何学习游戏的直觉。对于井字棋，我们面对的是一个更大的状态空间。

你可以在 [这个仓库](https://github.com/sebastiengilbert73/tutorial_learnbyplay)中找到 Python 代码。执行训练的脚本是 [*learn_tictactoe.sh*](https://github.com/sebastiengilbert73/tutorial_learnbyplay/blob/main/learn_tictactoe.sh)：

```py
#!/bin/bash
declare -i NUMBER_OF_GAMES=30000
declare -i NUMBER_OF_EPOCHS=5

export PYTHONPATH='./'

python preprocessing/generate_positions_expectations.py \
 --outputDirectory=./learn_tictactoe/output_tictactoe_generate_positions_expectations_level0 \
    --game=tictactoe \
    --numberOfGames=$NUMBER_OF_GAMES \
    --gamma=0.95 \
    --randomSeed=1 \
    --agentArchitecture=None \
    --agentFilepath=None \
    --opponentArchitecture=None \
    --opponentFilepath=None \
    --epsilons="[1.0]" \
    --temperature=0

dataset_filepath="./learn_tictactoe/output_tictactoe_generate_positions_expectations_level0/dataset.csv"

python train/train_agent.py \
  $dataset_filepath \
  --outputDirectory="./learn_tictactoe/output_tictactoe_train_agent_level1" \
  --game=tictactoe \
  --randomSeed=0 \
  --validationRatio=0.2 \
  --batchSize=64 \
  --architecture=SaintAndre_1024 \
  --dropoutRatio=0.5 \
  --learningRate=0.0001 \
  --weightDecay=0.00001 \
  --numberOfEpochs=$NUMBER_OF_EPOCHS \
  --startingNeuralNetworkFilepath=None

for level in {1..16}
do
 dataset_filepath="./learn_tictactoe/output_tictactoe_generate_positions_expectations_level${level}/dataset.csv"
 python preprocessing/generate_positions_expectations.py \
  --outputDirectory="./learn_tictactoe/output_tictactoe_generate_positions_expectations_level${level}" \
  --game=tictactoe \
  --numberOfGames=$NUMBER_OF_GAMES \
  --gamma=0.95 \
  --randomSeed=0 \
  --agentArchitecture=SaintAndre_1024 \
  --agentFilepath="./learn_tictactoe/output_tictactoe_train_agent_level${level}/SaintAndre_1024.pth" \
  --opponentArchitecture=SaintAndre_1024 \
  --opponentFilepath="./learn_tictactoe/output_tictactoe_train_agent_level${level}/SaintAndre_1024.pth" \
  --epsilons="[0.5, 0.5, 0.1]" \
  --temperature=0 

 declare -i next_level=$((level + 1))
 python train/train_agent.py \
  "./learn_tictactoe/output_tictactoe_generate_positions_expectations_level${level}/dataset.csv" \
  --outputDirectory="./learn_tictactoe/output_tictactoe_train_agent_level${next_level}" \
  --game=tictactoe \
  --randomSeed=0 \
  --validationRatio=0.2 \
  --batchSize=64 \
  --architecture=SaintAndre_1024 \
  --dropoutRatio=0.5 \
  --learningRate=0.0001 \
  --weightDecay=0.00001 \
  --numberOfEpochs=$NUMBER_OF_EPOCHS \
  --startingNeuralNetworkFilepath="./learn_tictactoe/output_tictactoe_train_agent_level${level}/SaintAndre_1024.pth"

done
```

该脚本循环调用两个程序：

+   [generate_positions_expectations.py](https://github.com/sebastiengilbert73/tutorial_learnbyplay/blob/main/preprocessing/generate_positions_expectations.py)：模拟比赛并存储带有折现期望回报的游戏状态。

+   [train_agent.py](https://github.com/sebastiengilbert73/tutorial_learnbyplay/blob/main/train/train_agent.py)：在最新生成的数据集上对神经网络进行几个周期的训练。

# 训练周期

代理的游戏学习通过生成比赛和训练以预测比赛结果从游戏状态的周期进行：

![](../Images/2857c9ef68ed84a281a997ede02e21cb.png)

图 1：比赛生成和神经网络训练的周期。图片由作者提供。

# 匹配的生成

该周期以随机玩家之间的比赛模拟开始，即从给定游戏状态的合法动作列表中随机选择的玩家。

> 为什么我们要生成随机进行的比赛？

这个项目涉及通过自我对战进行学习，因此我们不能给代理任何关于*如何玩*的先验信息。在第一个周期，由于代理对好坏动作一无所知，比赛必须通过随机对战生成。

图 2 显示了随机玩家之间比赛的示例：

![](../Images/da8234cf38f237660b7fbdf3c20e3c8c.png)

图 2：随机进行的井字棋比赛示例。图片由作者提供。

我们可以从观察这个比赛中学到什么教训？从‘X’玩家的角度来看，我们可以认为这是一个糟糕的表现示例，因为它以失败告终。我们不知道哪些步棋导致了失败，所以我们假设**‘X’玩家做出的所有决策**都是错误的。如果有些决策是正确的，我们依靠统计数据（其他模拟可能会经历类似的状态）来纠正他们的预测状态值。

玩家‘X’的最后一个动作被赋值为-1。其他动作则根据一个几何递减因子γ（gamma）∈ [0, 1] 递减，作为我们回溯到第一次移动时的折扣负值。

![](../Images/c439c76cde3f0ff250db2afc3f0197f3.png)

图3：游戏状态的目标值。图片由作者提供。

从获胜的比赛中得到的状态会获得类似的正折扣值。平局的状态则赋值为零。代理从第一和第二玩家的角度来观察。

## 游戏状态作为张量

我们需要一个游戏状态的张量表示。我们将使用一个[2x3x3]张量，其中第一个维度表示通道（‘X’为0，‘O’为1），另外两个维度表示行和列。一个（行，列）方格的占用情况被编码为（通道，行，列）项中的1。

![](../Images/bc8d3134b11af7a2a484723be077ad5c.png)

图4：由一个[2x3x3]张量表示的游戏状态。图片由作者提供。

生成匹配所获得的（状态张量，目标值）对构成了神经网络在每轮训练时使用的数据集。数据集在周期开始时建立，利用了之前轮次中发生的学习。虽然第一轮生成完全随机的游戏，但随后的轮次会生成逐渐更逼真的匹配。

## 向游戏中注入随机性

第一轮的比赛生成对抗随机玩家。随后的轮次则使代理对抗自身（因此称为“自我对弈”）。代理配备了一个回归神经网络，经过训练以预测比赛结果，这使其能够选择产生最高预期值的合法动作。为了促进多样性，代理基于epsilon贪婪算法选择动作：以概率（1-ε）选择最佳动作；否则选择随机动作。

# 训练

图5显示了在最多17轮训练中的五个周期内验证损失的演变：

![](../Images/918c2b226de14d865912cc13b549f8d3.png)

图5：不同训练轮次下验证损失的演变。图片由作者提供。

我们可以看到，前几轮训练中验证损失快速下降，然后似乎在均方误差损失0.2左右出现了平稳期。这一趋势表明，代理的回归神经网络在预测对自身进行的比赛结果方面变得更好，因为动作是非确定性的，因此比赛结果的可预测性有限。这也解释了为什么验证损失在一些轮次后停止改进。

## 从一轮到另一轮的改进

对于游戏[*SumTo100*](https://medium.com/towards-data-science/training-an-agent-to-master-a-simple-game-through-self-play-88bdd0d60928)*,* 我们可以在1D网格上表示状态。然而，对于井字游戏，我们无法直接显示状态值的演变。我们可以做的一件事是让代理人与其先前版本对战，观察胜负差异。

对于两位玩家的首个动作使用 ε = 0.5，对于其余比赛使用 ε = 0.1，每次比较运行1000场比赛，结果如下：

![](../Images/d436f33f9948cc6bbba94e340dd29473.png)

图6：代理人与其先前版本的比较。图片由作者提供。

胜场数超过了负场数（显示了改进），直到第10轮训练。之后，代理人的表现没有随轮次改善。

# 测试

现在来看看我们的代理人如何玩井字游戏！

回归神经网络的一个有用特性是可以显示代理人对每个合法动作的评估。让我们与代理人进行一场游戏，展示它如何判断自己的选择。

## 手动游戏

代理人开始，进行‘X’：

![](../Images/c687722e5b02c5ca5227a569d58e4ba7.png)

图7：对抗代理人的比赛及其动作评估。图片由作者提供。

这就是你如何被一个无灵魂的井字游戏机器无情击败！

当我将‘O’放入(1, 0)方格时，预期回报从0.142增加到0.419，我的命运已定。

让我们看看当代理人第二个行动时的表现：

![](../Images/a9e4e2badfd31f24fe08fcbafa46a8ca.png)

图8：对抗代理人的比赛及其动作评估。图片由作者提供。

它没有陷入陷阱，比赛以平局告终。

## 对抗随机玩家的比赛

如果我们[模拟](https://github.com/sebastiengilbert73/tutorial_learnbyplay/blob/main/test/tictactoe_run_multiple_games.py)大量对抗随机玩家的比赛，结果如下：

![](../Images/42a2362e86da81ba20a4b78922b38929.png)

图9：对抗随机玩家的1000场比赛结果。图片由作者提供。

在1000场比赛中（代理人在一半比赛中先手），代理人赢得了950场比赛，没有输掉任何一场，平局50场。这不能证明我们的代理人表现最优，但它确实达到了一个不错的水平。

# 结论

作为[**通过自我对弈训练代理人掌握简单游戏**](/training-an-agent-to-master-a-simple-game-through-self-play-88bdd0d60928)的后续，其中游戏容易破解且状态空间较小，我们采用了相同的技术来掌握井字游戏。虽然这仍然是一个玩具问题，但井字游戏的状态空间足够大，需要代理人的回归神经网络在状态张量中找到模式。这些模式允许对未见过的状态张量进行泛化。

代码可以在[这个仓库](https://github.com/sebastiengilbert73/tutorial_learnbyplay)中找到。试试看，告诉我你的想法！
