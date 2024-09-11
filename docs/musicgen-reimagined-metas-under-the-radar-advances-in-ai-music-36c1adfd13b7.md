# MusicGen 重新构想：Meta 在 AI 音乐领域的低调进展

> 原文：[https://towardsdatascience.com/musicgen-reimagined-metas-under-the-radar-advances-in-ai-music-36c1adfd13b7?source=collection_archive---------6-----------------------#2023-11-21](https://towardsdatascience.com/musicgen-reimagined-metas-under-the-radar-advances-in-ai-music-36c1adfd13b7?source=collection_archive---------6-----------------------#2023-11-21)

## 探索被忽视但非凡的 MusicGen 进展

[](https://medium.com/@maxhilsdorf?source=post_page-----36c1adfd13b7--------------------------------)[![Max Hilsdorf](../Images/01da76c553e43d5ed6b6849bdbfd00da.png)](https://medium.com/@maxhilsdorf?source=post_page-----36c1adfd13b7--------------------------------)[](https://towardsdatascience.com/?source=post_page-----36c1adfd13b7--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----36c1adfd13b7--------------------------------) [Max Hilsdorf](https://medium.com/@maxhilsdorf?source=post_page-----36c1adfd13b7--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fd0c085a74ae8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmusicgen-reimagined-metas-under-the-radar-advances-in-ai-music-36c1adfd13b7&user=Max+Hilsdorf&userId=d0c085a74ae8&source=post_page-d0c085a74ae8----36c1adfd13b7---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----36c1adfd13b7--------------------------------) · 7分钟阅读 · 2023年11月21日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F36c1adfd13b7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmusicgen-reimagined-metas-under-the-radar-advances-in-ai-music-36c1adfd13b7&user=Max+Hilsdorf&userId=d0c085a74ae8&source=-----36c1adfd13b7---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F36c1adfd13b7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmusicgen-reimagined-metas-under-the-radar-advances-in-ai-music-36c1adfd13b7&source=-----36c1adfd13b7---------------------bookmark_footer-----------)![](../Images/c3484466a64a8a80c37a2d3e3344d68c.png)

一张象征 Music AI 产品如何提升音乐创作的图像。图像通过与 ChatGPT 和 DALL-E-3 的对话生成。

# 事情是怎么开始的……

在2023年2月，谷歌推出了其生成音乐 AI *MusicLM*，引起了轰动。那时，两个事实变得很清楚：

1.  **2023年将成为基于 AI 的音乐生成突破年**

1.  **一个新模型很快就会超越 MusicLM**

许多人预期下一个突破性模型在模型参数和训练数据方面将是 MusicLM 的十倍。这也会引发相同的伦理问题，包括对源代码的限制访问以及使用受版权保护的训练材料。

今天，我们知道这些说法只有一半是正确的。

发布于 2023 年 6 月，Meta 的 MusicGen 模型带来了一些巨大的改进，包括…

1.  更高质量的音乐输出（24kHz → 32kHz）

1.  更自然的乐器声音

1.  可以根据任何旋律进行生成的选项（我写了一篇关于此的 [博客文章](https://medium.com/towards-data-science/how-metas-ai-generates-music-based-on-a-reference-melody-de34acd783)）

…同时使用**更少**的训练数据，开源代码和模型权重，仅使用商业授权的训练材料。

六个月后，炒作逐渐平息。然而，Meta 的研究团队*FAIR*继续发表论文并更新代码，以逐步改进 MusicGen。

# …当前的进展

自发布以来，Meta 在两个关键方面升级了 MusicGen：

1.  使用多频段扩散进行更高质量的生成

1.  由于立体声生成，输出更加生动

尽管这听起来像是两个小改进，但差别很大。自己听听吧！以下是使用原始 MusicGen 模型（3.3B 参数）生成的 10 秒钟片段：

从官方 MusicGen [演示页面](https://ai.honu.io/papers/musicgen/) 提取的生成曲目。

使用的提示是：

> 具有自然质感、环保意识、尤克里里融入、和谐、轻快、随意、自然乐器、柔和的节奏

现在，这里有一个基于相同提示生成的 MusicGen 输出示例，展示了六个月后的效果：

使用 MusicGen 3.3B 立体声生成的曲目由作者创作。

如果你通过智能手机扬声器收听，可能不会很明显。使用其他设备时，你应该能够听出整体声音更加清晰自然，立体声使得作品更加生动和令人兴奋。

在这篇博客文章中，我想展示这些改进，解释它们的重要性和工作原理，并提供一些示例生成。

# 多频段扩散——这有什么用？

要了解多频段扩散是什么及其重要性，让我们看看原始 MusicGen 模型 **[1]** 是如何生成其输出的。

34kHz 采样率的 30 秒音频在计算机中表示为近 100 万个数字。逐样生成这样的音频相当于使用 ChatGPT 生成 10 部完整的小说。

相反，Meta 依赖于神经音频压缩技术。他们的压缩模型 EnCodec **[2]** 可以将音乐从 34kHz 压缩到大约 0.05kHz，同时保持重建到原始采样率所需的相关信息。EnCodec 包括一个编码器，用于压缩音频，以及一个解码器，用于重建原始声音（*图 1*）。

![](../Images/9a4432d91696820637aea7862ec61b7c.png)

图1 — Encodec：Meta的神经音频压缩模型。图片作者提供。

现在回到MusicGen。它不是以全采样率生成音乐，而是以0.05kHz生成，并让EnCodec“重建”，从而在最小的计算时间和成本下实现高保真输出（*图2*）。

![](../Images/98f254e3ca2879f08d9c6f73760b1225.png)

图2 — MusicGen：用户提示（文本）被转换为编码音频信号，然后解码以产生最终结果。图片作者提供。

尽管EnCodec是一项令人印象深刻的技术，但其压缩并非无损。与原始音频相比，重建后的音频中存在明显的伪影。请亲自聆听！

**原始音频**

EnCodec音乐示例取自官方的[EnCodec演示页面](https://ai.honu.io/papers/encodec/samples.html)。

**重建音频**

EnCodec音乐示例取自官方的[EnCodec演示页面](https://ai.honu.io/papers/encodec/samples.html)。

由于MusicGen完全依赖于EnCodec，因此它是生成音乐质量的主要瓶颈。这就是为什么Meta决定改进EnCodec的解码部分。2023年8月，他们开发了一个更新的解码器，利用了多频带扩散**[3]**。

Meta发现EnCodec的原始解码器有一个问题，就是它倾向于先生成低频，然后生成高频。不幸的是，这意味着低频中的任何错误/伪影也会扭曲高频，从而大幅降低输出质量。

多频带扩散通过在组合之前独立生成频谱的不同部分来解决这个问题。研究人员发现，这一过程显著改善了生成的输出。从我的角度来看，差异非常明显。请用原始EnCodec解码器和多频带扩散解码器聆听相同的曲目：

**原始解码器**

生成的曲目取自多频带扩散[演示页面](https://ai.honu.io/papers/mbd/)。

**多频带扩散解码器**

生成的曲目取自多频带扩散[演示页面](https://ai.honu.io/papers/mbd/)。

当前文本到音乐系统的核心问题之一是其生成的声音总是有一种不自然的质量，尤其是对于声学乐器。多频带扩散使输出的声音更加干净自然，将MusicGen提升到了一个新的水平。

# 为什么立体声如此重要？

到目前为止，大多数生成音乐模型生成的是单声道声音。这意味着MusicGen不会将声音或乐器放置在左侧或右侧，从而导致混音较少生动和兴奋。立体声被大多忽视的原因是生成立体声并非一项简单的任务。

作为音乐家，当我们制作立体声信号时，我们可以访问混音中的各个乐器轨道，并将它们放置在我们想要的位置。MusicGen 并不是分别生成所有乐器，而是生成一个合成的音频信号。在没有这些乐器源的情况下，创建立体声音效是困难的。不幸的是，将音频信号拆分成各个源是一个棘手的问题（我已经发布了一篇 [博客文章](https://medium.com/towards-data-science/ai-music-source-separation-how-it-works-and-why-it-is-so-hard-187852e54752) 讨论了这个问题），而且技术仍然未完全成熟。

因此，Meta 决定将立体声生成直接集成到 MusicGen 模型中。他们使用包含立体声音乐的新数据集，训练 MusicGen 生成立体声输出。研究人员声称，生成立体声与单声道相比没有额外的计算成本。

尽管我觉得论文中对立体声过程的描述并不十分清楚，但我的理解是这样的（*图 3*）：MusicGen 已经学会生成两个压缩音频信号（左声道和右声道），而不是一个单声道信号。这些压缩信号必须分别解码，然后再结合起来生成最终的立体声输出。这个过程不会花费两倍的时间，因为 MusicGen 现在可以在大约生成一个信号的时间内同时生成两个压缩音频信号。

![](../Images/48157fe5a80cc2bfa76eab5aed542464.png)

图 3 — MusicGen 立体声更新。请注意，由于论文中对该过程的记录不够充分，我不能 100% 确定这一点。请将其视为一个有根据的猜测。图片由作者提供。

能够生成令人信服的立体声效果确实使 MusicGen 从其他先进模型如 MusicLM 或 Stable Audio 中脱颖而出。从我的角度来看，这个“微小”的增加在生成音乐的生动性方面产生了巨大的差异。自己听听吧（在智能手机扬声器上可能很难听到）：

**单声道**

**立体声**

# 结论

自发布之日起，MusicGen 就给人留下了深刻的印象。然而，从那时起，Meta 的 FAIR 团队不断改进他们的产品，提供了更高质量、更逼真的效果。在生成音频信号（而非 MIDI 等）的文本到音乐模型方面，我认为 MusicGen 在竞争对手中处于领先地位（截至 2023 年 11 月）。

此外，由于 MusicGen 及其相关产品（EnCodec、AudioGen）都是开源的，它们成为了令人难以置信的灵感来源和有志于成为 AI 音频工程师的首选框架。如果我们看看 MusicGen 在短短 6 个月内取得的进步，我只能想象 2024 年将是一个激动人心的年份。

另一个重要的点是，Meta通过其透明的方法，也为希望将这项技术集成到音乐软件中的开发人员做出了基础工作。生成样本、头脑风暴音乐创意或者改变现有作品的风格 —— 这些是我们已经开始看到的一些令人兴奋的应用。通过足够的透明度，我们可以确保我们正在构建一个未来，使得人工智能不仅仅是对人类音乐能力的威胁，而是让音乐创作变得更加令人兴奋的未来。

注意：虽然MusicGen是开源的，但预训练模型可能不允许商业使用！访问audiocraft的[GitHub仓库](https://github.com/facebookresearch/audiocraft)以获取有关其所有组件预期使用的更详细信息。

## **参考文献**

**[1] Copet等人（2023）**。简单可控的音乐生成。[https://arxiv.org/pdf/2306.05284.pdf](https://arxiv.org/pdf/2306.05284.pdf)

**[2] Défossez等人（2022）**。高保真神经音频压缩。[https://arxiv.org/pdf/2210.13438.pdf](https://arxiv.org/pdf/2210.13438.pdf)

**[3] Roman等人（2023）**。从离散标记到高保真音频使用多频带扩散。[https://arxiv.org/abs/2308.02560](https://arxiv.org/abs/2308.02560)

## 关于我

嗨！我是一名音乐学家和数据科学家，分享我对人工智能和音乐当前主题的见解。这里是我之前与本文相关的一些工作：

+   **Meta的AI如何基于参考旋律生成音乐**：[https://medium.com/towards-data-science/how-metas-ai-generates-music-based-on-a-reference-melody-de34acd783](https://medium.com/towards-data-science/how-metas-ai-generates-music-based-on-a-reference-melody-de34acd783)

+   **MusicLM：Google解决了AI音乐生成问题吗？**：[https://medium.com/towards-data-science/musiclm-has-google-solved-ai-music-generation-c6859e76bc3c](https://medium.com/towards-data-science/musiclm-has-google-solved-ai-music-generation-c6859e76bc3c)

+   **AI音乐源分离：工作原理及其难点**：[https://medium.com/towards-data-science/ai-music-source-separation-how-it-works-and-why-it-is-so-hard-187852e54752](https://medium.com/towards-data-science/ai-music-source-separation-how-it-works-and-why-it-is-so-hard-187852e54752)

在[Medium](https://medium.com/@maxhilsdorf)和[Linkedin](https://www.linkedin.com/in/max-hilsdorf/)找到我！
