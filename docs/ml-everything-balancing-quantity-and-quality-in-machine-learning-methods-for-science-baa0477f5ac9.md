# “ML-Everything”？在科学中的机器学习方法中平衡数量与质量

> 原文：[`towardsdatascience.com/ml-everything-balancing-quantity-and-quality-in-machine-learning-methods-for-science-baa0477f5ac9`](https://towardsdatascience.com/ml-everything-balancing-quantity-and-quality-in-machine-learning-methods-for-science-baa0477f5ac9)

## 需要进行适当的验证和优质的数据集，这些数据集要客观、平衡，并且预测在实际场景中要有用。

[](https://lucianosphere.medium.com/?source=post_page-----baa0477f5ac9--------------------------------)![LucianoSphere (Luciano Abriata, PhD)](https://lucianosphere.medium.com/?source=post_page-----baa0477f5ac9--------------------------------)[](https://towardsdatascience.com/?source=post_page-----baa0477f5ac9--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----baa0477f5ac9--------------------------------) [LucianoSphere (Luciano Abriata, PhD)](https://lucianosphere.medium.com/?source=post_page-----baa0477f5ac9--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----baa0477f5ac9--------------------------------) ·12 min read·2023 年 3 月 14 日

--

![](img/61425b6c60082fe85bbe42e083edbd99.png)

[Tingey Injury Law Firm](https://unsplash.com/@tingeyinjurylawfirm?utm_source=medium&utm_medium=referral) 提供的照片，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 引言

近期的机器学习（ML）研究在各个领域取得了显著进展，包括科学应用。然而，为了确保新模型的有效性、测试和验证程序的质量，以及所开发模型对实际问题的适用性，仍然存在一些需要解决的限制。这些限制包括不公平、主观和不平衡的评估，这些评估可能不是故意的，但却存在；使用无法准确反映真实世界使用情况的数据集（例如“过于简单”的数据集）；将数据集分割成训练、测试和验证子集的错误方法等。在本文中，我将讨论所有这些问题，并使用生物学领域的例子来说明，生物学领域正被机器学习方法所变革。

在此过程中，我还将简要讨论机器学习模型的可解释性，尽管目前非常有限，但它非常重要，因为它可以帮助澄清文章第一部分讨论的需要解决的限制的许多方面。

此外，我将强调，虽然某些机器学习模型可能被过度宣传，但这并不意味着它们没有用处，或没有产生能够推动机器学习某些子领域发展的新知识。

## 每个月都有许多新的机器学习模型出现，这只是我工作领域中的一部分！

随着针对科学应用的机器学习（ML）论文数量的激增，我开始质疑… 这些工作是否如其所宣称的那样具有革命性和实用性？当然，AlphaFold 2 是革命性的，还有类似理论框架所启发的后续 ML 工具，例如 ProteinMPNN。但更广泛地说，领域内发生了什么？科学家们如何能同时生产出如此多的 ML 工具，并且它们都被称为“最佳”？这些研究的质量是否足够高？假设这些工作是新颖且优秀的，它们的评估是否始终公平、客观和平衡？实际应用是否如宣传的那样具有革命性？

每次看到一种新的结构生物学 ML 方法时，我都会思考如何评估其优点，特别是其实际性能和对我研究的实际效用。

> 研究人员越来越多地将神经网络的最新发展应用于各种领域的长期存在的问题，从而取得了显著进展。然而，确保评估公平、客观和均衡，并确保数据集和预测能力准确反映 ML 模型在现实世界中的适用性是至关重要的。

社交网络帖子、预印本服务器和同行评审的论文显示了现代神经网络模块和概念（如 transformers、扩散模型等）在解决科学长期存在的问题上的迅猛应用。这些研究本身就是一种积极的现象，因为这种方法在各个领域取得了显著进展。以两个主要的例子为例，AlphaFold 2 在 CASP14 中获胜所带来的蛋白质结构预测进展，以及 D. Baker 的 ProteinMPNN 在蛋白质设计方面的应用，其预测的蛋白质序列在实验工作中得到了广泛测试，证实了该方法的有效性。如果你想了解更多关于这些方法的信息，可以查看我的博客文章：

[## 一年多的 AlphaFold 2 免费使用及其在生物学中引发的革命](https://medium.com/advances-in-biological-science/over-one-year-of-alphafold-2-free-for-everyone-to-use-and-of-the-revolution-it-triggered-in-biology-f12cac8c88c6?source=post_page-----baa0477f5ac9--------------------------------)

### 对蛋白质结构的自信建模、预测其与其他生物分子的相互作用，甚至蛋白质...

[## 这里是我关于蛋白质建模、CASP 和 AlphaFold 2 的所有同行评审和博客文章](https://medium.com/advances-in-biological-science/over-one-year-of-alphafold-2-free-for-everyone-to-use-and-of-the-revolution-it-triggered-in-biology-f12cac8c88c6?source=post_page-----baa0477f5ac9--------------------------------)

### 我在这里汇总了所有经过同行评审的文章（一些论文、几篇综述、一篇观点）以及关于...

[我的同行评审和博客文章汇总](https://lucianosphere.medium.com/here-are-all-my-peer-reviewed-and-blog-articles-on-protein-modeling-casp-and-alphafold-2-d78f0a9feb61?source=post_page-----baa0477f5ac9--------------------------------)

在许多情况下，新方法至少在某种程度上被过度宣传。例如——我认为我的怀疑在这里最为明显——蛋白质设计的情况。大多数甚至所有近期展示现代机器学习模型用于蛋白质设计的工作，都将恢复序列相对于输入蛋白质结构的序列身份作为成功指标。起初这可能有意义，但深入思考并了解蛋白质序列和结构后，显然一个好的序列匹配并不一定意味着（完全不）蛋白质会按预期折叠。例如，即使是单个突变体蛋白质也非常常见，可能会折叠失败并从溶液中崩溃，尽管它们与野生型蛋白质几乎相同，并且在“序列身份”指标方面基本完美。此外，单个突变体有时可能引发折叠交换，导致几乎完美的序列身份但没有结构的保留。相反，发现折叠方式几乎相同的蛋白质虽然序列完全不同，因此序列身份非常低，也并不少见。总之，序列恢复作为实际成功的指标*非常有限*。

> 序列恢复是合理的，但作为实际蛋白质设计成功的指标却*非常有限*。

目前，在蛋白质设计中，唯一真正有意义的测试是亲自进入湿实验室，生产设计的蛋白质，并通过实验确定其结构，查看它是否与输入结构匹配——至少要合理匹配，当然我们不能期望完全匹配。虽然 ProteinMPNN 论文和一些其他工作进行了这种方法的实验验证，但我看到的大多数预印本和论文完全忽视了这一点，专注于序列恢复和类似指标。而且，不，发现 AlphaFold 可以回溯预测输入的结构也不是设计有效的安全指标！充其量，它只能用于识别不良序列。

如果你想了解更多关于我刚刚提到的 ProteinMPNN：

[## 新深度学习工具高精度设计新型蛋白质](https://towardsdatascience.com/new-deep-learned-tool-designs-novel-proteins-with-high-accuracy-41ae2a7d23d8?source=post_page-----baa0477f5ac9--------------------------------)

### 这款来自 Baker 实验室的新软件设计的蛋白质在湿实验室中实际上有效。你可以用它来……

[新深度学习工具高精度设计新型蛋白质](https://towardsdatascience.com/new-deep-learned-tool-designs-novel-proteins-with-high-accuracy-41ae2a7d23d8?source=post_page-----baa0477f5ac9--------------------------------)

# 数据集和评估的潜在问题

一个重要的问题，我在这里一般性地描述，因为我不想对任何特定工作进行负面评价，是许多研究在数据集上评估其模型，而这些数据集并不适合任务。我看到的两个主要问题是数据集未能反映 ML 模型的实际应用，以及数据集包含与训练数据重叠的条目。

我并不是说有不良意图。训练 ML 模型需要非常大的数据集，这些数据集大到无法手动整理，自动整理管道总是有局限性。另一方面，预印本和论文往往只展示那些说明 ML 模型在某些生物学问题中的积极应用的案例，隐藏或忽视那些缺乏生物学意义、难以解释的案例，或者——这纯粹是不良的科学实践——那些与已有知识不符的案例。

这些问题并非特定于某一领域，而是科学领域普遍存在的显著问题的特定表现：只有积极结果被认为有贡献，因此负面和不正确的结果在文献中并不常见，尽管它们和积极结果一样重要，特别是在避免资源和时间的徒劳浪费方面。发布或死亡的文化推动了主要是积极结果的发表，这些结果往往被装饰以声称过度的新颖性和优越性。要了解更多关于我对科学问题尤其是出版问题的看法，请参见此：

[](https://medium.com/predict/how-could-scientific-publishing-evolve-25977238dbdb?source=post_page-----baa0477f5ac9--------------------------------) [## 科学出版如何发展？

### 总结和反思由期刊 eLife 的新标题引发。

[medium.com](https://medium.com/predict/how-could-scientific-publishing-evolve-25977238dbdb?source=post_page-----baa0477f5ac9--------------------------------)

根据我上述的观点，我认为像[CASP](https://lucianosphere.medium.com/i-just-answered-a-few-questions-on-alphafold-and-casp-again-547fd628ec29)（或 CAMEO，或 CAPRI 结构预测等）这样的竞赛以及致力于客观基准测试现有方法的研究，比绝大多数报告新模型的论文更能推动该领域的发展。实际上，我如此坚信，以至于对我来说，在像 CASP 这样的竞赛或独立基准测试研究中排名靠前，就排除了任何描述（可能的）新方法的结果不佳的论文（尽管这并不一定意味着它可能涉及某些潜在的未来兴趣的新方法）。

> 像 CASP（或 CAMEO，或 CAPRI 结构预测等）这样的竞赛以及致力于客观基准测试现有方法的研究，比绝大多数报告新模型的论文更能推进这一领域。

关于评价的一点需要注意，特别是针对 AlphaFold 2 之后的蛋白质三级结构预测，是现在只有小幅度改进的可能性，这使得不同新方法的比较变得复杂——这个问题 AlphaFold 2 当然没有面临，因为当时的标准还不高。在其他领域中，这不是一个大问题，例如在药物设计、对接、构象动力学预测等领域，预测仍然较差到中等水平，而蛋白质三级结构预测已算是基本解决了：

[](/whats-up-after-alphafold-on-ml-for-structural-biology-7bb9758925b8?source=post_page-----baa0477f5ac9--------------------------------) ## AlphaFold 之后的蛋白质结构预测：机器学习的新进展

### AI 驱动的生物学革命会继续吗？我们能期待新的突破吗？现在发生了什么…

towardsdatascience.com

# 这些方法被过分炒作并不意味着它们没有用处或无法推动领域的进步！

当科学家们探索解决问题的替代方法时——如这里所关注的机器学习——他们可能会提出看似革命性的解决方案，并具有良好的前景，但最终却未能达到预期。这并不一定意味着这些新发展对于某些应用没有用处，或没有产生对未来工作有用的新知识，也有助于推动某些领域的进步。

一个很好的例子是语言模型在结构生物学预测中的应用。最早报告的这类方法之一显示的性能远低于 AlphaFold 2，但执行时间却快了几个数量级，这可能在某些应用中带来一些好处：

[](/protein-structure-prediction-a-million-times-faster-than-alphafold-2-using-a-protein-language-model-d71c55e6a4b7?source=post_page-----baa0477f5ac9--------------------------------) ## 使用蛋白质语言模型的蛋白质结构预测：比 AlphaFold 2 快一百万倍

### 方法及其相关性的总结；尝试在 Google Colab 中运行；并测试其是否真的有效…

towardsdatascience.com

可能是这些基于语言的蛋白质结构预测模型中最受欢迎（对我来说也最有用）的 Meta 的 ESMFold 很快就出现了。Meta 推出的这个新神经网络及其与预计算的大型蛋白质结构模型数据库一起发布，掀起了一阵短暂的炒作，我认为这本身也是对 Deepmind 的 AlphaFold 2 在该领域引发的革命的一个较小但实质性的贡献：

[](/how-huge-protein-language-models-could-disrupt-structural-biology-6b98193f880b?source=post_page-----baa0477f5ac9--------------------------------) ## 巨型蛋白质语言模型如何颠覆结构生物学

### 具有与 AlphaFold 相似的准确度但速度提高至 60 倍 — 并且开发了新的 AI 方法…

towardsdatascience.com

然而，尽管 ESMFold 非常快速，并且在结构预测上表现远超之前的蛋白质语言模型，它的预测仍略逊于 AlphaFold 2（此外，ESMFold 在例如自定义模板的使用或蛋白质复合物结构预测方面存在局限）。你可以亲自查看 ESMFold 在今天标准下相对较差的表现，具体可以参考在 CASP 第十五届评估中进行的官方评价：

[](https://predictioncenter.org/casp15/results.cgi?tr_type=regular&source=post_page-----baa0477f5ac9--------------------------------) [## 结果 — CASP15

### 由美国国家普通医学科学研究所（NIH/NIGMS）资助的蛋白质结构预测中心…

predictioncenter.org](https://predictioncenter.org/casp15/results.cgi?tr_type=regular&source=post_page-----baa0477f5ac9--------------------------------)

但请不要误解我的意思。Meta 的模型非常有用且具有巨大潜力，在实际应用中铺平了使用其蛋白质语言模型的新工具的开发道路。我认为这个模型从结构预测的内部网络机制角度尤其有趣，这可能在未来进一步发展。例如，贝克实验室和 Meta 的联合工作表明，尽管语言模型仅在序列上进行训练，但其学习深度足以设计出超越自然蛋白质的结构，甚至包括在已知蛋白质中未观察到的结构动机（甚至经过实验验证！）：

[](https://www.biorxiv.org/content/10.1101/2022.12.21.521521v1?source=post_page-----baa0477f5ac9--------------------------------) [## 语言模型超越自然蛋白质的推广

### 从进化中的序列学习蛋白质设计模式，可能在生成蛋白质方面具有前景…

www.biorxiv.org](https://www.biorxiv.org/content/10.1101/2022.12.21.521521v1?source=post_page-----baa0477f5ac9--------------------------------)

# 可解释性及其如何帮助赢得对 ML 模型的信任，缓解其一些局限

使用机器学习时的一个限制，不仅在结构生物学中，我认为在大多数科学或工程应用中也是如此，就是缺乏可解释性，或至少是明确的可解释性。换句话说，机器学习模型在很大程度上作为黑箱工作，因此在其被设计执行的任务或预测中是实用的，但对于它们如何以及为何表现如此好（或不好，视情况而定）却没有提供太多（如果有的话）洞见。

理想情况下，即便是完美的模型，假设其具有很高的准确性和可靠性，理解模型在何时何地表现良好或不佳仍然是有价值的。此外，理想的情况是这些解释能够根植于领域的基础科学中，比如说从潜在的物理学或化学的角度，以某种明确的方式连接独立变量，就像常规建模使用我们可以理解的方程一样。

特别是在结构生物学中，我们面临的问题是，蛋白质结构预测的机器学习模型表现非常好，但对于它们如何实现如此好的预测却知之甚少。因此，目前尚不清楚它们是否学到了我们不了解的蛋白质结构知识，或者（我认为更可能的是）它们可能学到了我们了解但难以量化，从而难以以“分析”方式应用于结构预测的知识。此外，我们甚至不知道这些结构预测的机器学习方法是否仅仅是对折叠状态的优秀预测者，还是也能预测折叠路径中的中间体、替代构象状态、内在无序区域的结构倾向等。我猜它们不能，或者至少信心不高，因为按设计，它们偏向于预测良好折叠状态的结构。一个关于它们如何实现对这些状态如此好预测的明确解释，以及系统中流动的信息类型，或许可以证明我的猜测是错的，并且确实能帮助方法开发者理解限制并尝试用改进的模型克服这些限制。

机器学习模型缺乏可解释性可能会在多个方面造成问题。首先，当模型表现不如预期时，诊断和纠正错误可能会很困难。这在处理牵强的外推时尤为重要，例如预测一个蛋白质的结构，而这个蛋白质的结构实际上与所有已知结构差异很大。如果不了解模型如何得出预测结果，就很难知道如何修复它，并评估每个预测的可靠性——尽管现代机器学习工具正越来越多地纳入预测可靠性的指标。

其次，缺乏可解释性限制了我们对解释系统行为的基础物理、化学或生物学的洞察能力，即使我们能够正确预测这种行为。对实际应用来说，这不是大问题，但对于科学寻求的基本理解而言，显然是不完整的。

最后，缺乏可解释性可能限制我们对机器学习模型建立信任的能力。如果我们无法理解一个模型是如何得出预测的，我们可能会不愿在准确性至关重要的情况下使用它。特别是在结构生物学中，不准确的模型可能导致对生物分子功能的错误结论，并阻碍所有相关研究和发展的进展。

本文中可解释性的要点是，更具可解释性的机器学习模型可以缓解与其构建、训练和应用于实际问题相关的许多问题；甚至可能在实际应用之前识别潜在问题，从而在数量与质量之间的平衡中提高质量。

> 更具可解释性的机器学习模型可以缓解与其构建、训练和应用于实际问题相关的许多问题，从而在数量与质量的平衡中提高质量。

有人在研究机器学习模型的可解释性问题，包括一些专门在科学应用背景下工作的人员。我将很快在这里写一篇关于此的博客文章。

# 相关资料

在整理我的文章时，我阅读了其他博主和科学家的精彩帖子，其中这些非常推荐——尽管我们并不总是达成一致：

[## 对神秘宇宙的一些思考

### 由**穆罕默德·阿尔库拉伊希**编写

[moalquraishi.wordpress.com](https://moalquraishi.wordpress.com/?source=post_page-----baa0477f5ac9--------------------------------) [https://www.blopig.com/blog/author/carlos/?source=post_page-----baa0477f5ac9--------------------------------] [## 牛津蛋白质信息学组

### Jamboree（1）大型集会，如政治党派或体育联盟的团队，通常包括一系列程序……

[www.blopig.com](https://www.blopig.com/blog/author/carlos/?source=post_page-----baa0477f5ac9--------------------------------) [## 科学的艰难之路

### 是否曾感觉到，作为一名科学家，主要工作就是在大量值得怀疑的论文中摸索……

[jgreener64.github.io](http://jgreener64.github.io/posts/science_the_hard_way/?source=post_page-----baa0477f5ac9--------------------------------)

# 喜欢这篇文章？那就看看我的其他文章吧

科学家们正在接近首个接近原子级的全细胞模拟

### 在 AI 对生物学影响之后，这可能代表另一个里程碑，这次根植于纯物理学

***towardsdatascience.com*** [](https://medium.com/predict/8-modern-and-upcoming-technologies-that-you-must-know-about-1ecf1a09aea7?source=post_page-----baa0477f5ac9--------------------------------) [## 8 种你必须了解的现代和即将出现的技术

### 从最现代的计算机技术和硬核物理学到生物技术的前沿

[***medium.com***](https://medium.com/predict/8-modern-and-upcoming-technologies-that-you-must-know-about-1ecf1a09aea7?source=post_page-----baa0477f5ac9--------------------------------) [## 结构生物学和生物信息学的关键网站和程序

### 来自硕士和博士课程的实际笔记

[***lucianosphere.medium.com***](https://lucianosphere.medium.com/key-websites-and-programs-for-structural-biology-and-bioinformatics-d42c8506082e?source=post_page-----baa0477f5ac9--------------------------------) [](https://pub.towardsai.net/building-customized-chatbots-for-the-web-using-gpt-3-5-turbo-190424827493?source=post_page-----baa0477f5ac9--------------------------------) [## 使用 gpt-3.5-turbo 构建定制化聊天机器人

### 摘要、源代码已准备好使用，还有一个示例聊天机器人可以立即体验

[***pub.towardsai.net***](https://pub.towardsai.net/building-customized-chatbots-for-the-web-using-gpt-3-5-turbo-190424827493?source=post_page-----baa0477f5ac9--------------------------------)

[***www.lucianoabriata.com***](https://www.lucianoabriata.com/) *我写作和拍摄关于我广泛兴趣领域的所有内容：自然、科学、技术、编程等。* [***成为 Medium 会员***](https://lucianosphere.medium.com/membership) *以访问所有故事（我通过该平台的附属链接获得少量收入，对你没有费用）并且* [***订阅以获取我的新故事***](https://lucianosphere.medium.com/subscribe) ***通过电子邮件****。要* ***咨询小型工作***，*请查看我的* [***服务页面在这里***](https://lucianoabriata.altervista.org/services/index.html)*。你可以* [***在这里联系我***](https://lucianoabriata.altervista.org/office/contact.html)***。***
