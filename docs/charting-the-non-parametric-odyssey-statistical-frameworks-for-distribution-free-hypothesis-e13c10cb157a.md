# 绘制非参数探索：无分布假设检验的统计框架

> 原文：[`towardsdatascience.com/charting-the-non-parametric-odyssey-statistical-frameworks-for-distribution-free-hypothesis-e13c10cb157a?source=collection_archive---------7-----------------------#2023-05-10`](https://towardsdatascience.com/charting-the-non-parametric-odyssey-statistical-frameworks-for-distribution-free-hypothesis-e13c10cb157a?source=collection_archive---------7-----------------------#2023-05-10)

## 评估符号检验和 Wilcoxon 符号秩检验的机制

[](https://namanagr03.medium.com/?source=post_page-----e13c10cb157a--------------------------------)![Naman Agrawal](https://namanagr03.medium.com/?source=post_page-----e13c10cb157a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e13c10cb157a--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----e13c10cb157a--------------------------------) [Naman Agrawal](https://namanagr03.medium.com/?source=post_page-----e13c10cb157a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F5bbb90aa727&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcharting-the-non-parametric-odyssey-statistical-frameworks-for-distribution-free-hypothesis-e13c10cb157a&user=Naman+Agrawal&userId=5bbb90aa727&source=post_page-5bbb90aa727----e13c10cb157a---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e13c10cb157a--------------------------------) ·19 分钟阅读·2023 年 5 月 10 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe13c10cb157a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcharting-the-non-parametric-odyssey-statistical-frameworks-for-distribution-free-hypothesis-e13c10cb157a&user=Naman+Agrawal&userId=5bbb90aa727&source=-----e13c10cb157a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe13c10cb157a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcharting-the-non-parametric-odyssey-statistical-frameworks-for-distribution-free-hypothesis-e13c10cb157a&source=-----e13c10cb157a---------------------bookmark_footer-----------)![](img/a20ca4c61e3ea8da4ffa4dec3594bbf3.png)

图片由 [Kiwihug](https://unsplash.com/@kiwihug?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/Hld-BtdRdPU?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 内容

1.  介绍

1.  工具 1：单样本符号检验

1.  工具 2：双样本符号检验

1.  符号检验的局限性

1.  工具 3：单样本 Wilcoxon 符号秩检验

1.  威尔科克森符号秩检验：可能的并发症

1.  工具 4：两样本威尔科克森符号秩检验

1.  结论

# 介绍

统计学是知识工具的集合，允许我们通过包括但不限于参数估计、置信区间构建和假设检验等工具从数据中推断以验证我们的假设。在本文中，我们将学习允许我们检验有关不同数据分位数值的假设的框架，即符号检验和威尔科克森符号秩检验。这些框架的独特之处在于，与 z 检验或 t 检验等流行的假设检验不同，这些检验不需要对数据做任何假设，无论是通过直觉还是通过中央极限定理强加的，即，它们是无分布的或非参数的。您只需拥有来自对称和连续分布的数据，您将配备检验如下主张的工具：

![](img/7149e1e2a3a1cab6e3a9dda81e9ac472.png)

# 工具 1：单样本符号检验

让我们从一个简单的检验开始，它只涉及计算小于或等于阈值的值，用于假设检验：符号检验（即二项检验）。特别是，我们考虑一个大小为 n 的样本 X₁, …, Xₙ 以及以下陈述的简单零假设和相应的替代假设：

![](img/6d5f260a110c17c2da87371691a5f9d6.png)

其中 m 表示给定数据的中位数。作为起点，我们来考虑在传统 t 检验设置下我们如何处理这个问题。在这种情况下，我们会假设 n 很大，使得中央极限定理发挥作用。此外，由于分布总是假设为对称的，检验中位数应该与检验均值相同。因此，我们可以定义检验统计量和相应的临界区域如下：

![](img/160f30a10e64f30d7379995d0f9e3b53.png)

这很简单且相当强大（正如我们稍后将看到的），但它基于一个假设，即分布收敛到一个自由度为 n — 1 的 t 分布。但这可能不一定成立，特别是当 n 不够大时。这需要一个替代框架，让我们可以在不引入数据的分布假设的情况下检验这个假设。这引出了符号检验，它只是涉及统计总数大于 m⁰ 的观察值。例如，考虑以下的 n 个样本数据：

![](img/ac112324978024a1015d6b8b3ecec54f.png)

假设如下：

![](img/53c8ef19a49b100c48b1f8a01b969911.png)

如上所述，我们首先计算大于 m⁰ 的 Xᵢ 的数量。令 N⁺ 表示大于 m⁰ 的 Xᵢ 的随机变量，令 n⁺ 为给定样本的实际值：

![](img/a3de8510815e024d68553abeb14035ac.png)

现在，我们确定在原假设下上述检验统计量的分布。由于在原假设下 m = m⁰，样本大于 m⁰ 的概率必须是 0.5。因此，N⁺ 衡量的是大于 m⁰ 的样本数，每个样本在原假设下有 0.5 的概率贡献到 N⁺。换句话说，N⁺ 计算的是成功次数，每次成功的概率为 0.5。这定义了一个二项随机变量，参数为样本数 n = 10 和成功概率 p = 0.5。因此，

![](img/cf2c52e71931808c4b07c8baec97d280.png)

现在，已知检验统计量在原假设下的分布，我们可以继续计算给定样本的 p 值。请记住，p 值表示在原假设为真的假设下观察到比检验统计量样本值更极端值的概率。因此，

![](img/45dcadd08e7adba1f1997e2ec5197546.png)

显然，p 值相当大。因此，即使在 10% 的显著性水平下，由于 p 值 > 0.10，我们也未能拒绝原假设！这就结束了简单却有用的符号检验应用。这也引出了另一个重要问题：如果备择假设在另一个方向，或者如果是双侧的情况呢？过程依然相同，只是我们计算比 m⁰（N⁻）更少的观察次数。具体来说，假设我们有兴趣进行测试：

![](img/965d2ea4b8cd84913e0386c5117223b0.png)

然后，

![](img/b65704d808e73534bff8750b4764d96a.png)

请注意，分布仍然遵循二项分布，参数与前一个案例的推理相同。最后，我们讨论双侧假设的情况：

![](img/5ed99ed2fdba0d80f8c7afeb20190ebd.png)

对于这种情况，我们计算样本的 N⁺ 和 N⁻：

![](img/3096e3bcc4adb035237f55b4760a9869.png)

与之前一样，p 值给出了比 6 或更多大于 0.5 的数值更极端的结果的概率。因为这是一个双侧检验，所以极端结果可以是 6 或更多大于 0.5 的数值，或者 4 或更少大于 0.5 的数值。

![](img/111ba9f360355fde73fbafca6939ccc1.png)

和以前一样，p 值相当大。因此，即使在 10% 的显著性水平下，由于 p 值 > 0.10，我们也未能拒绝原假设！这难道不令人着迷吗？仅仅是简单的计数和最基本的概率分布就能让我们测试这样的假设。但我们能否推广结果？换句话说，我们能否将此检验通用于任何特定分位数，而不仅仅是中位数？当然，方法论保持完全相同。例如，假设对于上述相同的数据集，我们有以下假设：

![](img/d4e2262542896795b3f25a1ae2ca7192.png)

注意π₀.₂₅表示数据的第 25 百分位数或第 1 四分位数。和以前一样，我们计算小于 3 的 Xᵢ数量：

![](img/f475e13496091f7c24ea9139bef648e0.png)

现在，我们确定在零假设下上述检验统计量的分布。由于在零假设下π₀.₂₅ = 3，因此样本小于 3 的概率必须是 0.25。因此，N⁻测量小于 3 的样本数量，每个样本在零假设下有 0.25 的概率贡献给 N⁻。换句话说，N⁻计算成功的数量，每个成功的概率是 0.25。这定义了一个参数化为样本数量 n = 10，成功概率 p = 0.25 的二项随机变量。因此，

![](img/7dfe18fb1315742ddba34587bb884631.png)

因此，

![](img/17afc994e96435ab7aed6b4776631c88.png)

这是一个较低的 p 值，但仍不足以在 5%或 10%的显著性水平下拒绝。由此，我们结束了对单样本符号检验的讨论。

# 工具 2：两样本符号检验

在本节中，我们将尝试将前面介绍的符号检验推广到两样本的情况。之前，我们提供了数据（由单一样本 X 的观察值组成），我们可以检验中位数（或任何分位数）是否大于、小于或不等于给定的阈值。在本节中，我们将上述概念扩展到两样本的情况。特别地，假设我们得到独立的样本对数据(x₁, y₁)，· · ·, (xₙ, yₙ)。零假设声明两个样本大于对方的概率相等，即它们的中位数差为 0，而备择假设则建议两个样本之间存在差异，即中位数差异为正、负或非零。数学上，零假设和备择假设如下：

![](img/888d7fd887aba34d28e1fcd5ce6bae06.png)

示例：假设我们给出了以下配对数据：

![](img/3fff6daa8be6e28ab714982a7d8170e4.png)

假设如下：

![](img/ac944abab676dad739e10e7a9fe7ebfc.png)

推导检验统计量并找到其分布的过程保持类似。在前面的案例中，我们只是检查了样本是否大于或小于阈值（零假设下的值）。在这个案例中，我们通过比较每对观察值来检查符号。换句话说，我们检查每个观察值的 Wᵢ = Xᵢ − Yᵢ的符号，从 i = 1 到 n：

![](img/366f47d45d869d73401c9080f995cbfb.png)

表 1：作者提供的图片

检验统计量定义为：

![](img/e355fce9aaffb46fc05120fe5d9e8d3c.png)

根据之前的逻辑，在原假设下，检验统计量的分布是 Bin(n, 0.5)（可以将检验统计量的值看作在 n 次试验中的成功次数，其中每次成功即 Xᵢ > Yᵢ 的概率为 0.5，因为两个样本都同样可能大于对方）。因此，p 值由以下公式给出：

![](img/8a3eb7f9c7d1b9d9ca0bf9437bdba61b.png)

确实，p 值要小得多，我们可以在 10%的显著性水平下拒绝原假设！最后，如果备择假设是双侧的，我们来尝试计算相同数据的 p 值：

![](img/479870ebc4b08fd9556963ccfcc87318.png)

因为这是一个双侧检验，极端结果可以是 8 个或更多正符号，或者 2 个或更少正符号：

![](img/9239649e4e05566d23bd61de4208a0a1.png)

因此，我们无法在 10%的显著性水平下拒绝双侧假设。这结束了我们关于进行单样本和双样本符号检验的讨论。

# 符号检验的局限性

在前两节中，我们使用符号检验进行不同类型的假设检验。由于其非参数性（即分布无关性），符号检验被发现非常有用。然而，一般来说，符号检验的能力并不是很强（即，对于相同的显著性水平，第二类错误的概率仍然很高）。主要原因是符号检验只考虑值的符号，即它们是否小于原假设下的阈值（对于单样本）或是否小于其对应值（对于双样本）。它没有考虑差异的大小，即偏离 0 的程度。在实践中，符号检验很少使用，但由于其极大的简单性，它仍然是引入非参数假设检验讨论的一个很好的工具。这引导我们进入一个新的假设检验框架：威尔科克森符号秩检验，这在某种程度上是对符号检验的扩展，考虑了偏离 0 的大小顺序，并为每个样本值分配秩。在下一节中，我们将更详细地描述该检验。

# 工具 3：单样本威尔科克森符号秩检验

威尔科克森符号秩检验背后的理论稍微复杂一些。但不用担心。让我们重新审视上一节中的例子，并依次应用威尔科克森检验：

![](img/fc108f5bcb37fb2a7048f67047f719b8.png)

**步骤 1：** 计算每个 Xᵢ 和 m⁰ 之间差值的绝对值：

![](img/d21708499c47097c1fb6a483ce7bc46d.png)

表 2：作者提供的图片

**步骤 2：** 对步骤 1 中计算出的每个值进行排名，即为每个 | Xᵢ − m⁰| 分配排名 (Rᵢ)，从 1 到 n：

![](img/2ce047c5f4f0c4d83b90ecc4956d36d8.png)

表 3：作者提供的图片

**步骤 3：** 计算每个样本的符号秩 (Rₛᵢ)，其中符号由 Xᵢ − 4 的符号给出（与符号检验中使用的符号相同），即：

![](img/d8c1bfd3e5e51ac3824ebad1331e69e2.png)

表 4：作者提供的图像

**步骤 4：** 计算 Wilcoxon 符号秩检验统计量，其由所有数据点的符号秩之和给出：

![](img/fd65a5daa17b5c852e64fb85700f2a6b.png)

在讨论 W 的分布之前，让我们暂停一下，思考这些步骤的逻辑。在符号检验中，我们只是计算了大于或小于 m⁰ 的样本数量。在这里，我们通过秩来加权计数，这衡量了不同样本相对于 m⁰ 的绝对偏差。这使我们能够对远小于或远大于 m⁰ 的值给予更高的权重，帮助我们克服符号检验的局限性。

现在，让我们评估 W 的分布。需要提到的是，W 的分布没有封闭形式的公式（虽然其矩生成函数有封闭形式的表达式，但其质量函数没有这样的表达式）。一种方法是使用表格（是的，有专门为不同样本量设计的 W 临界值表）。另一种替代方法是使用 Lyapunov CLT 来近似 W 的分布（CDF）：

![](img/52d6b3880e0e15f38fee20209b521b2e.png)

其中 Φ 是标准正态分布的累积分布函数。精确的数学公式相当复杂，超出了本文的范围。但是，我们仍将尽力理解它。让我们尝试计算 W 的期望和方差：

![](img/8a48db14ae72f5da24cfec3696d1678d.png)

其中 Sᵣ 表示第 rᵗʰ 秩的符号（所有秩值将从 1 到 n）。因此，

![](img/501de44e03949ded7717efd6e8ea0650.png)

如果我们对 W 进行标准化，我们得到：

![](img/998878c6f62e828369cf8ef4c545abcf.png)

确实，它与 Φ 内部的表达式非常相似。实际上，通过 Lyapunov CLT（不是传统的 CLT，因为尽管 rSᵣ 是独立的，但它们不一定是同分布的），我们 h

![](img/c59742d3d0fcf9f0116742c20d496908.png)

这导致了 W 分布中的Φ项。你可能还会注意到 W 的 CDF 包含一个额外的 1\. 这只是一个单位修正。回忆一下 W 是一个离散随机变量。每当离散随机变量被连续随机变量（例如正态分布）近似时，添加修正项总是很重要的，通常是半单位修正。半单位修正通过调整连续分布的概率，使其更接近离散分布的概率，从而帮助解决这个差异。具体来说，这个修正涉及将用于定义连续分布的区间的中点向相反的四舍五入约定方向移动 0.5 单位。这确保了连续分布中分配给区间的概率更接近离散分布的概率，从而提高了近似的准确性。然而，对于近似 W，我们倾向于使用全单位修正（而不是传统的半单位修正）。这是因为 W 的值只能以 2 的倍数不同。这样考虑：如果你有一个 W 值为 w，并且你有兴趣将 W 值减少 1，你将不得不翻转一个正排名，这会使 W 的值减少 2。例如，在我们的示例中，W 被计算为 3。如果我们想减少 W 值 1，我们能做的最好是将+1 翻转为-1，但这将使 W 的值减少 2（-1–1 = -2）。类似的逻辑适用于增加 W 的值。因此，为了控制 W 可以取的值范围，使用全单位修正（0.5 × 2 = 1）而不是传统的半单位修正（0.5 × 1 = 0.5）。因此，在应用全单位修正并使用 Lyapunov CLT 之后，我们得到：

![](img/aecc4da2e81e30e1cba89df13c63137e.png)

现在我们已经获得了 W 分布的近似表达式，我们继续计算我们示例的 p 值。回忆一下，p 值表示在原假设为真的假设下观察到比样本观测值更极端的值的概率。因此，

![](img/b3cc00d2b559b6336f7e0984e9004090.png)

显然，p 值相当大。因此，即使在 10%的显著性水平下，由于 p 值 > 0.10，我们未能拒绝原假设！这就是 Wilcoxon 符号秩检验的简单而有用的应用。现在让我们考虑当备择假设在另一个方向上的情况。特别是，假设我们有兴趣测试：

![](img/30e90198eb4ebb7234a0561685b9746c.png)

检验统计量仍然保持不变。然而，由于支持备择假设的方向发生变化，p 值计算也相应调整（根据 p 值的定义）：

![](img/84457dbe74ae797fd38ca5b91604d086.png)

再次，p 值较大，我们在 10% 的显著性水平下未能拒绝原假设。最后，让我们看一下双边情况：

![](img/cb6c8cc38de0452c1843b843aa916512.png)

检验统计量仍然保持不变。然而，就像之前一样，由于支持替代假设的方向发生了变化，p 值的计算也会相应调整（按照 p 值的定义）：

![](img/87573c3ac3c9c5e16c8bc6b9167e39f6.png)

再次，p 值较大，我们在 10% 的显著性水平下未能拒绝原假设。现在，让我们将检验统计量推广到任何特定的分位数，而不仅仅是中位数。方法完全相同。例如，假设对于上面的相同数据集，我们给出以下假设：

![](img/1a3f055fa21768ef737cc7dea8083603.png)

就像之前一样，我们构建表格并汇总符号秩以得到检验统计量：

![](img/ffab3d6dab8aaf9cc6f2b86ee592345e.png)

表 5：作者提供的图片

![](img/700490045d868650679012a0fd7f5e29.png)

现在，我们确定上述检验统计量在原假设下的分布：

![](img/1cdc7c457538a3dfe450ac5564c7bf4e.png)

如果我们标准化 W，我们得到

![](img/c82f74a0f706c24491685058bc6a9f11.png)

根据 Lyapunov CLT，我们有：

![](img/4148114ea1018e89ae55aba5dcfd8fa6.png)

因此，

![](img/0e08e44242febcb158fc5e1b00f50f4f.png)

这是一个较低的 p 值，但仍然不够低，无法在 5% 或 10% 显著性水平下拒绝原假设。至此，我们结束了对单样本 Wilcoxon 符号秩检验的讨论。

# Wilcoxon 符号秩检验：可能的复杂情况

使用 Wilcoxon 符号秩检验时可能会出现两种可能的复杂情况：

1.  对于 i ≠ j，|Xᵢ − m⁰| = |Xᵢⱼ − m⁰| 即，绝对偏差与 m⁰ 相同。在这种情况下，哪个观察值会被分配更高的秩？首先，在数据遵循连续分布的假设下，相同的数据理论上不应出现多次。但是，实际上，往往会发生这种情况。有很多方法可以解决这个问题，但最常见的策略是为每个样本分配秩的平均值。例如，如果有 4 个样本值，它们与 m⁰ 的绝对偏差相同。如果这些观察值的秩为 7、8、9 和 10，则为每个样本分配平均秩：(7 + 8 + 9 + 10)/4 = 8.5。

1.  如果 |Xᵢ − m⁰| = 0，对于某些 i，我们从分析中排除该样本。我们仅使用减少后的样本（以及相应的减少样本量）来计算检验统计量及其分布。

例如，如果我们给出以下数据和假设：

![](img/a015e993a056970dc9c291a2d699b2e0.png)

我们排除第 5 个样本（因为 |X₅ − 3| = 0），并使用剩余的样本来计算平均秩：

![](img/0dab4e06fd86eec6e5f79244ee2386a1.png)

表 6：作者提供的图片

![](img/77f18ab626682d0c20958d49f5ae98cf.png)

# 工具 4: 两样本 Wilcoxon 符号秩检验

作为我们最后的非参数假设检验工具包，让我们看一下 Wilcoxon 符号秩检验的两样本扩展。如前所述，原假设认为中位数的差异为 0，而备择假设则建议两个样本之间存在差异，即中位数的差异为正、负或非零。数学上，原假设和备择假设如下：

![](img/ef702dac012ea64f002954ac0f17a3dd.png)

首先看一下总体框架，然后考虑一个示例。假设我们有两个样本的数据：X₁, X₂, · · ·, Xₙ₁ 和 Y₁, Y₂, · · ·, Yₙ₂。我们将两个样本组合成一个样本，并将所有观测值按升序排列。对于每个观测值，我们分配排名（如有争议则使用平均排名），从 1, 2, · · ·, n1 + n2 开始。最后，我们计算所有 X 样本观测值的排名总和，我们称之为测试统计量 W。例如，考虑以下配对数据：

![](img/369ba34c44549ad4c38c0da167c1e3fc.png)

假设如下：

![](img/ff0e0773c4372fb79616f8acb9bc88ce.png)

**步骤 1:** 组合 Xᵢ 和 Yᵢ：

24.27, 8.63, 16.76, 21.92, 29.59, 4.01, 7.28, −7.75, −6.61, 13.05, 13.47, 24.6, −4.97, 0.07, 6.96, −0.53, 7.26, −11.7, −5.01, −4.43

**步骤 2:** 将组合排列成升序（记得跟踪哪些观测值来自哪个样本）：

![](img/44f8ba660c689d962bd6620b9c2793f2.png)

表 7: 作者提供的图片

**步骤 3:** 分配排名：

![](img/ec2489951f660a98fb1b02787730f5d8.png)

表 8: 作者提供的图片

**步骤 4:** 通过对 X 样本的排名求和来计算测试统计量：

![](img/0715f185b6814f23386d3c95f258556d.png)

接下来，我们找到 W 在原假设下的分布。如前所述，W 的分布没有封闭公式，因此我们使用 Lyapunov CLT 来近似 W 的分布（CDF）：

![](img/ae0861a77c9553bb309ca28d49d15839.png)

其中 Φ 是标准正态分布的累积分布函数。我们可以通过计算 W 的期望和方差来检查这一点。请注意，这一计算相当复杂，因此您可以跳过它，直接进行 p 值计算。但我们仍包括这一部分以求完整。让 Rˣᵢ 表示分配给 X 的第 i 个样本的排名，而 Rʸⱼ 表示分配给 Y 的第 j 个样本的排名：

![](img/dfabb1c2ac2f622639f73d96329b1095.png)

在原假设下，由于分布相同，Rˣᵢ 和 Rʸⱼ 必须在 1 ≤ i ≤ n₁; 1 ≤ j ≤ n₂ 的范围内完全分布。因此，

![](img/7337e05c048d922e13f8e97ae05be341.png)

方差的计算略微复杂。我们利用以下事实：

![](img/6239363e48f08be0411db609063975d1.png)

因此，

![](img/1d2ab73f023515aa323446553ac02f49.png)

类似地，对于 i ̸= j：

![](img/5a1ac30dfd3e5cfcbada0a17abc6c043.png)

因此，根据方差和协方差的定义，我们有：

![](img/3f67eecb40e3eddc06099957956e5cab.png)![](img/fe5d37ef1c600689926f6c20ceee52cf.png)

因此，通过方差的扩展和求和：

![](img/2b35a6826f737a5b486242ac2ecdd1e9.png)

因此，如果我们标准化 W 并应用 CLT，我们得到：

![](img/130f11157c699f6171657e875d00d8d1.png)

因此，

![](img/0f00cecb1587c1b22ad732810e47ac1f.png)

因此，在 10% 的显著性水平下，由于 p 值 < 0.10，我们可以拒绝零假设！最后，让我们尝试计算相同数据的 p 值，如果备择假设是双侧的：

![](img/0298ea43f2349b54fb58a1fe5c9b3d20.png)

测试统计量仍然保持不变。然而，就像以前一样，由于支持备择假设的方向改变，p 值的计算相应地进行了调整（根据 p 值的定义）：

![](img/c6fe463fc4703b076fc0f2e9550e5693.png)

因此，在双侧情况下，我们无法在 10% 的显著性水平下拒绝零假设。这结束了我们对进行 Wilcoxon 符号秩检验进行单样本和双样本情况讨论。

# 结论

在本文中，我们熟悉了一些最知名的非参数或无分布假设检验框架。我们查看了符号检验和 Wilcoxon 符号秩检验在单样本和双样本情况下的表现，并比较了它们在样本观察中的表现。我们探讨了每个测试的单侧和双侧情况，以及如何将它们推广为测试数据的任何给定分位数。符号检验虽然一般不太强大，但非常简单，仅依赖于二项分布的累积分布函数来推导相关的 p 值。另一方面，Wilcoxon 符号秩检验在理论上更为复杂，但往往能够给出更好的结果，因为它不仅考虑值是否小于或大于阈值，还考虑它们绝对偏差的相对大小。

希望您喜欢阅读本文！如果您有任何疑问或建议，请在评论框中回复。

如果您喜欢我的文章并希望阅读更多，请随时通过 邮件 联系我。访问此 [链接](https://medium.com/@namanagr03)。

注：所有图片均由作者制作。
