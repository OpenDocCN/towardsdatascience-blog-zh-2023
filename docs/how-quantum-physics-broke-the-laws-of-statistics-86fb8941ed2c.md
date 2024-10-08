# 当 1+1≠2：量子物理学如何打破统计学定律

> 原文：[`towardsdatascience.com/how-quantum-physics-broke-the-laws-of-statistics-86fb8941ed2c`](https://towardsdatascience.com/how-quantum-physics-broke-the-laws-of-statistics-86fb8941ed2c)

## 揭示 2022 年物理学诺贝尔奖背后的数据科学

[](https://tim-lou.medium.com/?source=post_page-----86fb8941ed2c--------------------------------)![Tim Lou, PhD](https://tim-lou.medium.com/?source=post_page-----86fb8941ed2c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----86fb8941ed2c--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----86fb8941ed2c--------------------------------) [Tim Lou, PhD](https://tim-lou.medium.com/?source=post_page-----86fb8941ed2c--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----86fb8941ed2c--------------------------------) ·阅读时间 12 分钟·2023 年 5 月 3 日

--

统计学是数据科学的核心支柱，但其假设并非总是经过充分检验。这在量子计算的崛起中更加明显，因为即使是统计学公理也可能被违反。在这篇文章中，我们将深入探讨量子物理学如何打破统计学，并揭示使用数据科学类比来理解它的方法。

![](img/40fd156b4ea851346d5ed01ce5e16f8a.png)

2022 年的物理学诺贝尔奖归结于掷硬币，发现量子物理学违背了统计学的基本定律（背景： [Dan Dennis](https://unsplash.com/@cameramandan83)，硬币 3D 模型： [hyperionforge](https://sketchfab.com/hyperionforge)，组合由作者提供）

让我们玩一个掷硬币的游戏：掷三枚硬币，并尽量让它们都落在不同的面上。这看似是不可能完成的任务，因为无论硬币如何伪造，它最多只能有两个面。所有三次掷硬币的结果都不同的可能性实在太少。

然而，凭借量子物理学的力量，这样一个看似不可能的成就可以在统计上实现：三次掷硬币可以全部落在不同的面上。而获胜的奖励？2022 年物理学诺贝尔奖，授予了阿兰·阿斯佩、约翰·克劳斯和安东·蔡林格，颁奖日期为 2022 年 10 月 4 日。

根据 [nobelprize.org](https://www.nobelprize.org/prizes/physics/2022/press-release/)，他们的成就

> *“对于纠缠光子的实验，确立了贝尔不等式的违反，并开创了量子信息科学。”*

这句话充满了术语：*纠缠光子*、*贝尔不等式*和*量子信息*科学。我们需要一个更简单的、通俗的描述来解释这样一个重要的成就。以下是翻译：

> 科学家们通过展示量子物理学可以违背看似不可能的几率，证明了我们对世界的统计学视角存在缺陷。

这些不可能几率的细节被称为*贝尔不等式*的数学公式捕捉到。研究人员通过激光（使用*纠缠光子*束）展示了这些不可能的几率，而不是掷硬币。

这与数据科学有什么关系？由于我们的量子力学世界是数据的**终极来源**，统计规律中的缺陷可能会破坏数据科学的根基。如果统计学确实不完整，我们将无法信任从中得出的结论。

幸运的是，在我们的宇宙中，这些统计缺陷通常非常微小和可忽略。尽管如此，理解经典统计学需要如何修改仍然很重要，因为在遥远的未来，数据科学可能需要结合这些缺陷（例如，量子计算机）。

在回答量子物理如何违背统计规律之前，我们首先需要理解统计学如何有效地描述我们的世界。

# 以概率取代不可预测性

![](img/3ba042699fb5c0b3004301e0451dfb9c.png)

一堆硬币中有多少个正面/反面？答案大约是 50%。概率是混沌世界的近似模型（图片来源：[Claudio Schwarz](https://unsplash.com/@purzlbaum?utm_source=medium&utm_medium=referral)于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)）

掷硬币，你得到正面/反面。然而硬币并不完全是随机的：一个控制完美的机器人可以严重操控掷硬币的结果。

50/50 的概率是什么意思？硬币的方向对其周围微小细节非常敏感。这使得预测硬币的着陆方向变得困难。因此，我们选择一个非确定性的结果，而不是解决非常复杂的方程来得出确定的结果。我们观察到典型的硬币在正面/反面方面相当对称。在没有任何特定偏差的情况下，50/50 的几率将是一个很好的近似值（尽管研究表明这些几率可以被改变，例如，[Clark MP 等人](https://pubmed.ncbi.nlm.nih.gov/?term=Clark+MP%5BAuthor%5D)）。

总结一下，

> 概率是建模复杂系统细节的近似值。复杂的物理学被不确定性取代，以简化数学运算。

从天气模式到经济和医疗保健，不确定性都可以追溯到复杂的动态。数学家将这些近似值转换为基于公理的严格定理，以帮助我们操作和从不可预测的结果中得出见解。

# 计算概率

![](img/82ea30c34cdbdddf944fdf50e2b10982.png)

统计公理帮助我们每天做出决策，这包括医学领域（照片由[Towfiqu barbhuiya](https://unsplash.com/@towfiqu999999?utm_source=medium&utm_medium=referral)拍摄，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)）

量子物理学如何打破统计规律？它违反了*可加性公理*。

这个公理是如何工作的？让我们考虑一些常见的场景，其中我们使用统计数据来做出决策：

1.  当外面下雨🌧时，我们会带上雨伞☔️。

1.  当我们生病时，医生会开药💊帮助我们康复。

在雨天的场景中，尽管可能有数万亿种雨滴落下的方式，但这些可能性中的大多数会使我们变湿变冷，因此我们带上雨伞。

在医生场景中，给定一个诊断，有多个可能性：不同的疾病进展、副作用、恢复率、生活质量，甚至误诊……等等。我们选择能够带来最佳整体结果的治疗方案。

加法公理是一个形式化的声明，我们可以将概率分解成可能性：

![](img/1524a82842110f7f763db3624deb451d.png)

这个公理是合理的，因为统计学旨在量化我们对系统的无知。就像我们给硬币翻转分配 50/50 一样，我们使用加法公理通过对其组成部分的所有可能轨迹进行平均来推导系统的属性。

尽管这一切听起来很直观，但这真的就是自然的运作方式吗？通过实验，我们可以确认宏观物体是这样工作的，但当我们放大到微观时会发生什么？它与宏观世界一样，亚原子演员从一个场景移动到下一个场景吗？还是更像是电影屏幕，上面的抽象像素在闪烁开/关，创造出一个故事的幻觉？

结果是，像素类比更为准确。当我们放大时，可能性的不确定路径变得更加模糊。因此，加法公理被违反。

我们的公理的替代是什么？它是量子物理学的定律。

# 量子可能性的计数

![](img/f48da6e1f29030d5fea00b012994cbb1.png)

结果是，我们的基本量子世界与 AI 模型非常相似（照片由[Steve Johnson](https://unsplash.com/@steve_j?utm_source=medium&utm_medium=referral)拍摄，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)）

虽然量子物理学相当复杂，但我们可以通过数据科学类比来理解其要点。量子物理学基于线性代数，因此可以被看作是一个特殊的机器学习模型。

以下是与机器学习类比相关的关键量子公理：

1.  世界由一长串（复杂的）数字描述，称为*量子状态*——类似于图像的像素值，或更抽象的机器学习嵌入向量。

1.  随着时间的推移，这个量子状态会发生变化。这个更新可以通过将我们的量子状态传递通过类似神经网络的函数来计算，这个函数称为*算符*（技术上是[单位矩阵](https://en.wikipedia.org/wiki/Unitary_matrix)）：

![](img/94268956a200f450e1aecacf92ed6f6c.png)

量子状态变化的示意图（作者提供）

继续我们的机器学习类比，我们可以把宇宙看作一个巨大的神经网络。每个算子代表一个（线性）网络层。通过这个网络，每次发生的交互都被印刻在我们宇宙的量子态上。这个计算从时间开始以来就不断运行。这是看待我们世界的一种深刻方式：

> 我们的一致现实源于量子态中的孤立分组。
> 
> 我们对物体存在的宏观感知源于我们算子的特定神经网络连接。

听起来有些抽象，我们来考虑一个具体的例子：量子物理如何描述降落在我们头上的雨滴？

1.  空气分子和我们在开放环境中的数据被捕捉在一个量子态中。

1.  当水分子感受到地球的引力时，量子态会通过相应的算子进行更新。

1.  在经历了类似神经网络的多层更新之后，量子态会获取一些特定的数值。

1.  物理定律决定了这些数字往往会形成簇。这些簇中的一些转化为这些雨滴的一致存在，这最终与我们的神经元感知到这些雨滴相关联。

从现代观点来看，没有理由认为加法公理必须成立。因为

> 类似于机器学习黑箱，我们不总是能够跟踪量子态的所有物理属性。因此，物理结果并不总是附带一系列中间可能性。

在雨滴场景中，这意味着我们不能总是找到导致特定水分子下落的量子态中的具体数字。事实上，量子态通常包含多个位置的分子数据（例如，叠加态），我们对其物理位置的感知可能是所有这些数据的复杂总和。

这可能看起来很矛盾，因为我们在日常生活中并没有感觉到奇怪的差异和叠加态！但原因是这些差异非常微小，它们的微小性可以通过技术[退相干理论](https://en.wikipedia.org/wiki/Quantum_decoherence)来证明，这超出了我们的范围（虽然[这里](https://medium.com/physicist-musings/quantum-physics-without-all-the-weirdness-the-consistent-histories-approach-cfbd91b16420)是我的一篇文章，可能会提供一些启示）。

尽管如此，小并不等于零。量子效应有时可能是显著的，并且可能导致看似不可能的统计数据。

如何做呢？让我们来找出答案。

# 测试 1+1 = 2

为了使普通统计学定律无效，我们需要考虑一些简单但不可能的情景。其中最简单的涉及 3 枚硬币。

想象有 3 个机器人进行 3 次独立的掷币实验。在经典统计学中，我们可以利用加法公理完全指定统计数据：通过列出所有 8 种结果及其概率（注意：机器人/硬币可能被操控）：

![](img/19170184ed38b4ce2d236ab8172a6427.png)

翻转 3 枚硬币的所有 8 种可能性（作者提供）

从实验上来看，我们可以通过重复这些抛币来测量这些概率。

无论概率选择如何，都存在一个理智约束：一个硬币只有 1+1 = 2 面，因此当我们抛 3 枚硬币时，至少有 2 枚会落在相同面上。因此，如果我们随机（均匀地）选择一对硬币进行检查，我们应该预期至少有 1/3 的机会观察到它们是相等的。

让我们尝试一些示例，将三枚硬币标记为*A*、*B*、*C*

1.  如果 3 枚硬币都是公平和独立的，那么我们选择一对相等的硬币的机会是 1/2。

1.  如果*A* = *B*，但*A* ≠ *C*。无论*A* 如何落地，只有一对是相等的。选择这一对的机会是 1/3。

我们看到相同对的概率始终至少为 1/3。这可以总结为*贝尔不等式*（参考 L. Maccone 的[论文](https://arxiv.org/abs/1212.5214)）

![](img/2f6eb94fa9b483baa86471de6a29cc5d.png)

贝尔不等式的最简单示例（作者提供的图像）

尽管测试如此显而易见的事物可能看起来荒谬，但事实证明，这一不等式实际上可以被*违反——*这证明它们并非如此显而易见。

# 诺贝尔级别的抛币

![](img/5602d1cb3d4a85494c7c6d6bfc0b69e2.png)

量子抛币通常使用类似于上述设置的激光（维基媒体：[Adelphi Lab Center](https://commons.wikimedia.org/wiki/File:Optoelectronics_experiment.jpg)，[许可证](https://creativecommons.org/licenses/by/2.0/deed.en)）

为了观察贝尔不等式的违反，物理学家不能仅依赖传统的硬币。相反，他们需要利用由激光制成的量子硬币，这些硬币具备了抛币所需的所有要素：

1.  抛硬币：将激光发送到光束中

1.  观察正面/反面：在两个探测器中的一个上获取读数*

1.  随机性：读数通常是不可预测的，除非进行操控

(* 如果没有探测器观察到任何内容，可能会有故障读数)

现在，我们可以将激光设置为不同的方向，以模拟 3 次不同的抛币。那么量子硬币如何处理不可能的情况呢？如果我们观察三次抛币的字面结果，看到三种不同的结果在逻辑上是不可能的。

这就是贝尔不等式的作用：它将关于 3 枚硬币的逻辑陈述分解为仅涉及 2 枚硬币的概率陈述。因此，如果我们抛 3 枚硬币，但每次仅观察 2 枚，那么在保持逻辑的同时，可以违反统计规律。在量子物理学中，抛硬币与观察硬币遵循两个不同的相互作用：

+   **量子**：抛硬币和观察硬币由两个不同的算符来支配。尚未被观察的抛币不需要被赋予一个确定的结果*。

这与经典统计学形成对比

+   **经典**：硬币被掷出时，正面/反面已确定。这是由加法性公理保证的。无论我们是否决定观察它，这一点都不影响结果。

(*这就是“远距幽灵行动”发挥作用的地方，因为任何时刻任何人都可以启动探测器来观察第三个硬币，从而破坏我们的结果。)

那么我们如何进行实验呢？我们需要准备我们的硬币处于特定的量子态。这里，我们构建一个系统，其中三个硬币的量子态可以由平面上的三个向量表示，如下图所示*：

(* 从技术上讲，量子态涉及更复杂的[纠缠光子](https://en.wikipedia.org/wiki/Quantum_entanglement)，但为了简洁起见，我们将跳过这些细节)

![](img/a4b0e749c4617db61deaac73144407f4.png)

违背贝尔不等式的三硬币量子态（作者）

两次掷币结果相同的概率是多少？答案来自物理学，并被设计为余弦相似度的平方：

![](img/3c9882e93f025773d317184863354688.png)

相同掷币概率（作者）

现在，如果我们随机选择一对量子硬币进行检查*，它们相同的概率只有 1/4；这比逻辑上的 1/3 保证还要低！

(*实验需要设置成这样，选择是在硬币被掷出之后进行的，以便排除粒子和设备之间的幽灵勾结)

用我们的贝尔不等式重新表述，我们有

![](img/ffa9b2689c6b0a6aa544cddd3c0c3514.png)

贝尔不等式违背（作者）

我们的理智检查被违背了！如果我们假装经典统计仍然适用，这将意味着至少 1/4 的时间，所有三次掷币结果都不同！

请注意，虽然我们的三硬币实验易于理解，但在实验结果中存在实验难度和潜在漏洞。因此，典型实验往往涉及更多的掷币和更复杂的观察（例如，[GHZ 实验](https://en.wikipedia.org/wiki/GHZ_experiment)由[潘建伟等](https://www.nature.com/articles/35000514)进行）。

# 新的统计显著性

那么，我们看到量子概率有时会导致意想不到的结果，这有什么大不了的，我们为什么要在意？

首先，让我们从实际应用开始。随着技术不断推进，将更多计算能力压缩到更小的体积中，量子物理学将变得越来越重要。最终，我们的计算范式需要进行彻底的改革，以充分利用量子设备。因此，虽然贝尔不等式的违背可能很微妙，但这表明我们在设计量子算法时需要仔细思考。

其次，这些违背暴露了传统统计推理的基本限制。例如，如果有人赢得了彩票，将其原因归因于彩票球以特定方式出来是完全合理的。然而，我们不能放大并因果关联中奖彩票与房间内所有分子（量子）状态。因此，我们的因果推理统计理论有一个物理极限！

最后，量子效应挑战我们重新思考我们的宇宙。尽管量子物理已经被反复验证，但它仍然可能只是一个近似值。未来，我们可能会发现它被更抽象的基本法则所取代。

作为历史教训，甚至爱因斯坦也被量子物理的怪异性所劝阻，以至于他宣称“上帝不会掷骰子”来拒绝它。然而，量子物理继续取得胜利，并在推进我们现代技术和对世界的理解中发挥了基础性作用（参见我的[文章](https://medium.com/physicist-musings/quantum-physics-is-everywhere-and-we-should-all-learn-it-26c9b1dedd2a)）。

总结来说，量子物理主宰了世界，而 2022 年的物理学诺贝尔奖突显了其与统计学和数据科学的深刻联系。虽然量子物理不常被教授，但我们都应该努力理解并接受它的重要性。

如果你喜欢这篇文章，你可能会喜欢下面这些相关的文章。请留下评论/反馈，因为我花费了无数小时来完善上述许多物理和数据科学的解释，我很想听听你的想法。如果你想支持我，只需分享我的文章，并帮助我传播科学知识。👋

[](https://medium.com/physicist-musings/quantum-physics-is-everywhere-and-we-should-all-learn-it-26c9b1dedd2a?source=post_page-----86fb8941ed2c--------------------------------) [## 量子物理无处不在，我们都应该学习它

### 从我们的存在到计算机芯片的工作方式，量子物理对于理解宇宙至关重要。

medium.com](https://medium.com/physicist-musings/quantum-physics-is-everywhere-and-we-should-all-learn-it-26c9b1dedd2a?source=post_page-----86fb8941ed2c--------------------------------) [](https://medium.com/physicist-musings/quantum-physics-without-all-the-weirdness-the-consistent-histories-approach-cfbd91b16420?source=post_page-----86fb8941ed2c--------------------------------) [## 量子物理没有所有怪异性：一致性历史方法

### 量子物理不一定是怪异的。在这里，我们探讨了一种现代且直观的量子系统解释……

[为什么我们不生活在模拟中](https://medium.com/physicist-musings/why-we-dont-live-in-a-simulation-a-physicist-s-perspective-1811d65f502d?source=post_page-----86fb8941ed2c--------------------------------)

### 将现实描述为模拟大大低估了我们世界的复杂性。这里是为什么模拟……

[为什么因果关系是相关性的：物理学家的观点（第一部分）](https://medium.com/physicist-musings/why-causation-is-correlation-a-physicist-s-perspective-1811d65f502d?source=post_page-----86fb8941ed2c--------------------------------)

### 我们都听过“相关性不意味着因果性”这句话，但没有人真正谈论因果性到底是什么……

[为什么因果关系是相关性的：物理学家的观点（第一部分）](https://towardsdatascience.com/why-causation-is-correlation-a-physicists-perspective-part-1-742696d130e8?source=post_page-----86fb8941ed2c--------------------------------)
