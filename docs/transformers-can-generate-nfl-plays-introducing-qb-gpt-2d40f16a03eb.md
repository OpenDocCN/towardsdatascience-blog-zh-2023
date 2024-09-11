# Transformers可以生成NFL比赛：介绍QB-GPT

> 原文：[https://towardsdatascience.com/transformers-can-generate-nfl-plays-introducing-qb-gpt-2d40f16a03eb?source=collection_archive---------14-----------------------#2023-11-07](https://towardsdatascience.com/transformers-can-generate-nfl-plays-introducing-qb-gpt-2d40f16a03eb?source=collection_archive---------14-----------------------#2023-11-07)

## 弥合GenAI与体育分析之间的差距

[](https://medium.com/@sam.chaineau?source=post_page-----2d40f16a03eb--------------------------------)[![Samuel Chaineau](../Images/53523a7fb804971410841d38a47457c7.png)](https://medium.com/@sam.chaineau?source=post_page-----2d40f16a03eb--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2d40f16a03eb--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----2d40f16a03eb--------------------------------) [Samuel Chaineau](https://medium.com/@sam.chaineau?source=post_page-----2d40f16a03eb--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F71edae854a7f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftransformers-can-generate-nfl-plays-introducing-qb-gpt-2d40f16a03eb&user=Samuel+Chaineau&userId=71edae854a7f&source=post_page-71edae854a7f----2d40f16a03eb---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----2d40f16a03eb--------------------------------) · 10分钟阅读 · 2023年11月7日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F2d40f16a03eb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftransformers-can-generate-nfl-plays-introducing-qb-gpt-2d40f16a03eb&user=Samuel+Chaineau&userId=71edae854a7f&source=-----2d40f16a03eb---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F2d40f16a03eb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftransformers-can-generate-nfl-plays-introducing-qb-gpt-2d40f16a03eb&source=-----2d40f16a03eb---------------------bookmark_footer-----------)![](../Images/c8da7b29154888dc16655b0835a0b189.png)

由 [Zetong Li](https://unsplash.com/@zetong?utm_source=medium&utm_medium=referral) 摄影，发表于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

*自从我撰写了关于* [*StratFormer*](https://medium.com/better-programming/transformers-will-guess-nfl-playbooks-f90e7420835b)*的第一篇文章后，我收到了相对较多的反馈和想法（所以首先谢谢大家！）。这促使我深入研究，并尝试进一步的步骤：* ***构建一个足球战术生成器****。在这篇文章中，我介绍了* ***QB-GPT****，一个能够在提供一些元素后有效生成足球战术的模型。可以在* [*这里*](https://huggingface.co/spaces/samchain/QB-GPT) *找到一个专门的 HuggingFace 空间以进行尝试。我将在本月晚些时候分享我关于如何使用这种生成模型作为基础来更好地预测 NFL 战术的工作和发现。行业也在关注这一领域，因为* [*DeepMind 安全研究*](https://medium.com/u/55e08ddea42e?source=post_page-----2d40f16a03eb--------------------------------) *目前正在与利物浦合作研究足球，了解球员在场上的移动方式。*

![](../Images/9191405f2c4641d82bf08476efed5054.png)

QB-GPT 生成的轨迹

![](../Images/b845679a6346a8d6a7cc8c3e4ab33570.png)

真实轨迹

[**StratFormer**](https://medium.com/better-programming/transformers-will-guess-nfl-playbooks-f90e7420835b)，这是我在 2021 年 10 月开始的第一个想法，是一个仅编码器模型，它以轨迹作为输入，尝试完成它并预测与之相关的一些上下文元素（如团队、位置和战术）。虽然这个模型显示出有趣的模式（例如理解真正使 RB 和 WR 区别开来的因素），但它依赖于对战术的“后验”观察。更有趣的可能是深入理解玩家的布置以及一些上下文元素如何实际影响团队的路线。换句话说，当两队在进攻线对峙时，会发生什么？

通过构建这样的算法，我们现在能够建立一个“真正”理解足球比赛的模型，因为它基本上试图从少量元素中重现战术。这就是**QB-GPT**的目标。GPT 在这里的作用是因为它依赖于任何 GPT 模型使用的解码概念。

这篇文章将介绍模型和我为使其“正常”而必须实现的一些必要技巧。一个附加的 HuggingFace 空间可以在 [这里](https://huggingface.co/spaces/samchain/QB-GPT) 生成一些战术，如果你愿意，我会根据成本限制开放一段时间。虽然我认识到它有局限性，并且容易改进，但我认为这样的应用值得与更广泛的受众分享。如果有兴趣讨论或深入了解一些方面，我的联系方式在应用程序和文章末尾。再次声明，我是独自完成了这项工作，使用了我能找到的数据及其相关的不完美之处（如果有人在 NFL/NGS 团队工作看到这篇文章，私信随时欢迎）

## **第一部分：数据**

执行此任务所需的数据非常难以获取。我依赖于 NFL 提供的**Next Gen Stats (NGS)** 工具的特定数据类型，其中，对于任何给定的进攻，我可以跟踪最多 22 名在场上的球员。问题是，这些数据不能通过经典的 API 请求访问。然而，自 2019 年以来，NFL 在 Kaggle 上提供了各种数据竞赛，通常附带 NGS 的数据集。此外，一些人在 GitHub 上曾经抓取过 NGS 浏览器。我没有尝试这样做，只依赖于具有可用格式的数据。

合并数据之间花费了大量时间（大约 200 小时）。最重要的任务是：

+   使用 nflVerse 查找场上的球员

+   关联他们的位置

+   使用 nflVerse 的逐场数据定义进攻线

+   通过从轨迹的每个实例中减去球员的原始位置来规范化轨迹。因此，轨迹的每个元素是时间 i 和时间 0 之间的距离。

+   将距离转换为索引（类似于为 BERT 构建词汇表）

+   执行一些合理性检查（例如，移除未在场上的球员）

+   将数据转换为适用于 TensorFlow 的数组和字典格式

我在整个过程中使用了**Polars**。我强烈推荐任何数据科学家、机器学习工程师、数据工程师或处理大量表格数据的人员将这个令人印象深刻的包快速添加到他们的工具包中。*TLDR：Polars 在小数据集上优于 pandas，在大数据集上优于 pypsark*。

总的来说，我汇编了**47,991 个不同的进攻**，代表**870,559 个不同球员的场上轨迹**（平均每场监测 18 名球员，不幸的是从未有 OL…）。

我以 0.2 秒的帧率监控每个球员的位置，总共**28,147,112 个场上位置**。我将数据限制在前 10 秒内，因为轨迹在之后往往变得越来越混乱，因此从概率的角度进行建模较为困难。

我的数据集从 2018 年到 2022 年，涵盖了**3,190 名独特球员**。数据并不完美，但提供了一个良好的样本，可能足以评估变压器是否有帮助。

总结一下，我的数据来源是：

+   NFL 大数据碗 2021 [link](https://www.kaggle.com/competitions/nfl-big-data-bowl-2021/data)

+   NFL 大数据碗 2022 [link](https://www.kaggle.com/competitions/nfl-big-data-bowl-2022/data)

+   NFL 大数据碗 2023 [link](https://www.kaggle.com/competitions/nfl-big-data-bowl-2023/data)

+   公开的 git 仓库 NGS Highlights [link](https://github.com/asonty/ngs_highlights)

下面是一个表示单个球员输入嵌入的示意图：

![](../Images/df7540caf17fae50eb5087db156cd23b.png)

然后将十一名球员按比赛进行连接，然后按帧截断，以始终保持标记数等于 256。为了确保每名球员的帧数始终相同，我将每名球员的帧数限制为 21（最大 = 21*11 = 231）。因此，我不得不从比赛的某个特定时刻直接创建新的轨迹，因为我的大多数比赛有超过 21 帧。我创建了一个 12 帧的填充步骤，这意味着轨迹现在被分成子轨迹，每次偏移 12 帧。这个过程往往使得预测第 12、24、36 和 48 帧的任务变得更加困难，正如我们稍后将看到的。

![](../Images/0e7ae7ecea369218fa4f514c12181dea.png)

元素可以讨论。例如，用 1 码为基础对场地进行划分的相关性，或为何使用 0.2 秒的帧率。我认为模型（即其训练数据）是一个起点，我想承认并非一切都是完美的。反馈和意见都是欢迎的，只要它们是相关的。

## 第 2 部分：模型

该模型完全受 OpenAI GPT 架构的启发。它依赖于一个嵌入层，将不同的上下文元素添加到输入标记中。然后，将这些嵌入传递到一个使用 3 个头的多头注意力的单个 Transformer 模块中。在大型模型中，应用了第二个 Transformer 模块。输出随后传递到一个具有“relu”激活的全连接层中。为了获得预测结果，我们对 logits 应用 soft-max 激活。

为了适应架构和训练，需要两个技巧：

+   **多时间步因果掩码**：在经典 GPT 中，位置 i 处的嵌入只能关注从位置 0 到 i-1 的标记。在我们的案例中，由于我正在**完全**解码团队，我需要时间 i 处的标记能够关注时间 0 到 i-1 之间的所有标记。与其使用所谓的“下三角掩码”，你最终会得到一个多三角掩码。

![](../Images/5af61ad139a1017cd0cefe0aaeb132e0.png)

注意力掩码

![](../Images/2236dd3b9ce207f1d2382c1e1305a0f8.png)

原始注意力得分和注意力掩码减去后的得分

![](../Images/7eae47de8f4a51ded66bf92c18af574c.png)

不同 vmax 规模下的注意力得分

+   **预层归一化：** 受到 [Sik-Ho Tsang](https://medium.com/u/aff72a0c1243?source=post_page-----2d40f16a03eb--------------------------------) 研究的启发，我实现了其提出的 Transformer 模块，其中归一化在多头注意力和 FFN 之前完成。

该模型不需要大量的注意力层，因为要建模的基本模式相当简单。我使用相同的设置训练了 4 种不同模型的架构。结果显示，嵌入维度在我们能达到的准确度水平中发挥了至关重要的作用，而注意力模块的数量对准确度的提升并不显著：

+   ***Tiny：*** 嵌入维度为 64，表示**1,539,452** **参数**

+   ***小型***：嵌入维度为 128，代表**3,182,716** 个参数

+   ***中等***：嵌入维度为 256，代表**6,813,308** 个参数

+   ***大型***：嵌入维度为 128，但具有两个注意力模块，代表**7,666,556** 个参数

我认为大模型经过深入审查调度器后可能会获得更好的性能。

（用于比较，GPT3 有 1750 亿个参数）

以下图表展示了不同模型在训练过程中的准确率和损失的比较：

![](../Images/0352c1ed1fed7fa5a95e05dd4ac38f8b.png)

四个模型的损失和准确率

## 第三部分：训练

训练集由 80% 的轨迹组成，共有**205,851** 个样本。测试集由 20% 的轨迹组成，共有**51,463** 个样本。

模型使用简单的回调调度器在 9 个周期中训练，学习率从 1e-3 开始，最终为 5e-4，批量大小为 32。

损失是一个类别交叉熵，具有基于训练集（更常见的整数权重在损失中权重较小）的类别权重。设置为 -100 的标签不计入损失。

使用的度量指标包括：

+   **准确率**：下一步团队动作是否与标记的动作完全相同？

+   **前 3 准确率**：下一步标记的动作是否在前 3 个预测中？

+   **前 5 准确率**：下一步标记的动作是否在前 5 个预测中？

![](../Images/9c29b03acaafbc50978382c019d188b5.png)

四个模型的前 3 和前 5 准确率

最终，对与每个动作相关的 x 和 y 坐标进行了 RMSE 检查。此检查使我们能够监控，除了预测不准确之外，它不会偏离真实情况太远（预测一个 1 码的平方轨迹可能很困难）。

## 第四部分：结果

总体而言，不同的模型仍然能够学习潜在的动态模式，并在我的数据集上表现得相对良好。我们可以看到，增加嵌入维度会提高模型的准确率。猜测一个 1 码平方的任务确实具有挑战性，因为足球场上的动作并没有那么小的范围。

我决定将这些预测分为 3 类：时间、比赛类型和位置。

![](../Images/1e6997c9489e5ef9220a740a64359825.png)

准确率和 RMSE 随时间（帧）的变化

对模型来说，前 5 帧相对较容易预测，因为球员经常以相似的动作开始。帧 5 到 10 之间的准确率下降了 40-50%。这是动作趋于不同且有各自路径的时刻。在四个模型中，小模型在轨迹末端特别困难。中等模型即使在长期（超过 20 帧）中也表现得非常好。峰值与填充轨迹的开始相关，因为这些数据在没有过去轨迹知识的情况下输入。

![](../Images/73ffae52dc35c5994bf1536ff488e289.png)

比赛类型的准确性和 RMSE

相对静态的比赛（毫不意外）更容易预测。跑动、传球、开球和踢球（毫不意外）是最难猜测的，因为球员的移动更多，且可能存在混乱的模式。模型之间没有显著差异。

![](../Images/ff1363859b77dc66ea07fa772feba2d0.png)

位置的准确性和 RMSE

位置是一个非常显著的因素，不同位置之间的准确性差异可以达到 10% 到 20%。总体而言，移动较多的位置也是最难预测的。小型和微型模型通常比其他模型的准确性要差。

## **第 5 部分：我通过玩这个模型所见的一切**

该模型的理想温度似乎在 1.5 和 2.5 之间。我测试了 10、20 和 50 的选择。温度和选择值越高，模型往往会变得越来越疯狂，产生奇怪的模式（球员离场、改变方向、两个帧之间的巨大间隙）。

然而，它似乎能够生成有趣的模式，这些模式可以用于模拟和战术手册识别。

## 接下来是什么？

这个模型是原始的。它可以用于后续探索对手策略、定义新的策略，甚至用于提升球员侦察系统。

然而，核心思想是看看这个模型是否能有效地预测比赛结果，即码数增益。我将很快分享我关于 NFL 比赛预测框架的工作，该框架基于 QBGPT 的发现。通过利用这种模型的生成能力，你可以从中绘制出大量场景，从而更容易预测比赛 ;)

与此同时，你可以在这个 hugging face 空间里玩 QB-GPT。如果你喜欢这篇文章、这些工作和发现，别忘了分享和点赞！

（除非另有说明，每张图片均由作者提供）

Samuel Chaineau : [Linkedin](https://www.linkedin.com/feed/)
