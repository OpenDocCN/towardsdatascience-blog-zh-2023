# 克服自动语音识别挑战：下一个前沿

> 原文：[`towardsdatascience.com/overcoming-automatic-speech-recognition-challenges-the-next-frontier-e26c31d643cc?source=collection_archive---------2-----------------------#2023-03-30`](https://towardsdatascience.com/overcoming-automatic-speech-recognition-challenges-the-next-frontier-e26c31d643cc?source=collection_archive---------2-----------------------#2023-03-30)

## 自动语音识别技术在各个领域的进展、机遇和影响

[](https://medium.com/@talrosenwein?source=post_page-----e26c31d643cc--------------------------------)![Tal Rosenwein](https://medium.com/@talrosenwein?source=post_page-----e26c31d643cc--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e26c31d643cc--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----e26c31d643cc--------------------------------) [Tal Rosenwein](https://medium.com/@talrosenwein?source=post_page-----e26c31d643cc--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fc25fa765131b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fovercoming-automatic-speech-recognition-challenges-the-next-frontier-e26c31d643cc&user=Tal+Rosenwein&userId=c25fa765131b&source=post_page-c25fa765131b----e26c31d643cc---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e26c31d643cc--------------------------------) ·17 min 阅读·2023 年 3 月 30 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe26c31d643cc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fovercoming-automatic-speech-recognition-challenges-the-next-frontier-e26c31d643cc&user=Tal+Rosenwein&userId=c25fa765131b&source=-----e26c31d643cc---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe26c31d643cc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fovercoming-automatic-speech-recognition-challenges-the-next-frontier-e26c31d643cc&source=-----e26c31d643cc---------------------bookmark_footer-----------)![](img/f292fc6b02eed72e8b2ba7f32a1dc8c9.png)

由 [Andrew DesLauriers](https://unsplash.com/@andrewdesla?utm_source=medium&utm_medium=referral) 拍摄于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## TL;DR:

*这篇文章聚焦于自动语音识别（ASR）技术的进展及其对各个领域的影响。ASR 已在多个行业中变得普及，其准确性通过扩大模型规模和构建更大规模的标注和未标注训练数据集得到了提升。*

*展望未来，ASR 技术预计将随着声学模型规模的扩大和内部语言模型的增强而持续改进。此外，自监督和多任务训练技术将使低资源语言受益于 ASR 技术，而多语言训练将进一步提升性能，使许多低资源语言能够进行基本使用，如语音命令。*

*ASR 在生成式 AI 中也将发挥重要作用，因为与虚拟角色的交互将通过音频/文本接口进行。随着无文本 NLP 的出现，一些最终任务，如语音对语音翻译，可能在不使用任何明确的 ASR 模型的情况下得到解决。将发布能够通过文本、音频或两者进行提示的多模态模型，并生成文本或合成音频作为输出。*

*此外，具有语音交互界面的开放式对话系统将提高对转录错误以及书面和口头形式之间差异的鲁棒性。这将增强对挑战性口音和儿童语言的鲁棒性，使 ASR 技术成为许多应用中的重要工具。*

*一个端到端的语音增强-ASR-分段系统将发布，使 ASR 模型能够个性化，并提高在重叠语音和挑战性声学场景中的表现。这是解决 ASR 技术在现实世界场景中的挑战的重要一步。*

*最后，预计将出现一波语音 API。同时，仍然存在小型初创公司在技术/数据采集使用和技术采纳率低的群体中超越大型科技公司的机会。*

# **2022 年回顾**

自动语音识别（ASR）技术在教育、播客、社交媒体、远程医疗、呼叫中心等多个行业中正在获得越来越多的关注。一个很好的例子是语音交互界面（HMI）在消费产品中的普及，如智能汽车、智能家居、智能辅助技术[1]、智能手机，甚至酒店中的人工智能（AI）助手[2]。为了满足对快速且准确响应的日益增长的需求，低延迟 ASR 模型已被部署用于关键词检测[3]、端点检测[4]和转录[5]等任务。带有说话人属性的 ASR 模型[6–7]也越来越受到关注，因为它们能够实现产品个性化，为终端用户提供更大的价值。

**数据的普遍性。** 流媒体音频和视频平台，如社交媒体和 YouTube，导致了未标记音频数据的轻松获取[8]。新引入的自监督技术可以在无需真实标签的情况下利用这些音频[9–10]。这些技术在目标领域提高了 ASR 系统的性能，即使在该领域未对标记数据进行微调[11]。另一种因能够利用这些未标记数据而受到关注的方法是使用伪标签的自训练[12–13]。主要概念是使用自动语音识别（ASR）系统自动转录未标记的音频数据，然后将生成的转录本作为真实标签，用于以监督方式训练另一个 ASR 系统。OpenAI 采取了不同的方法，假设他们可以在网上大规模找到人工生成的转录本。他们通过抓取公开的音频数据及其人工生成的字幕，生成了一个高质量且大规模（640K 小时）的训练数据集。利用这个数据集，他们以完全监督的方式训练了一个 ASR 模型（即 Whisper），在零-shot 设置下在多个基准测试中达到了**最先进的**结果[14]。

**损失。** 尽管 End-2-end（E2E）损失主导了**最先进的**ASR 模型[15–17]，新的损失函数仍在不断发布。一种名为混合自回归转换器（HAT）的新技术[18]被引入，能够通过分离空白和标签后验来测量内部语言模型（ILM）的质量。后来工作[19]使用这种因子分解有效地适配了 ILM，仅利用文本数据，提高了 ASR 系统的整体性能，特别是对专有名词、俚语和名词的转录，这些是 ASR 系统的主要痛点。还开发了新的度量标准，以**更好地对齐人类感知**，克服了词错误率（WER）的语义问题[20]。

**架构选择。** 在声学模型的架构选择方面，Conformer [21] 仍然是流媒体模型的首选，而 Transformers [22] 是非流媒体模型的默认架构。至于后者，引入了仅编码器（基于 wav2vec2 [23–24]）和编码器-解码器（Whisper [14]）的多语言模型，并在多个基准测试中超越了**最先进的**结果。由于模型大小、训练数据量和更大的上下文，这些模型优于流媒体模型。

**科技巨头的多语言 AI 发展。** Google 宣布了其“1,000 语言计划”，以构建支持 1000 种最常用语言的 AI 模型[25]，而 Meta AI 则宣布了其长期努力，旨在构建包含大多数世界语言的语言和机器翻译（MT）工具[26]。

**语音语言突破。** 多模态（语音/文本）和多任务预训练 seq-2-seq（编码器-解码器）模型，如 SpeechT5 [27]，已经发布，在各种语音语言处理任务中取得了巨大成功，包括 ASR、语音合成、语音翻译、语音转换、语音增强和说话人识别。

这些 ASR 技术的进步预计将推动进一步创新，并在未来几年对广泛的行业产生影响。

# **展望未来**

尽管面临挑战，但自动语音识别（ASR）领域预计将在多个领域取得重大进展，从声学和语义建模到对话式和生成式 AI，甚至包括说话人归属的 ASR。本节提供了这些领域的详细见解，并分享了我对 ASR 技术未来的预测。

![](img/09a0b0698a10de53a291ac944b8d7b5e.png)

图片来源：[Nik](https://unsplash.com/@helloimnik?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## **一般改进：**

预计 ASR 系统在声学和语义方面都会有所改进。

在声学模型方面，预计更大的模型和训练数据集将提升 ASR 系统的整体性能，类似于 LLMs 领域的进展。尽管扩展 Transformer 编码器，如 Wav2Vec 或 Conformer，存在挑战，但预计会有突破使其能够扩展，或出现向编码器-解码器架构（如 Whisper）转变的趋势。然而，编码器-解码器架构有一些需要解决的缺陷，如幻觉。优化技术，如 faster-whisper [28] 和 NVIDIA-wav2vec2 [29]，将减少训练和推理时间，从而降低部署大型 ASR 模型的门槛。

在语义方面，研究人员将重点关注通过融入更大的声学或文本上下文来改进 ASR 模型。还将探索在 E2E 训练期间向 ILM 注入大规模无配对文本，如 JEIT [30]。这些努力将有助于克服准确转录命名实体、俚语和名词等关键挑战。

尽管 Whisper 和谷歌的通用语音模型（USM）[31]在多个基准测试中提高了 ASR 系统的性能，但一些基准测试仍需解决，因为词错误率（WER）仍然约为 20% [32]。使用语音**基础模型**、添加更多多样化的训练数据和应用多任务学习将显著提升此类场景中的性能，从而开辟新的商业机会。此外，预计将出现新的指标和基准，以**更好地对齐新的最终任务和领域**，例如医学领域中的非词汇对话声音[33]以及媒体编辑和教育领域中的填充词检测与分类[34]。可能会为此目的开发特定任务的微调模型。最后，随着多模态的发展，预计还会发布更多模型、训练数据集和新任务的基准[35–36]。

随着进展的不断推进，预计会出现一波类似自然语言处理（NLP）的语音 API。谷歌的 USM、OpenAI 的 Whisper 和 Assembly 的 Conformer-1 [37]是一些早期的例子。

尽管听起来有些荒谬，但强制对齐对许多公司来说仍然具有挑战性。一个开源代码可能会帮助许多人实现音频片段与其对应转录文本之间的准确对齐。

## **低资源语言：**

自监督学习、多任务学习和多语言模型的进展预计将显著提高低资源和未书写语言的性能。这些方法将通过利用预训练模型和在相对较少的标记样本上进行微调来实现可接受的性能[24]。另一种有前途的方法是双重学习[38]，这是一种半监督机器学习的范式，旨在通过同时解决两个对立任务（在我们的案例中为文本到语音（TTS）和 ASR）来利用无监督数据。在这种方法中，每个模型生成未标记样本的伪标签，这些伪标签用于训练另一个模型。

此外，使用无配对文本改进 ILM 可以增强模型的鲁棒性，这将特别有利于封闭集挑战，例如语音命令。在一些应用中，如 YouTube 视频的字幕，性能将是可接受的，但不是完美的，而在其他应用中，如法庭上的逐字记录，模型可能需要更多时间才能达到标准。我们预计公司将在 2023 年根据这些模型收集数据，同时手动纠正转录文本，并且我们将在 2024 年在专有数据上微调后看到低资源语言的显著改进。

## **生成式 AI：**

虚拟形象的使用预计将彻底改变人与数字资产的互动。在短期内，ASR 将作为生成式 AI 的基础之一，因为这些虚拟形象将通过文本/听觉界面进行交流。

但未来，随着注意力转向新的研究方向，可能会发生变化。例如，一个可能被采用的新兴技术是 Textless NLP，它代表了一种新的语言建模方法，用于音频生成[39]。这种方法使用可学习的离散音频单元[40]，并以自回归的方式一次生成一个离散音频单元，类似于文本生成。这些离散单元可以随后解码回音频领域。目前，这项技术已经能够生成在句法和语义上都合理的语音继续，同时保持说话人的身份和韵律，对于未见过的说话人也能做到，如 GSLM/AudioLM [39, 41]所示。这项技术的潜力巨大，因为可以在许多任务中跳过 ASR 组件（及其错误）。例如，传统的语音到语音（S2S）翻译方法如下：它们首先将源语言中的话语转录出来，然后使用机器翻译模型将文本翻译为目标语言，最后使用 TTS 引擎生成目标语言的音频。使用 textless-NLP 技术，S2S 翻译可以通过一个直接作用于离散音频单元的编码-解码架构来完成，而不使用任何显式的 ASR 模型[42]。我们预测，未来的 Textless NLP 模型将能够解决许多其他任务，而无需经过显式转录，例如问答系统。然而，这种方法的主要缺点是回溯错误和调试，因为在离散单元空间中工作时，事情将变得不那么直观。

T5 [43] 和 T0 [44] 在 NLP 领域通过利用其多任务训练和展示零样本任务泛化取得了巨大成功。2021 年发布的 SpeechT5 [27] 在各种口语语言处理任务中表现出色。今年早些时候，VALL-E [45] 和 VALL-EX [46] 被发布。它们通过使用 textless NLP 技术展示了 TTS 模型的令人印象深刻的上下文学习能力，仅通过几秒钟的音频就能克隆说话人的声音，并且无需任何微调，甚至在跨语言环境中也能做到。

通过结合来自 SpeechT5 和 VALL-E 的概念，我们可以期待类似 T0 的模型的发布，这些模型可以通过文本、音频或两者来进行提示，并根据任务生成文本或合成音频。一个新时代的模型将开始出现，因为上下文学习将使得在零样本设置中对新任务进行泛化成为可能。这将允许在音频上进行语义搜索，通过带有说话人属性的 ASR 转录目标说话人，或用自由文本描述它，例如，“那个咳嗽的年轻孩子说了什么？”此外，它将使我们能够使用音频或文本描述来分类或合成音频，并通过显式/隐式 ASR 直接解决 NLP 任务。

## **对话 AI：**

对话型 AI 主要通过任务导向的对话系统被采纳，即如亚马逊的 Alexa 和苹果的 Siri 这样的 AI 个人助理（PA）。这些 PA 因其通过语音命令快速访问功能和信息的能力而变得流行。随着大科技公司主导这一技术，对 AI 助理的新规定将迫使它们提供第三方语音助理选项，从而打开竞争[47]。随着这一变化，我们可以预期个人助理之间会实现互操作性，也就是说它们将开始进行通信。这将非常棒，因为人们可以使用任何设备连接到世界任何地方的对话代理[48]。从 ASR 的角度来看，这将带来新的挑战，因为上下文化将变得更加广泛，助理必须具备对不同口音的鲁棒性，并可能支持多语言。

近年来，基于文本的开放式对话系统发生了巨大的技术飞跃，例如 Blender-Bot 和 LaMDA [49–50]。最初，这些对话系统是基于文本的，即它们通过文本输入并训练输出文本，全部在书面领域内。随着 ASR 性能的提高，开放式对话系统被增强了语音基础的人机界面（HMI），这导致了由于口语和书面形式之间的差异而出现的**模态不一致**。主要挑战之一是弥合这一差距，通过克服由于音频相关处理引入的新类型错误，例如口语和书面形式之间的流利度差异和实体解析，以及转录错误，如发音错误[51–52]。

可能的解决方案包括改进转录质量和强大的 NLP 模型，这些模型能够有效处理转录和发音错误。可靠的声学模型置信度评分[53]将作为这些系统的关键角色，帮助指出说话者错误或作为 NLP 模型或解码逻辑的另一个输入。此外，我们预计 ASR 模型将能够预测非语言线索，如讽刺，使代理能够更深入地理解对话并提供更好的回应。

这些改进将推动进一步的发展，使对话系统具备语音 HMI，以支持挑战性的口音和儿童语言，例如在 Loora [54]和 Speaks [55]中。

更进一步，我们预计将发布一个 E2E 多任务学习框架，用于语音语言任务，采用语音和 NLP 问题的联合建模，如 MTL-SLT [56]。这些模型将以 E2E 的方式进行训练，减少顺序模块之间的累计误差，并处理如口语理解、口语总结和口语问答等任务，通过将语音作为输入，产生转录、意图、命名实体、摘要和文本查询答案等各种输出。

个性化将对 AI 助手和开放式对话系统产生巨大影响，这将引领我们进入下一个要点：说话人属性 ASR。

## **说话人属性 ASR：**

在家庭环境中涉及多个麦克风和方的远程对话的转录仍然存在挑战。即使是最先进（SoTA）的系统也只能实现约 35%的 WER[57]。

早期的联合 ASR 和分段系统于 2019 年发布[58]。今年，我们可以期待发布一种端到端的语音增强-ASR-分段系统，该系统将改善重叠语音的性能，并在挑战性的声学场景（如混响房间、远场设置和低信噪比(SNR)）中提供更好的表现。通过联合任务优化、改进的预训练方法（如 WavLM [10]）、应用架构更改[59]、数据增强以及在预训练和微调期间在领域内数据上的训练[11]，将实现这些改进。此外，我们可以期待针对个性化语音识别的说话人属性 ASR 系统的部署。这将进一步提高目标说话人语音的转录准确性，并将转录偏向用户定义的词汇，如联系人姓名、专有名词和其他命名实体，这些对智能助手至关重要[60]。此外，低延迟模型将继续成为重点领域，以增强边缘设备的整体体验和响应时间[61–62]。

## **初创公司在 ASR 领域相对于大科技公司的角色**

尽管大科技公司预计将继续凭借其 API 主导市场，但小型初创企业在特定领域仍然可以超越它们。这些领域包括由于法规原因在大科技公司的训练数据中被低估的领域，如医疗领域和儿童语言，以及尚未采用技术的群体，如具有挑战性口音的移民或全球学习英语的个人。在大科技公司不愿投资的市场中，例如那些不广泛使用的语言，小型初创企业可能会发现成功和获利的机会。

为了创造双赢的局面，大科技公司可以提供 API，允许完全访问其声学模型的输出，同时允许其他人编写解码逻辑（WFST/beam-search），而不仅仅是添加可定制词汇或使用当前模型适应功能[63–64]。这种方法将使小型初创企业能够通过在给定声学模型之上进行推理时结合引导或多语言模型来在其领域中脱颖而出，而不必自己训练声学模型，这在人力资本和领域知识上可能成本高昂。反过来，大科技公司将从其付费模型的更广泛采用中受益。

## **ASR 如何融入更广泛的机器学习领域？**

一方面，考虑到 ASR 作为最终任务时，其重要性与计算机视觉（CV）和 NLP 相当。这是低资源语言和领域中，转录是主要业务的当前情况，例如法庭、医疗记录、电影字幕等。

另一方面，在其他领域，ASR 已不再是瓶颈，只要它达到了某种可用性阈值。在这些情况下，NLP 才是瓶颈，这意味着将 ASR 性能提升到完美主义并不是提取最终任务见解的关键。例如，会议总结或行动项提取在许多情况下可以利用当前的 ASR 质量实现。

# **结束语**

ASR 技术的进步使我们更接近于实现人与机器之间的无缝沟通，例如在对话 AI 和生成 AI 中。随着语音增强-ASR-标记系统的持续发展以及无文本 NLP 的出现，我们有望在这个领域见证激动人心的突破。展望未来，我们不禁期待 ASR 技术将解锁的无限可能。

*感谢您花时间阅读这篇文章！我们非常重视和感激您对这些预测的看法和反馈。请随时分享您的评论和想法。*

## **参考文献：**

[1] [`www.orcam.com/en/home/`](https://www.orcam.com/en/home/)

[2] [`voicebot.ai/2022/12/01/hey-disney-custom-alexa-assistant-rolls-out-at-disney-world/`](https://voicebot.ai/2022/12/01/hey-disney-custom-alexa-assistant-rolls-out-at-disney-world/)

[3] Jose, Christin 等人。“关键词检测的延迟控制。” ArXiv，2022，[`doi.org/10.21437/Interspeech.2022-10608`](https://doi.org/10.21437/Interspeech.2022-10608)。

[4] Bijwadia, Shaan 等人。“统一的端到端语音识别与端点检测以实现快速高效的语音系统。” ArXiv，2022，[`doi.org/10.1109/SLT54892.2023.10022338`](https://doi.org/10.1109/SLT54892.2023.10022338)。

[5] Yoon, Ji 等人。“HuBERT-EE：早期退出的 HuBERT 以实现高效语音识别。” ArXiv，2022，[`doi.org/10.48550/arXiv.2204.06328`](https://doi.org/10.48550/arXiv.2204.06328)。

[6] Kanda, Naoyuki 等人。“转录到说话者标记：使用端到端说话者标记 ASR 进行无限数量的说话者的神经说话者标记。” ArXiv，2021，[`doi.org/10.48550/arXiv.2110.03151`](https://doi.org/10.48550/arXiv.2110.03151)。

[7] Kanda, Naoyuki 等人。“基于令牌级说话者嵌入的流式说话者标记 ASR。” ArXiv，2022，[`doi.org/10.48550/arXiv.2203.16685`](https://doi.org/10.48550/arXiv.2203.16685)。

[8] [`www.fiercevideo.com/video/video-will-account-for-82-all-internet-traffic-by-2022-cisco-says`](https://www.fiercevideo.com/video/video-will-account-for-82-all-internet-traffic-by-2022-cisco-says)

[9] Chiu, Chung, 等. “用于语音识别的随机投影量化器自监督学习。” ArXiv, 2022, [`doi.org/10.48550/arXiv.2202.01855`](https://doi.org/10.48550/arXiv.2202.01855)。

[10] Chen, Sanyuan, 等. “WavLM：用于全栈语音处理的大规模自监督预训练。” ArXiv, 2021, [`doi.org/10.1109/JSTSP.2022.3188113`](https://doi.org/10.1109/JSTSP.2022.3188113)。

[11] Hsu, Wei, 等. “鲁棒的 Wav2vec 2.0：自监督预训练中的领域偏移分析。” ArXiv, 2021, [`doi.org/10.48550/arXiv.2104.01027`](https://doi.org/10.48550/arXiv.2104.01027)。

[12] Lugosch, Loren, 等. “大规模多语言语音识别的伪标签。” ArXiv, 2021, [`doi.org/10.48550/arXiv.2111.00161`](https://doi.org/10.48550/arXiv.2111.00161)。

[13] Berrebbi, Dan, 等. “从开始进行连续伪标签。” ArXiv, 2022, [`doi.org/10.48550/arXiv.2210.08711`](https://doi.org/10.48550/arXiv.2210.08711)。

[14] Radford, Alec, 等. “通过大规模弱监督进行鲁棒的语音识别。” ArXiv, 2022, [`doi.org/10.48550/arXiv.2212.04356`](https://doi.org/10.48550/arXiv.2212.04356)。

[15] Graves, Alex, 等. “连接主义时间分类：使用递归神经网络标记未分段序列数据。” ICML, 2016, [`www.cs.toronto.edu/~graves/icml_2006.pdf`](https://www.cs.toronto.edu/~graves/icml_2006.pdf)。

[16] Graves, Alex. “使用递归神经网络进行序列转导。” ArXiv, 2012, [`doi.org/10.48550/arXiv.1211.3711`](https://doi.org/10.48550/arXiv.1211.3711)。

[17] Chan, William, 等. “听、注意和拼写。” ArXiv, 2015, [`doi.org/10.48550/arXiv.1508.01211`](https://doi.org/10.48550/arXiv.1508.01211)。

[18] Variani, Ehsan, 等. “混合自回归传输器（Hat）。” ArXiv, 2020, [`doi.org/10.48550/arXiv.2003.07705`](https://doi.org/10.48550/arXiv.2003.07705)。

[19] Meng, Zhong, 等. “模块化混合自回归传输器。” ArXiv, 2022, [`doi.org/10.48550/arXiv.2210.17049`](https://doi.org/10.48550/arXiv.2210.17049)。

[20] Kim, Suyoun, 等. “使用语义距离度量评估用户对语音识别系统质量的感知。” ArXiv, 2021, [`doi.org/10.48550/arXiv.2110.05376`](https://doi.org/10.48550/arXiv.2110.05376)。

[21] Gulati, Anmol, 等. “Conformer：用于语音识别的卷积增强变换器。” ArXiv, 2020, [`doi.org/10.48550/arXiv.2005.08100`](https://doi.org/10.48550/arXiv.2005.08100)。

[22] Vaswani, Ashish, 等. “注意力即一切。” ArXiv, 2017, [`doi.org/10.48550/arXiv.1706.03762`](https://doi.org/10.48550/arXiv.1706.03762)。

[23] Baevski, Alexei, 等. “Wav2vec 2.0：自监督语音表示学习框架。” ArXiv, 2020, [`doi.org/10.48550/arXiv.2006.11477`](https://doi.org/10.48550/arXiv.2006.11477)。

[24] Babu, Arun 等. “XLS-R: 大规模自监督跨语言语音表示学习。” ArXiv, 2021, [`doi.org/10.48550/arXiv.2111.09296`](https://doi.org/10.48550/arXiv.2111.09296)。

[25] [`blog.google/technology/ai/ways-ai-is-scaling-helpful/`](https://blog.google/technology/ai/ways-ai-is-scaling-helpful/)

[26] [`ai.facebook.com/blog/teaching-ai-to-translate-100s-of-spoken-and-written-languages-in-real-time/`](https://ai.facebook.com/blog/teaching-ai-to-translate-100s-of-spoken-and-written-languages-in-real-time/)

[27] Ao, Junyi 等. “SpeechT5: 统一模态编码器-解码器预训练用于口语语言处理。” ArXiv, 2021, [`doi.org/10.48550/arXiv.2110.07205`](https://doi.org/10.48550/arXiv.2110.07205)。

[28] [`github.com/guillaumekln/faster-whisper`](https://github.com/guillaumekln/faster-whisper)

[29] [`github.com/NVIDIA/DeepLearningExamples/tree/master/PyTorch/SpeechRecognition/wav2vec2`](https://github.com/NVIDIA/DeepLearningExamples/tree/master/PyTorch/SpeechRecognition/wav2vec2)

[30] Meng, Zhong 等. “JEIT: 语音识别的联合端到端模型与内部语言模型训练。” ArXiv, 2023, [`doi.org/10.48550/arXiv.2302.08583`](https://doi.org/10.48550/arXiv.2302.08583)。

[31] Zhang, Yu 等. “Google USM: 将自动语音识别扩展到 100 多种语言。” ArXiv, 2023, [`doi.org/10.48550/arXiv.2303.01037`](https://doi.org/10.48550/arXiv.2303.01037)。

[32] Kendall, T. 和 Farrington, C. “区域非裔美国语言语料库。” 版本 2021.07\. Eugene, OR: 非裔美国语言在线资源项目。[`oraal.uoregon.edu/coraal`](http://oraal.uoregon.edu/coraal), 2021

[33] Brian, D Tran 等. “‘嗯哼’，‘呃呃’：非词汇性对话声音是否会成为环境临床文档技术的致命缺陷？”，《美国医学信息学协会杂志》，2023, [`doi.org/10.1093/jamia/ocad001`](https://doi.org/10.1093/jamia/ocad001)

[34] Zhu, Ge 等. “填充词检测与分类：数据集与基准。” ArXiv, 2022, [`doi.org/10.48550/arXiv.2203.15135`](https://doi.org/10.48550/arXiv.2203.15135)。

[35] Anwar, Mohamed 等. “MuAViC: 多语言音频视觉语料库用于稳健的语音识别和稳健的语音到文本翻译。” ArXiv, 2023, [`doi.org/10.48550/arXiv.2303.00628`](https://doi.org/10.48550/arXiv.2303.00628)。

[36] Jaegle, Andrew 等. “Perceiver IO: 一种用于结构化输入和输出的通用架构。” ArXiv, 2021, [`doi.org/10.48550/arXiv.2107.14795`](https://doi.org/10.48550/arXiv.2107.14795)。

[37] [`www.assemblyai.com/blog/conformer-1/`](https://www.assemblyai.com/blog/conformer-1/)

[38] Peyser, Cal 等. “大词汇量设备端自动语音识别的双重学习。” ArXiv, 2023, [`doi.org/10.48550/arXiv.2301.04327`](https://doi.org/10.48550/arXiv.2301.04327)。

[39] Lakhotia, Kushal 等. “从原始音频生成的语言建模。” ArXiv, 2021, [`doi.org/10.48550/arXiv.2102.01192`](https://doi.org/10.48550/arXiv.2102.01192)。

[40] Zeghidour, Neil 等. “SoundStream: 一种端到端神经音频编解码器。” ArXiv, 2021, [`doi.org/10.48550/arXiv.2107.03312`](https://doi.org/10.48550/arXiv.2107.03312)。

[41] Borsos, Zalán 等. “AudioLM: 一种音频生成的语言建模方法。” ArXiv, 2022, [`doi.org/10.48550/arXiv.2209.03143`](https://doi.org/10.48550/arXiv.2209.03143)。

[42] [`about.fb.com/news/2022/10/hokkien-ai-speech-translation/`](https://about.fb.com/news/2022/10/hokkien-ai-speech-translation/)

[43] Raffel, Colin 等. “探索统一文本到文本变换器的迁移学习极限。” ArXiv, 2019, /abs/1910.10683。

[44] Sanh, Victor 等. “多任务提示训练实现零样本任务泛化。” ArXiv, 2021, [`doi.org/10.48550/arXiv.2110.08207`](https://doi.org/10.48550/arXiv.2110.08207)。

[45] 王成毅 等. “神经编解码语言模型是零样本文本到语音合成器。” ArXiv, 2023, [`doi.org/10.48550/arXiv.2301.02111`](https://doi.org/10.48550/arXiv.2301.02111)。

[46] 张子强 等. “用你自己的声音说外语：跨语言神经编解码语言建模。” ArXiv, 2023, [`doi.org/10.48550/arXiv.2303.03926`](https://doi.org/10.48550/arXiv.2303.03926)。

[47] [`voicebot.ai/2022/07/05/eu-passes-new-regulations-for-voice-ai-and-digital-technology/`](https://voicebot.ai/2022/07/05/eu-passes-new-regulations-for-voice-ai-and-digital-technology/)

[48] [`www.speechtechmag.com/Articles/ReadArticle.aspx?ArticleID=154094`](https://www.speechtechmag.com/Articles/ReadArticle.aspx?ArticleID=154094)

[49] Thoppilan, Romal 等. “LaMDA: 对话应用的语言模型。” ArXiv, 2022, [`doi.org/10.48550/arXiv.2201.08239`](https://doi.org/10.48550/arXiv.2201.08239)。

[50] Shuster, Kurt 等. “BlenderBot 3: 一个持续学习以负责任地互动的部署对话代理。” ArXiv, 2022, [`doi.org/10.48550/arXiv.2208.03188`](https://doi.org/10.48550/arXiv.2208.03188)。

[51] 肖舟 等. “用于实体解析的 ASR 鲁棒性语音嵌入。” Proc. Interspeech 2022, 3268–3272, doi: 10.21437/Interspeech.2022–10956

[52] 陈安琪 等. “教 BERT 等待：平衡流媒体不流畅检测的准确性和延迟。” ArXiv, 2022, [`doi.org/10.48550/arXiv.2205.00620`](https://doi.org/10.48550/arXiv.2205.00620)。

[53] 李秋佳 等. “提高端到端语音识别的领域外数据置信度估计。” ArXiv, 2021, [`doi.org/10.48550/arXiv.2110.03327`](https://doi.org/10.48550/arXiv.2110.03327)。

[54] [`loora.ai/`](https://loora.ai/)

[55] [`techcrunch.com/2022/11/17/speak-lands-investment-from-openai-to-expand-its-language-learning-platform/`](https://techcrunch.com/2022/11/17/speak-lands-investment-from-openai-to-expand-its-language-learning-platform/)

[56] Zhiqi, Huang 等. “MTL-SLT: 多任务学习用于口语语言任务。” NLP4ConvAI, 2022, [`aclanthology.org/2022.nlp4convai-1.11`](https://aclanthology.org/2022.nlp4convai-1.11)

[57] Watanabe, Shinji 等. “CHiME-6 挑战：应对未分段录音中的多说话者语音识别。” ArXiv, 2020, [`doi.org/10.48550/arXiv.2004.09249`](https://doi.org/10.48550/arXiv.2004.09249).

[58] Shafey, Laurent 等. “通过序列转导联合语音识别和说话者分离。” ArXiv, 2019, [`doi.org/10.48550/arXiv.1907.05337`](https://doi.org/10.48550/arXiv.1907.05337).

[59] Kim, Juntae 和 Lee, Jeehye. “通过稀疏自注意力层将 RNN-转导器推广到域外音频。” ArXiv, 2021, [`doi.org/10.48550/arXiv.2108.10752`](https://doi.org/10.48550/arXiv.2108.10752).

[60] Sathyendra, Kanthashree 等. “针对神经转导器的个性化语音识别的上下文适配器。” ArXiv, 2022, [`doi.org/10.48550/arXiv.2205.13660`](https://doi.org/10.48550/arXiv.2205.13660).

[61] Tian, Jinchuan 等. “Bayes 风险 CTC: 在序列到序列任务中可控的 CTC 对齐。” ArXiv, 2022, [`doi.org/10.48550/arXiv.2210.07499`](https://doi.org/10.48550/arXiv.2210.07499).

[62] Tian, Zhengkun 等. “Peak-First CTC: 通过应用 Peak-First 正则化减少 CTC 模型的峰值延迟。” ArXiv, 2022, [`doi.org/10.48550/arXiv.2211.03284`](https://doi.org/10.48550/arXiv.2211.03284).

[63] [`docs.rev.ai/api/custom-vocabulary/`](https://docs.rev.ai/api/custom-vocabulary/)

[64] [`cloud.google.com/speech-to-text/docs/adaptation-model`](https://cloud.google.com/speech-to-text/docs/adaptation-model)
