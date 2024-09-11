# 人工智能在政府反腐败中的积极作用

> 原文：[https://towardsdatascience.com/ais-proactive-role-in-outsmarting-corruption-in-government-267537dc8afb?source=collection_archive---------9-----------------------#2023-11-16](https://towardsdatascience.com/ais-proactive-role-in-outsmarting-corruption-in-government-267537dc8afb?source=collection_archive---------9-----------------------#2023-11-16)

![](../Images/593476e1bdbacf8dbbeefc18589a9e3c.png)

图片由作者创建，借助 DALL.E 3 的协助

## *通过生成对抗网络和合成数据革新努力*

[](https://medium.com/@Victoria-Walker?source=post_page-----267537dc8afb--------------------------------)[![Victoria Walker](../Images/32296700e27d9d4b45a7408af290e806.png)](https://medium.com/@Victoria-Walker?source=post_page-----267537dc8afb--------------------------------)[](https://towardsdatascience.com/?source=post_page-----267537dc8afb--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----267537dc8afb--------------------------------) [Victoria Walker](https://medium.com/@Victoria-Walker?source=post_page-----267537dc8afb--------------------------------)

·

[跟随](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe435c6c8715b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fais-proactive-role-in-outsmarting-corruption-in-government-267537dc8afb&user=Victoria+Walker&userId=e435c6c8715b&source=post_page-e435c6c8715b----267537dc8afb---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----267537dc8afb--------------------------------) ·9 分钟阅读·2023 年 11 月 16 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F267537dc8afb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fais-proactive-role-in-outsmarting-corruption-in-government-267537dc8afb&user=Victoria+Walker&userId=e435c6c8715b&source=-----267537dc8afb---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F267537dc8afb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fais-proactive-role-in-outsmarting-corruption-in-government-267537dc8afb&source=-----267537dc8afb---------------------bookmark_footer-----------)

**介绍**

近期产生的生成人工智能（AI）模型爆发引起了世界对伦理、风险和安全问题的关注，而本月早些时候在[布莱切利公园会议](https://www.gov.uk/government/publications/ai-safety-summit-2023-chairs-statement-2-november/chairs-summary-of-the-ai-safety-summit-2023-bletchley-park)上，整合了对新兴AI系统安全的国际合作承诺。然而，同样的系统也可以帮助解决其他伦理参与中的类似挑战。

政府腐败和系统偏见是繁荣社会的难以逾越的障碍，但难以解决。合成数据和生成AI，如生成对抗网络（GANs），不仅可能寻找腐败行为的方式，还可能预测腐败如何发生的创新方法。

**政府腐败的普遍性和成本**

透明国际的[腐败感知指数](https://www.transparency.org/en/cpi/)持续显示了这一问题在各国政府中的普遍性。其[多种形式](https://www.unodc.org/e4j/en/anti-corruption/module-4/key-issues/manifestations-and-consequences-of-public-sector-corruption.html)包括权力寻租、贿赂、裙带关系、证据操纵、回扣等行为，旨在通过滥用职能获取利益。这造成了显著的财务损失，世界银行预计每年为[2.6万亿美元](https://blogs.worldbank.org/governance/what-are-costs-corruption)，全球大约每年支付[1万亿美元贿赂](https://press.un.org/en/2018/sc13493.doc.htm#:~:text=According%20to%20the%20World%20Bank%2C,trillion%20in%20bribes%20every%20year)。这已经相当可观，但腐败的影响远不止此。腐败损害了对公共机构的信任，侵蚀了对法律的尊重，威胁到教育和健康等基本服务，并剥夺了应对[气候变化](https://www.transparency.org/en/projects/climate-governance-integrity-programme)所需的重要资金。对涉及者（或对问题视而不见者）而言，以及见证人可能面临的个人安全风险，都是强有力的威慑因素。AI为赋予反腐败单位的重要工具。

**AI在反腐败中的应用**

人工智能（AI）在试图理解具有复杂相互连接网络的复杂数据时显得非常有用。尽管不普遍，AI识别犯罪和腐败行为的例子确实存在。

2017年，西班牙研究员费利克斯·洛佩斯-伊图里亚加和伊万·帕斯托尔·桑斯使用神经网络构建了一个[腐败预测模型](https://link.springer.com/article/10.1007/s11205-017-1802-2)。他们利用[《世界报》](https://www.elmundo.es/grafico/espana/2014/11/03/5453d2e6268e3e8d7f8b456c.html)创建的腐败案件数据库中的真实数据，分析了在任何定罪之前的几年，以识别潜在的预警信号。这使得 AI 模型能够揭示未曾发现的关系和联系，比如房地产价格上涨和腐败案件。

使用 AI 来识别可疑行为并防范欺诈活动已经在[银行业](https://fintechmagazine.com/articles/how-ai-can-protect-against-bank-fraud-scams#)中广泛应用。

乌克兰用于识别采购风险的系统（PROZORRO）使用了一款名为[DOZORRO](https://ti-ukraine.org/en/news/dozorro-artificial-intelligence-to-find-violations-in-prozorro-how-it-works/)的程序，旨在识别公共采购中的潜在不当行为。用于训练 AI 的数据来自于专家对大约3,500个招标的风险评估。该系统可以独立评估招标中的腐败风险，并将发现与民间监督组织分享。[初步结果](https://scholarship.law.gwu.edu/cgi/viewcontent.cgi?article=2887&context=faculty_publications)表明，已节省了数十亿美元，政府的合法性得到提升，而且在当前背景下，外国公司更有可能进行投资。

世界银行于2021年推出了基于 AI 的[采购反腐败与透明度平台 (ProACT)](https://procurementintegrity.org)。该平台利用来自100多个国家的开放数据，允许任何人搜索和审查公共采购合同、其透明度评级以及潜在的诚信风险。

然而，AI 系统依赖数据，而在腐败研究中[可靠数据的稀缺](https://news.un.org/en/story/2023/08/1140282)是一个众所周知的挑战。由于腐败活动的隐蔽性、记录中缺乏正确的数据或存在错误，或缺乏有关新方法和系统的信息，获取准确数据可能会[很困难](https://ace.globalintegrity.org/dataexplainer/)。

**合成数据**

对于这些问题中的许多，使用合成数据可能是一种帮助。合成数据是通过算法生成的人工数据，而非从现实世界事件或过程收集的。它模拟真实数据，使其在实际数据不可用、不足或敏感时，用于训练机器学习模型成为可能。模型可以在模拟真实世界腐败情景的环境中学习和适应，而无需冒隐私泄露或敏感数据泄露的风险。

鉴于腐败网络的复杂性和数据的稀缺性，合成数据提供了训练 AI 检测细微腐败模式所必需的细粒度。通过构建逼真的数据集，AI 系统具备了探索、理解和预测的工具。

这些高质量的虚构数据集可以训练 AI 系统在各种复杂和精细的场景中。这种方法增加了参与敏感调查的个人的安全性和匿名性，并为 AI 辨识和预测腐败模式提供了复杂性。

此外，研究表明，训练在合成数据上的 AI 系统可以达到与在真实数据上训练相当的[准确性水平](https://techmonitor.ai/technology/ai-and-automation/ai-synthetic-data-edge-computing-gartner#:~:text=August%202%2C%202023)，但所需时间仅为真实数据的一小部分，而且没有通常与真实数据使用相关的伦理和法律问题。这意味着用合成数据训练 AI 是高效的，并且更有可能跟上犯罪分子（他们也使用[AI 加强其方法](https://enactafrica.org/enact-observer/ai-and-organised-crime-in-africa)）变化的步伐。

**生成对抗网络（GANs）：核心技术**

GANs 是一种使用两个神经网络 —— 生成器和辨别器 —— 在动态过程中生成合成数据的 AI 技术，每个网络通过试图欺骗对方来学习。

在有限的真实数据训练后，生成器创造出详细的合成腐败场景，而辨别器则试图确定其所检查的是真实的还是伪造的（再次利用其自身有限的真实数据训练）。最初的创作可能很粗糙，辨别器很容易识别出正确答案。然而，生成器从中学习，并改进其生成。随着生成器数据变得越来越真实，辨别器的区分能力也变得更加敏锐。这一迭代过程持续进行，直到最终的合成数据成为实际和潜在真实场景以及腐败的微妙模式的近似复制品。

这些数据的出现规避了上述真实数据相关的问题，特别是涉及有限可用性约束和数据的隐私/保密性。然后，这些合成数据库被用作训练特定 AI 系统执行腐败预测、检测或分析工作的基础。

**与GANs生成的合成数据的区别**

GANs 生成的合成数据在数量和复杂性上的增加使得政府和监管机构能够提升其检测能力，使其更擅长发现细微和复杂的腐败形式，这些形式否则可能被忽视。

通过模拟各种腐败场景，生成对抗网络（GANs）有助于创建既具有反应性（发现腐败事件），又具有预见性（识别可能导致腐败的现实情景，其发生可能性越来越高）的人工智能工具。在合成数据上训练的AI系统可以学习各种潜在的腐败场景，包括那些在现实世界数据中可能不充分表现或不可用的场景。这意味着AI系统增加了对腐败模式的理解，并开发了更强大和智能的系统来对抗腐败。

世界银行已经确定了几种[腐败行为可能发展的方式](https://blogs.worldbank.org/voices/corruption-has-modernized-so-should-anticorruption-initiatives)，包括引入复杂的数据模式和数字平台来隐藏腐败活动，将专业人士（银行家、律师和会计师）作为复杂网络中的促成者，以及更为复杂的腐败手段来规避新的国际标准。

当进行腐败活动的能力增强时，反腐败工作也需要变得更加复杂。人工智能能够快速生成、测试和重新生成场景，并且它能够创建的设置范围，使得跟上这种变化的步伐甚至超越这种变化的可能性变得可能。

**伦理维度和隐私关切**

虽然合成数据使用可以减少隐私风险，但开发它的初始训练阶段需要来自政府记录、审计报告、合规数据、公共部门数据库、法律和监管文件以及告发者报告和投诉的真实数据。这显然会引发几个风险：

1\. **敏感信息**：在政府背景下处理的数据通常包括个人详细信息、机密政府文件或敏感财务记录。招标中的信息无疑是商业机密。

2\. **嵌入偏见**：初始数据中包含的任何偏见或歧视（明示或暗示）都可能影响随后的系统。例如，在决策、任命或角色分配中的性别或种族歧视，无论多么微妙，都将被视为机器建立知识基础的“真相”一部分。

3\. **滥用或数据泄露风险**：政府AI系统可能会成为恶意目的的目标，因为它们可以访问大量潜在的机密数据集，从而导致数据泄露或信息滥用。

4\. **伦理AI发展**：由于AI系统被训练来检测腐败模式，它们必须在不侵犯个人权利或创建新伦理困境的情况下进行这些操作。

5\. **法律合规性**：严格的数据保护法越来越多地监管个人数据的处理（例如 [GDPR](https://gdpr.eu/what-is-gdpr/)）。

6\. **维护公众信任：** 如果公众认为AI工具侵犯了隐私或不道德，将会削弱对公共机构的信任。

为减少上述风险而采取的具体措施包括遵守适用的法律框架、实施强大的隐私保护措施、专家对学习数据进行偏见审查、个人数据使用保障措施以及公众透明地了解如何维护伦理规范。积极主动的方法将增加开发的AI的合法性和可信度，并减少因挑战所用系统而导致的任何后续腐败起诉的风险。

**引入GAN和合成数据到政府反腐败系统中**

下图是GAN和生成的合成数据在AI腐败检测系统中应用的示例。

![](../Images/add306c32cfc8f05271a5f9d91c622a5.png)

由作者创建的图表

1.  **用于初始训练的数据：** 这涉及识别易受腐败影响的政府部门，可能依靠外部反腐败民间社会提供专业知识。然后，跨这些领域的历史真实腐败和非腐败情况数据经过伦理保障的检查。

1.  **GAN的创建、训练和运行：** GAN模型接受基于真实数据的初始训练，然后放置在其两个网络之间运行以产生合成数据的交互中。

1.  **训练和改进AI检测模型：** GAN生成的合成数据用于训练新的AI模型，以检测指示腐败活动的模式。

1.  **持续监控伦理和偏见：** 虽然合成数据极大地减少了偏见和隐私问题的风险，但仍有可能出现残留问题。

1.  **试点测试：** 可以在受控环境中部署AI模型进行初步测试，以评估其在识别腐败行为方面的有效性（并向GAN提供反馈以进行调整）。

1.  **扩大规模：** 模型被整合到更广泛的政府系统中（继续保持持续监督和必要的调整）。

1.  **持续学习和适应性：** 从AI腐败检测系统的发现中可以反馈到GAN中，首先通过先前用于检查偏见并消除任何隐私或安全问题的相同过程，以确保系统保持灵活性和适应新的腐败策略和不断变化的数据环境。

**结论**

使用生成对抗网络（GANs）创建的合成数据是一种系统化的反腐败方法，它不仅可以检测当前的腐败行为，还能预测和适应新兴趋势。通过持续的、迭代的学习过程，系统有可能识别和适应未来的腐败方法。由于腐败数据的稀缺，尤其是允许检测系统捕捉到越来越微妙的指示符的详细程度，这些指示符可能分布在多个司法管辖区，GANs能够为各种不同的情境开发新的数据，从而缓解了这一问题。

尽管伦理问题仍然存在，但已经大大减少，因此保护措施和监督仍然至关重要。作为良好治理的基石，政府和监管机构需要将透明度和问责制作为首要优先事项。

尽管尚处于初期阶段，人工智能在打击腐败中的潜力仍然很大。腐败行为的创新速度正在加快，人工智能被用于创建越来越隐蔽和复杂的网络，以规避传统的检测工具。政府反腐败机构需要至少保持类似的创新轨迹，而投资于生成对抗网络（GAN）生成的合成数据提供了预测和缓解腐败的机会。此外，每年因腐败损失的数万亿美元使得在一个成功可以直接增加公共开支预算的领域发展人工智能显得尤为重要，追求这样的投资能够使政府兑现对人民的承诺。
