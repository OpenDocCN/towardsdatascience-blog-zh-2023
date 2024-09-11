# LLaMA: 适合每个人的LLM！

> 原文：[https://towardsdatascience.com/llama-llms-for-everyone-724e737835be?source=collection_archive---------1-----------------------#2023-07-11](https://towardsdatascience.com/llama-llms-for-everyone-724e737835be?source=collection_archive---------1-----------------------#2023-07-11)

## 高性能的开源语言模型……

[](https://wolfecameron.medium.com/?source=post_page-----724e737835be--------------------------------)[![Cameron R. Wolfe, Ph.D.](../Images/52bb88d7cf1105501be2fae5ccbe7a03.png)](https://wolfecameron.medium.com/?source=post_page-----724e737835be--------------------------------)[](https://towardsdatascience.com/?source=post_page-----724e737835be--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----724e737835be--------------------------------) [Cameron R. Wolfe, Ph.D.](https://wolfecameron.medium.com/?source=post_page-----724e737835be--------------------------------)

·

[查看](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F28aa6026c553&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fllama-llms-for-everyone-724e737835be&user=Cameron+R.+Wolfe%2C+Ph.D.&userId=28aa6026c553&source=post_page-28aa6026c553----724e737835be---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----724e737835be--------------------------------) ·15 min read·2023年7月11日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F724e737835be&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fllama-llms-for-everyone-724e737835be&user=Cameron+R.+Wolfe%2C+Ph.D.&userId=28aa6026c553&source=-----724e737835be---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F724e737835be&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fllama-llms-for-everyone-724e737835be&source=-----724e737835be---------------------bookmark_footer-----------)![](../Images/f673f8c76d03117a2c38c912c91cd911.png)

(照片由 [Raspopova Marina](https://unsplash.com/@raspopovamarisha?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来自 [Unsplash](https://unsplash.com/s/photos/llama?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText))

多年来，深度学习社区一直拥抱开放和透明，导致了像 [HuggingFace](https://huggingface.co/) 这样的巨大开源项目。许多深度学习领域最深刻的想法（例如，[transformers](https://newsletter.artofsaience.com/p/vision-transformers-from-idea-to) [2]，[自监督学习](https://cameronrwolfe.substack.com/i/76273144/self-supervised-learning) 等）都在网上开放获取，既有 [公共代码库](https://huggingface.co/docs/transformers/v4.27.2/en/model_doc/encoder-decoder#transformers.EncoderDecoderModel)，也有 [Arxiv](https://arxiv.org/list/cs.AI/recent)。虽然开源已成为常态，但大规模语言模型（LLMs）的流行（和商业应用）最近挑战了这一趋势。

许多现有的最强大语言模型（LLMs）只能通过 API 访问（例如，来自 [OpenAI](https://openai.com/blog/openai-api) 或 [Anthropic](https://console.anthropic.com/docs/api#accessing-the-api)），这使得研究人员和开发者无法访问源代码和模型参数。虽然我的目标不是引发关于 LLM 领域当前趋势的 [伦理讨论](https://futureoflife.org/open-letter/pause-giant-ai-experiments/)，但这些信息与本文主题相关：开放获取的 LLM。令人有趣的是，并不是所有强大的语言基础模型都隐藏在付费墙后。一些模型，如 LLaMA，既是开放可用的，又表现出色，从而在深度学习研究社区中保持了一种开放性。

**什么是 LLaMA？** LLaMA 不是单一的模型，而是一系列参数从 70 亿到 650 亿的 LLM。受到启发于……
