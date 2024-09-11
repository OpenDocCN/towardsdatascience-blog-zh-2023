# 为了确保人工智能决策的公正，女神需要眨眼

> 原文：[https://towardsdatascience.com/to-guarantee-impartial-ai-decisions-lady-justice-needs-to-blink-2992167b2591?source=collection_archive---------15-----------------------#2023-02-10](https://towardsdatascience.com/to-guarantee-impartial-ai-decisions-lady-justice-needs-to-blink-2992167b2591?source=collection_archive---------15-----------------------#2023-02-10)

## 为什么删除敏感属性不是解决人工智能公平性的简单方法？

[](https://medium.com/@boris-ruf?source=post_page-----2992167b2591--------------------------------)[![Boris Ruf](../Images/96dc4fc2f32add89fef6911195590cd8.png)](https://medium.com/@boris-ruf?source=post_page-----2992167b2591--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2992167b2591--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----2992167b2591--------------------------------) [Boris Ruf](https://medium.com/@boris-ruf?source=post_page-----2992167b2591--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fed341456850c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fto-guarantee-impartial-ai-decisions-lady-justice-needs-to-blink-2992167b2591&user=Boris+Ruf&userId=ed341456850c&source=post_page-ed341456850c----2992167b2591---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----2992167b2591--------------------------------) ·6分钟阅读·2023年2月10日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F2992167b2591&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fto-guarantee-impartial-ai-decisions-lady-justice-needs-to-blink-2992167b2591&user=Boris+Ruf&userId=ed341456850c&source=-----2992167b2591---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F2992167b2591&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fto-guarantee-impartial-ai-decisions-lady-justice-needs-to-blink-2992167b2591&source=-----2992167b2591---------------------bookmark_footer-----------)![](../Images/4e137101cd1e0766b899e5d02573b435.png)

图片来源于 [Pawel Czerwinski](https://unsplash.com/@pawel_czerwinski?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

*不希望的偏见被认为是人工智能（AI）广泛应用的主要风险。在* [*我的上一篇文章*](https://medium.com/towards-data-science/how-prejudice-creeps-into-ai-systems-3673646ae8e3) *中，我讨论了这些偏见的不同潜在来源。但如果我们完全忽视敏感属性会怎样？在接下来的文本中，我将解释为什么所谓的“通过无意识实现公平”并不是正确的解决方案。*

基于个人属于敏感群体的系统性不平等待遇被视为歧视。例如，当人们仅仅因为性别而面临不平等待遇或不利对待时，这被认为是[性别歧视](https://en.wikipedia.org/wiki/Gender_inequality)。当求职者或员工因年龄受到不公平对待时，我们称之为[年龄歧视](https://en.wikipedia.org/wiki/Ageism)。

我们社会上广泛认同这样一种观点：基于个人特征进行区分是不公平的，因为这些特征通常不是个人选择的结果。因此，大多数法律框架禁止这种行为。例如，关于欧盟的非歧视，**《保护人权和基本自由公约》**在[第14条](https://www.coe.int/en/web/conventions/full-list?module=treaty-detail&treatynum=005)中定义了“禁止歧视”。这一原则在**《欧盟基本权利宪章》**中进一步体现，[第21条](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&cd=&ved=2ahUKEwiRzLnMtO71AhXG4IUKHdo4Dx4QFnoECAgQAQ&url=https%3A%2F%2Feur-lex.europa.eu%2FLexUriServ%2FLexUriServ.do%3Furi%3DOJ%3AC%3A2010%3A083%3A0389%3A0403%3Aen%3APDF&usg=AOvVaw0BJqdnCuQHjC-VBG1oO1gL)指出：“任何基于性别、种族、肤色、民族或社会出身、遗传特征、语言、宗教或信仰、政治或其他任何观点、国家少数民族成员资格、财产、出生、残疾、年龄或性取向的歧视均应禁止。”

![](../Images/20dbb63f1efd8e3f28984dd0e640ef81.png)

图片来源：[Tingey Injury Law Firm](https://unsplash.com/@tingeyinjurylawfirm?utm_source=medium&utm_medium=referral) 于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 通过无意识实现公平

计算机不像人类那样有偏见，因此让机器消除歧视似乎很简单。以性别歧视为例：如果机器没有关于性别的信息，那就不会有偏见，对吧？不幸的是，情况要复杂得多。

将任何敏感属性从数据中排除，以便实现“通过无知获取公平”的原则，在法律学者中被称为“反分类”。在欧盟法律中，这在数据保护层面上得到了强制执行：[**通用数据保护条例 (GDPR)**](https://eur-lex.europa.eu/eli/reg/2016/679/oj) 规范了个人数据的收集和使用，包括敏感个人数据。对于许多用例，它严格禁止存储和处理被分类为受保护的属性列表，即敏感属性。

# 话题代理

对于非AI系统，当使用传统的确定性算法和可管理的数据量时，目前的方法

ch可以提供一种解决方案。然而，重要的是要指出，在恶意意图的情况下，反分类本身并不能防止歧视，正如过去所证明的“[红线划分](https://en.wikipedia.org/wiki/Redlining)”实践：有时候，非敏感属性可能与敏感属性强相关。因此，它们可以作为替代品或代理。例如，当许多来自相同族裔背景的人住在同一社区时，非敏感属性邮政编码可能与敏感属性种族相关联。因此，即使在非AI系统的背景下，看似无害的属性也可能被滥用，以便产生歧视性决策，目的是明确排除某个特定的敏感子群体。在没有对敏感属性的实际了解的情况下，这些行为很难被发现和防止。

![](../Images/9dc254aab32ca96707fe7b15e5ed6f2f.png)

图片由[Mika Baumeister](https://unsplash.com/@mbaumi?utm_source=medium&utm_medium=referral)拍摄，来自[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 进入大数据时代

当涉及到AI系统时，去除数据中的敏感属性以防止算法不公平的概念已被证明特别不足：这类系统通常依赖于高维度且强相关的数据集。这意味着决策是基于数百甚至数千个属性，这些属性的相关性对人眼来说并不明显。此外，这些属性中的一些通常包含强链接，这些链接对人类来说再次难以发现。即使在去除敏感属性后，这些数据中的复杂相关性仍可能提供许多意想不到的受保护信息链接。事实上，存在启发式方法来主动重建缺失的敏感属性。例如，[贝叶斯改进姓氏地理编码 (BISG)](https://pubmed.ncbi.nlm.nih.gov/18479410/) 方法试图根据姓氏和地理位置预测种族。虽然这种方法的可靠性通常受到争议，但它表明，仅仅通过技术设计禁止收集敏感属性并不能防止任何可能的滥用。

# 你能对简历进行匿名处理吗？

即使没有任何恶意的歧视意图，仍然存在隐藏的间接歧视的危险，这在结果中非常难以检测。为了说明这个问题，设想一个AI系统，它分析简历以提出新聘员工的起薪。进一步假设，过去由于女性薪资系统性地低于男性同事，女性曾受到歧视。如上所述，这种历史偏见无法通过在学习数据中排除敏感属性“性别”来克服，因为存在许多与非敏感属性的关联。例如，某些运动在女性或男性中更受欢迎。在具有语法性别的语言中，申请人的性别可能通过名词、代词或形容词的性别变化暴露出来。更复杂的是，在一个只对男性强制兵役的国家，大学入学年龄也可能提供性别的线索。即使尝试手动调整所有这些识别出的相关性，也仍然不可能建立足够的“无知”程度，以保证无歧视的决策：基于看似无害的代理变量，机器学习算法很快会识别出旧的模式，并继续对女性分配较低的薪资。

![](../Images/232d8c87f5287e89b87bfd0f9a747cf2.png)

照片来源：[Marjan Blan | @marjanblan](https://unsplash.com/@marjan_blan?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 积极公平

存储敏感属性时隐私保护的风险显而易见。例如，机密信息可能泄露到不受信任的环境中，并被第三方用于欺诈。因此，当前规则旨在解决这些问题的动机是可以理解的：既然没有个人数据被存储，就不会丢失任何个人数据。然而，当涉及到大数据时，由于数据之间的高度相关性，这一假设不再成立。对于AI应用来说，当前通过省略敏感属性来忽略敏感子群体的存在的做法，实际上可能比与数据收集相关的隐私问题更具风险。需要新的技术安全机制来保护敏感属性免遭滥用，同时允许其积极使用，以使敏感子群体可见，并对其进行考虑，以确保可验证的公平结果。无论是对AI利益相关者还是监管者，都需要应用统计措施，以测试结果是否失衡并检测任何类型的歧视。只有这种“积极公平”才能确保AI系统中的公平和非歧视标准得到尊重。

# 那么如何呢

忽视敏感属性以获得公正的AI决策是无效的，因为数据中存在许多相关性和代理变量。这样做甚至适得其反，因为它使检测不希望出现的偏见变得非常困难。相反，必须“看到”应该被保护的敏感子群体，以实现AI公平性。这需要积极使用敏感属性。

*非常感谢Antoine Pietri在撰写此文时提供的宝贵支持。在* [*我的下一篇文章*](/so-how-fair-is-your-ai-exactly-83f8defcf449)*中，我讨论了定义公平性这一具有挑战性但至关重要的任务。*

## 参考文献

B. Ruf & M. Detyniecki (2020). [主动公平而非无知](https://arxiv.org/abs/2009.06251)。*ArXiv, abs/2009.06251*。

A. Bouverot, T. Delaporte, A. Amabile 等 (2020). [算法：注意偏见！](https://www.institutmontaigne.org/en/publications/algorithms-please-mind-bias) Institut Montaigne 报告。ISSN: 1771–6756。

S. Corbett-Davies & S. Goel (2018). [公平性的衡量与误衡量：公平机器学习的批判性回顾](https://arxiv.org/abs/1808.00023)。*CoRR, abs/1808.00023*。

A Gano (2017). 不平等待遇与抵押贷款：初学者指南。科罗拉多大学法律评论，第88卷，1109–1166页。
