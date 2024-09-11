# 介绍 KeyLLM — 使用 LLM 进行关键词提取

> 原文：[https://towardsdatascience.com/introducing-keyllm-keyword-extraction-with-llms-39924b504813?source=collection_archive---------0-----------------------#2023-10-05](https://towardsdatascience.com/introducing-keyllm-keyword-extraction-with-llms-39924b504813?source=collection_archive---------0-----------------------#2023-10-05)

## 使用 KeyLLM、KeyBERT 和 Mistral 7B 提取关键词

[](https://medium.com/@maartengrootendorst?source=post_page-----39924b504813--------------------------------)[![Maarten Grootendorst](../Images/58e24b9cf7e10ff1cd5ffd75a32d1a26.png)](https://medium.com/@maartengrootendorst?source=post_page-----39924b504813--------------------------------)[](https://towardsdatascience.com/?source=post_page-----39924b504813--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----39924b504813--------------------------------) [Maarten Grootendorst](https://medium.com/@maartengrootendorst?source=post_page-----39924b504813--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F22405c3b2875&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroducing-keyllm-keyword-extraction-with-llms-39924b504813&user=Maarten+Grootendorst&userId=22405c3b2875&source=post_page-22405c3b2875----39924b504813---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----39924b504813--------------------------------) ·9 min read·2023年10月5日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F39924b504813&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroducing-keyllm-keyword-extraction-with-llms-39924b504813&user=Maarten+Grootendorst&userId=22405c3b2875&source=-----39924b504813---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F39924b504813&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroducing-keyllm-keyword-extraction-with-llms-39924b504813&source=-----39924b504813---------------------bookmark_footer-----------)![](../Images/47514c37ae551ce79a535285a4d4488a.png)

大型语言模型（LLMs）正在变得越来越小、速度更快、效率更高。直到我开始考虑将它们用于迭代任务，例如关键词提取。

在创建了 [KeyBERT](https://github.com/MaartenGr/KeyBERT) 后，我感觉是时候扩展这个包，加入 LLMs。它们非常强大，我希望为当这些模型可以在更小的 GPU 上运行时做好准备。

因此，介绍了 `[KeyLLM](https://maartengr.github.io/KeyBERT/guides/keyllm.html)`，这是对 KeyBERT 的一个扩展，允许你使用任何 LLM 来提取、创建，甚至微调关键词！

在本教程中，我们将使用最近发布的Mistral 7B模型进行与`[KeyLLM](https://maartengr.github.io/KeyBERT/guides/keyllm.html)`的关键词提取。

我们将从安装一系列在本示例中将要使用的包开始。

```py
pip install --upgrade git+https://github.com/UKPLab/sentence-transformers
pip install keybert ctransformers[cuda]
pip install --upgrade git+https://github.com/huggingface/transformers
```

我们正在从主分支安装`sentence-transformers`，因为它修复了社区检测的问题，我们将在最后几个用例中使用该功能。我们对`transformers`也做了相同的操作，因为它尚不支持Mistral…
