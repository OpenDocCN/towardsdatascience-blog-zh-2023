# 如何利用开源 LLM 在大型计算项目中实现巨额节省

> 原文：[`towardsdatascience.com/how-i-leveraged-open-source-llms-to-achieve-massive-savings-on-a-large-compute-project-bd8bb3c7267`](https://towardsdatascience.com/how-i-leveraged-open-source-llms-to-achieve-massive-savings-on-a-large-compute-project-bd8bb3c7267)

## 利用开源 LLM 和 GPU 租赁实现大型计算项目的成本效益。

[](https://medium.com/@ryanshrott?source=post_page-----bd8bb3c7267--------------------------------)![Ryan Shrott](https://medium.com/@ryanshrott?source=post_page-----bd8bb3c7267--------------------------------)[](https://towardsdatascience.com/?source=post_page-----bd8bb3c7267--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----bd8bb3c7267--------------------------------) [Ryan Shrott](https://medium.com/@ryanshrott?source=post_page-----bd8bb3c7267--------------------------------)

·发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----bd8bb3c7267--------------------------------) ·阅读时间 6 分钟·2023 年 8 月 30 日

--

![](img/83382f63b21df0de2b0d47d33ad7212d.png)

图片由 [Alexander Grey](https://unsplash.com/@sharonmccutcheon?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 介绍

在大型语言模型（LLM）的世界中，计算成本可能是一个重大障碍，尤其是在大型项目中。我最近开始了一个需要运行 4,000,000 个提示的项目，平均输入长度为 1000 个 tokens，平均输出长度为 200 个 tokens。这接近 50 亿 tokens！传统的按 token 计费的方法（如 GPT-3.5 和 GPT-4 所用的）会导致高额账单。然而，我发现通过利用开源 LLM，我可以将定价模型转变为按小时计算计算时间，从而节省大量成本。本文将详细说明我采取的方法，并对每种方法进行比较。请注意，虽然我分享了定价经验，但这些费用可能会变化，并可能因您的地区和具体情况而有所不同。关键点是，利用开源 LLM 和按小时租赁 GPU 的成本节省潜力，而不是具体的报价。

## ChatGPT API

我在一个小数据子集上使用了 GPT-3.5 和 GPT-4 进行了初步测试。两个模型都表现出色，但 GPT-4 在大多数情况下始终优于 GPT-3.5。为了让你了解成本，通过 Open AI API 运行全部 400 万个提示的成本大致如下：

![](img/35209c957d0dc0f7f6be806ff60f6cd8.png)

运行 400 万个提示的总成本，输入长度为 1000 个 tokens，输出长度为 200 个 tokens

虽然 GPT-4 提供了一些性能上的好处，但相较于其对我的输出所增加的增量性能，成本却过高。相比之下，尽管 GPT-3.5 Turbo 更加实惠，但在性能上有所欠缺，在 2–3% 的提示输入中出现明显错误。鉴于这些因素，我不愿意在一个本质上是个人项目的项目上投资 $7,600。

## 开源模型

开源模型来救援！使用开源模型，定价模式非常不同：**你只需按计算时间的小时收费**。因此，你的目标是最大化每小时的计算迭代次数。此外，像 Petals.ml 这样的解决方案，你可以免费运行你的计算（有一定限制）！

在尝试了 Hugging Face 上的各种模型后，我发现一个名为 [Stable Beluga 2](https://huggingface.co/stabilityai/StableBeluga2) 的 LLama-2 70B 微调版本给了我很好的性能，无需任何微调！它的表现优于 GPT-3.5 Turbo，但略低于 GPT-4。然而，这个模型仍然非常庞大，需要非常强大的 GPU。通常，最佳实践是使用尽可能小的模型来高效完成任务。因此，我尝试了 Stable Beluga 2 的 7B 版本。性能仍然不够好！

## 微调

为了提升任务性能，我使用了 GPT-4 和 [Petals.ml](https://github.com/bigscience-workshop/petals)（Stable Beluga 2 70B）来生成微调数据集。我通过 Petals.ml 生成了 25K 个提示完成对，并通过 GPT-4 API 生成了 2K 个。请注意，Petals.ml 允许你使用 bit-torrent 技术免费运行开源 LLM 模型。然而，推理时间并不理想：进行 4mm 迭代需要超过一年。

此外，Petals.ml 免费托管了这个模型。因此，我利用 Petals.ml 生成了 25,000 个提示完成对。 我还使用了 GPT-4 API 生成了额外的 2K 个提示完成对。事后看来，我可能在训练数据上做得过头了。我阅读过报告显示，少至 500 个微调样本就足以提升 LLM 的性能。无论如何，这里是使用 Open AI API 运行 2K 提示的总成本：

![](img/80c052ebe7c81d606c55085777984208.png)

运行 2K 个提示完成对的总成本

没错：$84。你能猜到接下来发生了什么吗？使用这 27K 个提示完成对，我对最小的 LLama 2 7B 进行了微调，结果，这个新模型在我的用例中表现极其出色。总微调成本仅为 $6.6，因为我租用了一个 A100 GPU 6 小时。**最终，我微调后的模型表现优于 GPT-3.5，略逊于 GPT-4。**

## 推理

现在让我们谈谈推断计算成本。我的模型至少需要 20GB 的 VRAM，因此我们至少需要 RTX 3090。目前，有四个很好的 GPU 租赁选项：AWS、LambdaLabs、[RunPod](https://runpod.io/?ref=lemrt56t)和[Vast.AI](https://cloud.vast.ai/?ref_id=79595)。根据我的经验，Vast.AI 的价格最好，但可靠性最差。AWS 非常可靠，但比其他选项更贵。RunPod 价格优越，用户界面也很好。我个人想尽量降低成本，所以决定使用 Vast.AI 进行这个项目。Vast.AI 是一个点对点 GPU 租赁服务，因此它可能也是最不安全的。经过在 Vast.AI 上寻找最佳性价比的服务器后，以下是我找到的最佳选项：

![](img/387407f56c236b653480400d7dc503f7.png)

Vast.AI 使用微调 LLama2 7b 模型的总项目成本。总成本是通过每秒的迭代次数、GPU 租赁的每小时价格和所需的总迭代次数来计算的。

如上所示，推断的总成本可以低于$99。在我的案例中，我启动了大约 10 台服务器，并计算了每台服务器每秒能产生的迭代次数。对于这个特别的项目，利用了微调的 Llama2–7B 变体，RTX A5000 在性价比方面是一个很好的选择。我还应该提到，如果没有开源库*[VLLM](https://github.com/vllm-project/vllm)*的**极快速度**，这一切都不可能实现。我的计算完全依赖于 VLLM 的支持。**没有 VLLM，计算时间将增加 20 倍！** 你也可以看看总运行时间并发笑——638 小时就是 26 天；然而，你可以轻松地在多个 GPU/服务器之间并行处理任务。

## 结论

总之，利用开源 LLM 以及将定价模式从按令牌计费改为按计算时间计费，使得在这个大型计算项目中实现了显著的成本节省。总成本，包括提示生成、微调和推断，仅为$189.58。这与使用像 GPT-4 或 GPT-3.5 Turbo 这样的模型的费用形成了鲜明对比，后者的费用分别为$167,810 和$7410.42。

这个项目的主要收获是利用开源 LLM 可以显著降低成本。通过按小时租用 GPU，费用与计算时间挂钩，而不是提示的数量，这可以带来巨大的节省，尤其是对于大型项目。这种方法不仅使这些项目在财务上更具可行性，而且为个人和小型组织提供了承担大型计算项目的机会，而不会产生过高的费用。

这个项目的成功突显了开源模型在大型语言模型和 AI 领域的价值与潜力，证明了创新且具有成本效益的解决方案在解决复杂计算任务中的力量。

## 定价说明

本文提供的定价信息基于我的个人经验，旨在作为一般比较。价格可能会根据你所在的地区和具体情况有所不同。**关键点是利用开源 LLM 和按小时租赁 GPU 的潜在成本节省，而不是具体的报价。**

# 📢 嗨！如果你觉得这篇文章有帮助，请考虑：

👏 鼓掌 50 次（这很有帮助！）

✏️ 留下评论

🌟 突出你觉得有洞察力的部分

👣 关注我

有任何问题吗？🤔 请随时提问。以这种方式支持我是一种免费且简单的方式来感谢我的详细教程文章！😊

**GPU 租赁推荐：**

最佳选择：[RunPod.io GPU 租赁](https://runpod.io/?ref=lemrt56t)

最便宜：[Vast.AI](https://cloud.vast.ai/?ref_id=79595)

**其他深度学习博客**

[高效服务开源 LLM](https://medium.com/p/5f0bf5d8fd59)

[Andrew Ng 的计算机视觉 — 11 节课学到的知识](https://medium.com/towards-data-science/efficiently-serving-open-source-llms-5f0bf5d8fd59)

Andrew Ng 的深度学习专修课程 — 21 节课学到的知识
