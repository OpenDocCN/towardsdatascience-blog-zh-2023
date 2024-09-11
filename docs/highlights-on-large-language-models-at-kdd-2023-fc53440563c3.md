# KDD 2023 大型语言模型亮点

> 原文：[https://towardsdatascience.com/highlights-on-large-language-models-at-kdd-2023-fc53440563c3?source=collection_archive---------3-----------------------#2023-09-11](https://towardsdatascience.com/highlights-on-large-language-models-at-kdd-2023-fc53440563c3?source=collection_archive---------3-----------------------#2023-09-11)

## 你无法参加 KDD 吗？阅读我的总结，了解会议上的热门话题：LLMs

[](https://gspmoreira.medium.com/?source=post_page-----fc53440563c3--------------------------------)[![Gabriel Moreira](../Images/f191c90e2310c4f105b4287bb27178ee.png)](https://gspmoreira.medium.com/?source=post_page-----fc53440563c3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----fc53440563c3--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----fc53440563c3--------------------------------) [Gabriel Moreira](https://gspmoreira.medium.com/?source=post_page-----fc53440563c3--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F351cff053b19&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhighlights-on-large-language-models-at-kdd-2023-fc53440563c3&user=Gabriel+Moreira&userId=351cff053b19&source=post_page-351cff053b19----fc53440563c3---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----fc53440563c3--------------------------------) ·8 分钟阅读·2023年9月11日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ffc53440563c3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhighlights-on-large-language-models-at-kdd-2023-fc53440563c3&user=Gabriel+Moreira&userId=351cff053b19&source=-----fc53440563c3---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ffc53440563c3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhighlights-on-large-language-models-at-kdd-2023-fc53440563c3&source=-----fc53440563c3---------------------bookmark_footer-----------)![](../Images/701204186bd64ea7fcdac60a750ddbdc.png)

作者提供的图片

几周前，我第一次有机会参加了 ACM SIGKDD（简称 KDD）会议。[KDD 2023](https://kdd.org/kdd2023/) 在加利福尼亚州长滩举行，是数据挖掘领域最古老且最重要的学术会议，开创了与数据科学和大数据相关的主题。

会议持续了5天，吸引了超过2200人参加，其中有大量来自业界的参与者。我对所涵盖的主题多样性感到印象深刻，但从我的角度来看，热点话题是大语言模型（LLMs）和图学习。同时也发现了很多关于推荐系统（RecSys）的内容，我对此特别关注。

在这篇文章中，我总结了我在参加的研讨会、教程和论文报告中对LLMs的亮点，并附上了额外信息的在线资源链接。

*警告*：接下来是一篇长文，包含大量资源链接！

# LLM革命主题演讲（Ed Chi - Google）

著名科学家及谷歌主任Ed H. Chi在备受期待的[LLM革命](https://www.linkedin.com/posts/gabrielspmoreira_kdd23-activity-7094719988606935041-2VPH?utm_source=share&utm_medium=member_desktop)主题演讲中发表了讲话。他回顾了我们经历的技术革命，从互联网到移动设备，再到深度学习的兴起，现如今是LLMs，这无疑令人瞩目。

他讨论了人类智能与ML的不同之处——（1）从少量示例中学习，（2）解释他们的预测/决策，（3）强大的分布外泛化能力——以及LLM如何开始填补这一差距。

随后，他讨论了使LLM能够进行一些推理的技术：（1）链式思维提示，（2）自我一致性，（3）最少到最多提示，以及（4）指令微调。更多内容请参见Denny Zhou在LLM日的演讲（下一部分）。

最后，他分享了他对LLM未来挑战的愿景：（1）责任与安全，（2）事实性、基础及归属，（3）人类<->AI内容循环与生态系统，以及（4）个性化和用户记忆。

# LLM日

KDD专门设立了一天用于LLM，有5位不同的研究人员就微软、Google DeepMind、Meta、智谱AI和OpenAI如何推动LLM技术的发展、面临的挑战以及他们对该领域未来演变的预见进行了详细讲解。[演示幻灯片](https://bigmodel.ai/llmday-kdd23/)已经发布，强烈推荐查看。

## **从文档到对话：LLMs如何塑造未来工作（Jaime Teevan — Microsoft）** ([幻灯片](https://lfs.aminer.cn/misc/teevan-kdd2023.pdf))

这次演讲涵盖了关于LLM质量问题的不同研究和应用主题（例如，如何处理低资源语言）、云端高效训练、检索增强生成（RAG）作为利用私人知识库（KB）的可持续方式、微调中的差分隐私、提示工程和聊天记录分析的最佳实践。

## **教会语言模型推理（Denny Zhou - Google DeepMind）(**[**幻灯片**](http://dennyzhou.github.io/Teach-LLMs-to-Reason-KDD-2023.pdf)**)**

他关注了ML的终极目标——推理——作为仅通过少量示例进行学习的方式。总结了一些使LLM如此强大的核心技术：

+   **思维链（CoT）**——逐步思考的提示技术，提供一些少量示例以概述推理过程

+   **最小到最大提示**（规划 + 推理）——将复杂问题分解为一系列子问题，依次解决

+   [**自洽（SC）解码**](https://arxiv.org/abs/2203.11171)——一种从采样的多样化推理路径生成不同答案的技术。最终答案不是贪婪选择的答案，而是这些不同答案的多数票。这种技术似乎对LLMs非常有效，让我想起了模型集成的力量！

+   **指令调优**——对预训练LLMs进行微调以遵循指令的过程。这使得零样本提示新任务成为可能。这对启用如Google Bard或Open.ai ChatGPT这样的问答系统至关重要。

## **Llama 2: 开放基础和微调聊天模型（Vedanuj Goswami - Meta FAIR）（**[**幻灯片**](https://lfs.aminer.cn/misc/Llama%202%20at%20KDD%20LLM%20Final.pdf)**）**

他介绍了Meta在训练Llama基础模型和使用SFT数据（高质量的27k收集样本）进行指令微调的历程。他们的奖励模型是在1M收集样本上训练的。他还描述了他们的迭代微调与RLHF，评估（人类，安全）。他在演讲结束时谈到了训练和部署LLMs面临的挑战，我在这里进行了记录：

+   获取更多数据，多语言，多模态

+   扩展到数千个GPU，并提高MFU（模型FLOPs利用率）

+   设计高效的训练和推理架构，硬件-软件共同设计

+   持续学习和更新知识

+   改善事实准确性和引用来源

+   减少幻觉和承认不确定性

+   删除有害、攻击性或偏见内容

+   适应超越训练数据的世界知识

## **从GLM-130B到ChatGLM（Peng Zhang - Zhipu AI）（**[**幻灯片**](https://lfs.aminer.cn/misc/kdd23-chatglm-peng-0808.pdf)**）**

我了解了Zhipu AI，这是一家在中文领域挑战Open.ai的公司。他们作为钻石赞助商在KDD上有强大存在，并在宴会庆典上发表了主题演讲。Zhipu展示了他们在许多任务中表现最佳的中文LLM结果，甚至优于GPT-4。他们描述了如何在其基础模型（GLM-130B）之上开发ChatGLM和VisualGLRM。他们在[HuggingFace上开源了ChatGLM-6B](https://huggingface.co/THUDM/chatglm-6b)。

## **大语言模型复兴：范式与挑战（Jason Wei — OpenAI）（**[**幻灯片**](https://docs.google.com/presentation/d/1BF0icnNER3Fy4v-9RSm0fKqXx3QuvU0dIi6mp6VpZHg/edit?pli=1#slide=id.g16197112905_0_0)**）**

关于**规模规律**的非常务实的演讲，讲述了如何达到当前LLMs的状态，以及当LLMs参数超过100B时可以观察到的**新兴能力**（包括推理）。还谈到了**通过提示技术进行推理**：Chain-of-Thought和Least-to-most提示。

# 大规模AI模型的基础与应用预训练、微调和基于提示的学习研讨会

我认为[LLM-AI研讨会](https://llm-ai.github.io/llmai/)是会议上最具争议的一个。我早上实在无法参加，因为在KDD早晨的主旨发言后，小房间被人群完全挤满了。幸运的是，在咖啡休息之后，我找到了一个座位，能够参加几场会议。

## **LLMs时代的NLP研究（Shafiq Joty - Salesforce）**

他描述了SalesForce的XGen LLM — 一个内部的JaxFormer库，继承了LLaMA-7B，并通过WizardLM进行了指令微调，能够基于非结构化和结构化数据（例如Spark和SQL数据库）回答问题。还介绍了一些他们用于推理准备的技术，包括通过[Chain-of-Thought](https://arxiv.org/pdf/2305.13269.pdf)来分解问题，并通过在自然句子、SPARQL和SQL上训练模型进行自适应查询生成（LoRA）来选择最相关的知识库。该过程为每个推理步骤生成一个查询，并在知识源上执行。

## **模块化大型语言模型和最小化人工监督的原则驱动对齐（YiKang Shen — IBM）**

这个演讲介绍了IBM的基础模型：（1）***Sandstone*** — 适合针对特定任务微调的编码器-解码器架构，（2）***Granite***— 仅解码器，类似于GPT的生成任务，（3）***Obsidian*** — 一种新的模块化架构，提供高效的推理能力和在各种任务中的表现水平

他还描述了他们在LLM方面面临的一些挑战：

+   **效率** — 如何训练和服务Llama 65B模型。

+   **可扩展性** — 如何用不断增长的训练语料库、不同语言和客户的私人数据来更新LLM

+   **灵活性** — 能够在不同设备上使用不同复杂度的LLM模型，满足不同的延迟要求。

他们展示了他们的[ModuleFormer](https://arxiv.org/pdf/2306.04640.pdf)，它通过稀疏专家混合（SMoE）来解决上述问题。它可以为每个输入激活其模块的子集，比密集型LLMs对灾难性遗忘的免疫力更强。对ModuleFormer进行微调可以使模块子集专门化，而与任务无关的模块可以被修剪，以实现轻量级部署。

# **教程**

这些教程是同时进行的，所以我不得不分配时间去参加两个讲座。幸运的是，他们的优秀幻灯片被提供了，并且非常详细。

## **面向下一代智能助理，利用 LLM 技术 — Meta (**[**幻灯片**](https://drive.google.com/file/d/1DlItXlC17nPpuMxj0STrIVnNJX1B30SF/view)**)**

关于智能助理的非常全面的教程，这些智能助理是多模态的，并且能够利用用户的位置、用户可以听到和看到的内容（例如，使用 Google Glasses、Meta Quest 2）作为上下文。教程描述了不同模块之间的连接方式：ASR、CV、NLU、对话状态跟踪器、NLG、TTS、KB、个性化/推荐和隐私保护等。

## **预训练语言表示用于文本理解：一个弱监督视角 - 伊利诺伊大学香槟分校 (**[**幻灯片**](https://yumeng5.github.io/kdd23-tutorial/)**)**

介绍了语言模型预训练的进展，将其与传统的 NLU 任务进行了比较，并描述了 LLM 如何用于提取实体和层次关系、主题发现和文档理解。我从这个教程中获得的一个良好见解是使用一些 NLU 技术来评估生成的答案是否回答了问题。

# 研究论文

这是我喜欢的一些 NLP / LLM 论文的简短列表。

## 端到端查询术语加权（Google）（[论文](https://dl.acm.org/doi/10.1145/3580305.3599815)）

这篇出色的论文结合了词汇和语义检索系统。他们在词汇检索器的基础上提出了一种术语加权 BERT（TW-BERT）模型来构建他们的解决方案。TW-BERT 学会预测单个 n-gram（例如，单字和双字）查询输入术语的权重。这些推断出的权重和术语可以直接被检索系统用来执行查询搜索。学习到的权重可以被标准词汇检索器（例如 BM25）以及其他检索技术（如查询扩展）轻松利用。

## UnifieR：大型检索的统一检索器（Microsoft）（[论文](https://dl.acm.org/doi/10.1145/3580305.3599927)）

另一个有趣的提议是将稠密向量和基于词汇的检索统一到一个具有双重表示能力的模型中。它通过两阶段自学习流程进行训练，并在最新的词汇和稠密检索模型上有所改进。

## 学会与对话搜索中的先前回合相关（[论文](https://dl.acm.org/doi/10.1145/3580305.3599411)）

通常在多轮对话中，历史查询用于扩展当前查询。然而，并非所有的先前查询都与下一个问题相关或有用。该论文提出了一种方法，用于选择对当前查询有用的相关历史查询。他们使用伪标注机制来标注相关的历史查询，并与检索器训练一起训练选择模型。

## GLM-Dialog：面向知识驱动对话生成的噪声容忍预训练（[论文](https://dl.acm.org/doi/10.1145/3580305.3599832)）

描述了如何通过基于RAG的对话系统使用嘈杂的私人知识库。他们提出了一种新颖的评估方法，使人们可以同时与多个部署的机器人对话，并隐性地比较其表现，而不是使用多维度指标进行显式评分。

## **集群语言模型以改善电子商务检索和排名：利用查询相似性和微调以实现个性化结果（Home Depot）（**[**论文**](https://drive.google.com/file/d/1J3db1I59IvsaKFr4Q-3PTaLU49XLHo4b/view)**）**

论文描述了Home Depot如何通过使用特定集群的语言模型来改善电子商务的语义搜索，而不是使用典型的双编码器架构。他们的方法首先使用K-Means将用户查询映射到集群，并使用所选的集群特定语言模型进行检索。

# **结论**

这些是我在KDD 2023上对LLM的重点总结。希望你能从这个总结和我编纂的资源中找到一些有用的信息和灵感。

*“抱歉，帖子写得太长。如果我有更多时间，我会写得更短一点”* :)
