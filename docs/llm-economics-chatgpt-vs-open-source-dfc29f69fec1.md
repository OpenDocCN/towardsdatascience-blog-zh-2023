# LLM 经济学：ChatGPT 与开源

> 原文：[`towardsdatascience.com/llm-economics-chatgpt-vs-open-source-dfc29f69fec1`](https://towardsdatascience.com/llm-economics-chatgpt-vs-open-source-dfc29f69fec1)

## 部署像 ChatGPT 这样的 LLM 成本是多少？开源 LLM 部署是否更便宜？有哪些权衡？

[](https://skanda-vivek.medium.com/?source=post_page-----dfc29f69fec1--------------------------------)![Skanda Vivek](https://skanda-vivek.medium.com/?source=post_page-----dfc29f69fec1--------------------------------)[](https://towardsdatascience.com/?source=post_page-----dfc29f69fec1--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----dfc29f69fec1--------------------------------) [Skanda Vivek](https://skanda-vivek.medium.com/?source=post_page-----dfc29f69fec1--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----dfc29f69fec1--------------------------------) ·阅读时间 6 分钟·2023 年 4 月 26 日

--

![](img/fa975d94faf37f22c807696c80da0324.png)

比较 LLM 成本的卡通示意图 | Skanda Vivek

TLDR: 对于每天请求数在 1000 个左右的低使用量，ChatGPT 的成本低于使用部署在 AWS 上的开源 LLM。对于每天请求数在百万级别的高使用量，部署在 AWS 上的开源模型成本更低。（截至撰写本文日期 2023 年 4 月 24 日。）

大语言模型正在席卷全球。变压器模型于 2017 年推出，随后出现了突破性的模型如 BERT、GPT 和 BART——拥有数亿参数；并能够执行多种语言任务，如情感分析、问答、分类等。

几年前——来自 OpenAI 和 Google 的研究人员记录了多篇论文，显示具有超过 10 亿参数的大语言模型开始展示出突现能力，它们似乎能够理解语言的复杂方面，并且在回答时几乎像人类一样。

![](img/f39567219981d5f1f495ab8bd9f9174f.png)

[GPT-3 论文](https://arxiv.org/abs/2005.14165)展示了大语言模型令人印象深刻的学习能力。

GPT-3 论文展示了参数超过 10-100 亿的模型在仅有几十个提示的情况下展现了令人印象深刻的学习能力。

然而，这些 LLM 的资源消耗非常大，以至于在大规模部署时经济上具有挑战性。直到最近 ChatGPT 的出现才改变了这种状况。ChatGPT 界面发布后不久，OpenAI 使 ChatGPT API 可用，以便开发者可以在他们的应用中使用 ChatGPT。让我们看看这些大规模部署的成本以及经济可行性。

# ChatGPT API 成本

ChatGPT API 按使用量计费。每 1K 个 token 的费用为 $0.002。每个 token 大约是一个单词的 3/4 —— 单次请求中的 token 数量是提示 + 生成的输出 token 的总和。假设你每天处理 1000 个小文本块，每个文本块为一页 —— 即 500 个单词或 667 个 tokens。**这将花费 $0.002/1000x667*1000= ~$1.3 一天。** 还不错！

但是，如果你每天处理一百万个这样的文档会发生什么？**那么，每天的费用为 $1,300 或每年 ~0.5 百万美元！** ChatGPT 从一个有趣的玩具变成了一个主要的开支（因此希望 —— 也是一个主要的收入来源）在一个数百万美元的业务中！

# 开源生成模型

在 ChatGPT 发布之后，出现了一些开源的倡议。Meta 发布了 [LLaMA——一个拥有数十亿参数的 LLM 模型](https://ai.facebook.com/blog/large-language-model-llama-meta-ai/)，其性能优于 GPT-3。斯坦福大学随后对 LLaMA 的 7B 版本进行了 52K 指令跟随演示的微调，发现 [他们的 Alpaca 模型](https://crfm.stanford.edu/2023/03/13/alpaca.html) 优于 GPT-3。

[最近有研究团队展示了一种 13B 参数的微调 LLaMA 模型，称为 Vicuna，其质量达到了 ChatGPT 的 >90%](https://vicuna.lmsys.org/)。公司可能选择使用开源生成模型而非 OpenAI 的 GPT 系列模型的原因有多个，包括对 OpenAI 故障的较低敏感性、定制更为容易，以及可能更便宜。

虽然开源模型免费使用，但托管和部署它们的基础设施却不是。而且，早期的 transformer 模型如 BERT 可以在配备良好的 CPU 和基础 GPU 的个人计算机上轻松运行和微调，但 LLMs 更为资源密集。一个常见的解决方案是使用云服务提供商如 AWS 来托管和部署这些模型。

让我们深入了解托管开源模型的 AWS 成本。

## AWS 成本

首先，让我们讨论在 AWS 上部署模型并将其作为 API 提供服务的标准架构。通常有三个步骤：

1.  使用 AWS Sagemaker 将模型部署为端点。

1.  将 Sagemaker 端点连接到 AWS Lambda。

1.  通过 API 网关将 Lambda 函数作为 API 提供服务

![](img/f6023397d6b436e188b548f74e4fc97a.png)

[使用 API 网关和 Lambda 调用 Sagemaker 模型端点](https://aws.amazon.com/blogs/machine-learning/call-an-amazon-sagemaker-model-endpoint-using-amazon-api-gateway-and-aws-lambda/)

当客户端向 API 网关发起 API 调用时，会触发 Lambda 函数，该函数解析请求并将其发送到 Sagemaker 端点。模型端点随后进行预测，并将信息发送到 Lambda。Lambda 解析这些信息并将其发送到 API，最终返回给客户端。

Sagemaker 的成本对托管模型的计算实例类型非常敏感。LLMs 使用相当大的计算实例。

![](img/b32a9a0d869317c427aa9307917cd4f7.png)

[Sagemaker 实例定价](https://aws.amazon.com/sagemaker/pricing/?p=pm&c=sm&z=2)

例如，这篇由 AWS 的某人撰写的文章详细说明了如何在 AWS 上部署 Flan UL2 —— 一个 200 亿参数的模型：

[](https://betterprogramming.pub/deploy-flan-ul2-on-a-single-gpu-1778dac605f3?source=post_page-----dfc29f69fec1--------------------------------) [## 使用 Amazon SageMaker 在单个 GPU 上部署 Flan-UL2

### Hugging Face + AWS 的合作使得实验开源最先进语言模型变得比以往更加容易……

[betterprogramming.pub](https://betterprogramming.pub/deploy-flan-ul2-on-a-single-gpu-1778dac605f3?source=post_page-----dfc29f69fec1--------------------------------)

文章使用了 ml.g5.4xlarge 实例。虽然上面的 Sagemaker 定价没有列出这个特定实例的价格，但看起来大约在每小时 ~5$ 的范围。这就意味着每天 150$！这只是实例托管的费用，我们还没有计算 Lambda 和 API 网关的费用。

以下是 AWS Lambda 定价的详细信息——这取决于内存使用量和请求频率。

![](img/76c3c94e61983612142278210fbaa61c.png)

[AWS Lambda 定价](https://aws.amazon.com/lambda/pricing/)

假设获取响应需要 5 秒。128 MB 足够了，因为我们将数据路由到 AWS Sagemaker 端点。所以这将花费 5 * .128 * 1000 * $0.0000166667 = 0.01$ 每 1000 次请求，或 10$ 每 1M 次请求。

最终成本是 API 网关的费用：

![](img/bae2fc46e6a3b3c1ae8ba8e38e0c60e5.png)

[AWS API 网关定价](https://aws.amazon.com/api-gateway/pricing/)

如你所见，API 网关相当便宜——每百万次请求 1$。

因此，**在 AWS 上托管开源 LLM，如 Flan-UL2 的成本是每天 1000 次请求 150$，每天 1M 次请求 160$。**

但我们是否总是需要如此昂贵的计算实例？对于像 BERT 这样的较小语言模型（参数数量为亿级）——你可以使用更便宜的实例，比如 **$0.23/小时和 ~5$ 每天**。这些模型也相当强大，并且比那些似乎能够理解复杂语言细微差别的 LLM 更加针对任务和训练数据。

# 总结思考

那么哪个更好？使用像 OpenAI 的 GPT 系列这样的付费服务 LLM？还是开放访问 LLM？这取决于使用场景：

![](img/8da5d346bdd3bdd5e95c61253fdf48cf.png)

付费服务 LLM 的利与弊 | Skanda Vivek

![](img/edbb5072fea5e8b9d77b42a82d1b4cb9.png)

开放访问 LLM 的利与弊 | Skanda Vivek

注意：由于这是一个迅速发展的领域，由于大规模的需求，未来相对较近的时期，部署成本大幅降低的可能性很大。（请记住，虽然托管开源 LLM 是一个挑战，但像 BERT 这样具有数亿参数的小型语言模型在特定任务中仍然是一个很好的选择。我已撰写有关如何通过对 BERT 基础模型进行微调来实现类似人类表现的任务，如 问答系统 和 垃圾邮件检测 的文章。）

但哪个模型更好呢？ChatGPT 和 GPT-4 的回应比开源 LLM 的回应更具相关性。然而，开源模型正在快速赶上。而且，使用开源模型而不是封闭 API 可能有非常好的理由。

公司希望在其特定数据源上对开源模型进行微调。ChatGPT 和后续的 OpenAI 模型可能无法像在领域特定数据上微调的开源模型那样表现良好；由于这些模型的通用性质。我们已经看到像 BloombergGPT 这样的领域特定模型在生成式 AI 方面取得了强大的进展。

哦——我们都祈祷 OpenAI 不会提高 ChatGPT API 的价格。当 ChatGPT API 刚推出时，令人惊讶的是它的定价比早期的 GPT-3 API 便宜了 10 倍。

我们生活在激动人心的时代！

*如果你喜欢这篇文章，请关注我——我撰写关于将最先进的 NLP 应用于现实世界应用的主题，更一般地说，也涉及数据与社会的交集。*

*欢迎通过* [*LinkedIn*](https://www.linkedin.com/in/skanda-vivek-01619311b/)*与我联系！*

*如果你还不是 Medium 的会员，并且想要支持像我这样的作者，欢迎通过我的推荐链接注册：* [*https://skanda-vivek.medium.com/membership*](https://skanda-vivek.medium.com/membership)
