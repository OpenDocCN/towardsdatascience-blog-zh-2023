# LLMs 的无限巴别图书馆

> 原文：[`towardsdatascience.com/the-infinite-babel-library-of-llms-90e203b2f6b0`](https://towardsdatascience.com/the-infinite-babel-library-of-llms-90e203b2f6b0)

## | 人工智能 | 未来 | 变压器 |
## | --- | --- | --- |

## 开源、数据和关注：LLMs 未来将如何改变

[](https://salvatore-raieli.medium.com/?source=post_page-----90e203b2f6b0--------------------------------)![Salvatore Raieli](https://salvatore-raieli.medium.com/?source=post_page-----90e203b2f6b0--------------------------------)[](https://towardsdatascience.com/?source=post_page-----90e203b2f6b0--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----90e203b2f6b0--------------------------------) [Salvatore Raieli](https://salvatore-raieli.medium.com/?source=post_page-----90e203b2f6b0--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----90e203b2f6b0--------------------------------) ·16 分钟阅读·2023 年 5 月 8 日

--

![](img/662b901cf99f5b6eb661b1c961669369.png)

作者使用 OpenAI DALL-E 创建的图像

“‘[人工智能的教父’ 离开谷歌并警告前方危险](https://www.nytimes.com/2023/05/01/technology/ai-google-chatbot-engineer-quits-hinton.html)” 是纽约时报的标题。如果语言模型没有开源，我们怎么知道它们是否对人类构成威胁？实际上发生了什么？语言模型的世界如何在变化的边缘。

# 号召开源运动

![](img/6d9b96a4ae8fdb8e31a90144a6a87261.png)

图像由 [Nik Shuliahin](https://unsplash.com/it/@tjump) 提供，来自 Unsplash.com

不久前 [GPT-4](https://openai.com/research/gpt-4) 向公众公开，我认为我们都去阅读了技术报告，但感到失望。

![](img/7a0bed6c4021763586fbc5da9961724a.png)

技术报告 GPT-4。由作者截屏，图像来源：[这里](https://arxiv.org/pdf/2303.08774.pdf)

最近，[《自然》也讨论了这个问题](https://www.nature.com/articles/d41586-023-01295-4)：我们需要 [大型语言模型](https://en.wikipedia.org/wiki/Large_language_model) (LLMs) 开源。

许多 LLMs 是专有的，没有发布，我们不知道它们是用什么数据训练的。这不允许对它们进行检查和测试，特别是关于偏见的方面。

此外，与 ChatGPT 分享信息和代码存在泄漏的风险 [如三星所发现](https://gizmodo.com/chatgpt-ai-samsung-employees-leak-data-1850307376)。更不用说，一些州认为 [这些公司的数据存储违反了 GDPR](https://blog.davidlibeau.fr/chatgpt-will-probably-never-comply-with-gdpr/)。

这就是为什么我们需要大语言模型开源，并且应当更多地投资于新大语言模型的开发，比如[BLOOM](https://pub.towardsai.net/a-new-bloom-in-ai-why-the-bloom-model-can-be-a-gamechanger-380a15b1fba7) 联盟（一个由学术联盟开发的 1700 亿参数的大语言模型）。

近年来，经常出现对这些大语言模型实际能力和人工智能风险的夸张报道。如果研究人员无法测试模型，他们就无法真正评估其能力，分析风险也是如此。此外，开源模型要透明得多，社区也可以尝试找出问题行为的源头。

此外，这并不是学术界的要求，机构对人工智能感到警惕。欧盟这些天正在讨论可能重塑 LLM 未来的欧盟人工智能法案。与此同时，[白宫正在施压科技首席执行官](https://www.nytimes.com/2023/05/04/technology/us-ai-research-regulation.html?utm_source=tldrai)以限制人工智能的风险。因此，开源可能实际上成为语言模型的未来要求。

# 为什么 ChatGPT 如此优秀？

![](img/318475e0d08ae720f761191b50879a3f.png)

我们都听说过 ChatGPT，以及它如何看起来具有革命性。但它是如何训练的呢？

[](https://medium.com/data-driven-fiction/everything-but-everything-you-need-to-know-about-chatgpt-546af7153ee2?source=post_page-----90e203b2f6b0--------------------------------) [## 关于 ChatGPT 的一切，你需要知道的一切

### 了解现状，获取最新新闻，了解其影响和变化，一篇文章全涵盖。

medium.com](https://medium.com/data-driven-fiction/everything-but-everything-you-need-to-know-about-chatgpt-546af7153ee2?source=post_page-----90e203b2f6b0--------------------------------)

让我们从一个事实开始，ChatGPT 是基于一种大语言模型（准确地说是 GPT 3.5）进行训练的。通常，这些类似 GPT 的语言模型是通过预测[序列中的下一个标记](https://en.wikipedia.org/wiki/Language_model)进行训练的（从标记序列 w 中，模型必须预测下一个标记 w+1）。

该模型通常是一个变换器：由一个接收输入序列的编码器和一个生成输出序列的解码器组成。这个系统的核心是[多头自注意力机制](https://en.wikipedia.org/wiki/Attention_(machine_learning))，它允许模型学习有关上下文和序列各部分之间依赖关系的信息。

![](img/b2da300060a6991f8bfb378428c961b5.png)

图片来源：[这里](https://arxiv.org/pdf/1706.03762.pdf)

[GPT-3](https://en.wikipedia.org/wiki/GPT-3) 就是采用这种原则进行训练的（像其他生成预训练变换器系列中的模型一样），只不过有更多的参数和数据（570GB 的数据和 1760 亿个参数）。

[GPT3](https://en.wikipedia.org/wiki/GPT-3)具有巨大的能力，但在生成文本时，它经常出现幻觉、缺乏帮助、不可解释，并且常常包含偏见。这意味着模型与我们期望的像人类一样生成文本的模型并不对齐。

> 我们如何从 GPT-3 中获得 ChatGPT？

这个过程称为[来自人类反馈的强化学习](https://huggingface.co/blog/rlhf)（RHLF），作者在这篇文章中进行了描述：

[](https://arxiv.org/abs/2203.02155?source=post_page-----90e203b2f6b0--------------------------------) [## 通过人类反馈训练语言模型以遵循指令

### 让语言模型变得更大并不会自动提高它们遵循用户意图的能力。例如，大型…

[arxiv.org](https://arxiv.org/abs/2203.02155?source=post_page-----90e203b2f6b0--------------------------------)

在这里，我将非常简要地描述它。具体来说，它包括三个步骤：

1.  **监督微调**是第一步，其中 LLM 被微调以学习监督策略（基线模型或 SFT 模型）。

1.  **模仿人类偏好**，在这一步中，注释员必须对基线模型的一组输出进行投票。这些经过整理的数据集用于训练新模型，即奖励模型。

1.  **近端策略优化（PPO）**，在这里，奖励模型用于微调 SFT 模型并获得策略模型。

![](img/3e1261510ad93d65caeb90c9526f8059.png)

图片来源：[这里](https://arxiv.org/pdf/2203.02155.pdf)

为了准备第一步，OpenAI 收集了一系列提示，并要求人类注释员写下预期的响应（12–15K 提示）。其中一些提示来自 GPT-3 用户，因此在 ChatGPT 上写的内容可能会用于下一个模型。

作者使用了作为模型的 GPT-3.5，该模型已经在编程代码上进行了微调，这也解释了 ChatGPT 的代码能力。

然而，这一步并不完全具有可扩展性，因为它是监督学习。无论如何，由此获得的模型尚未对齐。

![](img/da31c4b18f10f3ac161b16a6c788db60.png)

图片来源：[这里](https://arxiv.org/pdf/2203.02155.pdf)

注释员根据 SFT 模型的响应范围进行标注，根据响应的期望程度（从最差到最好）。我们现在拥有一个更大的数据集（10 倍），并将 SFT 模型的响应提供给新模型，新模型必须按偏好顺序进行排名。

在这一阶段，模型正在学习关于数据的一般策略，以及如何最大化其奖励（当它能够很好地排名输出时）。

![](img/74cd14b951a1055a96b3f03687234a47.png)

图片来源：[这里](https://arxiv.org/pdf/2203.02155.pdf)

因此，我们有了 SFT 模型，并使用其权重来初始化一个新的 PPO 模型。该模型使用近端策略优化（PPO）进行微调。

换句话说，我们使用了强化学习算法。PPO 模型接收到一个随机提示并对其做出响应，然后它会收到惩罚或奖励。与经典的 [Q-learning](https://it.wikipedia.org/wiki/Q-learning) 相比，这里模型策略会在每次响应后更新（模型直接从经验中学习，在策略上）。

此外，作者使用每个 token 的 [Kullback-Leibler (KL)](https://en.wikipedia.org/wiki/Kullback%E2%80%93Leibler_divergence) 惩罚，使模型的响应分布类似于 SFT 模型的分布。这是因为我们希望通过 RL（由于奖励模型）优化模型，但我们仍然不希望它忘记在步骤 1 中学到的内容，这些是由人类策划的提示。

最后，该模型在三个方面进行评估：有用性、真实性和无害性。毕竟，这正是我们希望优化的方面。

一个值得注意的事实是，当在经典基准测试（问答、摘要、分类）上评估时，该模型的表现低于 GPT-3。这就是对齐的代价。

# Alpaca，一种革命性的动物

![](img/46194285bf0755a3f0a2eb2df10dab3c.png)

图片由 [Dong Cheng](https://unsplash.com/it/@dongcheng) 在 Unsplash 提供

如前所述，确实需要研究这些模型的行为，只有在它们是开源的情况下才能做到这一点。另一方面，任何语言模型都可以使用 RHLF 进行对齐。

RHLF 比从头开始训练模型要便宜且计算密集度低得多。另一方面，它要求有标注者（你确实需要一个包含指令的数据集）。**但这些步骤不能自动化吗？**

第一步是 [Self-instruct](https://arxiv.org/pdf/2212.10560.pdf)，在这篇 2022 年的文章中，作者提出了一种半自动化的方法。实际上，一般的想法是从一组手动编写的指令开始。这些指令集既作为种子，又确保涵盖了大多数 [NLP](https://en.wikipedia.org/wiki/Natural_language_processing) 任务。

从仅有的 175 个指令开始，模型生成了数据集（50k 指令）。然后，该数据集用于指令调整。

![](img/3614a712dd4be63ad2280e789d181ed2.png)

SELF-INSTRUCT 的高级概览。图片来源：[这里](https://arxiv.org/pdf/2212.10560.pdf)

仅有一个模型需要的方法。ChatGPT 基于 OpenAI GPT-3.5，但难道不能使用更小的模型吗？**它真的需要超过 100 B 的参数吗？**

相反，斯坦福研究人员使用了 LLaMA，特别是 7B 版本，并按照 self-instruct 方法生成了 52 K 指令（使用 OpenAI 的 text-davinci-003 生成的指令）。Alpaca 的真正价值在于作者简化了流程，大大降低了成本，使任何学术实验室都能复制这个过程（在 [这个库](https://github.com/tatsu-lab/stanford_alpaca) 中）。实际上如所述：

> 在我们的初步运行中，微调一个 7B 的 LLaMA 模型在 8 台 80GB 的 A100 上花费了 3 小时，这在大多数云计算提供商上花费不到 100 美元。([source](https://crfm.stanford.edu/2023/03/13/alpaca.html))

初步模型评估显示，Alpaca 的表现几乎与 GPT-3.5 一致（在某些情况下甚至超过了 GPT-3.5）。这可能让人感到惊讶，因为这是一个体积小了 20 倍的模型。另一方面，该模型在一系列输入下表现得像 GPT（因此训练相当于一种知识蒸馏）。另一方面，该模型也具有典型语言模型的相同局限性，表现出幻觉、毒性和刻板印象。

Alpaca 证明了任何学术实验室都可以训练自己的 ChatGPT 版本（使用[ LLaMA](https://medium.com/mlearning-ai/metas-llama-a-small-language-model-beating-giants-5065948e0b7f)，该模型仅供研究使用）。另一方面，任何使用其他模型的公司都可以调整并创建自己的 ChatGPT 版本。此外，类似的模型甚至可以部署在[手机](https://twitter.com/thiteanish/status/1635188333705043969)或[树莓派计算机](https://msproul.github.io/AlpacaPi/)上。

作者发布了一个演示，但在短时间内被[关闭](https://www.theregister.com/2023/03/21/stanford_ai_alpaca_taken_offline/)（出于安全考虑）。此外，尽管使用 LLaMA 需要申请（并访问模型权重），但几天后模型[被泄露到网上](https://www.theverge.com/2023/3/8/23629362/meta-ai-language-model-llama-leak-online-misuse)。

# LLM 是否处于革命的边缘？

![](img/b409e6e6cbb3677fc3eb3fe8c02c99f3.png)

图片来源：[这里](https://arxiv.org/pdf/2304.13712.pdf)

看起来自从 ChatGPT 发布以来已经过了好几年，但实际上仅仅只有几个月。到那个时候，我们在讨论幂律法则，如何让一个模型拥有更多的参数、更多的数据和更多的训练，以便使新兴行为得以出现。

这些想法促成了我们可以为语言模型定义一种[摩尔定律](https://en.wikipedia.org/wiki/Moore%27s_law)的想法。在某种意义上，近年来我们几乎看到了一个指数法则（我们从 GPT-2 的 15 亿参数发展到 GPT-3 的 1750 亿参数）。

> 有什么变化？

对这个理论的第一次冲击可以称之为，[Chinchilla](https://en.wikipedia.org/wiki/Chinchilla_AI)的到来。DeepMind 的模型表明，这不仅仅是数据量的问题，还有数据质量的问题。其次，META 的 LLaMA 表明，即使是使用精心挑选的数据集的小型模型也能取得与大型模型相似甚至更好的结果。

这不仅仅是模型的问题。数据是另一个问题。人类生产的数据不足，可能不够支持任何需要按照幂律法则所需的 GPT-5。其次，数据将不再像以前那样容易获取。

实际上，Reddit（一个受欢迎的数据资源）已宣布[AI 开发者将需付费](https://www.reddit.com/r/reddit/comments/12qwagm/an_update_regarding_reddits_api/)才能访问其内容。即使是[维基百科也有相同的想法](https://voicebot.ai/2021/03/17/wikipedia-will-start-charging-tech-giants-and-their-voice-assistants-for-data-access/)，现在[StackOverflow](https://www.zdnet.com/article/stack-overflow-joins-reddit-and-twitter-in-charging-ai-companies-for-training-data/)也在朝着这个方向发展，它将要求公司支付费用。

> Stack Overflow 的 Chandrasekar 说：“社区平台为 LLMs 提供支持，绝对应该得到补偿，以便像我们这样的公司可以重新投资于我们的社区，使其继续繁荣。”他说：“我们非常支持 Reddit 的做法。” ([来源](https://www.wired.com/story/stack-overflow-will-charge-ai-giants-for-training-data/))

即使有人设法获得数据，对于公司而言，数据的安全性也未必得到保障。[Getty 已经起诉了一款 AI 艺术生成器](https://www.theverge.com/2023/2/6/23587393/ai-art-copyright-lawsuit-getty-images-stable-diffusion)，但艺术家们也提出了诉讼。更不用说，程序员们也对[GitHub Copilot](https://www.euronews.com/culture/2023/03/27/from-lawsuits-to-tech-hacks-heres-how-artists-are-fighting-back-against-ai-image-generatio)提起了诉讼，GitHub Copilot 是基于代码库中的代码进行训练的。此外，音乐行业（以诉讼著称）[对 AI 生成的音乐表示反对](https://www.ft.com/content/aec1679b-5a34-4dad-9fc9-f4d8cdd124b9)，并呼吁反对流媒体服务。如果即使 AI 公司[援引合理使用](https://www.theverge.com/2023/1/28/23575919/microsoft-openai-github-dismiss-copilot-ai-copyright-lawsuit)进行上诉，未来它们是否能继续获得相同的数据访问权限仍然不确定。

除了通过异质模态扩展模型之外，还需考虑另一个因素，自 2017 年起，transformer 架构没有发生变化。所有语言模型都基于这样一种信条：只需多头自注意力机制即可，无需更多。直到最近，Sam Altman 仍然认为架构的可扩展性是 AGI 的关键。但正如他在最近的[MIT 活动](https://www.imaginationinaction.co/)上所说，AGI 的关键不在于更多的层和参数。

![](img/e452bf39507b9ec47868c33d02c1b4a7.png)

图像来源：[这里](https://arxiv.org/pdf/1706.03762.pdf)

transformer 存在明确的局限性，这在语言模型中得到了反映：幻觉、毒性和偏见。现代大型语言模型（LLMs）不具备批判性思维能力。诸如思维链和提示工程等技术作为补丁，试图缓解这些问题。

此外，已经证明，多头自注意力能够解决 RNN 派生的问题，并且允许行为的出现，因为上下文学习具有二次成本。最近，已发现不能用非二次的注意力变体替代自注意力，否则会失去表达力。然而，像 [Spike-GPT](https://levelup.gitconnected.com/spikegpt-a-260-m-only-parameters-lm-not-afraid-of-competition-e262431d67aa) 和 [Hyena](https://levelup.gitconnected.com/welcome-back-80s-transformers-could-be-blown-away-by-convolution-21ff15f6d1cc) 这样的工作表明，存在不基于自注意力的成本较低的替代方案，并且在构建语言模型时可以获得类似的结果。

同样，使用 RHLF 对齐模型在各种任务中的表现也有一定的成本。因此，语言模型不会取代“专家模型”，但未来可能会成为其他模型的协调者（如 [HuggingGPT](https://levelup.gitconnected.com/hugginggpt-give-your-chatbot-an-ai-army-cfadf5647f98) 所建议的）。

# 你无法阻止开源，为什么它总是能胜出

![](img/3a6b25bb1fc139ea22e046a0242924a8.png)

图片由 [Steven Lelham](https://unsplash.com/it/@slelham) 提供

是 MidJourney 还是 DALL-E 更好？也许很难说。可以肯定的是，stable diffusion 是制胜的技术。由于 stable diffusion 是开源的，它催生了许多应用，并且成为了许多衍生研究的灵感（ControlNet、医学成像的合成数据、大脑的类比）。

通过社区的努力，Stable diffusion 在其各种版本中得到了改进，并且有无尽的变体。另一方面，没有一个 DALL-E 的应用没有基于 stable diffusion 的对应物（但反之则不成立）。

> 那么，为什么语言模型没有发生同样的情况呢？

到目前为止，主要问题是训练语言模型是一项费用高昂的任务。BigScience 的 BLOOM 确实是一个巨大的联盟。然而，LLaMA 已经表明，较小的模型也能与超过 100 B 参数的巨型模型竞争。Alpaca 表明，语言模型的对齐也可以以较低的成本（总成本少于 1,000 美元）完成。这些因素使 Simon Willson 能够说“[大型语言模型正在迎来它们的 Stable Diffusion 时刻。](https://simonwillison.net/2023/Mar/11/llama/)”

从 Alpaca 到今天，出现了许多 [开源](https://twitter.com/dctanner/status/1643263959263322115) 模型。不仅 [Stability AI 发布了多个与巨头竞争的模型](https://stability.ai/blog/stability-ai-launches-the-first-of-its-stablelm-suite-of-language-models)，并且可以被所有人使用，而且其他公司也发布了聊天机器人和模型。在短短几周内，我们见证了：[Dolly](https://github.com/databrickslabs/dolly)、[HuggingChat](https://huggingface.co/chat/)、Koala 等众多新模型。

![](img/2dc4d41faad26b91c012f8ac3afb3148.png)

作者提供的截图。图片来源：[这里](https://huggingface.co/chat/)

现在，提到的一些模型确实是开源的，但它们仅用于非商业用途。虽然这些模型开放用于学术研究，但这意味着感兴趣的公司不能利用它们。

这只是故事的一部分。实际上，HuggingFace 上已经有可以轻松训练的模型（模型、数据集和管道），目前已有多个商业化的模型（至今[超过 10 个](https://github.com/eugeneyan/open-llms)）：

![](img/d374e3ba279d7afd606a009bf17ab876.png)

作者提供的截图。来源：[这里](https://github.com/eugeneyan/open-llms)

# 开源模型、私人数据和新应用

![](img/6138c6a0e3c35ffb7b83692a0a099541.png)

图片由[Muhammad Zaqy Al Fattah](https://unsplash.com/it/@dizzydizz)在 Unsplash 提供

Dario Amodei，[Anthropic 的首席执行官正在寻求数十亿资金](https://venturebeat.com/ai/as-anthropic-seeks-billions-to-take-on-openai-industrial-capture-is-nigh-or-is-it/)以击败 OpenAI 的大型模型。然而，世界的其他地方却在朝另一个方向发展。例如，Bloomberg 虽然在 AI 领域并不知名，[已发布了一个金融 LLM](https://arxiv.org/abs/2303.17564v1)（训练于来自金融来源的 3630 亿个标记）。

> 为什么我们需要用于金融的 LLM？为什么不直接使用 ChatGPT？

Google MedPalm 显示，通用模型的表现不如针对特定主题（在此案例中为医学、科学等文章数据集）微调的模型。

微调 LLM 显然成本很高，尤其是当我们谈论参数达到数百亿的模型时。较小的模型成本较低，但仍然不便宜。META 的 LLaMA 作为开源模型在一定程度上解决了这个问题。事实上，[LLaMA-Adapter 的作者展示了仅需增加 1.2 百万参数](https://arxiv.org/pdf/2303.16199.pdf)即可进行微调（训练时间少于一小时）。

尽管 LLaMA 确实不可商业化，但仍有许多其他模型可以使用（从小到大）。显然，在特定领域成功应用的关键是数据。

[正如三星不愉快地发现的](https://www.engadget.com/three-samsung-employees-reportedly-leaked-sensitive-data-to-chatgpt-190221114.html?guccounter=1&guce_referrer=aHR0cHM6Ly93d3cuZ29vZ2xlLmNvbS8&guce_referrer_sig=AQAAAJvMJ0sb8_IHUybp8w3JmFkI5VkTKP1MgcrB9zWTaUKcBrEm8Fs-D18iKE4b9jOIFn_s-1p86ZmF0fG3V-LEFDiHRvBAlqBIDOMFFraTYxYbXzdcBNMfh8ppicFde-u_RCNbZLZGq7cgfor-7u9h-MLCn1cxhNHMVug6WJrcutz0)，在公司内部使用 ChatGPT 是有风险的。即使[ChatGPT 现在允许用户禁用聊天记录或拒绝使用他们的数据](https://arstechnica.com/information-technology/2023/04/chatgpt-users-can-now-opt-out-of-chat-history-and-model-training/)来训练模型，公司也会认为分享他们的数据存在风险。

许多公司会考虑训练自己的聊天机器人，这是一种基于自身企业数据进行微调的模型，并且将保持内部使用。毕竟，这项技术即使对于预算较小的公司也变得可用和负担得起。此外，低成本使他们能够在新数据到来时或如果有更好的开源模型发布时定期进行微调。现在拥有数据的公司会更不愿意将数据分享出去。

此外，我们已经看到拥有优质数据的重要性。医学和许多其他领域的数据收集困难（昂贵、受规管、稀缺），而拥有这些数据的公司具有优势。OpenAI 可能会花费数十亿试图收集医学数据，但除了成本之外，患者招募需要多年时间和已建立的网络（而 OpenAI 并不具备）。现在拥有数据的公司在分享这些数据时会更加严格。

![](img/1d2779573a760ce361c54cce016342cf.png)

图片来源 [Petrebels](https://unsplash.com/it/@petrebels) 在 Unsplash

此外，像 HuggingGPT 和 [AudioGPT](https://github.com/AIGC-Audio/AudioGPT) 这样的作品表明 LLM 是用户与专家模型（文本到图像、音频模型等）互动的界面。在过去几年中，许多公司已经聘请数据科学家并开发了不同的专业模型以满足其需求（制药公司用于药物发现和设计的模型、制造公司用于组件设计和预测性维护的模型等）。因此，现在数据科学家可以指导 LLM 连接到他们之前训练的模型，并允许内部非技术用户通过文本提示与其互动。

还有另一个因素指向这样的情景，即生成式 AI 的规章不明确（例如，谷歌未发布其生成音乐模型以避免版权侵权）。除了版权问题，关于责任的问题仍然悬而未决。因此，许多公司可能会在接下来的几个月里内化这项技术，创建自己的 AI 助手。

# 告别的思考

![](img/19a35058c4bd9cf5b03a62422a64c50e.png)

图片来源于 [Saif71.com](https://unsplash.com/it/@saif71) 在 Unsplash.com

> 辛顿博士说，当人们曾经问他如何能从事潜在危险的技术工作时，他会引用罗伯特·奥本海默的话，奥本海默领导了美国的原子弹研制工作：“当你看到一些技术上很甜美的东西时，你就去做它。”
> 
> 他现在不再这样说了。 ([source](https://www.nytimes.com/2023/05/01/technology/ai-google-chatbot-engineer-quits-hinton.html))

辛顿最近表示，我们需要讨论人工智能的风险。但我们不能在一个黑箱中研究炸弹爆炸的风险。这就是为什么模型越来越迫切需要开源。

LLMs 正处于变革的阶段。创建越来越大的模型是不可持续的，并且不再提供曾经的优势。下一代 LLM 的未来将取决于数据，并且可能基于不再依赖自注意力的新架构。

然而，数据不会像以前那样容易获取；公司开始限制对数据的访问。 [微软表示愿意允许公司](https://www.cnbc.com/2023/02/07/microsoft-will-offer-chatgpt-tech-for-companies-to-customize-source.html) 创建自己版本的 ChatGPT。但公司会持怀疑态度。

一些公司担心他们的业务（似乎 ChatGPT 已经宣称 [其第一个受害者](https://www.greatandhra.com/articles/special-articles/edtech-firm-says-chatgpt-killing-its-business-128974)），而其他公司则担心数据泄漏。或者，仅仅是因为技术最终几乎触手可及，每家公司都将创建一个符合自己需求的聊天机器人。

总之，我们可以看到不同的趋势（这些趋势在某种程度上已经在发生）：

+   对人工智能日益增长的恐惧正在推动开源模型的发展

+   这导致了开源 LLM 模型的不断发布。这反过来显示，你可以使用更小的模型并降低对其对齐的成本。

+   大型语言模型（LLM）对不同的企业构成威胁，公司担心这些模型可能威胁到他们的业务。因此，不同的公司正在减少对其数据的访问或要求人工智能公司支付费用。

+   成本降低、对竞争的担忧、对专有数据的新相关性以及开源模型的新可用性正促使公司使用开源模型在自己的数据上训练自己的聊天机器人。

**你对 LLM 的未来有什么看法？在评论中告诉我**

# 如果你觉得这个话题有趣：

*你可以查看我的其他文章，也可以* [***订阅***](https://salvatore-raieli.medium.com/subscribe) *以在我发布文章时获得通知，你也可以* [***成为 Medium 会员***](https://medium.com/@salvatore-raieli/membership) *以访问所有故事（这是平台的附属链接，我可以从中获得少量收入，但不会对你产生费用），你还可以通过*[***LinkedIn***](https://www.linkedin.com/in/salvatore-raieli/)***与我联系或找到我。***

*这是我 GitHub 仓库的链接，我计划在这里收集与机器学习、人工智能及其他相关的代码和许多资源。*

[## GitHub - SalvatoreRa/tutorial: 机器学习、人工智能、数据科学的教程…](https://github.com/SalvatoreRa/tutorial?source=post_page-----90e203b2f6b0--------------------------------)

### 机器学习、人工智能、数据科学的教程，包含数学解释和可重用代码（用 Python…

[GitHub - SalvatoreRa/tutorial: 机器学习、人工智能、数据科学的教程…](https://github.com/SalvatoreRa/tutorial?source=post_page-----90e203b2f6b0--------------------------------)

*或者你可能对我最近的一篇文章感兴趣：*

[## 欢迎回到 80 年代：变压器可能会被卷积所超越](https://levelup.gitconnected.com/welcome-back-80s-transformers-could-be-blown-away-by-convolution-21ff15f6d1cc?source=post_page-----90e203b2f6b0--------------------------------)

### Hyena 模型展示了卷积如何可能比自注意力更快

[## META DINO: 自监督学习如何改变计算机视觉](https://levelup.gitconnected.com/meta-dino-how-self-supervised-learning-is-changing-computer-vision-1666a5e43dbb?source=post_page-----90e203b2f6b0--------------------------------)

### 筛选数据、视觉特征和知识蒸馏：下一代计算机视觉模型的基础

[## 透过你的眼睛：谷歌 AI 模型如何通过眼睛预测你的年龄](https://levelup.gitconnected.com/meta-dino-how-self-supervised-learning-is-changing-computer-vision-1666a5e43dbb?source=post_page-----90e203b2f6b0--------------------------------)

### 新模型通过分析眼睛照片可以揭示衰老的秘密

[levelup.gitconnected.com](https://levelup.gitconnected.com/looking-into-your-eyes-how-google-ai-model-can-predict-your-age-from-the-eye-857979339da9?source=post_page-----90e203b2f6b0--------------------------------) [](https://levelup.gitconnected.com/the-mechanical-symphony-will-ai-displace-the-human-workforce-9baeb786efa4?source=post_page-----90e203b2f6b0--------------------------------) [## 机械交响乐：人工智能会取代人类劳动力吗？

### GPT-4 展示了令人印象深刻的技能：这将对劳动市场产生什么影响？

[levelup.gitconnected.com](https://levelup.gitconnected.com/the-mechanical-symphony-will-ai-displace-the-human-workforce-9baeb786efa4?source=post_page-----90e203b2f6b0--------------------------------)
