# 人工智能中的伦理：偏见算法的潜在根源

> 原文：[https://towardsdatascience.com/ethics-in-ai-potential-root-causes-for-biased-algorithms-890091915aa3?source=collection_archive---------1-----------------------#2023-01-27](https://towardsdatascience.com/ethics-in-ai-potential-root-causes-for-biased-algorithms-890091915aa3?source=collection_archive---------1-----------------------#2023-01-27)

## 理解数据偏见的另一种方法

[](https://medium.com/@jonas_dieckmann?source=post_page-----890091915aa3--------------------------------)[![Jonas Dieckmann](../Images/e1f2d236e6bda6ec1e14fd5eaa9d205e.png)](https://medium.com/@jonas_dieckmann?source=post_page-----890091915aa3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----890091915aa3--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----890091915aa3--------------------------------) [Jonas Dieckmann](https://medium.com/@jonas_dieckmann?source=post_page-----890091915aa3--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1c8d1cf684f2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fethics-in-ai-potential-root-causes-for-biased-algorithms-890091915aa3&user=Jonas+Dieckmann&userId=1c8d1cf684f2&source=post_page-1c8d1cf684f2----890091915aa3---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----890091915aa3--------------------------------) ·12分钟阅读·2023年1月27日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F890091915aa3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fethics-in-ai-potential-root-causes-for-biased-algorithms-890091915aa3&user=Jonas+Dieckmann&userId=1c8d1cf684f2&source=-----890091915aa3---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F890091915aa3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fethics-in-ai-potential-root-causes-for-biased-algorithms-890091915aa3&source=-----890091915aa3---------------------bookmark_footer-----------)![](../Images/e5c4b13259f189e635f5142221eb191e.png)

图片来源：[Pixabay](https://pixabay.com/vectors/lady-justice-allegory-justice-7313900/)

随着数据科学应用数量的增加，滥用的潜力也在增加。很容易指责开发人员或分析团队算法结果的责任，但他们真的是主要的罪魁祸首吗？以下文章试图从不同角度讨论这个问题，并**得出结论认为伦理滥用可能是我们社会数据中的真正问题**。

> 更美好的世界不会仅仅因为我们使用数据而到来；数据有其黑暗的一面。¹

# 偏见算法

如今，数据科学的讨论常常与商业和工业中的重大机遇相关联，以提供更准确的预测和分类解决方案，并改善人们在健康和环境领域的生活。² ³ 也许和这些好处一样重要的是，在评估数据分析中的新解决方案和方法时，应该考虑数据伦理挑战。作为伦理学的一个特定分支，数据伦理处理与数据、算法及相关实践有关的道德问题，以制定和支持道德良好的解决方案。⁴ 总体而言，数据的使用与滥用之间似乎存在一条微妙的界限。公司不仅收集数据以增加利润，还为了向客户提供量身定制的体验。⁵ 然而，当公司开始将其数据用于其他收集目的之外的外部利用时，就会出现伦理问题。除了这些隐私相关的问题外，另一个挑战是数据分析偏差，它可能以多种方式出现。例如，由有特定意图或框架的人设计的调查问题、从具有特定背景的群体中选择性收集数据，或数据来源本身的偏差。⁶

![](../Images/8cf06feccb2e3d60dd59b0cee91c5553.png)

图片来源：[Pixabay](https://pixabay.com/photos/play-figures-green-blue-play-wood-4541731/)

## 算法歧视的流行例子

这些问题绝非理论上的——它们引发了实际关注，并在多个案例研究中引发了严重的讨论：

+   **犯罪预测中的种族歧视**

    一个有问题且广为人知的例子是犯罪预测。2016年，上海交通大学的一篇机器学习论文研究了是否可以通过面部特征分析来检测犯罪行为。⁷ 结果显示，那些外貌与平均水平“不同”的人更可能被怀疑犯有罪行。⁸ 另一个例子是美国用于预测被逮捕个体再犯可能性的风险评估算法。结果表明，底层算法倾向于将黑人被告的再犯可能性预测为白人被告的两倍，而白人被告则常被错误分类为低风险，即使他们实际上是再犯者。⁹

+   **招聘中的性别歧视**

    另一个受偏见算法影响的领域是招聘。2014年，亚马逊使用基于机器学习的软件来评估和排名潜在候选人的简历，以寻找新的顶级人才。2015年，该公司发现其新系统在评估申请者时并未性别中立，特别是在软件开发职位上。根据过去十年的招聘情况，该系统惩罚了所有在简历中包含“女性”一词的申请。¹⁰

+   **社交媒体中的刻板印象**

    性别和种族偏见也可以在Facebook的广告中找到。在他们2019年的实证研究中，Ali等人发现，根据广告与不同受众的相关性，可能会对特定用户投放广告时存在偏见规则。这种相关性通常基于男性/女性刻板印象，通过分析数字广告媒体的图像分析软件进行分类和关联。¹¹

## 大数据伦理原则

如何防止这种情况的发生？这些案例只是现代世界中算法被偏见性使用的众多实例之一。许多专家讨论并评估避免数据分析偏见的方法，这在Herdiana发布的《大数据伦理5原则》之一中有所体现：“大数据不应固化诸如种族主义或性别歧视等不公平偏见。”¹² 虽然这一原则被认为是数据伦理中的通用规则，但给出的示例“如种族主义或性别歧视”似乎是最常见的情况。然而，更通用的描述可能是：

> “大数据不应固化导致歧视的不公平偏见。”

加拿大人权委员会（CHRC）将歧视描述为因称为“理由”的原因而对某人或某群体的不公平对待。这些理由受到《加拿大人权法案》的保护，包括*种族、民族或族裔来源、肤色、宗教、年龄、性别、性取向、性别认同或表现、婚姻状况、家庭状况、残疾、遗传特征以及已获赦免或注册被暂停的罪行*。¹³ 在社会学中，歧视描述了基于个人或群体特征的贬低行为，这种行为是系统性地进行的，因此构成了严重的人权侵犯。¹⁴ 欧洲人权公约（ECHR）也在第14条中禁止在上述所有领域的歧视，并将*语言、政治观点*和*财产*等添加到CHRC的名单中。¹⁵

欧洲经济和社会委员会（EESC）的一项研究题为“**大数据的伦理：在欧盟政策背景下平衡经济利益和大数据的伦理问题**”将算法偏见描述为大数据的关键伦理问题之一。他们将对算法的信任定义为一个伦理问题，因为大多数人认为机器天生是中立的。实际上情况并非如此，因此风险可能非常高。¹⁶ 但这种缺乏中立性的可能原因是什么？很难想象世界上最优秀的数据科学家在开发算法时有这样的目标。

# 根本原因分析

算法本身并没有偏见。然而，许多算法产生的结果是偏颇的，而对这些结果的原因和驱动因素的深入评估往往是肤浅的。此外，缺乏一个假设世界中没有算法的可能替代方案和结果。因此，对算法偏见的问题不应仅仅集中在算法的中立性上，还应强调（缺失的）原因。因此，以下分析旨在进一步探讨与偏见相关的问题，以及今天对开发者的伦理压力如何理解为一种集体责任。然而，必须强调消除技术和非技术世界中的歧视的重要性，因为这两者直接相关并相互影响。

1.  **一个有偏见的社会会导致有偏见的数据** 从设计上讲，**歧视在数据收集之前就已经发生，因为数据集是历史的集合，受到了社会本身的影响**。要深入思考这个问题的技术方面，需要检查可能导致偏见系统的原因。在大多数论文和研究中，作者指出算法本身存在偏见，导致了歧视。¹⁷ ¹⁸ 然而，很难将歧视的根本原因归咎于算法本身，因为算法根据给定的输入和参数的交互以自动化方式执行决策规则。¹⁹ 机器学习算法背后的原理通常是一个优化问题，其中规则根据过去对结果的影响应用于参数。²⁰ 因此，在给定变量（例如性别）与目标（例如职位适合性）之间的交互中出现的偏见，并不是由算法本身造成的，而是由基础数据集造成的。以亚马逊招聘的例子来看，偏见的结果（即，男性申请者比女性申请者更受青睐）可以归因于最初输入到算法中的训练数据集。假设技术职位的前员工大多数是男性，算法就预期这会成为一个成功因素。因此，歧视并不是从算法或其开发者个人开始的，而是从公司长期以来的招聘行为开始的。

1.  **变量之间的隐藏偏见** 挑衅地说，**有人可能会认为每个数据集都在某种程度上存在偏见，因为它源于一个可能存在偏见的环境**。算法只能考虑（偏见的）数据中存在的内容，而数据通常仅包含（偏见的）非技术世界中的内容。一种合理且符合常识的方法是，在开始建模之前，将这些歧视性变量从数据集中移除。然而，移除一个变量并不能保证消除该变量对数据集其余部分的存在或影响。我们可以设想一个虚拟的数据集，其中包含变量*性别*和*运动*。这两个变量可能存在互动，例如“男性与足球、女性与骑马之间的相关性”，尽管这个直白的例子可能不能完全代表我们的社会，因为可能有女性更喜欢足球而非骑马，反之亦然。然而，由于数据集的不平衡（例如男性偏多），算法可能会间接地偏向于有足球偏好的人，因为训练过程是基于偏见信息的。这个例子表明，移除关键变量可以降低歧视的风险，但不足以完全防止歧视。此外，数据集如何平衡地代表变量的不同表现也非常重要。如果表达均匀分布，敏感（歧视性）项目可能不那么问题严重。如果某一部分条目被低估，算法可能会在负面上考虑这一点。因此，很难确定一个数据集是否真正无偏。²¹

1.  **技术决策与人工决策**

    **没有保证人类做出的决策比算法更少偏见**。除了任何数据集都可能存在偏见并导致算法过程中的偏见结果这一观点外，另一个关键的指标是不完整数据形式的信息缺乏。²² 很难保证给定的数据集具有完整的可视性来解释目标行为。数据缺乏的常见原因可能是有限的访问权限、主动排除信息或对额外信息边际影响的缺乏了解。歧视的另一风险不仅源于技术实现，还可能根植于我们社会的现有结构。大数据与机器学习算法的结合优势在于能够将人类规则和决策应用于大量信息。如果算法被认为是有偏见的，人们可能会怀疑人类如何处理给定数据的分析。回顾社交媒体广告，个性化广告旨在根据个人的偏好做出个体化建议。这意味着算法支持个体主义。然而，算法却有相反的效果，因为它们基于一般模式而非个体模式处理自动化和标准化的决策规则。最终，可以认为**算法支持集体主义**而非个体主义。²³

# 结论

我们的分析表明，算法中的偏见主要是当今社会偏见的结果。后者支持收集带有偏见的数据集，这些数据集可能包含（隐藏的）关系，最终导致偏见结果。此外，我们质疑人类决策是否比算法规则更少偏见。最后，我们得出结论，更复杂的算法往往过于不透明。

![](../Images/14d1030419c35000674f8e36d30d092c.png)

图片来源：Pixabay

**缓解策略**

在关于机器学习算法偏见结果的责任讨论中，开发者或数据科学团队常常被指责为歧视性行为的源头。一方面，这种说法过于简单化，责任分配需要进一步区分。另一方面，任何使用机器学习算法的开发者都应意识到他们任务的敏感性及其相应的责任。由于算法支持集体主义，调查这些集体算法是否对个人做出决策或建议的公平性是重要的。文献建议了其他减少偏见的技术，例如设计一个对小样本查询进行随机化的系统，或使用倾向加权技术。²⁴ 对于基于人类内容的偏见，有几种量化歧视和评估算法公平性的思路。²⁵

**公平作为共同责任：在技术和现实世界中** 数据团队有责任识别和最小化其数据模型中的偏见行为。然而，鉴于本文所考察的歧视性因素，确保没有人通过技术系统或人为行为经历这种不公正是一项全球责任。

1.  人类必须平等对待他人，不带有歧视

1.  算法必须平等对待人类，不带有歧视

1.  我们不应借助算法来为偏见和歧视性的人类行为辩解。

**展望：接下来怎么办？**

评估决策或预测时，不仅应考虑结果本身，还需考虑在解决方案中涉及的伦理问题。我们还需要重新思考涉及偏见行为的系统。然而，没有证据表明替代方案（以人为基础的决策）会导致总体上的更多平等。因此，我们不应仅关注*技术内部*的偏见问题，而应将讨论扩大到*技术外部*的偏见问题。然而，*正因为如此*，**无偏见的系统应有助于改善我们当前的社会弱点**，并可能带来**未来世界中的更多平等**。人们可以利用AI更容易地检测非技术世界中的歧视。因为**只要我们在数据中发现偏见，社会中也存在偏见**。

[Jonas Dieckmann - Medium](https://medium.com/@jonas_dieckmann?source=post_page-----890091915aa3--------------------------------) 

### 阅读Jonas Dieckmann在Medium上的文章。分析经理兼产品负责人 @ Philips | 对…充满热情并撰写关于…

[Jonas Dieckmann - Medium](https://medium.com/@jonas_dieckmann?source=post_page-----890091915aa3--------------------------------)

希望你觉得这些内容有趣。请告诉我你的想法，并随时在LinkedIn [https://www.linkedin.com/in/jonas-dieckmann](https://www.linkedin.com/in/jonas-dieckmann/) 上联系我，或者在medium上关注我。

# 也可以查看我的其他文章：

[如何使用Keras API和Google Colab开始使用TensorFlow](https://towardsdatascience.com/how-to-set-started-with-tensorflow-using-keras-api-and-google-colab-5421e5e4ef56?source=post_page-----890091915aa3--------------------------------) 

### 使用神经网络分析人类活动的逐步教程

[案例研究：将数据科学过程模型应用于现实世界场景](https://towardsdatascience.com/how-to-set-started-with-tensorflow-using-keras-api-and-google-colab-5421e5e4ef56?source=post_page-----890091915aa3--------------------------------) 

### 供应链中材料规划的机器学习模型开发

[towardsdatascience.com](/case-study-applying-a-data-science-process-model-to-a-real-world-scenario-93ae57b682bf?source=post_page-----890091915aa3--------------------------------)

# 参考文献

[1]: Loukides, M., Mason, H., & Patil, D. (2018). 伦理与数据科学。

[2]: Floridi, L., & Taddeo, M. (2016). 什么是数据伦理？ 皇家学会哲学学刊 A: 数学、物理与工程科学，374(2083)，20160360. doi: 10.1098/rsta.2016.0360

[3]: Google (2021). 社会公益中的人工智能：将人工智能应用于全球最大挑战. 检索日期：2021年3月10日，来源：[https://ai.google/social-good](https://ai.google/social-good)

[4] Mittelstadt, B., & Floridi, L. (2015). 大数据伦理：生物医学背景中的当前和可预见问题. 科学与工程伦理，22(2)，303–341. DOI: 10.1007/s11948–015- 9652–2

[5] Hand, D. (2018). 在变化的世界中数据伦理的各个方面：我们现在处于何种状态？ 大数据，6(3)，176–190. DOI: 10.1089/big.2018.0083

[6] Broad, E., Smith, A., & Wells, P. (2017). 帮助组织应对其数据实践中的伦理问题. 开放数据研究所。

[7] 吴晓林，张熙. (2016). 使用面部图像的犯罪推断自动化。

[8] Sullivan, B. (2016). 一个新程序判断你是否是罪犯通过你的面部特征. 检索日期：2021年3月10日，来源：[https://www.vice.com/en/article/d7ykmw/new-programdecides-criminality-from-facial-features](https://www.vice.com/en/article/d7ykmw/new-programdecides-criminality-from-facial-features)

[9] Angwin, J., Larson, J., Mattu, S., & Kirchner, L. (2016). 机器偏见. 检索日期：2021年2月28日，来源：[https://www.propublica.org/article/machine-bias-risk-assessments-in-criminal-sentencing](https://www.propublica.org/article/machine-bias-risk-assessments-in-criminal-sentencing)

[10] Dastin, J. (2018). 亚马逊取消了一个对女性有偏见的秘密人工智能招聘工具. 检索日期：2021年2月28日，来源：[https://www.reuters.com/article/us-amazon-com-jobsautomation-insight-idUSKCN1MK08G](https://www.reuters.com/article/us-amazon-com-jobsautomation-insight-idUSKCN1MK08G)

[11] Ali, M., Sapiezynski, P., Bogen, M., Korolova, A., Mislove, A., & Rieke, A. (2019). 通过优化进行歧视. ACM人机交互学刊，3(CSCW)，1–30. DOI: 10.1145/3359301

[12] Herdiana (2013), “大数据伦理的5项原则,” 化学信息与建模学刊. 检索日期：2021年3月10日，来源：[https://aiforceos.com/2018/06/16/big-data-ethics](https://aiforceos.com/2018/06/16/big-data-ethics)

[13] 加拿大人权委员会. 什么是歧视. 检索日期：2021年2月28日，来源：[https://www.chrc-ccdp.gc.ca/eng/content/what-discrimination](https://www.chrc-ccdp.gc.ca/eng/content/what-discrimination)

[14] Hartfiel, G., & Hillmann, K. (1994). 社会学词典. 斯图加特：Kröner

[15] 欧洲委员会（1950）。《保护人权和基本自由的欧洲公约》。检索日期：2021年3月10日，来自[www.refworld.org/docid/3ae6b3b04](http://www.refworld.org/docid/3ae6b3b04)

[16] 欧洲经济和社会委员会。（2017）。大数据的伦理。

[17] Eslami, M., Vaccaro, K., Karahalios, K. 和 Hamilton, K.（2017）‘小心；事情可能比看起来更糟’：理解评级平台中的偏见算法和用户行为。《国际AAAI网络与社会媒体会议论文集》，11(1)。

[18] Kim, P.（2017）审计算法以防歧视，166 U. Pa. L. Rev. 检索日期：2021年3月10日，来自scholarship.law.upenn.edu/penn_law_review_online/vol166/iss1/10

[19] Mittelstadt, B., Allo, P., Taddeo, M., Wachter, S., & Floridi, L.（2016）。算法的伦理：辩论的映射。《大数据与社会》，3(2)，205395171667967。DOI: 10.1177/2053951716679679

[20] Lehr, D., Ohm, P.（2017）。数据的游戏：法律学者应当了解的机器学习，《UCDL评论》51：653，671。

[21] Calders, T., & Žliobaitė, I.（2013）。为何无偏计算过程可能导致歧视性决策程序。《应用哲学、认识论与理性伦理研究》，43–57。DOI: 10.1007/978–3–642–30487–3_3

[22] Rosen, R.（2019）。缺失数据和插补。检索日期：2021年3月10日，来自[https://towardsdatascience.com/missing-data-and-imputation-89e9889268c8](/missing-data-and-imputation-89e9889268c8)

[23] Saake. I., Nassehi, A.（2004）。伦理的文化化。卢曼文化概念的时代诊断应用。法兰克福/美因：102，135

[24] Ridgeway, G., Kovalchik, S., Griffin, B., & Kabeto, M.（2015）。带权重数据的倾向得分分析。《因果推断期刊》，3(2)，237–249。doi: 10.1515/jci-2014–0039

[25] Floridi, L.（2016）。无过错责任：关于分布式道德行为的性质和道德责任的分配。《皇家学会A类：数学、物理和工程科学哲学交易》，374(2083)，20160112。doi: 10.1098/rsta.2016.0112
