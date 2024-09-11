# 提示集使大型语言模型更可靠

> 原文：[`towardsdatascience.com/prompt-ensembles-make-llms-more-reliable-ae57ec35b5f7?source=collection_archive---------1-----------------------#2023-08-14`](https://towardsdatascience.com/prompt-ensembles-make-llms-more-reliable-ae57ec35b5f7?source=collection_archive---------1-----------------------#2023-08-14)

## 从任何语言模型中获取最大收益的简单策略…

[](https://wolfecameron.medium.com/?source=post_page-----ae57ec35b5f7--------------------------------)![Cameron R. Wolfe, Ph.D.](https://wolfecameron.medium.com/?source=post_page-----ae57ec35b5f7--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ae57ec35b5f7--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----ae57ec35b5f7--------------------------------) [Cameron R. Wolfe, Ph.D.](https://wolfecameron.medium.com/?source=post_page-----ae57ec35b5f7--------------------------------)

·

[阅读](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F28aa6026c553&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fprompt-ensembles-make-llms-more-reliable-ae57ec35b5f7&user=Cameron+R.+Wolfe%2C+Ph.D.&userId=28aa6026c553&source=post_page-28aa6026c553----ae57ec35b5f7---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ae57ec35b5f7--------------------------------) ·18 分钟阅读·2023 年 8 月 14 日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fae57ec35b5f7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fprompt-ensembles-make-llms-more-reliable-ae57ec35b5f7&source=-----ae57ec35b5f7---------------------bookmark_footer-----------)![](img/917f4253782589036edfaf468a0ea247.png)

（照片由 [Manuel Nägeli](https://unsplash.com/@gwundrig?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/photos/7CcPLtywRso?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄）

任何使用过大型语言模型（LLMs）的人都知道，提示工程是一个非正式且困难的过程。对提示的小变化可能会导致模型输出的大幅变化，难以（甚至在某些情况下是不可能的）预测改变提示会产生什么影响，而且提示行为高度依赖于所使用的模型类型。当我们考虑使用 LLMs 创建应用程序时，提示工程的脆弱性是一种严峻的现实。如果我们无法预测模型的行为，*我们如何围绕这个模型构建一个可靠的系统？* 尽管 LLMs 非常强大，但这一问题使得它们在许多实际场景中的应用变得复杂。

> “提示是一个脆弱的过程，其中对提示的小修改可能会导致模型预测的巨大变化，因此需要投入大量精力来设计一个极其完美的任务提示。” *— 摘自 [2]*

鉴于 LLMs 的脆弱性，寻找使这些模型更准确可靠的技术最近已成为一个热门研究主题。在本综述中，我们将特别关注一种技术 —— *提示集成。* 简而言之，提示集成就是一组多样化的…
