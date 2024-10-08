# AI 中解释性的渐变的必要性

> 原文：[`towardsdatascience.com/the-necessity-of-a-gradient-of-explainability-in-ai-743ee0bcb848`](https://towardsdatascience.com/the-necessity-of-a-gradient-of-explainability-in-ai-743ee0bcb848)

## 过多的细节可能让人不堪重负，而不足的细节则可能产生误导。

[](https://medium.com/@kevin.berlemont?source=post_page-----743ee0bcb848--------------------------------)![凯文·贝尔蒙特博士](https://medium.com/@kevin.berlemont?source=post_page-----743ee0bcb848--------------------------------)[](https://towardsdatascience.com/?source=post_page-----743ee0bcb848--------------------------------)![数据科学的未来](https://towardsdatascience.com/?source=post_page-----743ee0bcb848--------------------------------) [凯文·贝尔蒙特博士](https://medium.com/@kevin.berlemont?source=post_page-----743ee0bcb848--------------------------------)

·发表于 [数据科学的未来](https://towardsdatascience.com/?source=post_page-----743ee0bcb848--------------------------------) ·4 分钟阅读·2023 年 7 月 29 日

--

![](img/053eb6288d97c8586d780c90ace9e905.png)

图片由 [No Revisions](https://unsplash.com/@norevisions?utm_source=medium&utm_medium=referral) 提供，发布在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

“*任何足够先进的技术都与魔法无异*” — **阿瑟·克拉克**

随着自动驾驶汽车、计算机视觉和最近的大型语言模型的发展，科学有时会让人觉得像魔法一样！模型每天都在变得越来越复杂，当试图向新受众解释复杂模型时，很容易挥手一抹，随口说点关于反向传播和神经网络的话。然而，有必要描述一个 AI 模型、其预期影响和潜在偏见，这就是可解释 AI 的作用所在。

在过去十年中，AI 方法的爆炸式增长使得用户接受他们得到的答案而不加质疑。整个算法过程通常被描述为黑箱，即使是开发它的研究人员，也不总是直接或甚至可能理解模型如何得出特定结果。为了建立用户的信任和信心，公司必须描述它们所采用系统的公平性、透明度和基本决策过程。这种方法不仅导致对 AI 系统的负责任态度，而且增加了技术的采纳 ([`www.mckinsey.com/capabilities/quantumblack/our-insights/global-survey-the-state-of-ai-in-2020`](https://www.mckinsey.com/capabilities/quantumblack/our-insights/global-survey-the-state-of-ai-in-2020))。

AI 解释性中最困难的部分之一是明确界定正在解释的内容的边界。高管和 AI 研究人员对信息的需求和接受度不同。找到在简单解释和所有可能路径之间的合适信息层次需要大量的训练和反馈。与普遍观点相反，去除数学和复杂性的解释并不会使其毫无意义。确实存在过度简化的风险，可能会误导对方认为他们对模型及其功能有深入理解。然而，运用正确的技术可以在适当的层次上提供清晰的解释，从而引导对方向数据科学家等进一步询问以增加知识。这个过程的关键在于进行有效的对话和沟通，以确保必要的信息得到传达。

如何获得有效沟通的经验？与普遍观点相反，获得解释经验并不需要达到高级职位。虽然确实解释复杂概念的技能通过试错过程随着时间的推移而提高，但初级员工往往在这方面非常有效，因为他们刚刚学习了相关内容。关键是通过实践获得向非技术观众解释的经验，任何人都可以做到这一点，而不需要等待成为高级员工。事实上，理解复杂概念并能够向非技术观众解释这些概念并不互相排斥。要提高这一技能，唯一的方法就是：实践、实践、再实践。

解释复杂概念可能因被称为知识诅咒的因素而具有挑战性。需要从不同角度反复讲解以创造持久的记忆。生成性 AI 通过大型语言模型正变得越来越普及，这也产生了对理解的需求。虽然有关于 ChatGPT 提供错误信息的担忧，但理解其原因对于掌握技术的能力和局限性是很重要的。我们都熟悉手机和电子邮件中的预测文本，大型语言模型的工作原理也是如此，只是规模更大。像我们十年前的手机一样，它们并不总能正确预测下一个词。回顾技术的众多进步，很明显一切都是渐进的，利用这种渐进性是解释否则看似魔术的概念的关键。

AI 中的可解释性不仅在向他人解释概念时很重要；在现有的机器学习模型中加入可解释性也可能具有挑战性。在决定选择哪种模型时，应考虑需求和最终用户，以确保在复杂模型和可解释性之间进行权衡。有时，线性回归的简单性胜过更复杂模型的复杂性。对某人生活产生重大影响的决策，例如授权银行贷款，需要解释。特别是当模型的输出不是期望结果时，信息尤为宝贵。在这种情况下，拥有可解释性流程可以揭示模型或使用的训练数据中的缺陷。

总结来说，AI 中的可解释性发生在不同的阶段。向最终用户解释概念，以确保他们理解潜在的局限性。向同行和非技术观众解释模型，以了解算法的详细情况。解释模型应用结果的决策，以确保符合规定且不存在隐性偏见。这三个方面对 AI 的发展都至关重要，如果这些方面中的一个对你正在解决的问题来说过于复杂，那么可能需要考虑所用模型是否是最合适的。

如果你喜欢这个故事，请不要犹豫地点赞或在评论中与我联系！关注我在[Medium](https://medium.com/@kevin.berlemont)的账号，获取更多关于数据科学的内容！
