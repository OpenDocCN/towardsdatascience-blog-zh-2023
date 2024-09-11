# 超越 LLaMA：开放 LLM 的力量

> 原文：[`towardsdatascience.com/beyond-llama-the-power-of-open-llms-cef807a54a4f?source=collection_archive---------10-----------------------#2023-07-18`](https://towardsdatascience.com/beyond-llama-the-power-of-open-llms-cef807a54a4f?source=collection_archive---------10-----------------------#2023-07-18)

## LLaMA 如何让开源重新变得酷炫

[](https://wolfecameron.medium.com/?source=post_page-----cef807a54a4f--------------------------------)![Cameron R. Wolfe, Ph.D.](https://wolfecameron.medium.com/?source=post_page-----cef807a54a4f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----cef807a54a4f--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----cef807a54a4f--------------------------------) [Cameron R. Wolfe, Ph.D.](https://wolfecameron.medium.com/?source=post_page-----cef807a54a4f--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F28aa6026c553&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbeyond-llama-the-power-of-open-llms-cef807a54a4f&user=Cameron+R.+Wolfe%2C+Ph.D.&userId=28aa6026c553&source=post_page-28aa6026c553----cef807a54a4f---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----cef807a54a4f--------------------------------) · 18 分钟阅读·2023 年 7 月 18 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fcef807a54a4f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbeyond-llama-the-power-of-open-llms-cef807a54a4f&user=Cameron+R.+Wolfe%2C+Ph.D.&userId=28aa6026c553&source=-----cef807a54a4f---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fcef807a54a4f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbeyond-llama-the-power-of-open-llms-cef807a54a4f&source=-----cef807a54a4f---------------------bookmark_footer-----------)![](img/29a535e5c06be6b564ccfb3b42238ab5.png)

（照片由[Paz Arando](https://unsplash.com/@pazarando?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)拍摄，来源于[Unsplash](https://unsplash.com/s/photos/llama?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)）

尽管大型语言模型（LLMs）最近取得了进展，但许多最强大的模型只能通过 [付费 API](https://console.anthropic.com/docs/api) 访问，并且使用大量 [专有数据](https://openai.com/research/gpt-4) 进行训练，从而限制了研究社区对这些模型的访问或再现。这一趋势引发了对 LLMs 是否会主要被少数几个集中化的团体控制的严重担忧，这些团体迫使其他人付费与这些模型互动。这种情况严格阻止了大多数研究人员直接访问或自行改进 LLMs。

> “[许多] LLMs 训练需要巨大的计算资源，并且通常使用大型且专有的数据集。这表明未来高能力的 LLMs 将主要由少数几个组织控制。” *— 来自 [5]*

鉴于训练和托管 LLMs 的计算负担，我们可能会怀疑开源这些模型是否对研究社区有帮助。如果我们不是拥有广泛计算资源的大型组织的一部分，*我们是否能用 LLMs 做出有用的研究？* 如果不能，也许我们注定要面临一个 LLMs 集中控制和访问的世界。这些模型似乎有太大的“引力”（即需要大量的数据和计算），让大多数人难以轻松操作……
