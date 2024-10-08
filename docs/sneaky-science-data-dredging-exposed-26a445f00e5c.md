# 狡猾的科学：数据开采曝光

> 原文：[`towardsdatascience.com/sneaky-science-data-dredging-exposed-26a445f00e5c`](https://towardsdatascience.com/sneaky-science-data-dredging-exposed-26a445f00e5c)

![](img/9e84b1147db69bce68371669f5efa5df.png)

从比萨饼到研究的黑暗面。图片由作者使用 Dall·E 3 创建。

## 探讨 p-hacking 的动机和后果

[](https://hennie-de-harder.medium.com/?source=post_page-----26a445f00e5c--------------------------------)![Hennie de Harder](https://hennie-de-harder.medium.com/?source=post_page-----26a445f00e5c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----26a445f00e5c--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----26a445f00e5c--------------------------------) [Hennie de Harder](https://hennie-de-harder.medium.com/?source=post_page-----26a445f00e5c--------------------------------)

·发布在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----26a445f00e5c--------------------------------) ·10 分钟阅读·2023 年 10 月 18 日

--

**《纽约客》最近的头条新闻写道，** [**“他们研究了不诚实。他们的工作是谎言吗？”**](https://www.newyorker.com/magazine/2023/10/09/they-studied-dishonesty-was-their-work-a-lie)**。这背后有什么故事？行为经济学家丹·阿瑞里和行为科学家弗朗西斯卡·基诺，这两位在各自领域都很有声望的学者，正因涉嫌研究不端行为而受到审查。直言不讳地说，他们被指控捏造数据以获得统计上显著的结果。**

不幸的是，这种情况并不罕见。科学研究中曾出现过一些欺诈行为。p-hacking 行为——例如操纵数据、在获得显著 p 值后停止实验，或仅报告显著结果——长期以来一直是一个关注的问题。在这篇文章中，我们将反思一些研究人员可能会被诱惑去篡改他们的发现的原因。我们将展示这些行为的后果，并解释你可以做些什么来防止在自己的实验中发生 p-hacking。

在我们深入探讨丑闻和秘密之前，先从基础开始——一个关于假设检验 101 的速成课程。这些知识将在我们探讨 p-hacking 的世界时非常有帮助。

# 假设检验 101

让我们回顾一下你需要了解的关键概念，以便完全理解这篇文章。如果你已经对假设检验（包括 p 值、I 型/II 型错误以及显著性水平）很熟悉，可以跳过这一部分。

## 最佳比萨测试

让我们前往那不勒斯，这座因比萨饼而闻名的意大利城市。两家比萨店，Port’Alba 和 Michele’s，自称制作世界上最好的比萨饼。你是一个好奇的美食评论家，决心找出哪家比萨店真正配得上这个称号。为了弄清楚，你决定举办“最佳比萨测试”（这本质上就是一个*假设检验*）。

你的调查始于两个*假设*：

+   **零假设**（H0）：Port’Alba 和 Michele 的比萨在味道上没有差异；任何观察到的差异都是由于偶然。

+   **替代假设**（H1）：Port’Alba 和 Michele 的比萨在味道上存在显著差异，表明其中一个比另一个更好。

测试开始了。你召集一组参与者，组织一次盲品测试。每个参与者会被提供两片比萨，一片来自 Port’Alba，一片来自 Michele，而不知道哪一片是哪一片。参与者将对两片比萨进行评分（0-10）。

你设定了严格的**alpha 水平**（显著性水平）为 0.05。这意味着你愿意容忍 5%的 I 型错误概率，即在这种情况下，错误地声称一家比萨店的比萨更好而实际上并非如此。

在收集和分析数据后，你发现参与者 overwhelmingly 更喜欢 Michele 的比萨。以下是得分分布的样子：

![](img/98cbbcc6eef33c86288e2088a2b639c3.png)

图像由作者提供。

假设检验中有两个风险：

+   **I 型错误**：有 5%的小概率（即显著性水平 alpha），你可能会错误地得出 Michele 的比萨更好的结论，而实际上没有显著差异。你不想不公平地贬低 Port’Alba 比萨店。

+   **II 型错误**：另一方面，有 II 型错误。如果实际上 Michele 的比萨更好，但你的测试未能检测到这一点，你会觉得错过了最佳比萨！

在一个矩阵中（你可以将其与混淆矩阵进行比较）：

![](img/b1b89378f42dc5369aa2a3ce2b6f1e9f.png)

I 型和 II 型错误的可视化。图像由作者提供。

为了确保你的发现，你计算**p 值**。结果是一个很小的数字，远小于 0.05。这意味着在零假设为真的情况下，得到如此极端结果的概率极低。我们有一个赢家！以下是用两样本 t 检验计算 p 值的示例：

```py
import numpy as np
from scipy import stats

# Sample data for taste scores (out of 10) for the pizzerias
np.random.seed(42)
portalba_scores = np.random.normal(7.5, 1.5, 50)  
michele_scores = np.random.normal(8.5, 1.5, 50)
michele_scores = [round(min(score, 10), 1) for score in michele_scores]
portalba_scores = [round(min(score, 10), 1) for score in portalba_scores] 

# Perform a two-sample t-test
t_stat, p_value = stats.ttest_ind(portalba_scores, michele_scores)

# Set the significance level (alpha)
alpha = 0.05

# Compare the p-value to alpha to make a decision
if p_value < alpha:
    print("We reject the null hypothesis: {} < {}".format(round(p_value, 7), alpha))
    print("There is a significant difference in taste between Port'Alba's and Michele's pizzas.")
else:
    print("We fail to reject the null hypothesis: {} >= {}".format(round(p_value, 7), alpha))
    print("There is no significant difference in taste between Port'Alba's and Michele's pizzas.")
```

输出：

```py
We reject the null hypothesis: 3.1e-06 < 0.05
There is a significant difference in taste between Port'Alba's and Michele's pizzas.
```

现在你对关键概念有了了解（希望在比萨故事后你不会饿），让我们继续本帖的隐秘部分：p-hacking。

![](img/a1811e389266f25f9a21ba1d2ffed3ab.png)

图像由 Dall·E 3 生成，作者提供。

# P-Hacking

在学术界，期刊充当知识的守门人。这些守门人偏好具有显著结果的研究。获得发表位置的压力可能非常大。这种偏见可能会 subtly 鼓励研究人员参与 p-hacking，以确保他们的工作能够公之于众。不幸的是，这种做法使科学文献中过度代表了积极结果。

P-hacking（也称为数据挖掘或数据窥探）定义为：

> 操作统计分析或实验设计以获得期望的结果或获得统计显著的 p 值。

因此，在研究中，你有可能操控你的分析和数据以获得有趣的结果以便发表。你不断调查、分析，有时甚至修改数据，直到 p 值显著。P-hacking 通常由对认可、发表的渴望以及显著发现的吸引力驱动。研究人员可能会无意或故意陷入 p-hacking 陷阱，被快速认可的承诺和竞争激烈的学术环境的压力所诱惑。

陷入这种陷阱的一个简单方法是*对数据进行广泛探索*。研究人员可能会发现自己处于数据集包含丰富变量和子组的情况下，等待探索。*测试多种组合*的诱惑可能很强。每一个变量、每一个子组都可能提供显著结果的机会。这里的陷阱是挑选数据——只突出那些支持预期结果的变量和子组，而忽略了所做的许多比较。这可能导致误导性和不具代表性的发现。

另一个陷阱是*在实验正式结束前查看 p 值*。这看起来可能无害，但事实并非如此。这里的陷阱是当达到理想的 p 值阈值时，*提前停止数据收集*的诱惑。这可能导致偏倚和不可靠的发现，因为样本可能不具代表性。即使有一个具代表性的样本，在结果给出显著 p 值的时刻停止数据收集也是对数据的操控。

数据探索和查看 p 值可能会不经意间发生。以下示例无法避免：在追求显著性时，研究人员可能会遇到初始结果未能达到预期的情况。*调整主要结果*的欲望，微妙地改变目标标准，可能会让人难以抗拒。

总结：如何避免 p-hacking？以下是五个提示和指导原则：

+   避免广泛探索数据以寻找偶然的显著结果。如果你正在进行多重比较，请用[Bonferroni 校正](https://statisticsbyjim.com/hypothesis-testing/bonferroni-correction/)（将 alpha 值除以实验数量）来调整显著性水平。

+   不要只报告那些产生显著 p 值的分析。保持透明，同时报告那些未产生显著结果的分析。这对于变量也是适用的：不要测试多个变量后仅报告那些显著的变量。在这种情况下，你是在挑选变量。更好的是，你可以考虑预注册你的研究，以提前声明你的假设和分析计划。

+   HARKing（在结果已知后假设）：避免基于获得的结果形成假设；假设应在数据分析*之前*定义。

+   避免在达到显著结果时停止数据收集或分析。与此相关的是：不要在测试结束之前查看结果。事先确定样本大小。是的，即使查看数据也是错误的！

+   确保你满足统计测试的假设。例如，数据的正态性。如果你使用假设数据正态分布的测试，请确保数据确实是正态分布的（你可以通过[Shapiro-Wilk](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.shapiro.html)测试来验证）。

![](img/280edc00b01133f7c8745c4e5f987e6a.png)

由作者使用 Dall·E 3 创建的图像。

# 真实的（恐怖）故事

p-hacking 的后果是严重的，因为它导致假阳性（当零假设实际上为真时被拒绝）。研究是建立在先前研究基础上的，每一个假阳性都是对后续研究和决策的误导。此外，这还浪费了时间、金钱和资源，用于追求实际上不存在的研究方向。而也许最重要的后果是：它破坏了科学研究的信任和诚信。

现在，是时候谈论丑闻了…… 有许多数据操控、数据挖掘和 p-hacking 的例子。让我们看看一些真实的故事。

## Diederik Stapel 的伪造研究

Diederik Stapel 是一位著名的荷兰社会心理学家，他在 2011 年被发现[在多篇已发表的研究中伪造数据](https://www.nytimes.com/2013/04/28/magazine/diederik-stapels-audacious-academic-fraud.html)。他操控和伪造数据以支持他的假设，导致了心理学领域的重大丑闻。

在他被抓获之前，他发布了大量虚假的研究（根据[撤回观察](https://retractionwatch.com/the-retraction-watch-leaderboard/)有 58 篇论文被撤回）。例如，Stapel 伪造了证明[思考肉类会使人们变得更不社交](http://www.pepijnvanerp.nl/wordpress/wp-content/uploads/2012/08/vleesstudies-1.pdf)的数据（这篇论文并未发表）。实际上发表的研究之一是关于[杂乱环境如何促进歧视](https://www.science.org/doi/10.1126/science.1201068)的。显然，当他的欺诈行为被揭穿时，这项研究也被撤回了。不过，你仍然可以在网上找到[他的一些撤回论文](https://onlinelibrary.wiley.com/authored-by/Stapel/Diederik+A.?startPage=0&pageSize=20)。

## Boldt 案例

比之前的案例更令人震惊的是关于 Joachim Boldt 的案件。Boldt 曾被认为是医用胶体领域的杰出人物，并且是使用胶体羟乙基淀粉（HES）在手术过程中提高血压的坚定支持者。然而，一项故意排除 Boldt 的不可靠数据的荟萃分析揭示了不同的故事。这项分析显示，与其他复苏液体相比，静脉注射羟乙基淀粉与显著更高的死亡风险和急性肾损伤相关。[《电讯报》上的一则标题](https://www.telegraph.co.uk/news/health/8360667/Millions-of-surgery-patients-at-risk-in-drug-research-fraud-scandal.html)：

> 数百万患者曾因世界顶级麻醉学家之一的“欺诈性研究”而接受争议药物治疗。

正如你所预期的，这些揭露的后果非常严重。Boldt 失去了他的教授职位，并成为刑事调查的对象，面临伪造多达 90 项研究的指控。

是什么让他发布这些虚假的结果？为什么有人愿意冒生命危险来追求某种出版物或宣传？不幸的是，在 Boldt 的案例中，我们不得而知。虽然他可能在调查或法律程序中提供了解释或陈述，但没有对他的动机进行全面且广泛认可的描述。这里可以找到[一篇关于该案件的文章](https://www.bmj.com/bmj/section-pdf/187846?path=%2Fbmj%2F346%2F7900%2FFeature.full.pdf)，其中包含时间线。

## 再现性危机

网络上还有许多更多的恐怖故事。一般来说，研究的再现性非常重要。为什么？它允许验证和确认研究结果，确保结果不是仅仅由于偶然、错误或偏见。但是再现结果可能是具有挑战性的。实验条件可能会有所不同，设备差异，或者数据收集和分析中的微小错误。（或者有人进行了 p-hacking，这使得无法重现研究。）

一些研究人员试图展示再现性为何成为问题。一个有趣的例子是[一项关于巧克力以及其对体重减轻影响的研究](https://www.scribd.com/doc/266969860/Chocolate-causes-weight-loss)。尽管方法学上存在故意的缺陷，但该研究仍被广泛发布和分享。这一事件展示了薄弱的科学严谨性和耸人听闻的媒体报道的陷阱。

但在尝试重现现有研究时，也会遇到挑战。2011 年，拜耳的研究人员透露，[他们只能重复约四分之一](https://www.nature.com/articles/nrd3439-c1) (!) 的已发布的临床前研究。这一令人担忧的发现使许多人质疑一些早期药物研究的可靠性，强调了良好验证流程的迫切需求。同样，心理学领域也面临自己的问题。为了解决这个问题，[“许多实验室”项目](https://osf.io/wx7ck/) 被启动了。这个全球性努力让世界各地的实验室尝试重现相同的心理学研究，常常揭示出结果的显著差异。这些发现强调了多实验室协作确保研究结果真实的关键作用。

![](img/4daf05408b04877250a293168a082de3.png)

不幸的是，这并非事实。图片由作者使用 Dall·E 3 创建。

# 结论

这篇文章旨在让你意识到 p-hacking 对科学文献的微妙但重大影响。这提醒我们，并非每篇研究文章都可以轻信。作为好奇且批判性思考者，接近科学发现时必须保持敏锐的眼光。不要盲目相信每一篇出版物；相反，寻求多个支持相同论断的来源。当不同研究趋于相同的真理时，真正的知识才会显现。

但如果有一个解决学术界最紧迫挑战之一的方案呢？请在我即将发布的博客文章中关注我揭示 p-hacking 问题的潜在解药。敬请期待！

## 相关内容

[](https://medium.com/bigdatarepublic/embracing-the-unknown-lessons-from-chaos-theory-for-data-scientists-a992e8478600?source=post_page-----26a445f00e5c--------------------------------) [## 拥抱未知：数据科学家从混沌理论中获得的教训

### 理解预测模型局限性的见解

[medium.com](https://medium.com/bigdatarepublic/embracing-the-unknown-lessons-from-chaos-theory-for-data-scientists-a992e8478600?source=post_page-----26a445f00e5c--------------------------------) ## 机器学习项目中的伦理考虑

### 在构建 AI 系统时不要忽视这些话题

[towardsdatascience.com
