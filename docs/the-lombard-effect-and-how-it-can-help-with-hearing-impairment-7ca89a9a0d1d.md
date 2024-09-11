# [隆巴尔效应及其如何帮助听力障碍](https://towardsdatascience.com/the-lombard-effect-and-how-it-can-help-with-hearing-impairment-7ca89a9a0d1d?source=collection_archive---------4-----------------------#2023-09-11)

> 原文：[https://towardsdatascience.com/the-lombard-effect-and-how-it-can-help-with-hearing-impairment-7ca89a9a0d1d?source=collection_archive---------4-----------------------#2023-09-11](https://towardsdatascience.com/the-lombard-effect-and-how-it-can-help-with-hearing-impairment-7ca89a9a0d1d?source=collection_archive---------4-----------------------#2023-09-11)

[](https://medium.com/@domi.math.cece?source=post_page-----7ca89a9a0d1d--------------------------------)[![Dominika Woszczyk](../Images/ba59713052d80dfd4e4f45d64a3ef1bd.png)](https://medium.com/@domi.math.cece?source=post_page-----7ca89a9a0d1d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----7ca89a9a0d1d--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----7ca89a9a0d1d--------------------------------) [Dominika Woszczyk](https://medium.com/@domi.math.cece?source=post_page-----7ca89a9a0d1d--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fafc71d29e576&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-lombard-effect-and-how-it-can-help-with-hearing-impairment-7ca89a9a0d1d&user=Dominika+Woszczyk&userId=afc71d29e576&source=post_page-afc71d29e576----7ca89a9a0d1d---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----7ca89a9a0d1d--------------------------------) ·5分钟阅读·2023年9月11日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F7ca89a9a0d1d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-lombard-effect-and-how-it-can-help-with-hearing-impairment-7ca89a9a0d1d&user=Dominika+Woszczyk&userId=afc71d29e576&source=-----7ca89a9a0d1d---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F7ca89a9a0d1d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-lombard-effect-and-how-it-can-help-with-hearing-impairment-7ca89a9a0d1d&source=-----7ca89a9a0d1d---------------------bookmark_footer-----------)

***简而言之：*** *隆巴尔效应可以应用于声音转换和文本到语音，使合成语音在噪音中更易于理解。*

![](../Images/fb13d1e55c912183df8d7220403c43f6.png)

图片由作者提供

**你是否曾经想过为什么我们在嘈杂的房间里往往说得更大声？** 好吧，语言和言语研究者们也对此感到好奇，他们探讨了一个叫做**隆巴尔效应**的概念（由[Étienne Lombard](https://en.wikipedia.org/wiki/%C3%89tienne_Lombard)发现）。

**💬 隆巴尔效应概述**

想象一下你在一个聚会上，音乐在播放，大家聊天、欢笑，气氛愉快！为了让你的朋友听到你的声音，你的大脑会自动提高你声音的***音量***，调整你的***音调***，甚至调整你的***语速***。有趣的是，我们还会根据对面人的反馈和周围的噪音来调整声音，以确保他们能听到信息。

现在，考虑将这一效应应用于技术，如**文本到语音（TTS）系统**。如果 Alexa 或 Google Home 能够以 Lombard 效应进行对话会怎样？（这一场景已经被 [SNL](https://www.youtube.com/watch?v=YvT_gqs5ETk) 想象过）。

**🔊 Lombard 效应与文本到语音**

一些研究（见 [[1](https://www.pure.ed.ac.uk/ws/portalfiles/portal/84260765/Normal_to_Lombard_Adaptation_BOLLEPALLI_DoA180419_AFV.pdf)]，[[2](https://dipjyoti92.github.io/TTS-Style-Transfer/)）探讨了如何将 Lombard 风格应用于文本到语音（TTS）中以提高可理解性。他们的目标是看看是否可以通过 Lombard 风格的录音来训练语音，从而提高可理解性和自然性。他们发现这确实是一种比信号处理更自然的提高可理解性的方法！

**▴ 为什么这很重要**

我们可以在源头让语音听起来更清晰，而不仅仅是提高音量或在接收端处理信号（就像大多数助听器所做的那样）！

助听器是惊人的工程成果，但也有其挑战。它们并不总是舒适，可能很昂贵，有些人甚至选择不经常使用。但是，通过 Lombard 风格的 TTS，语音会自动调整为更清晰、更易于理解。这可能会成为游戏规则的改变者，不仅对那些佩戴助听器的人有用，对非母语人士（见 [[3](https://www.sciencedirect.com/science/article/pii/S016763932100131X)]）以及任何处于嘈杂环境中的人也有帮助！

**🚩 当前问题**

前面提到的研究使用了大量特定语音的音频样本数据集。如果没有这些数据会怎样？我们如何在没有录音（既疲惫、耗时，又对语音人才成本高）的情况下合成 Lombard 风格的语音？

**🔍 解决方案？**

语音转换，即将某人的声音转移到其他人的语音录音上，可以作为一种数据增强方法。这个想法是通过将发言者的身份转移到 Lombard 语音录音上，创建该人声的 Lombard 风格录音。

**📚 我们的研究**

在我们最近在[Clarity Workshop](https://claritychallenge.org/clarity2023-workshop/programme.html)的Interspeech 23'上发表的[论文](https://www.amazon.science/publications/voice-conversion-for-lombard-speaking-style-with-implicit-and-explicit-acoustic-feature-conditioning)中，我们决定研究如何在进行语音转换时保留Lombard效应。确实，目标说话者的信息可能会覆盖Lombard效应特征，未能给我们预期的结果。我们想回答以下问题：*我们能否在语音转换过程中保留负责可懂度的Lombard说话风格，同时转移说话者的身份？*

在给定的语音转换（VC）模型下，我们调查了不同的条件方式。以下图形中可以找到我们在实验中尝试的三种系统。

![](../Images/0b5d66c4cd64a63ae90a381268bce26f.png)

语音转换模型在三种条件设置中的示意图。图片由作者提供

+   **VC+features**（显式条件）：我们首先决定分离语音的三个关键元素：音高、音量和倾斜度。然后，我们将提取的特征直接输入模型的编码器。接着，我们在Lombard录音中提取这些特征，并将它们输入语音转换模型，以强制模型在最终录音中保留这些特征，同时转移我们希望转移的声音。

+   **VC+CLS**（隐式条件）：如果我们希望模型自行学习特征呢？我们通过添加一个风格分类器来测试这一点，该分类器强制模型在语音转换后保持源风格。这种设置有助于在没有对特征进行微调的情况下保留Lombard风格。

+   **融合**：该系统结合了经过精心选择的特征和分类器，强制模型保持原始说话风格。

**我们发现了什么？** 如下方显示的条形图所示，显示了高噪声水平下的可懂度，我们发现

![](../Images/53d10b597f03bcc3cfb5b3d032eae332.png)

每个系统的目标可懂度评分（通过SIIB指标测量），具有最佳表现的特征，适用于男性和女性目标声音。图片由作者提供

+   确实，在转换过程中丧失了Lombard效应

+   显式和隐式条件都有助于提高最终的可懂度

+   融合效果更佳，但丧失了目标说话者的信息，使其不那么有用

+   不同的特征对女性和男性声音的效果不同

**👉 结论是什么？**

过去的研究和我们的工作表明，Lombard风格的TTS确实在嘈杂环境中提高了语音可懂度。虽然自然度可能会受到影响，但在噪音中不那么显著，讲话者的身份也不会受到太大影响。在我们的研究中，我们发现基本的语音转换丧失了Lombard可懂度效应，但通过显式或隐式的条件设置，我们能够更好地转移这些效应！

**查看我们的论文** [**这里**](https://www.amazon.science/publications/voice-conversion-for-lombard-speaking-style-with-implicit-and-explicit-acoustic-feature-conditioning) **获取更多细节！**

**🚀 可理解语音的未来**

想象一个世界，语音合成模仿我们的自然调整，在嘈杂的地方使沟通更加顺畅。通过更多的研究和创新，Lombard风格的TTS可以帮助听力障碍者进行日常活动，如听音乐、观看YouTube视频、看电影等，并改善我们与智能助理和语音激活设备的互动！

**参考文献**

- [1] Bollepalli, Bajibabu等人。"[使用长短期记忆递归神经网络进行语音合成的Normal-to-Lombard适应。](https://www.pure.ed.ac.uk/ws/portalfiles/portal/84260765/Normal_to_Lombard_Adaptation_BOLLEPALLI_DoA180419_AFV.pdf)"。《语音通信》110 (2019)

- [2] Paul, Dipjyoti等人。[“使用语言风格转换增强文本到语音合成中的语音可理解性。”](https://www.isca-speech.org/archive_v0/Interspeech_2020/pdfs/2793.pdf)。《Interspeech会议论文集》(2020)。

- [3] Marcoux, Katherine等人。[“对于母语和非母语听者，母语和非母语语音的Lombard可理解性效益。”](https://www.sciencedirect.com/science/article/pii/S016763932100131X#:~:text=These%20studies%20on%20non%2Dnative,influences%20of%20the%20native%20language.) 《语音通信》136 (2022)
