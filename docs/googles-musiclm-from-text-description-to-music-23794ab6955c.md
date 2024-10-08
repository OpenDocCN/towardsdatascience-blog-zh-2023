# Google 的 MusicLM：从文本描述到音乐

> 原文：[`towardsdatascience.com/googles-musiclm-from-text-description-to-music-23794ab6955c`](https://towardsdatascience.com/googles-musiclm-from-text-description-to-music-23794ab6955c)

## 一个新模型仅凭文本提示就能生成令人印象深刻的音乐

[](https://salvatore-raieli.medium.com/?source=post_page-----23794ab6955c--------------------------------)![Salvatore Raieli](https://salvatore-raieli.medium.com/?source=post_page-----23794ab6955c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----23794ab6955c--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----23794ab6955c--------------------------------) [Salvatore Raieli](https://salvatore-raieli.medium.com/?source=post_page-----23794ab6955c--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----23794ab6955c--------------------------------) ·阅读时间 8 分钟·2023 年 2 月 1 日

--

![](img/effa592eb7e806938eac69eab439c2d5.png)

图片由作者使用 OpenAI DALL-E 生成

Google 发布了一种新模型，能够从 [文本描述生成音乐](https://arxiv.org/pdf/2301.11325.pdf)。结果？令人印象深刻。

**实际上，将生成性人工智能应用于音乐并不是一个新概念。** 近年来已经有几个尝试，包括 Riffusion、[Dance Diffusion](https://techcrunch.com/2022/10/07/ai-music-generator-dance-diffusion/)、微软的 Museformer 和 [OpenAI 的 Jukebox](https://openai.com/blog/jukebox/)。Google 自身之前发布过一个名为 [AudioML](https://ai.googleblog.com/2022/10/audiolm-language-modeling-approach-to.html) 的模型。**为什么这个模型会有所不同？**

[](https://medium.com/mlearning-ai/microsofts-museformer-ai-music-is-the-new-frontier-8dc5cb24459c?source=post_page-----23794ab6955c--------------------------------) [## Microsoft 的 Museformer：人工智能音乐是新前沿

### 人工智能艺术正在蓬勃发展，音乐可能是下一个。

[medium.com](https://medium.com/mlearning-ai/microsofts-museformer-ai-music-is-the-new-frontier-8dc5cb24459c?source=post_page-----23794ab6955c--------------------------------)

# 为什么用人工智能生成音乐如此困难？

![](img/bdab379e9038b789e6a153a8df2fdc7a.png)

图片由 [Marius Masalar](https://unsplash.com/it/@marius) 在 unsplash.com 提供

与此同时，之前的模型显示出明显的技术和质量问题。结果陈旧，歌曲复杂度不高，常常重复，并且音质依然不高。

生成音乐并不容易，[正如众多尝试所示。](https://www.vice.com/en/article/qvq54v/why-is-ai-generated-music-still-so-bad) 作者们常使用 MIDI，但生成高保真音乐是另一回事。**音乐还有复杂的结构**，你必须考虑旋律和和声，并且有重复的模式，这些模式在时间上和距离上都会重复。

正如文章的作者指出的那样，生成文本到图像相对容易，而文本到音乐的尝试仅限于：“由几个声音事件组成的简单声学场景，持续几秒钟。”

在这里，我们谈论的是从单一文本说明开始，并生成具有长期结构的复杂音频。**他们是如何实现这一点的？**

[](https://pub.towardsai.net/googles-audiolm-generating-music-by-hearing-a-song-s-snippet-c9512a9290cd?source=post_page-----23794ab6955c--------------------------------) [## 谷歌的 AudioLM：通过听歌曲片段生成音乐

### 无论是音乐还是语音，谷歌的新模型都能继续播放所听到的内容。

[pub.towardsai.net](https://pub.towardsai.net/googles-audiolm-generating-music-by-hearing-a-song-s-snippet-c9512a9290cd?source=post_page-----23794ab6955c--------------------------------)

同时，正如他们所解释的，AudioLM 被用作起点。之前的模型能够将旋律继续下去，并且保持一致性。**然而，仍然有几个技术限制需要克服**：

+   这种模型的第一个主要限制是“音频-文本配对数据的稀缺”。事实上，文本到图像的训练得以实现是因为有大量的图像，且可以使用替代文本作为说明。

+   用几句话描述音乐的显著特征，如声学场景或旋律的音色，并不容易。

+   此外，音乐在时间维度上展开，因此有可能出现说明和音乐之间的链接较弱的风险（与静态图像相比）。

# MusicLM：结构、训练、结果和限制

![](img/a1555ff223923fa9f981e34c6cd8a49c.png)

图片来源：[James Stamler](https://unsplash.com/it/@jamesstamler) 于 unsplash.com

**这个模型的第一个组件是 MuLan**（也是核心部分）。这个模型用于构建音乐-文本联合嵌入，由两个嵌入塔组成（一个用于文本输入，一个用于音乐输入）。这两个塔是预训练的[ BERT](https://en.wikipedia.org/wiki/BERT_(language_model)) 和 [ResNeT-50](https://www.kaggle.com/datasets/keras/resnet50) 的变体，用于音频。

MuLan 在音乐片段及其相应的文本注释对上进行训练。

![](img/c44a1e8372e26a7efcff602f99325887.png)

MusicLM 的三个组件已分别进行预训练。其他两个组件如下文所述。图片来源：[这里](https://arxiv.org/pdf/2301.11325.pdf)

正如我们在图像中看到的那样，作者提到还有两个其他组件。正如作者解释的：

> 我们使用 SoundStream 的自监督音频表示作为声学标记，以实现高保真合成，并使用 w2vBERT 作为语义标记，以促进长期一致的生成。在条件表示方面，我们在训练期间依赖 MuLan 音乐嵌入，在推断时依赖 MuLan 文本嵌入。

尽管这个系统看起来很复杂，但它有几个优点：它可以快速扩展，并且使用[对比损失](https://paperswithcode.com/method/supervised-contrastive-loss)进行嵌入训练可以提高鲁棒性。此外，**单独拥有预训练模型可以更好地处理带有文本输入的音乐**。

**在训练期间**，模型学习将 MuLan 生成的标记映射转换为语义标记（[w2w-BERT](https://arxiv.org/abs/2108.06209)）。然后，声学标记在 MuLan 音频标记和语义标记（[SoundStream](https://arxiv.org/abs/2107.03312)）的条件下进行处理。

![](img/ad0f9c2b66f7140c496a4559afe7afed.png)

训练期间的模型。图片来源：[这里](https://arxiv.org/pdf/2301.11325.pdf)

**在推断期间**，过程是将文本描述提供给 MuLan，MuLan 将其转换为条件信号，接着 w2w-BERT 将其转换为音频标记，然后由 SoundStream 解码器将其转换为波形。

![](img/0b9a6d25c39fd712f1ce6ec88928946d.png)

推断期间的模型。图片来源：[这里](https://arxiv.org/pdf/2301.11325.pdf)

使 MusicLM 如此强大的原因之一是它在 500 万条音频剪辑上进行了训练（总共 280,000 小时的音频）。此外，作者还创建了一个包含 5500 个由专业音乐家编写字幕的音乐剪辑的数据集（该数据集已发布[这里](https://www.kaggle.com/datasets/googleai/musiccaps)）。每个字幕描述了音乐的四句话，并附有音乐方面的列表（如流派、情绪、节奏等）。

正如结果所示，MusicML 在音频质量和文本一致性方面都优于之前的模型（Mubert 和[Riffusion](https://techcrunch.com/2022/12/15/try-riffusion-an-ai-model-that-composes-music-by-visualizing-it/)）。而且在听觉质量方面也是如此。实际上，听众被展示了剪辑并被要求选择哪个剪辑最能代表文本描述（胜利意味着听众在对比中更喜欢该模型）。

![](img/b5196c7011595bca45bbf5e39d318eff.png)

“使用来自 MusicCaps 数据集的字幕对生成样本进行评估。通过 Frechet 音频距离（FAD）比较音频质量，使用 Kullback–Leibler 散度（KLD）和 MuLan 循环一致性（MCC）以及人类听力测试中的胜利次数（Wins）来比较与文本描述的一致性。” 图片来源：[这里](https://arxiv.org/pdf/2301.11325.pdf)

![](img/f05c58aa78510760d95f4a20116631b3.png)

人类听众研究的用户界面。图片来源：[这里](https://arxiv.org/pdf/2301.11325.pdf)

**听到结果时，难免令人印象深刻**，考虑到模型中没有音乐家参与。毕竟，该模型能够捕捉到音乐细节，如乐器即兴演奏和旋律。

此外，该模型不仅限于文本描述，还可以以其他音频作为输入并继续处理（“可以以哼唱、歌唱、吹口哨或演奏乐器的形式提供”）。

**作者还描述了一种称为“故事模式”的方法**，即生成长音频序列，其中文本描述随着时间的推移而变化。该模型生成具有“平滑过渡、节奏一致且语义合理的音乐序列，同时根据文本描述改变音乐背景”的音乐序列。这些描述还可以包含“冥想时间”或“跑步时间”的描述，从而创建一个将适当音乐与之关联的叙事。

简而言之，该模型并不仅限于对某一乐器或音乐流派的描述，还可以根据活动、时代、地点或情绪的描述进行调整。因此，MusicLM 也可以用于电影配乐、锻炼应用等。

在此链接中，你还可以阅读字幕并听到模型的结果。

[## MusicLM

### Andrea Agostinelli, Timo I. Denk, Zalán Borsos, Jesse Engel, Mauro Verzetti, Antoine Caillon, Qingqing Huang, Aren…

[google-research.github.io](https://google-research.github.io/seanet/musiclm/examples/?source=post_page-----23794ab6955c--------------------------------)

**尽管模型令人印象深刻，但并不完美**，一些音频片段的质量存在失真（但未来可能通过训练调整得到改善）。此外，该模型还可以生成合唱和人声，但歌词通常不是英语，而是一种无意义的语言，并且声音听起来更像是多位歌手的混合，而非连贯的人声。因此，作者如是说：

> 未来的工作可能会集中在歌词生成上，同时改进文本条件和声音质量。另一个方面是建模高级的歌曲结构，如引言、诗句和副歌。以更高的采样率建模音乐是一个额外的目标。

Google 决定不分发该模型：“我们的模型及其处理的用例存在一些风险。”正如作者所指出的，该模型反映了训练数据中存在的偏见，这在为数据集中代表性不足的文化生成音乐时会带来问题。他们还指出，该模型提出了关于文化挪用的伦理问题。

![](img/91c6cb3068c877ccfccb293aaa961ea2.png)

“MusicCaps 中所有 5.5k 示例的流派分布，基于 AudioSet 分类器”。图片来源：[这里](https://arxiv.org/pdf/2301.11325.pdf)

另一个问题是，在 1%的情况下，模型生成了在训练期间听到的音乐，复制了现有歌曲的部分（甚至是受版权保护的元素）。[TechCrunch 认为这足以](https://techcrunch.com/2023/01/27/google-created-an-ai-that-can-generate-music-from-text-descriptions-but-wont-release-it/?guccounter=1&guce_referrer=aHR0cHM6Ly93d3cuZ29vZ2xlLmNvbS8&guce_referrer_sig=AQAAAK6OO6H73AxKjuqHai2HzS-KZFESx33sBYlc-8NKW0jnv6BUQlduIQ2-LqwaZr6ryfZq0BQ6F03HrFCY3mDS5CnYUWLZiNskZK-YzWxP943yOaC4gaKFfI7owLanSlNFG_kazxH79ZN6G_71N5ZVyK7AtziTDGsXAUVoNW_daWxZ)劝阻谷歌发布模型（尤其是在机构有意规范人工智能和生成工具的情况下）。

他们认识到与使用案例相关的创意内容潜在的误用风险，并进一步强调了深入研究和分析的必要性，得出结论：

> 我们强烈强调需要更多的未来工作来应对与音乐生成相关的这些风险——目前我们没有发布模型的计划。

[## 欧盟想要规范你最喜欢的人工智能工具](https://medium.com/mlearning-ai/the-eu-wants-to-regulate-your-favorite-ai-tools-7419d985f2f2?source=post_page-----23794ab6955c--------------------------------)

### 欧盟正在准备一项新的人工智能法案，生成式人工智能也被包含在内。

[medium.com](https://medium.com/mlearning-ai/the-eu-wants-to-regulate-your-favorite-ai-tools-7419d985f2f2?source=post_page-----23794ab6955c--------------------------------)

# **结论**

MusicLM 在从文本描述生成音乐方面表现出色，相较于之前的模型是一个质的飞跃。它生成连贯的音乐，长序列能够捕捉音乐的细微差别。尽管它已经用大量数据进行了训练，但仍然不完美。

同样，正如作者所描述的，由于模型生成了受版权保护的音乐，他们没有计划发布它。这可能也源于一些程序员最近[起诉了 GitHub Copilot](https://www.theverge.com/2022/11/8/23446821/microsoft-openai-github-copilot-class-action-lawsuit-ai-copyright-violation-training-data)和[Getty images 起诉了 Stability AI](https://www.theverge.com/2023/1/17/23558516/ai-art-copyright-stable-diffusion-getty-images-lawsuit)以及 MidJourney。

此外，正如作者所指出的，生成音乐模型正在从训练集中引入偏见。MusicLM 不仅生成器乐音乐，还可以生成声乐和合唱，这可能导致生成的音乐中包含偏见和有害内容。

此外，正如多位音乐家所指出的，流媒体已经稀薄了音乐家和作曲家的收入，而[生成式人工智能可能进一步减少他们的收入](https://biz.crast.net/what-happens-to-songwriters-when-ai-can-generate-music/)。

相反，作者建议这些模型将不会取代音乐家和作曲家，而是很快成为辅助人类的一组工具。

# 如果你觉得有趣：

你可以查找我的其他文章，你也可以[**订阅**](https://salvatore-raieli.medium.com/subscribe)以在我发布新文章时收到通知，还可以在[**LinkedIn**](https://www.linkedin.com/in/salvatore-raieli/)**上联系我**。感谢你的支持！

这是我 GitHub 代码库的链接，我计划在这里收集与机器学习、人工智能等相关的代码和资源。

[GitHub - SalvatoreRa/tutorial: Tutorials on machine learning, artificial intelligence, data science…](https://github.com/SalvatoreRa/tutorial?source=post_page-----23794ab6955c--------------------------------)

### 包含数学解释和可重用代码（Python）的机器学习、人工智能、数据科学教程……

[GitHub - SalvatoreRa/tutorial: Tutorials on machine learning, artificial intelligence, data science…](https://github.com/SalvatoreRa/tutorial?source=post_page-----23794ab6955c--------------------------------)
