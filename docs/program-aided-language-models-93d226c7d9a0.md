# 程序辅助语言模型

> 原文：[`towardsdatascience.com/program-aided-language-models-93d226c7d9a0?source=collection_archive---------2-----------------------#2023-08-22`](https://towardsdatascience.com/program-aided-language-models-93d226c7d9a0?source=collection_archive---------2-----------------------#2023-08-22)

## LLMs 可以编写代码，但如果它们能执行程序呢？

[](https://wolfecameron.medium.com/?source=post_page-----93d226c7d9a0--------------------------------)![Cameron R. Wolfe, Ph.D.](https://wolfecameron.medium.com/?source=post_page-----93d226c7d9a0--------------------------------)[](https://towardsdatascience.com/?source=post_page-----93d226c7d9a0--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----93d226c7d9a0--------------------------------) [Cameron R. Wolfe, Ph.D.](https://wolfecameron.medium.com/?source=post_page-----93d226c7d9a0--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F28aa6026c553&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fprogram-aided-language-models-93d226c7d9a0&user=Cameron+R.+Wolfe%2C+Ph.D.&userId=28aa6026c553&source=post_page-28aa6026c553----93d226c7d9a0---------------------post_header-----------) 发布于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----93d226c7d9a0--------------------------------) ·18 分钟阅读·2023 年 8 月 22 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F93d226c7d9a0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fprogram-aided-language-models-93d226c7d9a0&user=Cameron+R.+Wolfe%2C+Ph.D.&userId=28aa6026c553&source=-----93d226c7d9a0---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F93d226c7d9a0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fprogram-aided-language-models-93d226c7d9a0&source=-----93d226c7d9a0---------------------bookmark_footer-----------)![](img/3947104a20288bfb49a2b532e471fde9.png)

(照片由[Florian Olivo](https://unsplash.com/@florianolv?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)提供，拍摄于[Unsplash](https://unsplash.com/photos/4hbJ-eymZ1o?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText))

尽管大型语言模型（LLMs）被用于各种应用，但它们通常难以解决基于推理的任务。随着[思维链](https://cameronrwolfe.substack.com/p/chain-of-thought-prompting-for-llms)和[最少到最多提示](https://cameronrwolfe.substack.com/i/116166267/variants-of-cot-prompting)等提示技术的出现，这一问题显著减少。从高层次来看，这些技术通过在模型的提示中提供问题解决的例子，来鼓励 LLMs 进行推理行为。然后，模型可以学习输出这些推理，并为潜在问题提供逐步解决方案。值得注意的是，这是一种仅依赖提示的方法，不需要微调，表明只要有足够上下文的提示，LLMs 是有能力进行推理的。

尽管像思维链提示这样的技术很有效，但 LLM 仍然需要生成问题解决的思维链和最终答案。有趣的是，这种方法会导致一些特殊的失败案例，其中 LLM 可能生成一个准确的解决问题的推理，但仍然会产生一个错误的答案。通常，这些错误是由于简单的错误（例如，计算不准确）造成的。为了解决这个问题，近期研究探索了一种程序化的方法，鼓励 LLM 生成自然语言和代码的思维链……
