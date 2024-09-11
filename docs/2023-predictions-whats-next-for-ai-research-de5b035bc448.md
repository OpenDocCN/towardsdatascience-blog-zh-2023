# 2023年的预测：AI研究的下一步是什么？

> 原文：[https://towardsdatascience.com/2023-predictions-whats-next-for-ai-research-de5b035bc448?source=collection_archive---------2-----------------------#2023-01-08](https://towardsdatascience.com/2023-predictions-whats-next-for-ai-research-de5b035bc448?source=collection_archive---------2-----------------------#2023-01-08)

## 对于过去的一年感到兴奋，我们展望2023年，想知道它会是什么样子。

[](https://medium.com/@talrosenwein?source=post_page-----de5b035bc448--------------------------------)[![Tal Rosenwein](../Images/c5839727d9f63df6b5f26c7aa781679b.png)](https://medium.com/@talrosenwein?source=post_page-----de5b035bc448--------------------------------)[](https://towardsdatascience.com/?source=post_page-----de5b035bc448--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----de5b035bc448--------------------------------) [Tal Rosenwein](https://medium.com/@talrosenwein?source=post_page-----de5b035bc448--------------------------------)

·

[跟随](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fc25fa765131b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F2023-predictions-whats-next-for-ai-research-de5b035bc448&user=Tal+Rosenwein&userId=c25fa765131b&source=post_page-c25fa765131b----de5b035bc448---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----de5b035bc448--------------------------------) ·12分钟阅读·2023年1月8日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fde5b035bc448&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F2023-predictions-whats-next-for-ai-research-de5b035bc448&source=-----de5b035bc448---------------------bookmark_footer-----------)

这篇博客文章由[**Guy Eyal**](https://www.linkedin.com/in/guy-netser-eyal-84368445/)，Gong的NLP团队负责人，共同撰写。

## **简而言之：**

*2022年，大型模型在各类任务和领域中取得了最先进的成果。在自然语言处理（NLP）方面，当模型被训练以对齐用户意图和人类偏好时，实现了显著突破，从而提高了生成质量。展望2023年，我们可以期待看到改进对齐过程的新方法（例如带有AI反馈的强化学习）、理解对齐效果的自动化指标的开发，以及个性化对齐模型的出现，即使是在线方式。可能还会关注解决事实性问题以及开发开源工具和专门的计算资源，以支持对齐模型的工业规模发展。除了NLP，还可能在音频处理、计算机视觉和机器人等其他模态中取得进展，并发展多模态模型。*

# 2022年AI研究进展：年度回顾

2022年对人工智能/机器学习来说是优秀的一年，发布了大量语言模型（LLMs）并在各种基准测试中取得了最先进的结果。这些LLMs通过少量学习展示了其优越的表现，超越了在相同任务上进行了微调的小型模型[1–3]。这有可能减少对专业领域数据集的需求。诸如思维链[4]和自洽性[5]等技术也帮助提升了LLMs的推理能力，在推理基准测试中取得了显著提升。

对话系统也取得了显著进展， resulting in more helpful, safe, and faithful models that could stay up-to-date through fine-tuning on annotated data and the use of retrieval from external knowledge sources [6–7]。

在自动语音识别（ASR）中，使用编码器-解码器变换器架构实现了模型规模的更高效扩展，在多个ASR基准测试中减少了50%的词错误率而无需领域适配[8]。

扩散模型[9–10]在大型图像数据集上进行训练，在计算机视觉领域取得了显著进展，并引发了AI艺术的新趋势。此外，我们还看到了利用预训练大语言模型（LLMs）来提升从视觉到机器人等任务表现的多模态模型的初步出现[9–12]。

最终，ChatGPT [13] 的发布让用户瞥见了在各个领域和领域中与AI助手合作的未来。

![](../Images/564eb7d0f038ace83a24b12ab118876e.png)

图片来源：[Moritz Knöringer](https://unsplash.com/@mokngr?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 2023年预测：对齐年的到来

对过去一年充满期待，我们展望2023年，想知道它会是什么样子。以下是我们的想法：

最近几个月，人工反馈的强化学习（RLHF），一种使模型与用户意图和人类偏好对齐的监督方法，变得越来越受欢迎 [15]。这种监督方法在生成质量方面表现出良好的结果，比较vanilla GPT3 [16] 和ChatGPT的输出可以看出。RLHF的效果如此显著，以至于经过指令调优的模型超越了一个大于100倍的模型。

![](../Images/bb40f68f7f8a25709fa01b4f07eab9e9.png)

图片来源 — InstructGPT [15]

随着今年趋势的进一步发展，我们和许多人一样，预计对齐将继续是一个重要因素。然而，我们预测将有更多的核心功能进行积极研究，这些功能目前在大多数模型中缺失，因此限制了它们在许多领域的适用性。

## **人工智能反馈的强化学习（RLAIF）**

目前，RLHF需要人工策划的数据。虽然与预训练数据规模相比较小，但仍需要大量且昂贵的人力。例如，OpenAI使用了40名注释员编写和标注了64K个样本用于指令调优 [15]。我们认为，今年将被利用的一个有趣且令人兴奋的替代方案是使用不同的LLM作为指导者和标注者——人工智能反馈的强化学习（RLAIF）。RLAIF将能够降低成本并快速扩展对齐过程，因为机器将完成所有的端到端工作。Anthropic [17] 最近的一项有趣工作表明，通过良好的提示，可以引导语言模型对有害输出进行分类。这些分类结果反过来用于训练RLHF所需的奖励模型。

## **对齐的度量标准**

我们假设将会开发许多方法来实现模型输出与用户意图之间更好的对齐。这反过来将进一步提高生成质量。

为了理解哪种方法更优，自动度量标准应与当前的人类评估方法一起开发。这是NLP中的一个长期问题，因为以前的度量标准未能与人类注释对齐。

最近，引入了两种有前景的方法：MAUVE [18]，它通过使用发散前沿比较人类生成和模型生成输出的分布，以及模型编写的评估 [19]，它利用其他语言模型来评估生成输出的质量。在这些领域的进一步研究可能是2023年的一个有价值的方向。

## **个性化对齐模型**

期望一个模型与整个社会对齐是没有意义的，因为我们彼此之间并不对齐。因此，我们预计会看到许多不同的模型与不同的用途和用户对齐。我们称之为个性化对齐模型。

我们将看到各种公司根据自己的需求调整模型，而大型公司则将多个模型与不同的用户需求对齐。这将大大改善最终用户在使用LLMs时的体验，包括个人助理、互联网搜索、文本编辑等。

## **开源和专业计算**

要实现行业规模的个性化对齐模型，需要公开可用的两个组件：可对齐的模型和计算资源，这两个组件今天尚未完全存在。

**待对齐的模型和** **开源模型**作为对齐的候选者必须被开发，因为当前的模型，如Meta的OPT [20]，不够充分，因为它们与付费API不匹配。或者，我们将看到模型对齐的付费API：由Google / OpenAI / Cohere / AI21提供的非公开模型将提供全面的消费者服务选项，并将作为一种有效的商业模式。

**计算资源：** 尽管对齐比预训练便宜得多，但仍然需要非常专业的计算资源。因此，我们预测将出现争相生成这种公共可用基础设施的情况，可能是在云端。

## **处理事实问题**

LLMs生成的输出的明显流畅性可能会导致个人将模型视为事实准确且自信。然而，LLMs的一个已知限制是它们倾向于生成虚假的内容，这仍需要通过对齐来解决。因此，我们看到两个重要的研究方向将在今年蓬勃发展：文本输出的源（引用）和模型的置信度。

**当前输出的源**可以通过多种方式实现。一个有趣的方向是将LLMs与文本检索机制连接，这将有助于将输出与已知来源进行关联[21]。这也可能帮助模型保持相关性，即使它们的训练过程在过去某个时间点停止了。另一个最近提出的想法是在后处理过程中通过搜索最接近输出的文档[22]来实现。虽然后者不能解决虚假信息，但可以使用户更容易验证结果。

在不同领域（例如ASR [23]）的近期研究中训练了**具有两个输出的模型：令牌预测和每个令牌的置信度评分**。使用类似的方法，同时将置信度评分扩展到整个输出，将帮助用户谨慎对待结果。

## **在线对齐**

随着人们的兴趣、信仰、工作和家庭状况的变化，他们的个人助理也应该适应这些变化是有意义的。我们预测的一个非常有前景的研究方向是在线对齐。用户将能够在部署后继续个性化他们的模型。对齐过程将通过向模型提供反馈的在线学习算法不断更新[24]。

## **其他模态的情况如何？**

我们期望在音频和语音识别领域看到显著的改进。我们假设 Whisper [8] 将能够利用未标注的数据（如 Wav2Vec 2.0 [25] / HuBERT [26]），这将显著提高在挑战性声学场景中的表现。

SpeechT5 [27] 是一个早期的探索者，因此我们假设类似 T0 的模型 [28] 将在规模上（包括训练数据和模型大小）进行训练，从而改进音频嵌入。这将实现一个统一的语音增强、分离和转录系统。在长期来看，我们期待听觉模型能够回答类似于自然语言处理（NLP）模型的问题。这些听觉模型的基础上下文将是一个音频片段，该片段将作为查询的上下文，而无需隐式转录。

## **多模态模型**

明年的一个重要范式将是大型多模态模型。它们会是什么样子？我们猜测它们可能会非常类似于语言模型。我们所指的是，用户将以某种模态提示模型，模型将能够以不同的模态生成其输出（如 Unified-IO [29]）。

尽管非常令人兴奋，扩散模型 [9] 目前无法对图像进行分类。这可以通过输出类似于我们今天在分类任务中使用的 LLM 的文本来轻松解决。类似地，这些模型将能够通过良好的提示来转录、生成和增强音频和视频。

那么多模态模型的对齐情况如何？这是远未来的事！或者用我们当前领域的节奏来说——几个月后。

## **结束语**

本文展示了我们对2023年AI研究中所需进展的预测。大型模型可以执行广泛的学术任务，正如它们在标准基准测试中的出色表现所示。然而，这些模型在现实场景中仍会遇到令人尴尬的失败（不真实、有毒，或对用户没有帮助）。我们相信，通过将模型与用户需求对齐并保持其最新状态，可以解决许多这些问题。为此，我们关注了对齐过程的可扩展性和适应性。如果我们的假设正确，生成语言模型领域将很快发生重大变化。这些模型的潜在用途广泛，从编辑工具到可以在法律、会计和工程等行业中自动化手工劳动的领域特定 AI 助手。将上述声明与计算（GPT-4）的预测进展结合起来，并采用同样的方法应用于视觉和音频处理领域，预示着另一个令人兴奋的年头。

*感谢阅读！！如果您对这一2023年的预测有任何想法，我们热烈欢迎在评论中分享。*

## **参考文献：**

[1] Chowdhery, A., Narang, S., Devlin, J., Bosma, M., Mishra, G., Roberts, A., Barham, P., Chung, H. W., Sutton, C., Gehrmann, S., Schuh, P., Shi, K., Tsvyashchenko, S., Maynez, J., Rao, A., Barnes, P., Tay, Y., Shazeer, N., Prabhakaran, V., . . . Fiedel, N. (2022). PaLM: 通过路径扩展语言建模。*arXiv*。[https://doi.org/10.48550/arXiv.2204.02311](https://doi.org/10.48550/arXiv.2204.02311)

[2] Scao, T. L., Fan, A., Akiki, C., Pavlick, E., Ilić, S., Hesslow, D., Castagné, R., Luccioni, A. S., Yvon, F., Gallé, M., Tow, J., Rush, A. M., Biderman, S., Webson, A., Ammanamanchi, P. S., Wang, T., Sagot, B., Muennighoff, N., . . . Wolf, T. (2022). BLOOM: 一种176B参数的开放访问多语言模型。*arXiv*。[https://doi.org/10.48550/arXiv.2211.05100](https://doi.org/10.48550/arXiv.2211.05100)

[3] Zhang, S., Roller, S., Goyal, N., Artetxe, M., Chen, M., Chen, S., Dewan, C., Diab, M., Li, X., Lin, X. V., Mihaylov, T., Ott, M., Shleifer, S., Shuster, K., Simig, D., Koura, P. S., Sridhar, A., Wang, T., & Zettlemoyer, L. (2022). OPT: 开放预训练变换器语言模型。*arXiv*。[https://doi.org/10.48550/arXiv.2205.01068](https://doi.org/10.48550/arXiv.2205.01068)

[4] Wei, J., Wang, X., Schuurmans, D., Bosma, M., Ichter, B., Xia, F., Chi, E., Le, Q., & Zhou, D. (2022). 思维链提示在大型语言模型中引发推理。*arXiv*。[https://doi.org/10.48550/arXiv.2201.11903](https://doi.org/10.48550/arXiv.2201.11903)

[5] Wang, X., Wei, J., Schuurmans, D., Le, Q., Chi, E., Narang, S., Chowdhery, A., & Zhou, D. (2022). 自我一致性改善语言模型中的思维链推理。*arXiv*。[https://doi.org/10.48550/arXiv.2203.11171](https://doi.org/10.48550/arXiv.2203.11171)

[6] Thoppilan, R., De Freitas, D., Hall, J., Shazeer, N., Kulshreshtha, A., Cheng, H., Jin, A., Bos, T., Baker, L., Du, Y., Li, Y., Lee, H., Zheng, H. S., Ghafouri, A., Menegali, M., Huang, Y., Krikun, M., Lepikhin, D., Qin, J., . . . Le, Q. (2022). LaMDA: 对话应用的语言模型。*arXiv*。[https://doi.org/10.48550/arXiv.2201.08239](https://doi.org/10.48550/arXiv.2201.08239)

[7] Shuster, K., Xu, J., Komeili, M., Ju, D., Smith, E. M., Roller, S., Ung, M., Chen, M., Arora, K., Lane, J., Behrooz, M., Ngan, W., Poff, S., Goyal, N., Szlam, A., Boureau, Y., Kambadur, M., & Weston, J. (2022). BlenderBot 3: 一个持续学习以负责任地互动的部署对话代理。*arXiv*。[https://doi.org/10.48550/arXiv.2208.03188](https://doi.org/10.48550/arXiv.2208.03188)

[8] Radford, A., Kim, JW, Xu, T, Brockman, G., McLeavey, C., Sutskever, I. (2022). 通过大规模弱监督实现稳健的语音识别。OpenAI。[https://cdn.openai.com/papers/whisper.pdf](https://cdn.openai.com/papers/whisper.pdf)

[9] Rombach, R., Blattmann, A., Lorenz, D., Esser, P., & Ommer, B. (2021). 《使用潜在扩散模型的高分辨率图像合成》。*arXiv*。 [https://doi.org/10.48550/arXiv.2112.10752](https://doi.org/10.48550/arXiv.2112.10752)

[10] Ramesh, A., Dhariwal, P., Nichol, A., Chu, C., & Chen, M. (2022). 《基于 CLIP 潜变量的分层文本条件图像生成》。*arXiv*。 [https://doi.org/10.48550/arXiv.2204.06125](https://doi.org/10.48550/arXiv.2204.06125)

[11] Tang, Z., Yang, Z., Wang, G., Fang, Y., Liu, Y., Zhu, C., Zeng, M., Zhang, C., & Bansal, M. (2022). 《统一视觉、文本和布局以实现通用文档处理》。*arXiv*。 [https://doi.org/10.48550/arXiv.2212.02623](https://doi.org/10.48550/arXiv.2212.02623)

[12] Wang, P., Yang, A., Men, R., Lin, J., Bai, S., Li, Z., Ma, J., Zhou, C., Zhou, J., & Yang, H. (2022). 《OFA：通过简单的序列到序列学习框架统一架构、任务和模态》。*arXiv*。 [https://doi.org/10.48550/arXiv.2202.03052](https://doi.org/10.48550/arXiv.2202.03052)

[13] Ahn, M., Brohan, A., Brown, N., Chebotar, Y., Cortes, O., David, B., Finn, C., Fu, C., Gopalakrishnan, K., Hausman, K., Herzog, A., Ho, D., Hsu, J., Ibarz, J., Ichter, B., Irpan, A., Jang, E., Ruano, R. J., Jeffrey, K., . . . Zeng, A. (2022). 《做我能做的，不是我说的：将语言建立在机器人能力上》。*arXiv*。 [https://doi.org/10.48550/arXiv.2204.01691](https://doi.org/10.48550/arXiv.2204.01691)

[14] [https://chat.openai.com/chat](https://chat.openai.com/chat)

[15] Ouyang, L., Wu, J., Jiang, X., Almeida, D., Wainwright, C. L., Mishkin, P., Zhang, C., Agarwal, S., Slama, K., Ray, A., Schulman, J., Hilton, J., Kelton, F., Miller, L., Simens, M., Askell, A., Welinder, P., Christiano, P., Leike, J., . . . Lowe, R. (2022). 《训练语言模型以遵循人类反馈的指令》。*arXiv*。 [https://doi.org/10.48550/arXiv.2203.02155](https://doi.org/10.48550/arXiv.2203.02155)

[16] Brown, T. B., Mann, B., Ryder, N., Subbiah, M., Kaplan, J., Dhariwal, P., Neelakantan, A., Shyam, P., Sastry, G., Askell, A., Agarwal, S., Krueger, G., Henighan, T., Child, R., Ramesh, A., Ziegler, D. M., Wu, J., Winter, C., Hesse, C., . . . Amodei, D. (2020). 《语言模型是少样本学习者》。*arXiv*。 [https://doi.org/10.48550/arXiv.2005.14165](https://doi.org/10.48550/arXiv.2005.14165)

[17] Bai, Y., Kadavath, S., Kundu, S., Askell, A., Kernion, J., Jones, A., Chen, A., Goldie, A., Mirhoseini, A., McKinnon, C., Chen, C., Olsson, C., Olah, C., Hernandez, D., Drain, D., Ganguli, D., Li, D., Perez, E., Kerr, J., . . . Kaplan, J. (2022). 《宪法 AI：来自 AI 反馈的无害性》。*arXiv*。 [https://doi.org/10.48550/arXiv.2212.08073](https://doi.org/10.48550/arXiv.2212.08073)

[18] Pillutla, K., Swayamdipta, S., Zellers, R., Thickstun, J., Welleck, S., Choi, Y., & Harchaoui, Z. (2021). MAUVE: 使用发散边界测量神经文本与人类文本之间的差距。*arXiv*。 [https://doi.org/10.48550/arXiv.2102.01454](https://doi.org/10.48550/arXiv.2102.01454)

[19] Perez, E., Ringer, S., Lukošiūtė, K., Nguyen, K., Chen, E., Heiner, S., Pettit, C., Olsson, C., Kundu, S., Kadavath, S., Jones, A., Chen, A., Mann, B., Israel, B., Seethor, B., McKinnon, C., Olah, C., Yan, D., Amodei, D., . . . Kaplan, J. (2022). 通过模型编写的评估发现语言模型行为。*arXiv*。 [https://doi.org/10.48550/arXiv.2212.09251](https://doi.org/10.48550/arXiv.2212.09251)

[20] Iyer, S., Lin, X. V., Pasunuru, R., Mihaylov, T., Simig, D., Yu, P., Shuster, K., Wang, T., Liu, Q., Koura, P. S., Li, X., Pereyra, G., Wang, J., Dewan, C., Celikyilmaz, A., Zettlemoyer, L., & Stoyanov, V. (2022). OPT-IML: 通过泛化视角扩展语言模型指令元学习。*arXiv*。 [https://doi.org/10.48550/arXiv.2212.12017](https://doi.org/10.48550/arXiv.2212.12017)

[21] He, H., Zhang, H., & Roth, D. (2022). 通过检索重新思考：忠实的大型语言模型推理。*arXiv*。 [https://doi.org/10.48550/arXiv.2301.00303](https://doi.org/10.48550/arXiv.2301.00303)

[22] Bohnet, B., Tran, V. Q., Verga, P., Aharoni, R., Andor, D., Soares, L. B., Eisenstein, J., Ganchev, K., Herzig, J., Hui, K., Kwiatkowski, T., Ma, J., Ni, J., Schuster, T., Cohen, W. W., Collins, M., Das, D., Metzler, D., Petrov, S., . . . Webster, K. (2022). 属性问答：属性大型语言模型的评估与建模。*arXiv*。 [https://doi.org/10.48550/arXiv.2212.08037](https://doi.org/10.48550/arXiv.2212.08037)

[23] Gekhman, Z., Zverinski, D., Mallinson, J., & Beryozkin, G. (2022). RED-ACE: 使用置信嵌入进行鲁棒的错误检测。*arXiv*。 [https://doi.org/10.48550/arXiv.2203.07172](https://doi.org/10.48550/arXiv.2203.07172)

[24] Bai, Y., Jones, A., Ndousse, K., Askell, A., Chen, A., DasSarma, N., Drain, D., Fort, S., Ganguli, D., Henighan, T., Joseph, N., Kadavath, S., Kernion, J., Conerly, T., Elhage, N., Hernandez, D., Hume, T., Johnston, S., Kravec, S., . . . Kaplan, J. (2022). 通过人类反馈的强化学习训练有用且无害的助手。*arXiv*。 [https://doi.org/10.48550/arXiv.2204.05862](https://doi.org/10.48550/arXiv.2204.05862)

[25] Baevski, A., Zhou, H., Mohamed, A., & Auli, M. (2020). wav2vec 2.0：一种自监督学习语音表示的框架。*arXiv*。 [https://doi.org/10.48550/arXiv.2006.11477](https://doi.org/10.48550/arXiv.2006.11477)

[26] Hsu, W., Bolte, B., Tsai, Y., Lakhotia, K., Salakhutdinov, R., & Mohamed, A. (2021). HuBERT：通过掩蔽预测隐藏单元进行自监督语音表示学习。*arXiv*。 [https://doi.org/10.48550/arXiv.2106.07447](https://doi.org/10.48550/arXiv.2106.07447)

[27] Ao, J., Wang, R., Zhou, L., Wang, C., Ren, S., Wu, Y., Liu, S., Ko, T., Li, Q., Zhang, Y., Wei, Z., Qian, Y., Li, J., & Wei, F. (2021). SpeechT5: 统一模态编码器-解码器预训练用于口语语言处理。*arXiv*。 [https://doi.org/10.48550/arXiv.2110.07205](https://doi.org/10.48550/arXiv.2110.07205)

[28] Sanh, V., Webson, A., Raffel, C., Bach, S. H., Sutawika, L., Alyafeai, Z., Chaffin, A., Stiegler, A., Scao, T. L., Raja, A., Dey, M., Bari, M. S., Xu, C., Thakker, U., Sharma, S. S., Szczechla, E., Kim, T., Chhablani, G., Nayak, N., . . . Rush, A. M. (2021). 多任务提示训练实现零样本任务泛化。*arXiv*。 [https://doi.org/10.48550/arXiv.2110.08207](https://doi.org/10.48550/arXiv.2110.08207)

[29] Lu, J., Clark, C., Zellers, R., Mottaghi, R., & Kembhavi, A. (2022). Unified-IO: 统一模型用于视觉、语言和多模态任务。*arXiv*。 [https://doi.org/10.48550/arXiv.2206.08916](https://doi.org/10.48550/arXiv.2206.08916)

[30] Mildenhall, B., Srinivasan, P. P., Tancik, M., Barron, J. T., Ramamoorthi, R., & Ng, R. (2020). NeRF: 将场景表示为神经辐射场用于视图合成。*arXiv*。 [https://doi.org/10.48550/arXiv.2003.08934](https://doi.org/10.48550/arXiv.2003.08934)

[31] Chen, C., Gao, R., Calamia, P., & Grauman, K. (2022). 视觉声学匹配。*arXiv*。 [https://doi.org/10.48550/arXiv.2202.06875](https://doi.org/10.48550/arXiv.2202.06875)

[32] Zhu, Z., Peng, S., Larsson, V., Xu, W., Bao, H., Cui, Z., Oswald, M. R., & Pollefeys, M. (2021). NICE-SLAM: 神经隐式可扩展编码用于SLAM。*arXiv*。 [https://doi.org/10.48550/arXiv.2112.12130](https://doi.org/10.48550/arXiv.2112.12130)
