# 2024年值得期待的3项音乐AI突破

> 原文：[https://towardsdatascience.com/3-music-ai-breakthroughs-to-expect-in-2024-2d945ae6b5fd?source=collection_archive---------2-----------------------#2023-12-30](https://towardsdatascience.com/3-music-ai-breakthroughs-to-expect-in-2024-2d945ae6b5fd?source=collection_archive---------2-----------------------#2023-12-30)

## 2024年可能成为音乐AI的转折点

[](https://medium.com/@maxhilsdorf?source=post_page-----2d945ae6b5fd--------------------------------)[![Max Hilsdorf](../Images/01da76c553e43d5ed6b6849bdbfd00da.png)](https://medium.com/@maxhilsdorf?source=post_page-----2d945ae6b5fd--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2d945ae6b5fd--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----2d945ae6b5fd--------------------------------) [Max Hilsdorf](https://medium.com/@maxhilsdorf?source=post_page-----2d945ae6b5fd--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fd0c085a74ae8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F3-music-ai-breakthroughs-to-expect-in-2024-2d945ae6b5fd&user=Max+Hilsdorf&userId=d0c085a74ae8&source=post_page-d0c085a74ae8----2d945ae6b5fd---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----2d945ae6b5fd--------------------------------) ·11分钟阅读·2023年12月30日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F2d945ae6b5fd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F3-music-ai-breakthroughs-to-expect-in-2024-2d945ae6b5fd&user=Max+Hilsdorf&userId=d0c085a74ae8&source=-----2d945ae6b5fd---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F2d945ae6b5fd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F3-music-ai-breakthroughs-to-expect-in-2024-2d945ae6b5fd&source=-----2d945ae6b5fd---------------------bookmark_footer-----------)![](../Images/1af7a374365e9166ebbd91aeafabe31c.png)

图像由DALL-E 3生成。

# **回顾：2023年如何改变了音乐AI**

从我的角度来看，2023年是**音乐AI历史上最激动人心的一年**。以下是我们在这一年中体验到的一些突破：

+   **文本到音乐生成**已经跨越了“恐怖谷”阶段（例如 [MusicLM](https://google-research.github.io/seanet/musiclm/examples/)）

+   开源**旋律条件的音乐生成**发布（例如 [MusicGen](https://musicgen.com/)）

+   首个**基于提示的音乐搜索**产品上线（例如 [Cyanite](https://cyanite.ai/)）

+   开源的**具备音频理解/生成能力的聊天机器人**已发布（例如，[AudioGPT](https://github.com/AIGC-Audio/AudioGPT)）

+   开源的**音乐描述 AI**已发布（例如，[Doh 等，2023](https://arxiv.org/abs/2307.16372)）

+   **基于提示的源分离**试点（例如，[Liu 等，2023](https://arxiv.org/abs/2308.05037)）

+   …

从文本到音乐生成，再到全文本音乐搜索，2023 年充满了突破。这些进展是**冰山一角**，展示了音乐 AI 内在的潜力。然而，即使有这些令人兴奋的发展，该领域仍明显落后于其更大的兄弟语音 AI，甚至落后于其表亲 NLP 和计算机视觉。这一差距可以在两个关键方面观察到（或听到）：

**1.** **技术尚不成熟**。无论是音乐生成、基于文本的搜索还是神经嵌入：目前音乐 AI 中的所有技术在文本和图像领域至少已有 1 到 3 年的历史。该领域需要更多的资金、时间和智力支持。

**2.** **缺乏令人信服和流行的商业产品**。在音乐 AI 潜力变得明显之后，许多初创公司纷纷成立，开始致力于商业产品的开发。然而，随着这些产品的开发和测试，音乐家和企业急切地等待将 AI 技术融入他们的工作流程的机会。

然而，在 2023 年音乐 AI 技术成功之后，我对研究人员和公司在这两个方面取得进展感到乐观。在这篇文章中，我想强调我希望在 2024 年看到的三个具体发展，并解释它们为何重要。凭借这些预期的进展，**2024 年将站在通过 AI 彻底改变我们与音乐互动的前沿**。

# **1\. 灵活自然的源分离**

![](../Images/8c0180f5a1e606558e9e933cde71196c.png)

源分离的可视化。图像摘自作者的[博客文章](https://medium.com/towards-data-science/ai-music-source-separation-how-it-works-and-why-it-is-so-hard-187852e54752)。

## 什么是源分离？

音乐源分离是将完整制作的音乐分解为其原始乐器源（例如，人声、节奏、键盘）的任务。如果你从未听说过源分离，我写了一篇完整的[博客文章](https://medium.com/towards-data-science/ai-music-source-separation-how-it-works-and-why-it-is-so-hard-187852e54752)讲解了它是如何工作的，以及为什么这是一个如此具有挑战性的技术问题。

源分离领域的第一个重大突破发生在2019年，当时Deezer发布了[分离器（Spleeter）](https://github.com/deezer/spleeter)作为开源工具。自这一技术飞跃以来，该领域经历了相对稳定的**小步改进**。然而，如果将原始的Spleeter与现代开源工具如Meta的[DEMUCS](https://github.com/facebookresearch/demucs)或商业解决方案如[LALAL.ai](https://www.lalal.ai/)进行比较，差异显得非常明显。因此，在经历了多年缓慢、渐进的进展后，为什么我会期待源分离在2024年爆发呢？

## 为什么我们应该期待源分离的突破？

首先，源分离是**其他音乐AI问题的基石技术**。拥有一个快速、灵活且自然的源分离工具，可以将音乐分类、标记或数据增强提升到新水平。许多研究人员和公司正在仔细观察源分离领域的进展，准备在下一次突破发生时采取行动。

其次，**不同种类的突破**将推动该领域的发展。最明显的是分离质量的提升。虽然我们肯定会看到这方面的进展，但我不期望会有重大飞跃（如果错了我很乐意接受）。不过，除了输出质量，源分离算法还有两个其他问题：

**1\. 速度：** 源分离通常运行在大型生成神经网络上。对于单个音轨，这可能还可以。然而，对于商业应用中遇到的较大工作负载，速度通常仍然太慢——尤其是在推理过程中执行源分离时。

**2\. 灵活性：** 一般而言，源分离工具提供一套固定的源（例如“人声”、“鼓声”、“低音”、“其他”）。传统上，无法执行根据用户需求定制的源分离，因为这需要针对这一任务训练一个全新的神经网络。

一旦源分离的速度足够快以在推理过程中进行（即每次模型预测之前），许多有趣的应用将会出现。例如，我曾写过关于利用源分离[使黑箱音乐AI可解释](https://example.org)的潜力。我认为**速度优化的商业兴趣非常大**，这可能会推动明年的突破。

此外，当前代源分离AI的灵活性有限，使其在各种应用场景中无法使用，即便原则上具备潜力。在一篇名为[Separate Anything You Describe](https://arxiv.org/abs/2308.05037)的论文中，研究人员今年推出了**基于提示的源分离**系统。想象一下在文本框中输入“给我第二段的主合成器，但不带延迟效果”，然后得到你所需的源音频。这就是我们所期待的潜力。

## 摘要：源分离

总之，由于其在音乐AI中的重要性以及速度和灵活性的持续改进，音乐源分离在2024年可能会取得重大进展。新的发展，如基于提示的系统，使其更具用户友好性和适应性。这一切都预示着在行业中的更广泛应用，这可能激励该领域的研究突破。

# **2\. 通用音乐嵌入**

![](../Images/47b8a9da8c078044f181c43752a3b6f4.png)

图像由DALL-E 3生成。

## 自然语言处理（NLP）中的嵌入

为了理解音乐嵌入是什么以及它们为何重要，让我们来看看自然语言处理（NLP）这一术语的起源。在NLP中嵌入出现之前，该领域主要依赖于更简单的基于统计的方法来理解文本。例如，在简单的词袋（BoW）方法中，你只需统计词汇表中每个单词在文本中出现的频率。这使得BoW**不比一个简单的词云更有用**。

![](../Images/769978b4f394ddefecb223445cd6b2d0.png)

一个简单的词云示例。图像作者提供。

嵌入的引入显著改变了自然语言处理（NLP）的格局。嵌入是单词（或短语）的数学表示，其中单词之间的语义相似性通过这些嵌入空间中的向量之间的距离体现出来。**简单来说**，单词、句子或整本书的意义可以被压缩成一堆数字。通常，每个单词/文本的`100`到`1000`个数字已经足以数学上捕捉其意义。

![](../Images/622aa45a9a1d11d3db042d1460cc449f.png)

在[Tensorflow Embedding Projector](https://projector.tensorflow.org/)上使用t-SNE可视化的Word2Vec（10k）嵌入。突出显示了与“violin”最相似的前5个单词。截图由作者提供。

在上图中，你可以看到基于其数值嵌入的10,000个单词在三维图表中的表示。因为这些嵌入捕捉了每个单词的意义，我们可以简单地在图表中查找最接近的嵌入，以找到类似的术语。这样，我们可以轻松识别出与“violin”最相似的5个术语：“cello”、“concerto”、“piano”、“sonata”和“clarinet”。

嵌入的主要优势：

+   **上下文理解：** 与早期的方法不同，嵌入对上下文敏感。这意味着相同的单词可以根据在不同句子中的使用情况具有不同的嵌入，从而提供更为细致的语言理解。

+   **语义相似性：** 具有相似意义的单词通常在嵌入空间中靠得很近，这使得嵌入非常适合用于音乐搜索引擎或推荐系统中的检索任务。

+   **预训练模型：** 借助像BERT这样的模型，嵌入从大量文本中学习，并可以针对特定任务进行微调，从而显著减少对任务特定数据的需求。

## 音乐的嵌入

因为嵌入不过是数字，**原则上任何东西都可以压缩成有意义的嵌入**。下图给出了一个例子，其中不同的音乐类型根据其相似性在二维空间中进行可视化。

![](../Images/93c81ab4f59c412f3106c0e17b528fd7.png)

音乐类型嵌入在 [Every Noise at Once](https://everynoise.com/) 上的二维空间中可视化。截图由作者提供。

然而，尽管嵌入在工业和学术界已经成功使用了超过5年，我们仍然**没有广泛采用的领域特定音乐嵌入模型**。显然，利用嵌入技术在音乐领域具有巨大的经济潜力。以下是一些可以立即实施的嵌入使用案例，前提是能够获得高质量的音乐嵌入：

1.  **音乐相似性搜索**：在任何音乐数据库中搜索与给定参考曲目相似的曲目。

1.  **文本到音乐搜索**：通过自然语言搜索音乐数据库，而不是使用预定义标签。

1.  **高效机器学习**：基于嵌入的模型通常需要比传统基于频谱图或类似音频表示的方法少10到100倍的数据进行训练。

到2023年，我们在开源高质量音乐嵌入模型方面已经取得了很大进展。例如，[Microsoft](https://github.com/microsoft/CLAP) 和 [LAION](https://github.com/LAION-AI/CLAP) 都分别发布了针对通用音频领域训练的 CLAP 模型（特定类型的嵌入模型）。然而，这些模型大多是在语音和环境声音上训练的，使得它们在音乐方面的效果**较差**。随后，Microsoft 和 LAION 发布了针对音乐数据单独训练的音乐特定版本 CLAP 模型。 [M-A-P](https://m-a-p.ai/#) 今年也发布了多个令人印象深刻的音乐特定嵌入模型。

我在测试所有这些模型后的印象是，我们越来越接近目标，但仍未达到三年前文本嵌入所能实现的效果。在我看来，**主要瓶颈仍然是数据**。我们可以假设像 Google、Apple、Meta、Spotify 等所有主要参与者已经有效地使用音乐嵌入模型，因为他们可以访问巨量的音乐数据。然而，开源社区还没有能够赶上并提供一个令人信服的模型。

## 摘要：通用音乐嵌入

嵌入技术是一种有前景的技术，它使检索任务更准确，并在数据稀缺时支持机器学习。不幸的是，针对音乐的突破性领域特定嵌入模型尚未发布。我希望并怀疑，开源项目或甚至致力于开源发布的大型公司（如 Meta）将在 2024 年解决这个问题。我们已经很接近，一旦达到一定水平的嵌入质量，每家公司都将采用基于嵌入的音乐技术，以在更短时间内创造更多价值。

# **3\.** 弥合技术与实际应用之间的差距

![](../Images/c671d1a630565116a82998dd67eb9e8c.png)

使用 DALL-E 3 生成的图像。

2023 年是个奇怪的一年……一方面，AI 已成为科技界最大的热门词汇，几乎所有终端用户和企业都能找到 ChatGPT、Midjourney 等的应用场景。另一方面，只有少数实际完成的产品已被推出并广泛采用。当然，Drake 现在可以唱“我的心会继续”，但迄今为止，这项技术周围尚未构建出商业案例。而且，是的，AI 现在可以为节拍制作人生成声音样本。然而，实际上，一些作曲家正在努力微调自己的 AI 模型以应对 **缺乏有吸引力的商业解决方案**。

从这个角度看，音乐 AI 的最大突破可能不是花哨的研究创新。相反，它可能是基于 AI 的产品和服务在满足企业或最终用户需求上的成熟度提升。在这条路上，任何想要构建音乐 AI 产品的人都还面临许多挑战：

1.  **了解音乐行业或最终用户的需求**：技术本身通常对用例无特定要求。了解技术如何满足实际需求是一个关键挑战。

1.  **将花哨的演示转变为稳健的产品**：今天，数据科学家可以在一天内构建一个聊天机器人原型甚至一个音乐生成工具。然而，将一个有趣的演示转变为一个有用、安全和成熟的产品是要求高且耗时的。

1.  **应对知识产权和许可问题**：伦理和法律考虑使公司和用户在提供或采用基于 AI 的产品时犹豫不决。

1.  **确保资金/投资和首个收入来源**：在 2023 年，已经创立了无数音乐 AI 初创公司。强有力的愿景和明确的商业案例将是确保资金和推动产品开发的必要条件。

1.  **市场营销和用户采纳**：即使是最伟大的创新产品现在也容易被忽视。最终用户和企业被关于 AI 未来的报告和承诺淹没，难以触及目标受众。

例如，我们可以更详细地了解AI如何通过数字音频工作站（DAW）的新插件来影响音乐制作。在最近的[博客文章](https://blog.native-instruments.com/ai-powered-plugins/)中，Native Instruments展示了10个新的AI驱动插件。为了展示现有的可能性，我们来看一下[Audialab 的“Emergent Drums 2”](https://audialab.com/?ref=sergei18&gad_source=1&gclid=Cj0KCQiA1rSsBhDHARIsANB4EJZIqsQGOspW7hEmO_ybIio7oN06fqzjCom6R0pCpFdnr5DMH1PZADQaAvL7EALw_wcB)。Emergent Drums 允许音乐家用生成AI**从零开始设计他们的鼓样本**。该插件很好地集成到DAW中，并作为一个功能齐全的鼓机插件运行。请亲自查看：

演示视频：“Emergent Drums”由Audialab提供。

再次放眼远眺，音乐AI的潜在应用广泛，从音乐制作到教育、市场营销和分销。利用AI的巨大技术潜力为这些领域提供实际价值将是明年面临的关键挑战。

## 摘要：从研究到产品

2023年是音乐AI的一个重要年份，为未来铺平了道路。2024年的真正改变者是什么？不仅仅是技术——而是让它在现实场景中为真实的人群服务。预计音乐AI将走出实验室，融入我们的生活，影响我们创作和消费音乐的方式。

# 你好，2024。

2023年为AI及其可能性奠定了技术基础，并提高了公众意识。这就是为什么，我估计2024年可能是开始开发音乐AI产品的最佳年份。当然，**这些产品中的许多将会失败**，一些AI的承诺最终也会落空。看一下下面图中的著名的Gartner Hype Cycle，我们应该提醒自己这是正常现象。

![](../Images/042a0e664663c00fd7b16768e6b7cfd3.png)

Gartner Hype Cycle。图片由作者提供。

没有人可以确定我们目前在这个炒作周期的哪个位置（如果有人知道，请告诉我）。不过，考虑到今年创造的所有基础工作和公众意识，2024年有可能成为**历史性的**AI音乐技术里程碑年。我非常期待明年会带来什么。

**成为音乐家真是个好时光！**

# 关于我

我是一名音乐学家和数据科学家，分享我对AI与音乐当前话题的看法。以下是与本文相关的一些以往作品：

+   **Meta的AI如何基于参考旋律生成音乐**：[https://medium.com/towards-data-science/how-metas-ai-generates-music-based-on-a-reference-melody-de34acd783](https://medium.com/towards-data-science/how-metas-ai-generates-music-based-on-a-reference-melody-de34acd783)

+   **MusicLM：谷歌是否解决了AI音乐生成问题？**：[https://medium.com/towards-data-science/musiclm-has-google-solved-ai-music-generation-c6859e76bc3c](https://medium.com/towards-data-science/musiclm-has-google-solved-ai-music-generation-c6859e76bc3c)

+   **AI音乐源分离：它是如何工作的，以及为何如此困难**：[https://medium.com/towards-data-science/ai-music-source-separation-how-it-works-and-why-it-is-so-hard-187852e54752](https://medium.com/towards-data-science/ai-music-source-separation-how-it-works-and-why-it-is-so-hard-187852e54752)

在[Medium](https://medium.com/@maxhilsdorf)和[Linkedin](https://www.linkedin.com/in/max-hilsdorf/)上找到我！
