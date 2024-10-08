# AI 十年回顾

> 原文：[`towardsdatascience.com/ten-years-of-ai-in-review-85decdb2a540`](https://towardsdatascience.com/ten-years-of-ai-in-review-85decdb2a540)

## 从图像分类到聊天机器人治疗

[](https://thomasdorfer.medium.com/?source=post_page-----85decdb2a540--------------------------------)![Thomas A Dorfer](https://thomasdorfer.medium.com/?source=post_page-----85decdb2a540--------------------------------)[](https://towardsdatascience.com/?source=post_page-----85decdb2a540--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----85decdb2a540--------------------------------) [Thomas A Dorfer](https://thomasdorfer.medium.com/?source=post_page-----85decdb2a540--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----85decdb2a540--------------------------------) ·阅读时间 15 分钟·2023 年 5 月 23 日

--

![](img/93a252e44c987915acf7a177e15cf18e.png)

图片由作者提供。

过去十年对于人工智能（AI）领域来说是一段激动人心且充满变故的旅程。对深度学习潜力的初步探索转变为一个爆炸性的增长领域，现在涵盖了从电子商务中的推荐系统到自动驾驶汽车的目标检测，以及能够生成从逼真图像到连贯文本的一切生成模型。

在这篇文章中，我们将回顾一下让我们走到今天的一些关键突破。不论你是经验丰富的 AI 从业者还是对该领域最新进展感兴趣的读者，这篇文章都将为你提供一个全面的概述，展示 AI 成为家喻户晓名字的非凡进展。

## 2013 年：AlexNet 和变分自编码器

2013 年被广泛认为是深度学习的“成熟期”，这主要得益于计算机视觉领域的重大进展。根据最近对 Geoffrey Hinton 的[采访](https://venturebeat.com/ai/10-years-on-ai-pioneers-hinton-lecun-li-say-deep-learning-revolution-will-continue/)，到 2013 年*“几乎所有的计算机视觉研究都转向了神经网络”*。这股热潮主要由一年前图像识别领域的一个相当意外的突破所推动。

在 2012 年 9 月，[AlexNet](https://proceedings.neurips.cc/paper_files/paper/2012/file/c399862d3b9d6b76c8436e924a68c45b-Paper.pdf)，一个深度卷积神经网络（CNN），在 ImageNet 大规模视觉识别挑战赛（ILSVRC）中表现出色，展示了深度学习在图像识别任务中的潜力。它达到了 15.3%的[前五名错误率](https://machinelearning.wtf/terms/top-5-error-rate/)，比其最接近的竞争对手低了 10.9%。

![](img/f8ff42241e915cbf93d678a537e1123a.png)

图片由作者提供。

这些成功背后的技术进步对人工智能未来的发展轨迹至关重要，并且极大地改变了人们对*深度*学习的认知。

首先，作者应用了一个由五个卷积层和三个全连接线性层组成的深度卷积神经网络（CNN）——这一架构设计当时被许多人认为是不切实际的。此外，由于网络的深度产生了大量的参数，训练是在两个图形处理单元（GPU）上并行进行的，展示了在大规模数据集上显著加速训练的能力。通过将传统的激活函数，如 sigmoid 和 tanh，替换为更高效的修正线性单元（[ReLU](https://www.cs.toronto.edu/~fritz/absps/reluICML.pdf)），训练时间得到了进一步缩短。

![](img/c78fd65a530e2485ac3c12fe3bb854c8.png)

图片由作者提供。

这些共同促成 AlexNet 成功的进展标志着人工智能历史上的一个转折点，并激发了学术界和技术社区对深度学习的广泛兴趣。因此，2013 年被许多人认为是深度学习真正开始蓬勃发展的拐点。

虽然在 2013 年也发生了相关进展，但被 AlexNet 的声势所掩盖，即变分自编码器（[VAEs](https://arxiv.org/abs/1312.6114)）的发展——这种生成模型能够学习表示和生成图像、声音等数据。它们通过学习输入数据在一个低维空间中的压缩表示，称为*潜在空间*，从而生成新的数据。这使得它们能够通过从学习到的潜在空间中采样来生成新数据。变分自编码器后来被证明为生成建模和数据生成开辟了新途径，并在艺术、设计和游戏等领域中找到了应用。

## 2014 年：生成对抗网络

在接下来的一年，即 2014 年 6 月，深度学习领域见证了另一项重大进展，那就是 Ian Goodfellow 和同事们引入了生成对抗网络（[GANs](https://proceedings.neurips.cc/paper_files/paper/2014/file/5ca3e9b122f61f8f06494c97b1afccf3-Paper.pdf)）。

生成对抗网络是一种神经网络，能够生成与训练集相似的新数据样本。基本上，两个网络被同时训练：（1）生成器网络生成虚假的或合成的样本，（2）鉴别器网络评估这些样本的真实性。这种训练在一个类似游戏的设置中进行，生成器试图创建能够欺骗鉴别器的样本，而鉴别器则试图正确识别出虚假的样本。

当时，生成对抗网络（GANs）代表了一种强大而新颖的数据生成工具，不仅用于生成图像和视频，还用于音乐和艺术创作。它们还推动了无监督学习的发展，这一领域在很大程度上被认为是欠发展的且具有挑战性的，通过展示在没有明确标签的情况下生成高质量数据样本的可能性。

## 2015 年：ResNets 和 NLP 突破

在 2015 年，人工智能领域在计算机视觉和自然语言处理（NLP）方面取得了显著进展。

Kaiming He 及其同事发表了一篇名为[“图像识别的深度残差学习”](https://arxiv.org/abs/1512.03385)的论文，在论文中他们介绍了残差神经网络（ResNets）的概念——这种架构通过添加快捷连接使信息在网络中更容易流动。与普通神经网络不同，在普通神经网络中，每一层都将前一层的输出作为输入，而在 ResNet 中，增加了额外的*残差*连接，这些连接跳过一层或多层，并直接连接到网络中的更深层。

因此，ResNets 能够解决[梯度消失问题](https://en.wikipedia.org/wiki/Vanishing_gradient_problem)，这使得训练比当时认为可能的更深层次的神经网络成为可能。这反过来又在图像分类和物体识别任务中带来了显著的改进。

与此同时，研究人员在递归神经网络（[RNNs](https://en.wikipedia.org/wiki/Recurrent_neural_network)）和长短期记忆（[LSTM](https://pubmed.ncbi.nlm.nih.gov/9377276/)）模型的开发方面也取得了显著进展。尽管这些模型自 1990 年代以来就存在，但它们直到 2015 年左右才开始引起关注，这主要是由于以下几个因素：（1）训练用的数据集变得更大、更具多样性，（2）计算能力和硬件的提升，使得训练更深层次、更复杂的模型成为可能，（3）在此过程中进行的修改，如更复杂的门控机制。

因此，这些架构使语言模型能够更好地理解文本的上下文和含义，从而在语言翻译、文本生成和情感分析等任务中取得了巨大的进步。RNNs 和 LSTMs 在那个时期的成功为我们今天看到的大型语言模型（LLMs）的发展铺平了道路。

## 2016 年：AlphaGo

在 1997 年加里·卡斯帕罗夫被 IBM 的 Deep Blue 击败之后，另一场*人类对机器*的对决在 2016 年引起了游戏界的震动：谷歌的 AlphaGo 战胜了围棋世界冠军李世石。

![](img/61277a6cb3b23abf63d2fc77d2851572.png)

图片由[Elena Popova](https://unsplash.com/@elenapopova)提供，来源于[Unsplash](https://unsplash.com/photos/xdXxY5C9PUo)。

Sedol 的[失败](https://en.wikipedia.org/wiki/AlphaGo_versus_Lee_Sedol)标志着人工智能进步轨迹中的另一个重要里程碑：它证明了机器能够超越即使是最熟练的人类玩家，在一个曾经被认为过于复杂的游戏中。利用[深度强化学习](https://en.wikipedia.org/wiki/Deep_reinforcement_learning)和[蒙特卡罗树搜索](https://en.wikipedia.org/wiki/Monte_Carlo_tree_search)的结合，AlphaGo 分析了来自先前游戏的数百万种局面，并评估了最佳可能的走法——这种策略在此背景下远超人类决策。

## 2017: Transformer 架构与语言模型

可以说，2017 年是奠定我们今天所见生成式 AI 突破基础的关键一年。

2017 年 12 月，Vaswani 及其同事发布了基础性的[论文](https://arxiv.org/abs/1706.03762)“Attention is all you need”，介绍了利用[自注意力](https://en.wikipedia.org/wiki/Attention_(machine_learning))概念处理序列输入数据的 transformer 架构。这使得对长距离依赖的处理变得更加高效，而这以前一直是传统 RNN 架构的挑战。

![](img/a5a6f23cdbf2beae37cc8bacdc0f0de3.png)

照片由[Jeffery Ho](https://unsplash.com/@jefferyho)拍摄，发布在[Unsplash](https://unsplash.com/photos/x22UAIdif_k)。

Transformer 由两个基本组件组成：编码器和解码器。编码器负责对输入数据进行编码，例如，这可以是一系列单词。它然后对输入序列应用多层自注意力和前馈神经网络，以捕捉句子中的关系和特征，并学习有意义的表示。

本质上，自注意力使模型能够理解句子中不同单词之间的关系。与传统模型按固定顺序处理单词不同，transformers 实际上一次性检查所有单词。它们根据单词在句子中的相关性为每个单词分配一种叫做*注意力*的分数。

解码器则从编码器处获取编码后的表示，并生成一个输出序列。在诸如机器翻译或文本生成等任务中，解码器根据从编码器接收到的输入生成翻译序列。与编码器类似，解码器也由多层自注意力和前馈神经网络组成。然而，它包含一个额外的注意力机制，使其能够关注编码器的输出。这使得解码器在生成输出时能够考虑输入序列中的相关信息。

自那时以来，transformer 架构已成为 LLM 发展的关键组成部分，并在 NLP 领域（如机器翻译、语言建模和问答）带来了显著的改进。

## 2018 年：GPT-1、BERT 和图神经网络

在 Vaswani 等人发表了他们的基础论文几个月后，**G**enerative **P**retrained **T**ransformer，或 [GPT-1](https://cdn.openai.com/research-covers/language-unsupervised/language_understanding_paper.pdf) 于 2018 年 6 月由 OpenAI 推出，利用 transformer 架构有效捕捉文本中的长距离依赖关系。GPT-1 是最早展示无监督预训练效果并在特定 NLP 任务上进行微调的模型之一。

谷歌也利用了当时还相对新颖的 transformer 架构，2018 年末发布并开源了他们自己的预训练方法，称为**B**idirectional **E**ncoder **R**epresentations from **T**ransformers，或 [BERT](https://ai.googleblog.com/2018/11/open-sourcing-bert-state-of-art-pre.html)。与以单向方式处理文本的之前模型（包括 GPT-1）不同，BERT 同时考虑每个单词的前后上下文。为了说明这一点，作者提供了一个非常直观的例子：

> *在句子* “I accessed the bank account” *中，一个单向上下文模型会基于* “I accessed the” *来表示* “bank” *，而不是* “account” *。然而，BERT 通过使用前后上下文来表示* “bank” *——即* “I accessed the … account” *——从深度神经网络的最底层开始，使其具有深度的双向特性。*

双向性的概念如此强大，以至于 BERT 在各种基准任务上超越了最先进的 NLP 系统。

除了 GPT-1 和 BERT，图神经网络或 [GNNs](https://en.wikipedia.org/wiki/Graph_neural_network) 在那一年也引起了一些关注。它们属于专门设计用于处理图数据的神经网络类别。GNNs 利用消息传递算法在图的节点和边之间传播信息。这使得网络能够以更加直观的方式学习数据的结构和关系。

这一工作使得从数据中提取更深层次的见解成为可能，从而拓宽了深度学习应用的问题范围。借助 GNNs，社交网络分析、推荐系统和药物发现等领域取得了重大进展。

## 2019 年：GPT-2 和改进的生成模型

2019 年标志着生成模型的几项显著进展，特别是 [GPT-2](https://openai.com/research/gpt-2-1-5b-release) 的推出。这个模型通过在许多自然语言处理任务中实现最先进的性能而让同行相形见绌，而且能够生成高度逼真的文本，事后来看，这为我们展示了该领域即将到来的新进展。

该领域的其他改进包括 DeepMind 的 [BigGAN](https://www.deepmind.com/open-source/big-gan)，它生成了几乎无法与真实图像区分的高质量图像，以及 NVIDIA 的 [StyleGAN](https://github.com/NVlabs/stylegan)，它允许对生成的图像外观进行更好的控制。

总体而言，这些现在被称为生成式 AI 的进展将这一领域的边界推向了更远的地方，…

## 2020 年：GPT-3 和自监督学习

… 不久之后，另一个模型诞生了，它甚至在技术圈外也成为了家喻户晓的名字：[GPT-3](https://arxiv.org/abs/2005.14165)。这个模型在大型语言模型的规模和能力上迈出了重大的一步。为了让事情有个背景，GPT-1 只有区区 1.17 亿个参数。这个数字在 GPT-2 中增加到了 15 亿，而 GPT-3 则有 1750 亿个参数。

这个巨大的参数空间使 GPT-3 能够在各种提示和任务中生成非常连贯的文本。它在文本完成、问题回答甚至创意写作等各种自然语言处理任务中也表现出了令人印象深刻的性能。

此外，GPT-3 再次突出了使用自监督学习的潜力，这使得模型可以在大量未标记的数据上进行训练。这种方法的优点在于，这些模型可以在不需要大量任务特定训练的情况下获得广泛的语言理解，这使得它更加经济。

Yann LeCun 在推特上提到了一篇关于自监督学习的《纽约时报》文章。

## 2021 年：AlphaFold 2、DALL·E 和 GitHub Copilot

从蛋白质折叠到图像生成以及自动化编程辅助，2021 年因 AlphaFold 2、DALL·E 和 GitHub Copilot 的发布而变得格外引人注目。

[AlphaFold 2](https://www.nature.com/articles/s41586-021-03819-2) 被誉为对长期存在的蛋白质折叠问题的期待已久的解决方案。DeepMind 的研究人员扩展了 Transformer 架构，创建了 *evoformer blocks*——一种利用进化策略进行模型优化的架构——来构建一个能够基于蛋白质的 1D 氨基酸序列预测其 3D 结构的模型。这一突破在药物发现、生物工程以及我们对生物系统的理解等领域具有巨大的革命性潜力。

OpenAI 今年再次成为新闻焦点，发布了[DALL·E](https://openai.com/product/dall-e-2)。本质上，这个模型结合了 GPT 风格的语言模型和图像生成的概念，使得从文本描述中生成高质量图像成为可能。

为了说明这个模型的强大程度，请参见下面的图像，它是通过提示“未来世界的油画，飞行汽车”生成的。

![](img/f91c79fbddeaca925244055b4b80aa71.png)

由 DALL·E 制作的图像。

最后，GitHub 发布了后来成为每位开发者最佳伙伴的[Copilot](https://github.com/features/copilot)。这是与 OpenAI 合作实现的，OpenAI 提供了基础语言模型 Codex，该模型在大量公开可用的代码库上进行训练，从而学习理解和生成各种编程语言的代码。开发者只需提供一个描述他们尝试解决问题的代码注释，Copilot 就会建议实现解决方案的代码。其他功能包括能够用自然语言描述输入代码以及在编程语言之间翻译代码。

## 2022 年：ChatGPT 和 Stable Diffusion

在过去十年中，人工智能的快速发展 culminated in a groundbreaking advancement: OpenAI 的[ChatGPT](https://openai.com/blog/chatgpt)，这是一个于 2022 年 11 月发布的聊天机器人。这个工具代表了自然语言处理领域的尖端成就，能够对各种查询和提示生成连贯且具有上下文相关的回应。此外，它还可以进行对话、提供解释、提出创意建议、协助解决问题、编写和解释代码，甚至模拟不同的人物性格或写作风格。

![](img/4048ce5419b5576ca4eb2746aac5bb59.png)

作者提供的图像。

与聊天机器人交互的简单直观界面也刺激了其使用率的急剧上升。之前，主要是科技圈会玩弄最新的基于 AI 的发明。然而，如今，AI 工具几乎渗透到了每个专业领域，从软件工程师到作家、音乐家和广告商。许多公司也在利用这一模型自动化服务，如客户支持、语言翻译或回答常见问题。事实上，我们所见的自动化浪潮重新激发了一些担忧，并引发了有关哪些工作可能面临自动化风险的讨论。

尽管 ChatGPT 在 2022 年占据了大部分的焦点，但在图像生成方面也取得了显著的进展。[Stable Diffusion](https://stability.ai/blog/stable-diffusion-public-release)，一个能够从文本描述生成照片级真实图像的潜在文本到图像扩散模型，由 Stability AI 发布。

稳定扩散是传统扩散模型的扩展，其工作原理是通过迭代地向图像中添加噪声，然后逆转这一过程以恢复数据。它的设计目的是通过不直接处理输入图像，而是处理其较低维度的表示或潜在空间，从而加快这一过程。此外，扩散过程通过将用户的变换器嵌入文本提示添加到网络中来进行修改，使其能够在每次迭代中引导图像生成过程。

总的来说，2022 年 ChatGPT 和 Stable Diffusion 的发布突显了多模态生成 AI 的潜力，并引发了该领域进一步发展的巨大推动和投资。

## 2023：LLMs 和 Bots

今年无疑成为了 LLMs 和聊天机器人的一年。越来越多的模型以快速增长的速度被开发和发布。

![](img/5b90ef1273384e40071fe61dde42592a.png)

图片由作者提供。

例如，2 月 24 日，Meta AI 发布了 [LLaMA](https://ai.facebook.com/blog/large-language-model-llama-meta-ai/) —— 一个在大多数基准测试中超过 GPT-3 的 LLM，尽管其参数数量要少得多。不到一个月后，3 月 14 日，OpenAI 发布了 [GPT-4](https://openai.com/product/gpt-4) —— 一个比 GPT-3 更大、更强大且多模态的版本。尽管 GPT-4 的确切参数数量未知，但据推测已达到万亿级别。

3 月 15 日，斯坦福大学的研究人员发布了 [Alpaca](https://crfm.stanford.edu/2023/03/13/alpaca.html)，这是一个从 LLaMA 上经过指令跟随演示微调的轻量级语言模型。几天后，3 月 21 日，谷歌推出了其 ChatGPT 竞争对手：[Bard](https://blog.google/technology/ai/bard-google-ai-search-updates/)。谷歌还于本月 5 月 10 日发布了其最新的 LLM，[PaLM-2](https://ai.google/discover/palm2)。鉴于这一领域的发展步伐如此迅猛，极有可能在你阅读此文时，又有一个新模型出现了。

我们还看到越来越多的公司将这些模型融入到他们的产品中。例如，Duolingo 宣布了其 GPT-4 驱动的 [Duolingo Max](https://blog.duolingo.com/duolingo-max/)，这是一个新的订阅层级，旨在为每个人提供量身定制的语言课程。Slack 也推出了一个名为 [Slack GPT](https://slack.com/blog/news/introducing-slack-gpt) 的 AI 助手，可以进行诸如草拟回复或总结对话线程等任务。此外，Shopify 将 ChatGPT 驱动的助手引入了公司的 Shop 应用，该助手可以帮助客户使用各种提示识别所需的产品。

Shopify 在 Twitter 上宣布了其 ChatGPT 驱动的助手。

有趣的是，如今 AI 聊天机器人甚至被视为人类治疗师的替代品。例如，[Replika](https://replika.com/)，一个美国聊天机器人应用程序，为用户提供了一个“*关心的 AI 伴侣，总是倾听和交谈，总是在你身边*”。其创始人尤金尼亚·库伊达表示，该应用拥有广泛的用户群体，从自闭症儿童，他们使用它作为“*在人际互动前热身*”的方法，到孤独的成年人，他们仅仅需要一个朋友。

在我们总结之前，我想强调一下可能是过去十年人工智能发展高潮的事件：人们实际上在使用 Bing！今年早些时候，微软[推出](https://blogs.microsoft.com/blog/2023/02/07/reinventing-search-with-a-new-ai-powered-microsoft-bing-and-edge-your-copilot-for-the-web/)了其基于 GPT-4 的“*网络助手*”，该助手为搜索进行了定制，并且在……永远以来（？）首次成为谷歌在搜索领域长期霸主的真正竞争者。

## 回顾过去，展望未来

当我们回顾过去十年的人工智能发展时，很明显我们见证了一场对我们工作、商业和人际互动方式产生深远影响的变革。最近在生成模型，尤其是大型语言模型（LLMs）方面取得的显著进展似乎都遵循了“越大越好”的普遍信念，这指的是模型的参数空间。这在 GPT 系列中尤为明显，GPT-1 最初有 1.17 亿个参数，而每个后续模型的参数量大约增加一个数量级，最终发展到 GPT-4，可能拥有数万亿个参数。

然而，根据最近的[采访](https://techcrunch.com/2023/04/14/sam-altman-size-of-llms-wont-matter-as-much-moving-forward/?guccounter=1&guce_referrer=aHR0cHM6Ly93d3cuZ29vZ2xlLmNvbS8&guce_referrer_sig=AQAAAANxF1G0qEgpUn-9mD7CxuZrc77IDr8t-QvyX3Do6Koa10eGy5DYiq3lzXDAJRakptl0Jy49OkuxXU8zD-3-8l-h3YJxFgKRwk5HqIHdhG2BIXavq5Tfn1HHz6IKk8-y86xZbyHXZJwE_Q_OFXr4nrHygrl48-WxX7vdgTft_lhw)，OpenAI 首席执行官山姆·奥特曼认为我们已经迎来了“越大越好”时代的终结。他认为，未来参数数量仍会增长，但未来模型改进的主要关注点将是提高模型的能力、实用性和安全性。

后者尤为重要。考虑到这些强大的人工智能工具现在已经掌握在公众手中，而不再局限于受控的研究实验室环境，现在比以往任何时候都更加关键，我们要谨慎行事，确保这些工具的安全，并符合人类的最佳利益。希望我们在人工智能安全领域能看到与其他领域同样的开发和投资。

**附言：** 如果我遗漏了你认为应该包含在这篇文章中的核心 AI 概念或突破，请在下方评论告诉我！

## 喜欢这篇文章吗？

让我们保持联系吧！你可以在[Twitter](https://twitter.com/ThomasADorfer)、[LinkedIn](https://www.linkedin.com/in/thomasdorfer/)和[Substack](https://thomasdorfer.substack.com/)找到我。

如果你想支持我的写作，可以通过[Medium 会员](https://thomasdorfer.medium.com/membership)来实现，这样你可以访问我所有的故事以及 Medium 上成千上万其他作家的故事。

[](https://medium.com/@thomasdorfer/membership?source=post_page-----85decdb2a540--------------------------------) [## 通过我的推荐链接加入 Medium - Thomas A Dorfer

### 阅读 Thomas A Dorfer 的每一个故事（以及 Medium 上成千上万其他作家的故事）。你的会员费直接支持…

[medium.com](https://medium.com/@thomasdorfer/membership?source=post_page-----85decdb2a540--------------------------------)
