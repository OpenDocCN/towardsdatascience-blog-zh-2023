# 在生产中部署生成模型的三大挑战

> 原文：[https://towardsdatascience.com/three-challenges-in-deploying-generative-models-in-production-8e4c0fcf63c3?source=collection_archive---------5-----------------------#2023-08-07](https://towardsdatascience.com/three-challenges-in-deploying-generative-models-in-production-8e4c0fcf63c3?source=collection_archive---------5-----------------------#2023-08-07)

## 如何将大型语言模型和扩散模型部署到你的产品中，而不让用户感到恐惧。

[](https://mikhailiuk.medium.com/?source=post_page-----8e4c0fcf63c3--------------------------------)[![Aliaksei Mikhailiuk](../Images/f4bf3f15f3e0b42f34e50b3ffc436b2a.png)](https://mikhailiuk.medium.com/?source=post_page-----8e4c0fcf63c3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8e4c0fcf63c3--------------------------------)[![数据科学前沿](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----8e4c0fcf63c3--------------------------------) [Aliaksei Mikhailiuk](https://mikhailiuk.medium.com/?source=post_page-----8e4c0fcf63c3--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F30bef13bba71&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthree-challenges-in-deploying-generative-models-in-production-8e4c0fcf63c3&user=Aliaksei+Mikhailiuk&userId=30bef13bba71&source=post_page-30bef13bba71----8e4c0fcf63c3---------------------post_header-----------) 发表于 [数据科学前沿](https://towardsdatascience.com/?source=post_page-----8e4c0fcf63c3--------------------------------) · 9分钟阅读 · 2023年8月7日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F8e4c0fcf63c3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthree-challenges-in-deploying-generative-models-in-production-8e4c0fcf63c3&user=Aliaksei+Mikhailiuk&userId=30bef13bba71&source=-----8e4c0fcf63c3---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F8e4c0fcf63c3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthree-challenges-in-deploying-generative-models-in-production-8e4c0fcf63c3&source=-----8e4c0fcf63c3---------------------bookmark_footer-----------)![](../Images/4d0d67dbb4548f550d2b5fbbc3b28114.png)

图片由作者在 [SDXL 1.0](https://stability.ai/blog/stable-diffusion-sdxl-1-announcement) 中生成。

[OpenAI](https://openai.com/)、[Google](https://ai.google/discover/generativeai/)、[Microsoft](https://blogs.microsoft.com/blog/2023/05/23/microsoft-build-brings-ai-tools-to-the-forefront-for-developers/)、[Midjourney](https://www.midjourney.com/home/?callbackUrl=%2Fapp%2F)、[StabilityAI](https://stability.ai/)、[CharacterAI](https://beta.character.ai/)等公司——每个人都在竞相提供最佳的文本到文本、文本到图像、图像到图像和图像到文本模型解决方案。

原因很简单——这一领域提供了广阔的机会；毕竟，它不仅仅是娱乐，还有以前无法解锁的实用功能。从[更好的搜索引擎](https://blogs.microsoft.com/blog/2023/02/07/reinventing-search-with-a-new-ai-powered-microsoft-bing-and-edge-your-copilot-for-the-web/)到更令人印象深刻的[个性化广告活动](https://www.optimatik.ai/ai-product-photos)和友好的聊天机器人，如[Snap的MyAI](https://newsroom.snap.com/en-GB/say-hi-to-my-ai)。

尽管这一领域非常动态，许多模型检查点每天都在发布，但每个从事生成式AI的公司都在寻找解决方案来应对挑战。

在这里，我将讨论在生产中部署生成模型的主要挑战以及如何解决这些挑战。尽管有许多不同类型的生成模型，在这篇文章中，我将重点讨论最近在扩散模型和基于GPT的模型中的进展。然而，许多讨论的话题也适用于其他模型。

# 什么是生成式AI？

生成式AI广泛描述了一组可以生成新内容的模型。广泛知名的生成对抗网络通过学习真实数据的分布，并从添加的噪声中生成变异来实现这一点。

近期生成式AI的繁荣源于模型在大规模上达到了人类水平的质量。解锁这一转变的原因很简单——我们现在只有足够的计算能力（因此是[NVIDIA股价飙升](https://www.investors.com/news/nvidia-stock-2023-buy-now/)）来训练和维护足够容量的模型，以实现高质量的结果。当前的进步由两种基础架构驱动——变换器和扩散模型。

也许最近一年最重要的突破是OpenAI的ChatGPT——一个基于文本的生成模型，最新的ChatGPT-3.5版本有1750亿个参数，具有足够的知识库来维持各种话题的对话。虽然ChatGPT是一个单模态模型，因为它只能支持文本，但多模态模型可以处理多种输入和输出，例如文本和图像。

图像到文本和文本到图像的多模态架构在一个由文本和图像概念共享的潜在空间中操作。通过在需要两个概念的任务（例如，图像标注）上训练来获得潜在空间，通过惩罚同一概念在两个不同模态中的潜在空间距离。一旦获得了这个潜在空间，它可以用于其他任务。

![](../Images/20b680f4fdfaaeea4a1b4ecae945ed5b.png)

图像到文本模型的示例。图片由作者提供。

今年发布的著名生成模型有[DALL·E](https://openai.com/dall-e-2)/[Stable-Diffusion](https://stability.ai/blog)（文本到图像/图像到图像）和[BLIP](https://arxiv.org/abs/2201.12086)（图像到文本[实现](https://github.com/salesforce/BLIP)）。DALL·E模型以提示或图像作为输入，提示生成图像作为响应，而基于BLIP的模型可以回答关于图像内容的问题。

# 挑战与解决方案

不幸的是，机器学习没有免费的午餐，大规模生成模型在生产部署时遇到了一些挑战——大小和延迟、偏差和公平性以及生成结果的质量。

## 模型大小和延迟

![](../Images/809bb30040a03db448990c63422f1c66.png)

模型大小趋势。数据来自[P. Villalobos](https://arxiv.org/pdf/2207.02852.pdf)。图片由作者提供

先进的GenAI模型非常庞大。例如，文本到文本的[Meta的LLaMA](https://research.facebook.com/publications/llama-open-and-efficient-foundation-language-models/)模型范围在7亿到65亿参数之间，而ChatGPT-3.5有1750亿参数。这些数字是合理的——在简化的世界里，经验法则是模型越大，训练所用的数据越多，质量也越好。

尽管文本到图像模型较小，但仍显著大于其生成对抗网络的前身——Stable Diffusion 1.5检查点略低于10亿参数（占用超过三GB的空间），而DALL·E 2.0有35亿参数。很少有GPU能够拥有足够的内存来维持这些模型，通常你需要一整套设备来维护一个大型模型，这可能很快变得非常昂贵，更不用说将这些模型部署到移动设备上。

生成模型需要时间来产生输出。对于某些模型，延迟是由于其大小——在多个GPU上传播信号通过数十亿个参数即使需要时间，而对于其他模型，则是由于生成高质量结果的迭代性质。扩散模型在默认配置下需要50步来生成图像，减少步数会降低输出图像的质量。

**解决方案：** 将模型缩小通常有助于提高其速度 — [提炼、压缩和量化](/on-the-edge-deploying-deep-applications-on-constrained-devices-f2dac997dd4d) 模型也会减少延迟。[高通为此铺平了道路](https://www.qualcomm.com/news/onq/2023/02/worlds-first-on-device-demonstration-of-stable-diffusion-on-android)，通过将稳定扩散模型压缩到可以在移动设备上部署的程度。最近，[稳定扩散（tiny 和 small）的小型、提炼和更快速版本已被发布](https://blog.segmind.com/introducing-sd-small-and-sd-tiny-stable-diffusion-models/)。

特定模型的优化也有助于加快推理速度 — 对于扩散模型；可以生成低分辨率的输出，然后进行放大，或使用更少的步骤和不同的调度器，因为有些模型在较少的步骤下效果最好，而其他模型在更多迭代下会生成更高质量的结果。例如，[Snap 最近展示了八个步骤就足以](https://snap-research.github.io/SnapFusion/) 使用稳定扩散 1.5 创建高质量的结果，并在训练时采用了各种优化。

例如，使用 NVIDIAs [tensorrt](https://developer.nvidia.com/tensorrt) 和 [torch.compile](https://pytorch.org/tutorials/intermediate/torch_compile_tutorial.html) 编译模型，可以在最小的工程努力下大幅减少延迟。

[](/on-the-edge-deploying-deep-applications-on-constrained-devices-f2dac997dd4d?source=post_page-----8e4c0fcf63c3--------------------------------) [## 边缘 — 在移动设备上部署深度学习应用

### 在受限设备上打破效率和准确性权衡的技术

towardsdatascience.com](/on-the-edge-deploying-deep-applications-on-constrained-devices-f2dac997dd4d?source=post_page-----8e4c0fcf63c3--------------------------------)

## 偏见、公平性和安全性

你是否尝试过破解 ChatGPT？许多人成功揭露了偏见和公平性问题，值得称赞的是 [OpenAI 做得很好](https://openai.com/blog/how-should-ai-systems-behave) 解决这些问题。如果没有大规模的修复，聊天机器人可能会通过传播有害和不安全的思想和行为而造成现实世界的问题。

破坏模型的示例见[政治](https://www.brookings.edu/articles/the-politics-of-ai-chatgpt-and-political-bias/)；例如，ChatGPT[拒绝创作关于特朗普的诗歌，但会创作关于拜登的诗歌](https://www.forbes.com/sites/ariannajohnson/2023/02/03/is-chatgpt-partisan-poems-about-trump-and-biden-raise-questions-about-the-ai-bots-bias-heres-what-experts-think/)，特别是性别平等和职业——暗示某些[职业适合男性，某些适合女性](https://nationalcentreforai.jiscinvolve.org/wp/2023/01/26/exploring-the-potential-for-bias-in-chatgpt/)以及[种族](https://www.forbes.com/sites/janicegassam/2023/01/28/the-dark-side-of-chatgpt/)。

与文本到文本模型类似，文本到图像和图像到文本模型也存在偏见和公平性问题。[Stable Diffusion 2.1](https://huggingface.co/spaces/stabilityai/stable-diffusion)模型在生成医生和护士的图像时，通常生成的是白人男性作为医生和白人女性作为护士。有趣的是，偏见会根据提示中指定的国家而有所不同——例如，日本医生或巴西护士。

![](../Images/baa3dac2aafafa0b794e59e99ca6c871.png)![](../Images/8b3a9600a74a17914d49faa08ad66012.png)

询问Stable Diffusion模型生成一位医生和一位护士的图像。图片由作者使用[SD 2.1接口](https://huggingface.co/spaces/stabilityai/stable-diffusion)生成。

玩弄[BLIP图像到文本模型](https://replicate.com/salesforce/blip)，将一张超重人士和男女医生的图片输入时，我得到了一些带有评判和偏见的图像描述——“一个胖男人”，“一个男性医生”，“一位穿着实验室大褂、带着听诊器的女性”。

![](../Images/cf2feb754f75ca4371738cb1d773b8f1.png)![](../Images/fdc551bf4f5e9c224a664c52bc7adf34.png)![](../Images/d21ef055d5108c9a708d0614c95e4af6.png)

图片由作者使用Stable Diffusion 2.1生成，描述由BLIP模型生成，依次为[1]一个胖男人吃冰淇淋；[2]一位穿着白大褂和领带的男性医生；[3]一位穿着白色实验室大褂、脖子上挂着听诊器的女性。图片由作者使用[SD 2.1接口](https://huggingface.co/spaces/stabilityai/stable-diffusion)生成，并通过[BLIP](https://replicate.com/salesforce/blip)处理。

**如何测试这个问题：** 这是一个相当难以诊断的问题——在许多情况下，你需要知道要查找什么。拥有一个包含各种提示的独立基准数据集，其中可能会出现问题的情况、模板响应和我们将检测的每个答案的警告标志，以及来自不同背景和多个场景的人类数据集，存储关于图像中人物的所有可能属性，将会有所帮助。这些数据集需要包含数十万条记录，以获得可靠的统计数据。

**解决方案：**几乎所有的偏见、公平性和安全性问题都源于训练数据。我喜欢这样的类比：AI模型是人性的镜像，加剧了我们所有的偏见。在干净、公正的数据上训练会大大改善结果。然而，即使有了这些，模型仍然会出错。

结果后处理和过滤是另一个可能的解决方案；例如，Stable Diffusion 在包含裸露图像的数据上进行训练，配有 NSFW 内容检测器以捕捉潜在问题。类似的过滤器可以应用于文本到文本模型的输出。

## 输出质量、相关性和正确性

生成模型在解释用户请求时可以非常有创意，虽然最近的大规模模型达到了人类水平的质量，但对于每种使用情况，它们不会开箱即用，需要额外的调整和提示工程。

在早期阶段，图像到文本和文本到文本模型的质量评估相对简单——毕竟，相较于无意义的内容，改进是显而易见的。高质量生成模型开始展现出更难以察觉的行为；例如，文本到文本模型可能变得含糊其辞，自信地输出错误和过时的信息。

扩散模型在其他方面表现出输出缺陷。典型的图像基础模型问题包括几何破损、变异的解剖结构、提示与图像结果不匹配、图像传输中的肤色和性别不匹配。自动化美学和现实性评估滞后，典型的度量指标，如FID，无法捕捉这些变化。

![](../Images/2b8d4859a35e13936fa651c4f8e2f811.png)![](../Images/32cd2b6b56dc856a362fff98f44f625a.png)![](../Images/45c5f9cfd8461d6ab0e52c1424f8b012.png)

从左到右：[1] 两个人拥抱；[2] 一个竖起大拇指的男人；[3] 一只狗在公园里跑步。一张由作者生成的来自[Stable Diffusion 1.5](https://stablediffusionweb.com/#demo)的图像

**如何测试问题：** 测试生成模型的质量具有挑战性；毕竟，这些模型没有真实的标准——它们被设计用于提供新颖的输出。因此，到目前为止，没有一种度量标准能可靠地捕捉质量方面。最可靠的度量标准是人工评估。

与偏见和公平性评估一样，最好的方法是拥有一个大规模的提示和图像数据集来测试质量。随着文本到文本模型变得更加个性化并调整到每个用户，除了对话的连贯性、正确性和相关性，我们还希望评估它们记住关于对话的信息的能力。

**解决方案：** 许多质量问题可以归因于训练数据和模型的大小；它们可能还稍微有点小，不足以实现另一个质量飞跃（[考虑GPT-3.5与GPT-4](https://www.linkedin.com/pulse/chatgpt-35-vs-chatgpt-4-comparison-latest-gpt-based-chatbot-kunerth/)）——当前的潜在空间是一个抽象概念，并不是为了存储精确信息而设计的。许多问题可以通过更好的提示工程解决——无论是文本到文本和文本到图像的提示增强，还是文本到图像模型的负面提示。

文本到图像和图像到图像的模型可以使用附加工具来提升质量——图像增强，无论是通过传统的深度学习方法还是基于扩散的细化器。像[ControlNet](https://huggingface.co/blog/controlnet)这样的附加模块，与扩散架构正交，可以帮助对生成结果进行额外控制。针对特定应用进行微调的Dreambooth技术也有助于提升结果。调整额外参数，如调度器、CFG和扩散步骤数，可能会大幅度影响质量。

# 总结

生成模型开启了新的应用范围，包括有趣的[AI镜头](https://www.snapchat.com/lens/87aca516bfa74b86b708849c617e51c1?locale=en-GB)和商业用途，如更好的搜索引擎、共同助手和广告。同时，企业急于推出产品和消费者对新功能的兴奋，有时会使技术中的明显缺陷被忽视。

尽管有一种普遍的推动力，旨在通过发布大规模开源数据集、训练代码和评估结果来提高模型基准测试的透明度，但对大规模AI模型的严格监管也在不断推进。在理想的世界中，两者不会走向极端，而是相互促进，使AI更加安全和有趣。

# 喜欢这位作者？保持联系！

我是否遗漏了什么？请随时在[LinkedIn](https://www.linkedin.com/in/aliakseimikhailiuk/)或[Twitter](https://twitter.com/mikhailiuka)上留言、评论或直接给我发消息！

[](/deep-image-quality-assessment-30ad71641fac?source=post_page-----8e4c0fcf63c3--------------------------------) [## 深度图像质量评估

### 深入探讨全参考图像质量评估。从主观图像质量实验到深度目标评估…

towardsdatascience.com](/deep-image-quality-assessment-30ad71641fac?source=post_page-----8e4c0fcf63c3--------------------------------) [](/perceptual-losses-for-image-restoration-dd3c9de4113?source=post_page-----8e4c0fcf63c3--------------------------------) [## 感知损失用于深度图像修复

### 从均方误差到GAN——什么才是好的感知损失函数？

[towardsdatascience.com](/perceptual-losses-for-image-restoration-dd3c9de4113?source=post_page-----8e4c0fcf63c3--------------------------------)

**本博客中的观点仅代表我个人，与Snap无关，也不代表Snap的立场。**
