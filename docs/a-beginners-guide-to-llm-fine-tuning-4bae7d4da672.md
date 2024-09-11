# LLM 微调初学者指南

> 原文：[`towardsdatascience.com/a-beginners-guide-to-llm-fine-tuning-4bae7d4da672?source=collection_archive---------1-----------------------#2023-08-30`](https://towardsdatascience.com/a-beginners-guide-to-llm-fine-tuning-4bae7d4da672?source=collection_archive---------1-----------------------#2023-08-30)

## 如何使用一种工具微调 Llama 和其他 LLMs

[](https://medium.com/@mlabonne?source=post_page-----4bae7d4da672--------------------------------)![Maxime Labonne](https://medium.com/@mlabonne?source=post_page-----4bae7d4da672--------------------------------)[](https://towardsdatascience.com/?source=post_page-----4bae7d4da672--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----4bae7d4da672--------------------------------) [Maxime Labonne](https://medium.com/@mlabonne?source=post_page-----4bae7d4da672--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fdc89da634938&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-beginners-guide-to-llm-fine-tuning-4bae7d4da672&user=Maxime+Labonne&userId=dc89da634938&source=post_page-dc89da634938----4bae7d4da672---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----4bae7d4da672--------------------------------) ·8 分钟阅读·2023 年 8 月 30 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F4bae7d4da672&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-beginners-guide-to-llm-fine-tuning-4bae7d4da672&user=Maxime+Labonne&userId=dc89da634938&source=-----4bae7d4da672---------------------clap_footer-----------)

--

[无](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F4bae7d4da672&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-beginners-guide-to-llm-fine-tuning-4bae7d4da672&source=-----4bae7d4da672---------------------bookmark_footer-----------)![](img/3cd56f68c14e07ab9ae3eb624bd064ed.png)

图片由作者提供

对大型语言模型（LLMs）的兴趣日益增长，导致了**旨在简化其训练过程的工具和封装的激增**。

流行的选择包括 LMSYS 的[FastChat](https://github.com/lm-sys/FastChat)（用于训练[Vicuna](https://huggingface.co/lmsys/vicuna-13b-v1.5)）和 Hugging Face 的[transformers](https://github.com/huggingface/transformers)/[trl](https://github.com/huggingface/trl)库（用于我之前的文章）。此外，每个大型 LLM 项目，如[WizardLM](https://github.com/nlpxucan/WizardLM/tree/main)，往往都有自己的训练脚本，灵感来自原始的[Alpaca](https://github.com/tatsu-lab/stanford_alpaca)实现。

在本文中，我们将使用由 OpenAccess AI Collective 创建的[**Axolotl**](https://github.com/OpenAccess-AI-Collective/axolotl)工具。我们将利用它在一个包含 1,000 个 Python 代码样本的 evol-instruct 数据集上微调一个[**Code Llama 7b**](https://github.com/OpenAccess-AI-Collective/axolotl/blob/main/examples/llama-2/qlora.yml)模型。

# 🤔 为什么选择 Axolotl？

Axolotl 的主要吸引力在于它提供了一站式解决方案，包括众多功能、模型架构和一个活跃的社区。以下是我最喜欢的一些特点：

+   **配置**：用于训练 LLM 的所有参数都整齐地存储在一个 yaml 配置文件中。这使得共享和重现模型变得非常方便。你可以查看 Llama 2 的示例[在这里](https://github.com/OpenAccess-AI-Collective/axolotl/tree/main/examples/llama-2)。

+   **数据集灵活性**：Axolotl 允许指定多个数据集，并支持各种提示格式，例如……
