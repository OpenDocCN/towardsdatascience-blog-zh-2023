# 数据中的偏见（仍然）难以应对——但却是必要的

> 原文：[`towardsdatascience.com/confronting-bias-in-data-is-still-difficult-and-necessary-e0982fd7416c?source=collection_archive---------11-----------------------#2023-02-09`](https://towardsdatascience.com/confronting-bias-in-data-is-still-difficult-and-necessary-e0982fd7416c?source=collection_archive---------11-----------------------#2023-02-09)

[](https://towardsdatascience.medium.com/?source=post_page-----e0982fd7416c--------------------------------)![TDS Editors](https://towardsdatascience.medium.com/?source=post_page-----e0982fd7416c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e0982fd7416c--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----e0982fd7416c--------------------------------) [TDS Editors](https://towardsdatascience.medium.com/?source=post_page-----e0982fd7416c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7e12c71dfa81&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fconfronting-bias-in-data-is-still-difficult-and-necessary-e0982fd7416c&user=TDS+Editors&userId=7e12c71dfa81&source=post_page-7e12c71dfa81----e0982fd7416c---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e0982fd7416c--------------------------------) · 以 Newsletter 形式发送 · 3 min 阅读 · 2023 年 2 月 9 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe0982fd7416c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fconfronting-bias-in-data-is-still-difficult-and-necessary-e0982fd7416c&user=TDS+Editors&userId=7e12c71dfa81&source=-----e0982fd7416c---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe0982fd7416c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fconfronting-bias-in-data-is-still-difficult-and-necessary-e0982fd7416c&source=-----e0982fd7416c---------------------bookmark_footer-----------)

年复一年，数据集变得越来越大，云服务器运行得更快，分析工具也变得更加复杂。尽管不断取得进展，实践者们仍然面临偏见的问题——无论是隐藏在数据文件的阴暗角落里，出现在模型的输出中，还是影响着项目的根本假设。

对偏见的终极解决方案将需要远超于对数据团队工作流程的局部改动；期望战术性的修复能解决深层次的系统性问题是不现实的。然而，随着这一问题在科技及其他领域得到越来越多的关注，这确实是一种可以共同思考、讨论和解决的问题，令人感到希望。

本周，我们重点介绍了几篇以创造性、可操作和引发思考的方式，讨论偏见和数据（以及数据中的偏见）的文章。

+   [**你可能遇到的不同类型的偏见**](https://medium.com/towards-data-science/an-unbiased-guide-to-bias-in-ai-3841c2b36165)**。** 对于第一次探讨这个主题的任何人，[Shahrokh Barati](https://medium.com/u/67c421235d57?source=post_page-----e0982fd7416c--------------------------------)的入门读物是了解统计偏见和伦理偏见之间差异的重要读物：“两种不同类别的偏见，具有不同的根本原因和缓解措施”，如果不加以解决，每种偏见都可能危及数据项目（并伤害最终用户）。

+   **向你的反偏见工具包中添加一个强大的策略**。在机器学习模型投入生产后，它们会继续发展，因为团队会调整模型以优化性能。每一个微调都可能为偏见的渗入提供机会——这就是为什么[Jazmia Henry](https://medium.com/u/23c2e80e732a?source=post_page-----e0982fd7416c--------------------------------)倡导采用模型版本控制，这种方法“允许模型回滚，这不仅可以从长远来看为公司节省资金，更重要的是，当偏见出现时，有助于减少偏见。”

+   **谁在塑造语言模型输出的政治性？** 聊天机器人快速融入我们的日常生活，引发了关于其客观性的问题。[Yennie Jun](https://medium.com/u/12ca1ab81192?source=post_page-----e0982fd7416c--------------------------------)尝试测量 GPT-3 输出的政治倾向；她报告的引人入胜的结果提出了一系列关于训练和设计这些强大模型的人员责任和透明度的问题。

![](img/e4838a584869faf78824459399b36e80.png)

图片来源：[Charlotte Harrison](https://unsplash.com/it/@charlottelharrison?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

+   **数据偏差如何成为生死攸关的问题**。当我们想到数据科学和 ML 可以产生重大影响的领域时，医疗保健是一个常见的例子，许多实际应用已经在使用（或接近使用）。然而，[Stefany Goradia](https://medium.com/u/36e97dd8343d?source=post_page-----e0982fd7416c--------------------------------)展示了，健康数据科学家所依赖的数据集可能充满了各种形式的偏差，因此了解如何正确识别这些偏差至关重要。

+   **对 AI 系统中偏差如何运作的更深入理解**。为了完善我们的选择，我们推荐阅读[Boris Ruf](https://medium.com/u/ed341456850c?source=post_page-----e0982fd7416c--------------------------------)对模型内部工作原理的清晰解释——统计公式以及所有细节！—以及它们的设计如何使其易于产生偏差输出。

对于那些希望在接下来的几天里扩展到其他主题的人——从 A/B 测试到自然语言处理——我们很高兴分享一些我们最近的最爱。请享用！

+   准备深入了解关于 ChatGPT 的对话了吗？我们的二月版特刊中包含了你能找到的一些对这种无处不在的聊天机器人的最深思熟虑的写作。

+   我们很高兴分享一篇由[Shreya Rao](https://medium.com/u/99b63de2f2c3?source=post_page-----e0982fd7416c--------------------------------)撰写的回归基础的文章，聚焦于基本的 ML 概念，如梯度下降和线性回归。

+   听到那些恰巧是数据专家的作家讨论他们的创作总是很有趣；[Parul Pandey](https://medium.com/u/7053de462a28?source=post_page-----e0982fd7416c--------------------------------)的新访谈， featuring Lewis Tunstall（他在去年写了一本关于 NLP 和变换器的书）也不例外。

+   通过为你的 A/B 测试游戏添加贝叶斯风味，将其提升到新水平 — [Matteo Courthoud](https://medium.com/u/666130fb420f?source=post_page-----e0982fd7416c--------------------------------)的实用教程展示了如何实现这一目标。

+   [Chayma Zatout](https://medium.com/u/f7da1c34b82e?source=post_page-----e0982fd7416c--------------------------------)的最新深入探讨是对神经网络的耐心介绍，引导我们通过在 Python 中解决分类问题的方案。

我们希望你能考虑[成为 Medium 会员](https://bit.ly/tds-membership) — 这是支持我们发布工作的最直接和有效的方法。

直到下一个变量，

TDS 编辑
