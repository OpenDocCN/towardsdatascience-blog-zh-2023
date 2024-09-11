# 从交叉职能机器学习项目中获得的20个经验教训

> 原文：[https://towardsdatascience.com/20-learnings-from-delivering-cross-functional-machine-learning-projects-ad32371d09c1?source=collection_archive---------10-----------------------#2023-08-07](https://towardsdatascience.com/20-learnings-from-delivering-cross-functional-machine-learning-projects-ad32371d09c1?source=collection_archive---------10-----------------------#2023-08-07)

## 如何在复杂的跨团队项目中导航，推动及时解决方案，同时不破坏关系

[](https://mikhailiuk.medium.com/?source=post_page-----ad32371d09c1--------------------------------)[![Aliaksei Mikhailiuk](../Images/f4bf3f15f3e0b42f34e50b3ffc436b2a.png)](https://mikhailiuk.medium.com/?source=post_page-----ad32371d09c1--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ad32371d09c1--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----ad32371d09c1--------------------------------) [Aliaksei Mikhailiuk](https://mikhailiuk.medium.com/?source=post_page-----ad32371d09c1--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F30bef13bba71&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F20-learnings-from-delivering-cross-functional-machine-learning-projects-ad32371d09c1&user=Aliaksei+Mikhailiuk&userId=30bef13bba71&source=post_page-30bef13bba71----ad32371d09c1---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ad32371d09c1--------------------------------) ·6分钟阅读·2023年8月7日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fad32371d09c1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F20-learnings-from-delivering-cross-functional-machine-learning-projects-ad32371d09c1&user=Aliaksei+Mikhailiuk&userId=30bef13bba71&source=-----ad32371d09c1---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fad32371d09c1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F20-learnings-from-delivering-cross-functional-machine-learning-projects-ad32371d09c1&source=-----ad32371d09c1---------------------bookmark_footer-----------)![](../Images/606a9ccfeae624297ed8a6ee3c0fcd98.png)

图片由作者使用 SDXL 1.0 生成。

无论你是在领导还是协助跨团队项目，都会感到压力。虽然推动截止日期和管理复杂情况的压力常常有“什么没有杀死你就让你更强”这种说法，但这些情况是可以避免的，并且学习可以在一起完成大项目的有趣环境中发生。

跨团队的工程项目具有挑战性，而机器学习项目则增加了另一层复杂性，合作者通常需要帮助理解技术背后的知识、机器学习在生产中的影响和风险。

在与美国和亚洲公司及创业公司合作的过程中，这些公司具有非常不同的工作伦理、标准和期望，我很幸运地领导并参与了多个大规模的跨团队项目，交付了面向客户的机器学习解决方案。

在这里，我总结了在规划、开发过程、管理文档和沟通方面的经验，以及我从跨团队合作中提炼出的规则。

[](/what-i-learned-from-the-best-and-the-worst-machine-learning-team-leads-c9331f56da4d?source=post_page-----ad32371d09c1--------------------------------) [## 成为一名高效的机器学习团队负责人

### 管理沟通、基础设施和文档

[towardsdatascience.com](/what-i-learned-from-the-best-and-the-worst-machine-learning-team-leads-c9331f56da4d?source=post_page-----ad32371d09c1--------------------------------)

# 规划、截止日期和里程碑

+   当将团队聚集在一起处理某个特性时，准备一页纸总结该特性、说明其必要性的充分理由，并通过来自A/B测试/使用分析的数据或上级的请求来证明其优先级。

+   在接触团队时，要记住每个团队都有自己要实现的功能堆栈；如果他们承诺处理某项任务，就需要重新调整任务的优先级。

+   在规划新特性和定义工作范围时，始终首先联系经理。他们了解资源状态，并可以指引合适的人选。始终将经理纳入规划和沟通中；他们可能知道其他项目的相似性。

+   降低成本和提高货币化是团队总是愿意投入资源和时间的两个话题。如果你能将你的要求以这种格式呈现，达成一致将更容易。

+   不同的文化和国家对权力在团队成员之间的分配有着截然不同的看法。虽然只是作为指导，[霍夫斯泰德的权力距离](http://clearlycultural.com/geert-hofstede-cultural-dimensions/power-distance-index/)指数在多个场合上帮助我们更快速、顺利地达成优先级对齐。

# 开发过程

+   确定日期 — [制定明确且有依据的截止日期](https://davestewart.co.uk/blog/the-work-is-never-just-the-work/)。请团队制定工作范围并提出一个日期。截止日期本身并不总是有帮助，但它们作为团队成员在规划工作时的心理锚点，使工作更加集中。

+   确保您遵循适合当前时刻的开发模式和标准。当为了紧迫的截止日期而推动某些特性时，可能会带来短期思维的成本。确保记录下妥协的地方，并在截止日期后分配时间来完善项目。

+   当面临重大截止日期时，团队会变得更容易管理，人员通常愿意付出额外的努力；在节奏较慢的时期不要总是指望这一点。

+   在计划休假时，安排好交接工作，并在您离开时设定清晰的里程碑。分配职责，并指出您回来后需要处理的范围。

+   设定明确的基准，并报告与基准的结果——这样团队更容易看到进展并提供反馈。

# 文档

+   在为外部人员准备文档时，确保进行团队级审查，然后请一个不同团队的可信成员阅读文档，以确保对外部人员而言文档清晰，然后再与其他外部成员和高层管理人员分享。

+   记录会议内容，发送跟进邮件、总结并列出负责人员，确保每个人达成一致。

+   在与共同作者分享文档之前，总是要先询问，尤其是当文档尚未完成时。

+   无论是外部还是内部文档，所有文档必须包含POC、创建日期和最后一次重要更改。

+   使用共享文档进行讨论和对齐。口头协议往往会被误解和遗忘。拥有书面文本来对齐提供了一个起点和一个建设性讨论的锚点。如果您是文档的所有者，请允许第三方留下评论。

# 沟通

+   确保提供定期报告——这些不仅有助于早期标记问题，还能让您保持责任感。如果问题被足够早地标记出来，管理层将有时间提供资源以协助交付。决策过程中的可见性和透明度也使得批评变得更加容易，从而帮助形成更好的解决方案。

+   高层管理人员没有时间深入了解；但他们会对当前的方向和优先级有更好的了解。当向高层管理人员提供信息或潜在的开发选项时，不要过多深入细节。保持报告简洁明了，多用视觉材料，高层概述和现实世界的类比，早期标记问题，这些都是让管理层满意的要素。

+   在向高层管理人员沟通不同选项时，不要仅仅描述几个选项并要求他们选择一个。最好有一个预设的决定，例如，我们想选择X，因为它具备AB和C；然而，Y有DEF，但从长远来看，这一点不重要。您是否同意我们继续进行A？

+   沟通时，与其不抄送重要人员，不如多抄送一个不太相关的人。问问自己：“如果我不联系他们，他们会生气吗？”——项目可能因为有人生气而脱轨。

+   与团队成员以及跨团队保持良好关系是至关重要的。保持这种关系的最佳方式是保持关注，并开放接受反馈。主动请求反馈，尤其是负面的 — 发送消息表明你愿意沟通 — 建立信任。潜意识中，人们会注意到问题并在可能爆发之前告诉你。

[三种每位博士生免费获得的软技能](/three-soft-skills-every-phd-student-gets-for-free-f63f4b1d3f2d?source=post_page-----ad32371d09c1--------------------------------)

### 这是我在攻读机器学习博士学位期间学到的关于研究、沟通和团队合作的全面技巧和窍门列表…

[towardsdatascience.com](/three-soft-skills-every-phd-student-gets-for-free-f63f4b1d3f2d?source=post_page-----ad32371d09c1--------------------------------)

# 摘要

客户和高层管理者不需要问题。他们需要解决方案。这通常是员工和企业主之间的区别 — 员工擅长指出问题，而企业主推动解决方案。

拥有主人心态有助于将人们聚集在一起，建立更具鼓励性的环境。毕竟，当前方有明确的道路时，工作起来要容易得多，而领导者的任务就是定义这条道路。

然而，当争论出现时，当不确定正确决策时，问自己，“对团队和项目来说，什么是最好的？” — 这总是有助于做出正确的决策。

建立项目管理技能的好书包括：

+   [《谈判力：如何赢得谈判》](https://www.amazon.co.uk/Never-Split-Difference-Negotiating-Depended/dp/1847941494) 由**克里斯·沃斯**著 — 用于帮助谈判。

+   [《突然掌控：向上管理、向下管理、全面成功》](https://www.amazon.co.uk/Suddenly-Charge-Managing-Succeeding-Around/dp/1857885619) 由**罗伯塔·马图森**著 — 用于帮助建立与管理层和团队成员的沟通。

+   [《激进的坦诚》](https://www.radicalcandor.com/) 由**金·斯科特**著 — 用于提供诚实且可操作的反馈。

+   [《异类：成功的故事》](https://www.amazon.co.uk/Outliers-The-Story-of-Success/dp/B002SQ2NX6/ref=sr_1_1?adgrpid=55973437874&gclid=CjwKCAiAl-6PBhBCEiwAc2GOVLk_jumIaesPb40eMDXI7z2UTk7NdSQ_tm_pKlqdXmDHcWKLDV9xOhoCrVEQAvD_BwE&hvadid=259101838989&hvdev=c&hvlocphy=1006598&hvnetw=g&hvqmt=e&hvrand=8367469483684919876&hvtargid=kwd-307741383323&hydadcr=11434_1787544&keywords=outliers+book&qid=1643932211&sr=8-1) 由**马尔科姆·格拉德威尔**著 — 用于在挑战性情境中建立正确的态度。

+   [《部落领导力》](https://www.triballeadership.net/) 由**戴夫·洛根**著 — 用于在团队中建立正确的文化。

如果你喜欢这篇文章，请与朋友分享！要阅读更多关于机器学习和图像处理的主题，请按下订阅按钮！

# 喜欢这位作者吗？保持联系！

我是否遗漏了什么？请随时留言、评论或直接在 [LinkedIn](https://www.linkedin.com/in/aliakseimikhailiuk/) 或 [Twitter](https://twitter.com/mikhailiuka) 上给我发消息！

[## 如何在机器学习面试中脱颖而出](https://example.org/acing-machine-learning-interviews-aa73d6d7b07b?source=post_page-----ad32371d09c1--------------------------------)

### 机器学习面试的准备指南和资源。

[towardsdatascience.com](https://example.org/acing-machine-learning-interviews-aa73d6d7b07b?source=post_page-----ad32371d09c1--------------------------------) [## 我希望在攻读机器学习博士学位前掌握的九种工具](https://example.org/nine-tools-i-wish-i-mastered-before-my-phd-in-machine-learning-708c6dcb2fb0?source=post_page-----ad32371d09c1--------------------------------)

### 无论你是创建初创公司还是取得科学突破，这些工具都能提升你的机器学习流程。

[towardsdatascience.com](https://example.org/nine-tools-i-wish-i-mastered-before-my-phd-in-machine-learning-708c6dcb2fb0?source=post_page-----ad32371d09c1--------------------------------)

**本博客中的观点仅代表我个人，不代表Snap的观点。**
