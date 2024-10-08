# 数据共享挑战：隐私和安全问题

> 原文：[`towardsdatascience.com/data-sharing-challenges-privacy-and-security-concerns-ca436543a323?source=collection_archive---------15-----------------------#2023-02-01`](https://towardsdatascience.com/data-sharing-challenges-privacy-and-security-concerns-ca436543a323?source=collection_archive---------15-----------------------#2023-02-01)

## 实施数据共享时的隐私和安全挑战

[](https://medium.com/@louise.de.leyritz?source=post_page-----ca436543a323--------------------------------)![Louise de Leyritz](https://medium.com/@louise.de.leyritz?source=post_page-----ca436543a323--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ca436543a323--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----ca436543a323--------------------------------) [Louise de Leyritz](https://medium.com/@louise.de.leyritz?source=post_page-----ca436543a323--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fa926de8a6b3f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdata-sharing-challenges-privacy-and-security-concerns-ca436543a323&user=Louise+de+Leyritz&userId=a926de8a6b3f&source=post_page-a926de8a6b3f----ca436543a323---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ca436543a323--------------------------------) ·11 分钟阅读·2023 年 2 月 1 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fca436543a323&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdata-sharing-challenges-privacy-and-security-concerns-ca436543a323&user=Louise+de+Leyritz&userId=a926de8a6b3f&source=-----ca436543a323---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fca436543a323&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdata-sharing-challenges-privacy-and-security-concerns-ca436543a323&source=-----ca436543a323---------------------bookmark_footer-----------)![](img/9fd090463316d2457ba8a20a78bf04aa.png)

隐私与安全：数据共享的最大挑战 — 图片来自 [Castor](https://www.castordoc.com/)

数据共享可以为公司带来 [许多好处](https://medium.com/towards-data-science/5-benefits-of-data-sharing-5ad8efde3385)，但也伴随着一系列问题。公司常常面临的两个主要问题是**隐私** 和 **安全**。我们将在这一系列关于数据共享的第三篇文章中讨论这些概念。

没有人真正喜欢讨论这些话题。我会首先承认，这些话题并不是最令人兴奋的事情。但相信我，花几分钟时间关注它们是值得的。这可以帮助你的公司避免[数百万美元的罚款](https://www.bloomberg.com/news/articles/2021-07-30/amazon-given-record-888-million-eu-fine-for-data-privacy-breach#:~:text=Amazon%20(AMZN)%20Given%20Record%20%24888,for%20Data%20Privacy%20Breach%20%2D%20Bloomberg)。

尽管数据共享带来了巨大的好处，但它可能与隐私和安全原则相矛盾。

数据共享是关于给**业务团队**提供访问数据的权限，以帮助他们做出基于数据的决策。让我们回顾一下数据共享的原则：

+   **每个人**都应该能够访问他们所需的数据，而不仅仅是某些角色或职位。

+   应该**没有障碍**阻止人们获取他们需要的数据。

+   数据应该以一种组织和结构化的方式进行整理，使任何人都能**访问、理解和使用**它。

因此，认为隐私和安全与这些原则相冲突是自然的。隐私是关于赋予个人对其个人信息的控制权。安全则是保护信息不被未经授权的访问。这两个概念可能在数据共享时看似相矛盾。

> 如果你认为自己不受隐私和安全规则的影响，再想想吧。所有处理个人数据的公司都会受到影响。

更重要的是，这些规则通过严格的法规得以执行，例如欧洲的**通用数据保护条例** **（GDPR）**和美国的**加州消费者隐私法案（CCPA）**。

根据像 GDPR 和 CCPA 这样的法律，仅仅遵守是不够的，你还必须能够证明这一点！这就是问责原则。如果你不能展示你遵守规则，你将被认为是不合规的。而且，我们知道，不合规会带来巨额的代价。

本文探讨了在避免隐私和安全相关风险的同时实施数据共享。文章分为三个部分：

1.  处理**隐私**风险及其管理方式。

1.  **安全**问题以及如何缓解这些问题。

1.  演示**合规性**和问责原则。

# ‍1\. 隐私

当我们分享个人身份信息（PII）时，想要保持隐私是很自然的。隐私就是对信息共享、共享对象以及原因的控制。

个人信息包括我们的姓名、社会安全号码、电子邮件、邮寄地址和 IP 地址。保护这些数据对于防止从不便（如垃圾广告）到真正的威胁（如身份盗窃）的侵扰是非常重要的。

GDPR 和 CCPA 中最广泛认可的隐私规则被称为**目的限制**。根据这一规则，你应该仅为了特定、明确且合法的目的处理 PII。你必须在收集数据之前向数据主体说明这些目的。

这一原则确保收集的数据始终用于其指定目的。

假设你是一家零售商，收集客户地址用于产品配送。根据目的限制原则，你只能将这些数据用于产品配送。你没有权利将其用于其他目的，如营销活动。

当涉及数据共享时，我们如何确保数据仅用于其预期目的？当数据对更广泛的受众开放时，跟踪其使用情况可能会变得困难。利益相关者往往不知道数据收集的具体原因。没有这些知识，遵守规则可能会很困难。

此外，数据共享增加了 PII 数据潜在暴露点的数量。这为潜在的隐私侵犯（如身份盗窃）和对个人信息的控制丧失打开了大门。你对数据的开放程度越高，利益相关者利用数据进行恶意目的的机会就越多。

# 解决方案：文档与数据共享协议

在深入解决方案之前，重要的是要注意数据共享并不意味着对 PII 数据的*无限制*访问。

> PII 数据应仅暴露给需要查看的人。

我们将在稍后的访问控制管理部分讨论这些内容。

本节讨论隐私问题和确保有访问权限的人将数据用于预期目的。

这里有两个重要步骤可以帮助你避免组织内的数据滥用。

## 1. 管理数据访问

数据访问可能导致潜在的滥用，因为员工和分包商可能会获取他们不应拥有的机密或敏感信息。

有不同的方法来管理组织中的数据访问：

+   **实施二次认证措施**：验证单个用户的身份至关重要，以便准确知道是谁试图登录系统。在员工使用共享账户（如管理员或 root）时，这变得更加复杂。在这些情况下，实施二次认证方法非常重要。

+   **引入双因素认证**：凭证盗窃仍然是安全漏洞的一个普遍原因。双因素认证通过要求用户不仅提供他们知道的东西（如凭证），还需要他们拥有的东西（如智能手机）或是（如生物识别数据）来改善用户身份验证。

+   **分配特定的用户角色或访问属性到每个账户：** 一旦用户身份经过验证，可以通过分配特定的用户角色或访问属性到每个用户账户来实现细粒度的访问管理。

## 2\. 教育你的员工

不要忽视[员工教育](https://www.knowbe4.com/hubfs/2021-State-of-Privacy-Security-Awareness-Report-Research_EN-US.pdf?hsLang=en-us)在防止数据滥用中的影响。

教育员工的最佳方式是将数据安全信息纳入整体公司政策中。一项全面的政策作为内部程序和标准的可靠信息来源，包括网络安全。这是一种有效的方法，可以教育新员工了解他们可以和不可以如何处理公司数据。

另一种解决方案是数据文档化。对 PII 数据进行适当的文档记录是确保数据以道德和合法方式处理的重要步骤。

文档化涉及识别 PII 数据并在数据库中标记。你应该指定数据收集的目的，以及你将使用数据的具体用途。

为每个 PII 字段提供正确的背景信息可以确保每个人都知道它的用途。这样，各团队在访问数据时可以在目的限制原则下合法使用。

假设你在数据集中有一个标记为“电子邮件地址”的列。对于这个列，重要的是要包含数据使用的详细说明。这可能包括如下声明：“电子邮件地址，仅用于产品配送”

这确保了利益相关者使用数据的目的符合预期，而不是用于任何未经授权的活动。

一旦你的业务团队已经设置好可以轻松访问文档化数据，保持系统运作的另一种方法是设立一个数据共享协议 (DSA)。正如[Piethein Strengholt](https://piethein.medium.com/)所述，DSA 是一个具有法律约束力的合同，列出了数据共享和使用的所有条款和条件。

它概述了将共享哪些类型的数据、共享的原因以及如何保护这些数据。它还列出了每个人的责任，包括数据使用的任何限制，以及如果事情没有按计划进行会发生什么。这些协议在研究、商业和政府中被广泛使用。它们是确保每个人都遵守规则并将数据用于预定目的的绝佳方式。

# 2\. 安全

安全是关于实施**保护**个人信息的措施。

> PII 数据需要防止未经授权的访问、使用、披露、干扰、修改或销毁。

GDPR 中的一个重要安全规则是**完整性原则**。它规定，个人数据必须受到保护，防止未经授权的访问、更改或销毁。

实施数据共享就像是打开了洪水闸门，带来了多种潜在威胁，如黑客攻击或恶意软件。访问数据的人越多，未经授权方获取数据的机会就越多。而且，当数据被共享时，它可能还会存储在多个位置，使得监控变得更加困难。

即使公司的 IT 系统像金库一样坚固，数据共享仍可能带来安全风险。这是因为尽管强大的 IT 系统可能能够抵御外部威胁，但它可能无法防止内部威胁，例如内部泄露。

数据共享可能是一项棘手的业务。访问数据的人越多，你公司系统中的潜在弱点就越多。但这并不意味着一切都要悲观。即使数据受到更多人的关注，仍然可以在确保数据安全和合规的同时保护数据。这只是需要采取正确措施以保护数据。

# 解决方案：访问控制和数据最小化

在数据共享时，关键是找到访问与安全之间的正确平衡。一方面，你要确保正确的人可以访问他们完成工作所需的信息，另一方面，又不想让任何人都能随意进入。这就是**访问控制**和**数据最小化**发挥作用的地方。

+   **访问控制**旨在确保只有合适的人才能访问数据。这个话题在前面章节中已经讨论过。

+   [**数据最小化**](https://medium.com/@deshpandetanmay/gdpr-what-is-data-minimization-dcf9aee8d9)是另一个关键部分。其核心在于将共享的数据量保持在最低限度。与其分享所有数据，不如退一步思考哪些信息对各团队执行任务是真正必要的。一般来说，你可以在数据集中移除或遮蔽 PII 列，而不影响利益相关者。只分享必要的数据，可以将流动的 PII 信息量保持在最低水平。

当**访问控制**和**数据最小化**一起使用时，它们可以帮助你与更多人共享数据，同时保持数据的安全性和符合安全法规的要求。它们可以共同确保你的数据安全，同时向需要的人提供访问。

实践这一点的最佳方法是使用**数据共享平台**。可以把它看作是一个虚拟的“文件柜”，你可以在其中存储和分享你的数据给合适的人。这些平台通常配备了内置的访问控制，因此你可以确保只有那些应有访问权限的人才能看到数据。此外，它们通常还具有强大的安全措施，以保护你的数据免受错误人员的侵害。

通过使用数据共享平台来管理访问控制和数据最小化，你可以与更多人共享数据，同时保持其安全并符合安全法规。这就像一个组合锁，既能保护数据的安全，又能使数据对需要的人可用。这是一个双赢的局面。

# 3\. 责任：如何证明合规性？

如前所述，如果你不能证明你遵守了规则，那么你会被视为违反规则。这是数据法规如 GDPR 和 CCPA 中**责任原则**的基本理念。负责任意味着能够证明你遵循所有法规并确保个人数据安全。

> 责任原则规定，公司必须能够*展示*他们已采取适当的技术和组织措施，以满足法规下的义务。

想象一下，你是一个组织，监管机构正在进行审计以检查谁访问了敏感数据以及他们对这些数据做了什么。如果没有适当的流程，你将陷入困惑，试图弄清楚数据的使用情况。

为了证明符合通用数据保护条例（GDPR），你可以采取以下措施：

1.  **进行数据保护影响评估（DPIA）** — DPIA 是一种风险评估，帮助你识别、分析和减轻数据处理活动的隐私风险。它表明你已采取必要措施，以确保你的数据处理符合 GDPR。

1.  **保留处理活动记录** — 你必须保留所有数据处理活动的记录，包括处理的数据类别、处理目的、数据保留期限等。查看你的数据谱系也可以帮助你证明符合规定。[数据谱系](https://www.castordoc.com/product/data-lineage)，也称为数据家谱，是追踪数据在整个生命周期中的起源和流动的能力。它让你能够看到数据的来源、去向以及去过的地方。这意味着你可以轻松追踪谁访问了这些敏感数据，何时访问的，以及他们对数据做了什么。

1.  **实施适当的技术和组织措施** — 您必须实施适当的技术和组织措施来确保个人数据的安全，例如加密、访问控制和防火墙。这些措施帮助您证明您已采取必要步骤来保护个人数据。

1.  **任命数据保护官（DPO）** — 如果您的组织处理大量个人数据或进行定期和系统的数据主体监控，您必须任命一名 DPO。DPO 可以通过提供专家建议、培训和支持，帮助您证明您对 GDPR 的合规性。

1.  **定期进行隐私审计** — 定期进行隐私审计有助于评估您对 GDPR 的合规性，并识别需要改进的领域。它们还表明您致力于保护个人数据并保持对 GDPR 的合规性。

![](img/e8b13d276e3c530696af14246428c5f3.png)

数据血缘示例 — 图片由 [Castor](https://www.castordoc.com/) 提供

# 结论

虽然数据共享可以为公司带来显著的好处，但它也带来了一系列问题，特别是在隐私和安全方面。

隐私是关于控制信息的共享、共享对象及其原因，而安全性则是指保护数据免受未经授权的访问、更改或破坏。

管理隐私和安全风险可以通过实施一些关键策略来实现。为了保护个人信息，重要的是投资于适当的**文档**并建立明确的**数据共享协议**。此外，实施**访问控制**和遵循**数据最小化**实践可以帮助减少安全风险并确保敏感信息的安全。我们在下面的图片中总结了这些信息。

![](img/f7f7715a29fb9021bdaca78b07676749.png)

处理隐私与安全 — 图片由 [Castor](https://www.castordoc.com/) 提供

公司必须遵守严格的法规，如 GDPR 和 CCPA，以保持数据的安全和保护。**问责原则**还规定，公司必须能够证明其合规性。

然而，借助适当的工具和实践，公司可以在尊重安全性和隐私规则的同时有效管理数据共享。

# 关于我们

我们介绍了利用数据资产时涉及的所有过程：从 [现代数据堆栈](https://notion.castordoc.com/) 到数据团队组成，再到数据治理。我们的 [博客](https://www.castordoc.com/resources/blog) 涵盖了从数据中创造实际价值的技术和非技术方面。

在 Castor，我们正在为 Notion、Figma、Slack 一代构建数据文档工具。

想了解更多？[联系我们](https://www.castordoc.com/try-castor) 我们将为您展示一个演示。

*最初发布于* [*https://www.castordoc.com*](https://www.castordoc.com/blog/data-sharing-challenges)*.*
