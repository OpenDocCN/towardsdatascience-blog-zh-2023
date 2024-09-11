# 在因果推断中理解治疗加权的逆概率 (IPTW)

> 原文：[https://towardsdatascience.com/understanding-inverse-probability-of-treatment-weighting-iptw-in-causal-inference-4e69692bce7e?source=collection_archive---------1-----------------------#2023-01-11](https://towardsdatascience.com/understanding-inverse-probability-of-treatment-weighting-iptw-in-causal-inference-4e69692bce7e?source=collection_archive---------1-----------------------#2023-01-11)

## 对IPTW的直观解释及其与多元回归的比较

[](https://medium.com/@jonahbreslow?source=post_page-----4e69692bce7e--------------------------------)[![Jonah Breslow](../Images/c93e3a291f45fb134e084875bb2b4eb2.png)](https://medium.com/@jonahbreslow?source=post_page-----4e69692bce7e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----4e69692bce7e--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----4e69692bce7e--------------------------------) [Jonah Breslow](https://medium.com/@jonahbreslow?source=post_page-----4e69692bce7e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fef52614d34d8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-inverse-probability-of-treatment-weighting-iptw-in-causal-inference-4e69692bce7e&user=Jonah+Breslow&userId=ef52614d34d8&source=post_page-ef52614d34d8----4e69692bce7e---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----4e69692bce7e--------------------------------) ·14分钟阅读·2023年1月11日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F4e69692bce7e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-inverse-probability-of-treatment-weighting-iptw-in-causal-inference-4e69692bce7e&user=Jonah+Breslow&userId=ef52614d34d8&source=-----4e69692bce7e---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F4e69692bce7e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-inverse-probability-of-treatment-weighting-iptw-in-causal-inference-4e69692bce7e&source=-----4e69692bce7e---------------------bookmark_footer-----------)![](../Images/cd4cb39a8d0767fb1f100c8c05b193a8.png)

照片由 [Nadir sYzYgY](https://unsplash.com/@nadir_syzygy?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 介绍

在本帖中，我将提供逆概率治疗加权（IPTW）的直观和插图解释，IPTW是各种倾向评分（PS）方法之一。IPTW是[多元线性回归](https://simple.wikipedia.org/wiki/Linear_regression)在因果推断背景下的替代方法，因为两者都试图在混杂因素存在的情况下确定治疗对结果的影响。需要注意的是，目前的证据并不支持IPTW优于多元线性模型的说法（Glynn *et al.*, 2006）。然而，IPTW确实具有某些理论和实践上的好处，我们将在本帖中回顾这些内容。

在撰写时，查询“propensity scor*”在PubMed中已经识别出近45,000条引用（[PubMed查询](https://pubmed.ncbi.nlm.nih.gov/?term=propensity+scor*&filter=years.2000-2022)）。根据这个标准，2000年有45条引用，而2022年有8,929条引用，并且在此期间每年的引用数量都在增加（[PubMed查询](https://pubmed.ncbi.nlm.nih.gov/?term=propensity+scor*&filter=years.2000-2022)）。这种受欢迎程度的增加需要对方法论进行直接的解释。

# 治疗加权的逆概率：解释

## 随机对照试验

如果我们想确定治疗对某些可测量结果的影响，金标准方法是[**随机对照试验 (RCT)**](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6235704/)。在RCT中，治疗是随机分配给个体的。在样本量足够大的试验中，治疗在可能影响试验结果的所有测量和未测量变量中随机分配（Hariton *et al.*, 2018）。这些变量在本帖的其余部分将被称为*协变量*。这种设置使研究人员能够最接近地估计治疗对结果的因果影响。值得注意的是，即使RCT本身也不太可能证明因果关系，但它们确实提供了最强的证据。

## 简单的观察性示例

让我们首先设置一个简单的示例，其中包含接受过治疗的受试者、他们的性别和他们的结果。我们的目标是确定治疗对结果的影响。在这个玩具示例中，我们假设数据包含8名参与者，4名男性和4名女性。此外，治疗给予了4名男性中的2人以及4名女性中的2人，如图1所示。

![](../Images/492d3113ff28f0199703006c9b59eea6.png)

图1：简单示例

在这种情况下，知道受试者的性别对受试者是否接受治疗没有任何信息提供。接受治疗的整体概率是 50%。如果受试者是男性，接受治疗的概率是 50%。如果受试者是女性，接受治疗的概率也是 50%。换句话说，性别与治疗之间没有相关性。图 2 显示了[有向无环图 (DAG)](https://en.wikipedia.org/wiki/Directed_acyclic_graph)，该图显示了所谓的因果方向。

![](../Images/f6466912ec55231f67295f7c91384a03.png)

图 2：因果 DAG

这个 DAG 可以解释为显示治疗和性别对患者结果的影响。然而，由于性别不会影响治疗是否被施用，因此性别与治疗之间没有箭头。这正是 RCT（随机对照试验）在大样本中本质上保证的。事实上，这种保证适用于所有可能的协变量，即使是那些未被测量的协变量。

## 一个带有混杂因素的观察示例

现在我们将修改示例，展示如果性别突然与治疗的施用相关会发生什么。图 3 显示女性接受治疗的概率为 75%，而男性只有 25% 的概率接受治疗。

![](../Images/ca9d52640c6a908b04d5fbf1b30a0457.png)

图 3：一个现实的示例

接受治疗的整体概率仍然是 50%。但是，现在知道受试者的性别会提供关于受试者是否接受治疗的额外信息。性别和治疗不再是独立的，因为接受治疗的概率（50%）不等于受试者是女性（75%）或男性（25%）时接受治疗的概率。这被称为[选择偏差](https://en.wikipedia.org/wiki/Selection_bias)，因为未能实现性别上的随机化。现在，性别成为一个[混杂因素](https://en.wikipedia.org/wiki/Confounding)，这意味着它影响独立变量（治疗）和依赖变量（结果），这会妨碍我们直接测量治疗对结果的影响。图 4 显示了更新后的 DAG，其中有一条从性别到治疗的箭头。这条箭头表示了本节中描述的选择偏差。换句话说，受试者的性别影响受试者是否接受治疗，从而创建了统计混杂。

![](../Images/d53b89766d6cfbea2c56b1211dd77fa4.png)

图 4：带有混杂因素的 DAG

## 处理混杂因素的方法：多元线性回归

这时我们开始考虑用于应对混杂的工具，包括多变量线性回归。我不会在这里深入探讨线性回归的细节，因为它可能是 IPTW 的前提。如果你不熟悉线性回归并希望深入了解，我强烈推荐 Richard McElreath 的 [统计重思讲座和教科书](https://xcelab.net/rm/statistical-rethinking/)。若需更快的解释，StatQuest 在 YouTube 上有一个很好的 [线性回归解释视频](https://www.youtube.com/watch?v=7ArmBVF2dCs)。

简而言之，如果我们创建一个线性回归模型，形式为，

![](../Images/480772e662d901d68b60e070993117af.png)

我们将闭合性别 → 治疗 → 结果的路径。这就是我们所说的“控制”性别。这将使我们得到一个无偏的治疗效果估计，因为我们有效地去除了性别混杂因素带来的选择偏差。

## 关于混杂因素该怎么办：IPTW

IPTW 是一种去除混杂因素影响的替代统计方法。IPTW 的高级思路是创建个体观察的副本，以便在副本创建之后，混杂因素与感兴趣的治疗之间不再存在关系。其效果是将数据转化为*大致*随机。我们计算每个观察的副本数量的方法将是本节的其余部分讨论的主题。

我将开始用文字解释如何计算副本数量，这些将从现在开始被称为权重。如果对程序的解释不清楚，图5 将提供权重方案的直观视觉解释。

计算这种加权的机制如下：

1.  对于每个观察 *i*，找到其进入治疗组的概率 *p*（Chesnaye *et al.,* 2022 para 9）。这就是逆概率治疗加权中的“治疗概率”来源。

1.  计算个体观察的权重 *w* 为 1/*p*。这就是逆概率治疗加权中“逆”一词的来源（Chesnaye *et al.,* 2022 para 9）。

1.  使用这些权重创建“副本”。

我们将计算在我们例子中接受治疗的女性的权重。首先，我们需要找到治疗组中每个女性接受治疗的概率。由于4名女性中有3名接受了治疗，因此我们知道这个概率是75%。然后，我们通过倒数这个概率来计算这三名女性的权重。因此，1/0.75 等于 1.333。最后，我们使用这个权重创建“副本”。因为我们有3名女性，所以3 x 1.333 = 4。换句话说，我们将最终得到4名女性。有关此过程的清晰视觉解释，请参见图5。

![](../Images/0ce4f25a6fa8c9793f83ad94da2ec032.png)

图5：计算 IPTW 权重

这个过程使得某些观察的相对重要性增加。其效果是既增加了样本量，又平衡了协变量。我们称之为**伪人口**，因为我们实际上通过使用这种加权方案来向样本中添加个体。图6展示了使用这些权重对伪人口的影响。

![](../Images/c0b4a72ea240125b30d5e73c13797324.png)

图6：协变量平衡的伪人口

使用这些权重的效果是通过以一种使处理不再依赖于混杂因素的方式结构化伪人口，从而**控制**混杂变量。在这个伪人口中，知道一个受试者的性别不再提供关于受试者是否接受了处理的任何信息。这就是我们所说的平衡协变量。

现在，如果我们重新绘制如图7所示的因果DAG，我们将移除性别 → 处理的箭头。性别影响结果，但不再影响处理。因此，我们已经去除了混杂因素。

![](../Images/f6466912ec55231f67295f7c91384a03.png)

图7：无混杂因素的DAG

## 休息和退出通道

如果你已经看到了这里，做得很好。这是一个很好的停止点。你现在已经有了理解IPTW的坚实概念基础。接下来的两个部分将基于这个基础，介绍IPTW中的两个稍微复杂一些的主题。它们将包括：

1.  稳定化的IPTW，以及

1.  计算倾向评分

如果你想跳过这些部分，可以随意。不过，阅读*将IPTW与传统多变量模型进行比较*和*结论*部分可能是值得的。

## 稳定化的IPTW

在IPTW示例中，回顾一下我们如何将有效样本量从N=8增加到N=16。这个内容将在图8中总结。

![](../Images/1b5d7c552e6fa84e621e9c6de76826b0.png)

图8：增加样本量

尽管我们不再有不平衡的协变量，但我们引入了新的困境。随着样本量的增加，统计测试更可能发现效果。这是由于[中心极限定理](https://en.wikipedia.org/wiki/Central_limit_theorem)的性质。较大的样本意味着我们对样本应用的统计测试具有更大的[统计功效](https://en.wikipedia.org/wiki/Power_of_a_test)。通过人为地将样本量加倍，我们实际上增加了发现我们的处理对结果有影响的概率（Xu *et al.*, 2010）。这是一种称为重复抽样的现象。

为了说明重复抽样为何成问题，考虑两个理论上公平的硬币分别被掷4次和8次。在这个例子中，每个硬币在每次掷出时都产生了正面。第一个硬币产生4个正面的概率是6.25%，而第二个硬币产生8个正面的概率是0.39%。

![](../Images/b1237299602befbcbd2d2b7a15323d6d.png)

我们为两个不同硬币计算的概率类似于 [p 值](https://en.wikipedia.org/wiki/P-value)。它们表示在硬币公平的情况下事件发生的概率。现在我们想测试在观察到的数据存在的情况下公平硬币的断言是否成立。我们将使用 [假设检验](https://en.wikipedia.org/wiki/Statistical_hypothesis_testing)，其中 [零假设](https://en.wikipedia.org/wiki/Null_hypothesis) 是硬币是公平的，而 [备择假设](https://en.wikipedia.org/wiki/Alternative_hypothesis) 是硬币是有偏的。

考虑产生 4 次正面的硬币。如果我们假设零假设为真，这个事件的概率（p 值）为 6.25%。这个 p 值*通常*不会提供令人信服的证据证明硬币有偏。通常，我们会要求 p 值小于 5%。现在，假设我们通过将每次投掷的权重值设为 2（这与 IPTW 的工作方式直接类似）来人工将样本量翻倍。我们知道 8 次硬币投掷都出现正面的概率为 0.39%，这足以声称硬币有偏并拒绝零假设。然而，我们获得的数据仅包含 4 次硬币投掷的信息。因此，我们夸大了我们拒绝零假设（硬币公平）的概率，以支持备择假设（硬币有偏），这也被称为 [第一类错误](https://en.wikipedia.org/wiki/Type_I_and_type_II_errors#Type_I_error)。这正是我们在使用 IPTW 时遇到的问题。

为了修正这种人工增加的样本量，我们将引入**稳定化 IPTW**。简单来说，计算权重时

![](../Images/ee32ce95c9692f12fe42b69e0aaf1933.png)

我们将计算权重如下

![](../Images/02c8ba8f9b73829d04e02263741b8ead.png)

图 9 将展示我们如何计算稳定化加权方案中的分子。

![](../Images/3ab4e98a75de2e271564edc3d31d605c.png)

图 9：稳定化 IPTW 分子

图 10 将展示我们如何更新加权方案，以便不将伪人群的大小增加到比原始数据中的实际人群大得多。

![](../Images/60feb519704be0e15d71634f9cdd1682.png)

图 10：计算稳定化 IPTW 权重

使用这种稳定化加权方案的效果是，伪人群不再比原始人群大那么多，如图 11 所示。

![](../Images/ecae40c5ecd7fec8dd634a469fcba64c.png)

图 11：稳定化协变量平衡伪人群

由于我们不再增加伪人群的大小，相较于原始人群，第一类错误（假阳性）的概率不会被夸大。

## 计算倾向评分

计算受试者接受治疗的概率，也称为**倾向性**，往往不像前面的例子中那样简单。为了说明这一点，让我们在例子中添加一个额外的协变量——年龄，看看结果如何。

![](../Images/536e718f80a9ca336c9de2b91bd471e8.png)

图12：包含两个混淆因素的DAG

快速检查这个因果DAG，我们注意到性别仍然混淆了治疗对结果的影响，就像我们在前面的例子中看到的那样。此外，年龄作为混淆因素被添加进来。不幸的是，由于年龄是一个连续变量，我们不能像之前那样绘制治疗概率图。实际上，我们需要一种全新的方法来计算倾向性。

这就是我们将利用[**逻辑回归**](https://en.wikipedia.org/wiki/Logistic_regression)的地方。在这篇文章中，我不会深入探讨逻辑回归的工作原理。如果你对逻辑回归不熟悉，我建议观看[关于逻辑回归的StatQuest视频](https://www.youtube.com/watch?v=yIYKR4sgzI8)以获得一个非常易于理解的概述。关键点是，我们可以使用逻辑回归来计算在给定协变量（性别和年龄）的情况下接受治疗的倾向分数。

一旦我们使用逻辑回归计算倾向分数并重新加权数据，检查加权协变量的分布以确保它们平衡是至关重要的。由于使用逻辑回归估计倾向分数引入了额外的复杂性，我们需要检查拟合优度（Borah *et al.,* 2013）。这将简单地涉及检查接受治疗和未接受治疗者的年龄和性别分布是否大致相似。

## 比较IPTW与传统的多元模型

正如介绍中提到的，因果推断的金标准是RCT。然而，在现实世界中，构建一个完整的RCT并不总是可行。因此，我们只能使用接近RCT的统计技术，包括多元线性回归或倾向性评分模型，如IPTW。IPTW的优点在于它试图在观察到的协变量之间创建平衡，而这正是RCT所保证的。相反，多元线性回归并不试图平衡协变量。然而，“没有证据表明，相较于多元模型中的常规估计，利用倾向评分的分析将显著减少混杂带来的偏差”（Glynn *et al.,* 2006 para 31）。

尽管IPTW在理论上相对于线性回归具有一些优势，但“在实际使用中，倾向评分和回归模型估计之间的答案几乎没有显著差异”（Glynn *et al.,* 2006 para 8）。

自然地，为什么研究人员会选择使用IPTW而不是线性回归的问题就出现了。我将在下面简要回顾一些这些原因。

> 1\. PS方法允许研究人员使用有原则的方法来修剪研究人群。

![](../Images/362f69845fb594264ff6301d1a44b533.png)

图 13：暴露倾向评分 — **来源**: Glynn *等人, 2006*。

在图 13 中，虚线曲线表示未接受治疗的个体的倾向评分分布。实线曲线表示接受治疗的个体的倾向评分分布。在未治疗分布的左尾和已治疗分布的右尾，注意是否有个体从未接受治疗或总是接受治疗。将这些个体从研究人群中剔除带来了理论上的好处，因为这些观察结果“可能在多变量分析中产生不当影响和问题，因为[治疗组]与[未治疗组]之间的协变量重叠极少”（Glynn *等人,* 2006 第16段）。

> 2\. PS方法可以阐明治疗如何与接受治疗的倾向相互作用。

通过按倾向评分分层学科，可以识别治疗的有效性是否因每个受试者所在的倾向评分层次而有所不同。

> 3\. PS校准可以提高主研究的稳健性。

考虑一个例子，我们有两个研究：一个主研究和一个验证研究。两个研究都旨在评估相同治疗对结果的影响。主研究的样本量非常大，而验证研究较小。在主研究中，有一些预测变量由于未被测量而被遗漏。因此，主研究的倾向评分将受到[遗漏变量偏差](https://en.wikipedia.org/wiki/Omitted-variable_bias)的影响。然而，如果验证研究包含了更多详细的预测变量来修正主研究的遗漏变量偏差，那么验证研究“可能会提供更可靠的倾向评分估计”（Glynn *等人,* 2006 第20段）。然后可以使用验证研究来校准主研究中的倾向评分，从而减少偏差（Stürmer *等人,* 2005*）。

## **结论**

总结一下，我们回顾了IPTW的机制。IPTW的主要目标是确保协变量在治疗组之间平衡，从而尽可能减少由测量的协变量引起的混杂。此外，我们回顾了PS方法中的两个更复杂的话题，包括稳定权重和计算倾向评分。最后，我们简要讨论了使用IPTW而不是多变量线性回归的一些理论好处。本文的主要目的是让读者了解IPTW的工作原理，因为在过去20年里，作为一种统计方法，它的使用显著增加。我希望这个概述对你有所帮助！

*除非另有说明，所有图片均由作者提供。*

## 参考文献

[1] B. Borah, J. Moriarty, W. Crown 和 J Doshi，[倾向评分方法在观察性比较效果和安全性研究中的应用：我们已经取得了什么进展，还应该去向何处？](https://www.futuremedicine.com/doi/10.2217/cer.13.89) (2013)，《比较效果研究杂志》

[2] E. Hariton 和 J. Locascio，[随机对照试验——效果研究的金标准](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6235704/) (2018)，《英国妇产科杂志》

[3] N. Chesnaye, V. Stel, G. Tripepi, F. Dekker, E. Fu, C. Zoccali, K. Jager, [观察研究中治疗权重的逆概率介绍](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8757413/) (2022)，《临床肾脏杂志》

[4] R. Glynn, S. Schneeweiss 和 T. Stürmer，[倾向评分的指示及其在药物流行病学中的使用回顾](https://onlinelibrary.wiley.com/doi/full/10.1111/j.1742-7843.2006.pto_293.x) (2006)，《基础与临床药理学与毒理学》

[5] S. Xu, C. Ross, M. Raebel, S. Shetterly, C. Blanchette 和 D. Smith，[使用稳定逆倾向评分作为权重直接估计相对风险及其置信区间](https://www.sciencedirect.com/science/article/pii/S1098301510603725) (2010)，《健康价值》

[6] T. Stürmer, S. Schneeweiss, J. Avorn 和 Robert J Glynn，[使用倾向评分校准的验证数据调整未测量混杂因素的效果估计](https://pubmed.ncbi.nlm.nih.gov/15987725/) (2005)，《美国流行病学杂志》
