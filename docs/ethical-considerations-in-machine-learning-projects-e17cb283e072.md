# 机器学习项目中的伦理考虑

> 原文：[`towardsdatascience.com/ethical-considerations-in-machine-learning-projects-e17cb283e072`](https://towardsdatascience.com/ethical-considerations-in-machine-learning-projects-e17cb283e072)

![](img/22f131eeee213e6dddb5a6519e111693.png)

图片由 Dall-E 2 提供。

## 在构建 AI 系统时不要忘记这些话题

[](https://hennie-de-harder.medium.com/?source=post_page-----e17cb283e072--------------------------------)![Hennie de Harder](https://hennie-de-harder.medium.com/?source=post_page-----e17cb283e072--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e17cb283e072--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----e17cb283e072--------------------------------) [Hennie de Harder](https://hennie-de-harder.medium.com/?source=post_page-----e17cb283e072--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e17cb283e072--------------------------------) ·阅读时间 7 分钟 ·2023 年 2 月 11 日

--

**使用敏感数据、基于个人信息的歧视、影响人们生活的模型输出……数据产品或机器学习模型可能以多种不同方式造成伤害。不幸的是，** [**有很多例子**](https://www.amazon.com/Weapons-Math-Destruction-Increases-Inequality/dp/0553418815) **确实出现了问题。另一方面，许多项目是无害的：不使用敏感数据，项目仅仅是改进流程或自动化无聊的任务。当有可能造成伤害时，你应该投入时间关注本文的话题。**

是的，数据伦理并不是你能想到的最令人兴奋的主题。但如果你想负责任地使用数据，你可能会对不同的方式感兴趣，以确保这一点。本文包含六个重要的伦理主题和调查你的模型表现的方法。还有实用工具可以帮助你做到这一点。除了对个人造成的伤害，另一个需要负责任的重要原因是伦理考虑会影响技术的信任和信誉。如果技术被视为不可信和不透明，它可能会阻碍其采用并影响其有效性。人们会失去信任，不再愿意使用 AI 产品。

# 治理

首个要提到的话题是 AI 治理。数据科学和 AI 的治理方面包括为确保这些技术以伦理和负责任的方式使用而制定的监管、伦理指南和最佳实践。

治理是一个高层次的术语，包含了所有以下主题及更多。具体定义在不同组织和机构中可能有所不同。大型公司通常使用框架来定义 AI 治理的各个方面。这是[Meta](https://ai.facebook.com/blog/facebooks-five-pillars-of-responsible-ai/)的一个例子。像[谷歌的这个团队](https://research.google/teams/responsible-ai/)一样，有完整的研究团队专注于负责任的 AI。

AI 治理仍在不断发展。即将讨论的主题是基础内容，一个好的 AI 治理框架至少应包含这些主题。

![](img/71aa730c83a53e7d09e620fa95d49266.png)

AI 治理以及本文涉及的主题。图片来源：作者。

# 公平性

机器学习中的公平性指的是模型的预测不应对某些人群存在不公平的偏见。在将模型应用于现实世界时，特别是在医疗和刑事司法等敏感领域时，这是一项重要的考虑因素。不幸的是，曾经出现过许多失败的例子，比如[COMPAS](https://www.propublica.org/article/machine-bias-risk-assessments-in-criminal-sentencing)、[微软的聊天机器人 Tay](https://en.wikipedia.org/wiki/Tay_(bot))以及[招聘](https://www.reuters.com/article/us-amazon-com-jobs-automation-insight-idUSKCN1MK08G)和[抵押贷款](https://www.forbes.com/sites/korihale/2021/09/02/ai-bias-caused-80-of-black-mortgage-applicants-to-be-denied/)申请中的问题。

实际上，这是机器学习中的主要伦理问题之一：通过训练数据将偏见引入模型的可能性。解决偏见引入的方式以及可以采取的缓解措施非常重要。

有不同的方法可以提高模型的公平性并减少偏见。首先是使用多样化的训练数据。使用多样化且具有代表性的训练数据集，以确保模型不会对特定群体或结果产生偏见，这是实现公平 AI 系统的第一步。

为了提高公平性，开发了不同的工具：

+   [**IBM 360 度工具包**](https://github.com/Trusted-AI/AIF360)

    该工具包提供了一整套全面的公平性测试指标和缓解偏差的算法。这里是一个[演示](http://aif360.mybluemix.net/data)。每个指标都有解释，这是一个很好的起点。偏差缓解算法的示例有[重加权样本](https://aif360.readthedocs.io/en/stable/modules/generated/aif360.sklearn.preprocessing.Reweighing.html)和[优化预处理](https://aif360.readthedocs.io/en/v0.2.3/modules/preprocessing.html)（改变预处理）、[对抗性去偏](https://aif360.readthedocs.io/en/v0.2.3/modules/inprocessing.html#adversarial-debiasing)（改变建模）和[拒绝选项分类](https://aif360.readthedocs.io/en/v0.2.3/modules/postprocessing.html#reject-option-classification)（改变预测）。

+   [**Fairlearn**](https://fairlearn.org)是一个开源项目，旨在帮助数据科学家提高人工智能系统的公平性。他们提供了[示例笔记本](https://fairlearn.org/v0.8/auto_examples/index.html)和[全面的用户指南](https://fairlearn.org/v0.8/user_guide/index.html)。

# 透明性与可解释性

在数据科学和人工智能中，透明性指的是利益相关者能够理解模型如何工作以及它如何进行预测的能力。讨论透明性的重要性对于建立用户对模型的信任和审计模型的意义非常重要。有些模型易于理解，比如（不是很深的）决策树以及线性或逻辑回归模型。你可以直接解释模型的权重或遍历决策树。

透明的模型有一个缺点：它们通常比像随机森林、提升树或神经网络这样的黑箱模型表现更差。通常，在理解模型工作原理与模型性能之间存在权衡。与利益相关者讨论模型需要多透明是一个好的实践。如果你从一开始就知道模型应该是可解释的并且完全透明，你应该远离黑箱模型。

![](img/d3b3672c52d6fe8c58e92e57bbdbab93.png)

可解释性与性能之间的权衡。点击放大。图片由作者提供。

与透明性密切相关的是可解释性。随着复杂机器学习模型使用的增加，理解模型如何做出预测变得越来越困难。这就是模型可解释性发挥作用的地方。作为模型所有者，你希望能够解释预测。即使是黑箱模型，也有不同的方法来解释预测，例如使用 LIME、SHAP 或全局代理模型。[这篇文章](https://medium.com/towards-data-science/model-agnostic-methods-for-interpreting-any-machine-learning-model-4f10787ef504)深入解释了这些方法。

# 鲁棒性

鲁棒性指的是 AI 系统在面对意外或对抗性情况时保持其性能和功能的能力，例如输入与系统训练时显著不同，或试图恶意操控系统。一个鲁棒的 AI 系统应能正确处理这些情况，而不会导致重大错误或伤害。鲁棒性对确保 AI 系统在现实场景中的可靠和安全部署非常重要，因为这些场景可能与训练和测试时的理想化场景有所不同。一个鲁棒的 AI 系统可以增加对技术的信任，减少伤害风险，并帮助确保系统符合伦理和法律原则。

鲁棒性可以通过不同的方法来处理，例如：

+   [对抗训练](https://adversarial-ml-tutorial.org)，这可以通过对系统进行各种输入的训练，包括对抗性的或故意设计来挑战系统的输入。应内置对抗攻击的防御措施，如输入验证和异常行为监控。

+   定期测试和验证，使用与训练时显著不同的输入，以确保系统按预期功能运行。

+   人工监督机制，例如允许人工用户审查和批准决策，或在系统出错时进行干预。

另一个重要部分是定期（或自动）更新和监控模型。这有助于应对随时间发现的潜在漏洞或限制。

# 隐私

数据科学和人工智能通常依赖大量个人数据，这引发了隐私问题。公司应讨论数据的负责任使用方式以及保护个人信息的措施。欧盟数据保护法，[GDPR](https://gdpr.eu/what-is-gdpr/)，可能是全球最著名的隐私和安全法律。虽然这是一份庞大的文件，但幸运的是有很好的总结和检查表。简而言之，GDPR 要求处理欧盟公民个人数据的公司征得同意、通知安全漏洞、透明使用数据，并遵循[隐私设计](https://gdpr-info.eu/issues/privacy-by-design/)原则。[被遗忘权](https://gdpr.eu/right-to-be-forgotten/)是另一个重要概念。违反 GDPR 的公司可能面临高达公司年全球收入 4%的巨额罚款。

所有主要的云服务提供商都提供了确保敏感和个人数据得到保护，并以负责任和伦理的方式使用的方法。这些工具包括[Google Cloud DLP](https://cloud.google.com/dlp)、[Microsoft Purview 信息保护](https://learn.microsoft.com/en-us/microsoft-365/compliance/information-protection?view=o365-worldwide)和[Amazon Macie](https://aws.amazon.com/macie/)。还有[其他工具](https://www.comparitech.com/data-privacy-management/gdpr-compliance-software/)可供选择。

![](img/c4c49fb7e7344e68d534898f961a7e4e.png)

图片来源于 Dall-E 2。

# 责任

人工智能中的责任指的是个人、组织或系统对其行为、决策以及人工智能系统生成的结果进行解释和辩护的责任。组织需要确保人工智能系统的正常运作。

当人工智能做出错误决策时，需要有人负责。开发者之间对此的看法不尽相同，如你在[这个 StackOverflow 调查问卷](https://insights.stackoverflow.com/survey/2018/)中可以看到。几乎 60% 的开发者认为，管理层对代码的伦理问题负主要责任。但在同一问卷中，几乎 80% 的人认为开发者应考虑其代码的伦理影响。

每个人似乎都同意的一点是：人们对人工智能系统负有责任，并应承担这一责任。如果你想了解更多关于人工智能责任的内容，[这篇论文](https://www.jkroll.com/papers/dissertation.pdf)是一个很好的起点。它提到了一些人工智能责任的障碍，比如通常有很多人参与部署算法，并且错误是被接受和预期的。这使得人工智能治理成为一个复杂的话题。

# 结论

随着机器学习在我们生活中变得越来越普遍，设计和部署这些系统的方式必须符合社会价值观和伦理。这涉及隐私、透明度和公平性等因素。组织和个人应采取最佳实践，例如以维护隐私的方式收集和处理数据，设计透明且可解释的算法，以及检查系统以防止对某些群体的歧视。通过考虑这些伦理因素，机器学习技术可以赢得信任，并最小化负面影响，造福所有人。

## 相关

[](https://medium.com/bigdatarepublic/detecting-data-drift-with-machine-learning-adb177544312?source=post_page-----e17cb283e072--------------------------------) [## 利用机器学习检测数据漂移

### 通过简单的自动化过程了解你机器学习模型的性能退化情况。

[medium.com](https://medium.com/bigdatarepublic/detecting-data-drift-with-machine-learning-adb177544312?source=post_page-----e17cb283e072--------------------------------) [](/model-agnostic-methods-for-interpreting-any-machine-learning-model-4f10787ef504?source=post_page-----e17cb283e072--------------------------------) ## 适用于任何机器学习模型的模型无关解释方法

### 解释方法概述：置换特征重要性、部分依赖图、LIME、SHAP 等。

[towardsdatascience.com [](/interpretable-machine-learning-models-aef0c7be3fd9?source=post_page-----e17cb283e072--------------------------------) ## 可解释的机器学习模型

### 对逻辑回归模型和决策树的解释进行了详细介绍。

[towardsdatascience.com
