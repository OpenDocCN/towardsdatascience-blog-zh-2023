# 在 Colab Notebook 中微调你自己的 Llama 2 模型

> 原文：[`towardsdatascience.com/fine-tune-your-own-llama-2-model-in-a-colab-notebook-df9823a04a32?source=collection_archive---------0-----------------------#2023-07-25`](https://towardsdatascience.com/fine-tune-your-own-llama-2-model-in-a-colab-notebook-df9823a04a32?source=collection_archive---------0-----------------------#2023-07-25)

## 实用的 LLM 微调入门介绍

[](https://medium.com/@mlabonne?source=post_page-----df9823a04a32--------------------------------)![Maxime Labonne](https://medium.com/@mlabonne?source=post_page-----df9823a04a32--------------------------------)[](https://towardsdatascience.com/?source=post_page-----df9823a04a32--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----df9823a04a32--------------------------------) [Maxime Labonne](https://medium.com/@mlabonne?source=post_page-----df9823a04a32--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fdc89da634938&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffine-tune-your-own-llama-2-model-in-a-colab-notebook-df9823a04a32&user=Maxime+Labonne&userId=dc89da634938&source=post_page-dc89da634938----df9823a04a32---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----df9823a04a32--------------------------------) · 12 分钟阅读 · 2023 年 7 月 25 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fdf9823a04a32&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffine-tune-your-own-llama-2-model-in-a-colab-notebook-df9823a04a32&user=Maxime+Labonne&userId=dc89da634938&source=-----df9823a04a32---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fdf9823a04a32&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffine-tune-your-own-llama-2-model-in-a-colab-notebook-df9823a04a32&source=-----df9823a04a32---------------------bookmark_footer-----------)![](img/deab49c0869889d83573906da9f2c40b.png)

作者提供的图片

随着 LLaMA v1 的发布，我们见证了微调模型的寒武纪大爆发，包括[Alpaca](https://github.com/tatsu-lab/stanford_alpaca)、[Vicuna](https://huggingface.co/lmsys/vicuna-13b-v1.3)和[WizardLM](https://huggingface.co/WizardLM/WizardLM-13B-V1.1)等。这一趋势鼓励不同企业推出适合商业使用的基础模型许可证，如[OpenLLaMA](https://github.com/openlm-research/open_llama)、[Falcon](https://falconllm.tii.ae/)和[XGen](https://github.com/salesforce/xgen)等。Llama 2 的发布现在结合了两者的最佳元素：它提供了**高效的基础模型和更宽松的许可证**。

在 2023 年上半年，**API 的广泛使用**（如 OpenAI API）在基于大语言模型（LLMs）构建基础设施方面发挥了重要作用。诸如[LangChain](https://python.langchain.com/docs/get_started/introduction.html)和[LlamaIndex](https://www.llamaindex.ai/)这样的库在这一趋势中发挥了关键作用。进入下半年，**对这些模型的微调（或指令调优）将成为 LLMOps 工作流程中的标准程序**。这一趋势受到多种因素驱动：节省成本的潜力、处理机密数据的能力，甚至可能开发出在某些特定任务上超越像 ChatGPT 和 GPT-4 这样的知名模型的模型。

在这篇文章中，我们将探讨为什么指令调优有效以及如何在 Google Colab 笔记本中实现它以创建……
