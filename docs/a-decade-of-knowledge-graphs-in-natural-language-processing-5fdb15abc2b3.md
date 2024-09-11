# 自然语言处理中的知识图谱：十年回顾

> 原文：[https://towardsdatascience.com/a-decade-of-knowledge-graphs-in-natural-language-processing-5fdb15abc2b3?source=collection_archive---------8-----------------------#2023-03-14](https://towardsdatascience.com/a-decade-of-knowledge-graphs-in-natural-language-processing-5fdb15abc2b3?source=collection_archive---------8-----------------------#2023-03-14)

## 关于结合结构化和非结构化知识在自然语言处理中的研究现状

[](https://medium.com/@tim.schopf?source=post_page-----5fdb15abc2b3--------------------------------)[![Tim Schopf](../Images/7d98a87af243ae6a82f837aa04ac2675.png)](https://medium.com/@tim.schopf?source=post_page-----5fdb15abc2b3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5fdb15abc2b3--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----5fdb15abc2b3--------------------------------) [Tim Schopf](https://medium.com/@tim.schopf?source=post_page-----5fdb15abc2b3--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7fe3665aa3e3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-decade-of-knowledge-graphs-in-natural-language-processing-5fdb15abc2b3&user=Tim+Schopf&userId=7fe3665aa3e3&source=post_page-7fe3665aa3e3----5fdb15abc2b3---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5fdb15abc2b3--------------------------------) ·8分钟阅读·2023年3月14日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F5fdb15abc2b3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-decade-of-knowledge-graphs-in-natural-language-processing-5fdb15abc2b3&user=Tim+Schopf&userId=7fe3665aa3e3&source=-----5fdb15abc2b3---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5fdb15abc2b3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-decade-of-knowledge-graphs-in-natural-language-processing-5fdb15abc2b3&source=-----5fdb15abc2b3---------------------bookmark_footer-----------)![](../Images/c288205bdd2f7682d7f07f8bcfe638d9.png)

图片由 [Billy Huynh](https://unsplash.com/@billy_huy?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

*这篇文章基于我们在AACL-IJCNLP 2022的论文* [*《自然语言处理中的知识图谱：十年回顾》*](https://aclanthology.org/2022.aacl-main.46/)*。你可以在那里阅读更多细节。*

**知识图谱（KGs）自2012年Google推出KG以来在学术界和工业界都受到了极大关注**（[Singhal, 2012](https://blog.google/products/search/introducing-knowledge-graph-things-not/)）。作为实体之间语义关系的表示，知识图谱已被证明对自然语言处理（NLP）**特别相关**，并且近年来在流行度上迅速增长，这一趋势似乎正在加速🚀。鉴于这一领域的研究工作越来越多，NLP研究社区中已经对几种与知识图谱相关的方法进行了综述。然而，迄今为止，仍缺乏对已建立主题的综合研究，并对个别研究流的成熟度进行回顾。为弥补这一空白，我们系统地分析了507篇关于NLP中知识图谱的文献。因此，我们呈现了研究领域的结构化概述，提供了任务分类法，总结了我们的发现，并强调了未来工作的方向。

## 什么是自然语言处理？

自然语言处理（NLP）是[语言学](https://en.wikipedia.org/wiki/Linguistics)、[计算机科学](https://en.wikipedia.org/wiki/Computer_science)和[人工智能](https://en.wikipedia.org/wiki/Artificial_intelligence)的一个子领域，涉及计算机与人类语言之间的互动，特别是如何编程计算机以处理和分析大量的[自然语言](https://en.wikipedia.org/wiki/Natural_language)数据（[Wikipedia](https://en.wikipedia.org/wiki/Natural_language_processing)）。

## 知识图谱是什么？

知识图谱作为以机器可读格式语义化表示现实世界实体的一个方法已经出现。大多数研究隐含地采用了知识图谱的广泛定义，即*“一个旨在积累和传达现实世界知识的图谱，其中节点代表感兴趣的实体，边代表这些实体之间的关系”*（[Hogan et al., 2022](https://dl.acm.org/doi/10.1145/3447772)）。

# **我们为什么在NLP中使用知识图谱？**

其基本范式是结构化和非结构化知识的结合可以使所有类型的自然语言处理任务受益。例如，将知识图谱中的结构化知识注入语言模型中的上下文知识，可以提升下游任务的性能（[Colon-Hernandez et al., 2021](https://arxiv.org/abs/2101.12294)）。此外，鉴于目前对大型语言模型（如ChatGPT）的公众讨论，我们可以利用知识图谱来验证并在必要时纠正生成模型中的虚假和错误陈述。随着知识图谱重要性的日益增加，也有越来越多的努力在从非结构化文本中构建新的知识图谱。

# 知识图谱在自然语言处理中的应用是什么？

## 研究领域的特点 🏞️

下图展示了十年观察期内的出版物分布情况。

![](../Images/053913c7ce26b03916cfef5ffd7017a1.png)

2012年至2021年的论文数量分布（图像由作者提供）。

尽管第一篇出版物出现于2013年，但2013年至2016年间，年度出版物数量增长缓慢。从2017年起，出版物数量几乎每年翻倍。由于这些年内研究兴趣的显著上升，90%以上的出版物都来源于这五年。尽管2021年的增长趋势似乎停止了，这很可能是因为数据导出发生在2022年第一周，遗漏了许多2021年的研究，这些研究在2022年晚些时候才被列入数据库。然而，这一趋势清楚地表明，知识图谱正受到自然语言处理研究社区越来越多的关注。

此外，我们观察到，研究文献中探索的领域数量迅速增长，与年度论文数量相匹配。在下图中，显示了十个最频繁的领域。

![](../Images/621378379fd793ddc615aaa93f948828.png)

按最热门应用领域划分的论文数量（图像由作者提供）。

显著的是，健康领域远远是最突出的领域。健康领域出现的频率是学术领域的两倍多，后者排在第二位。其他热门领域包括工程、商业、社交媒体或法律。考虑到领域的多样性，显而易见，知识图谱自然适用于许多不同的情境。

## 研究文献中的任务 📖

基于文献中关于知识图谱的任务，我们开发了下图所示的实证分类法。

![](../Images/70692a9fa6f645027ae0d7db9eaa4756.png)

涉及自然语言处理中的知识图谱的任务分类（图像由作者提供）。

两个顶级类别包括知识获取和知识应用。**知识获取**包含从非结构化文本中构建知识图谱的自然语言处理任务（**知识图谱构建**）或对已经构建的知识图谱进行推理（**知识图谱推理**）。知识图谱构建任务进一步分为两个子类别：**知识提取**，用于将实体、关系或属性填充到知识图谱中，以及**知识整合**，用于更新知识图谱。**知识应用**作为第二个顶级概念，涵盖了通过知识图谱的结构化知识来增强的常见自然语言处理任务。

## **知识图谱构建 🏗️**

**实体提取**的任务是构建知识图谱（KGs）的起点，用于从非结构化文本中提取现实世界的实体。一旦相关实体被识别出来，就会通过**关系提取**任务找出它们之间的关系和互动。许多论文使用实体提取和关系提取来构建新的知识图谱，例如，用于新闻事件或学术研究。**实体链接**是将文本中识别出的实体与知识图谱中已存在的实体进行链接的任务。由于同义或类似的实体常常存在于不同的知识图谱或不同的语言中，**实体对齐**可以被执行，以减少未来任务中的冗余和重复。制定知识图谱的规则和方案，即其结构和知识展示的格式，是通过**本体构建**任务完成的。

## **知识图谱推理** 🧠

一旦构建完成，知识图谱包含结构化的世界知识，并且可以通过推理得出新的知识。因此，分类实体的任务被称为**实体分类**，而**链接预测**是推断现有知识图谱中实体之间缺失链接的任务，通常通过对实体进行排序作为查询的可能答案。**知识图谱嵌入**技术用于创建图的密集向量表示，以便可以用于下游的机器学习任务。

## **知识应用** 🛠️

现有的知识图谱可以用于多种流行的自然语言处理（NLP）任务。这里我们概述了最常见的任务。**问答（QA）**被发现是使用知识图谱的最常见NLP任务。这个任务通常被分为文本问答和基于知识库的问答（KBQA）。文本问答从非结构化文档中提取答案，而KBQA则从预定义的知识库中提取答案。KBQA自然与知识图谱紧密相关，而文本问答也可以通过使用知识图谱作为回答问题的常识知识来源来进行。这种方法不仅有助于生成答案，还使答案更加可解释。**语义搜索**指的是“有意义的搜索”，其目标不仅仅是搜索字面匹配，还要理解搜索意图和查询上下文。这个标签表示了利用知识图谱进行搜索、推荐和分析的研究。例如，大型语义网络概念网（ConceptNet）和学术交流及其关系的知识图谱——微软学术图谱（Microsoft Academic Graph）。**对话界面**是另一个可以受益于知识图谱中世界知识的NLP领域。我们可以利用知识图谱中的知识来生成在特定上下文中更具信息性和适当性的对话代理响应。

**自然语言生成（NLG）** 是 NLP 和计算语言学的一个子领域，关注于从头生成自然语言输出的模型。知识图谱在这个子领域中用于从知识图谱生成自然语言文本、生成问答对、多模态任务中的图像描述，或在低资源环境下进行数据增强。**文本分析** 结合了各种分析性 NLP 技术和方法，用于处理和理解文本数据。典型任务包括情感检测、主题建模或词义消歧。**增强型语言模型** 是将诸如 BERT ([Devlin et al., 2019](https://aclanthology.org/N19-1423.pdf)) 和 GPT ([Radford et al., 2018](https://gwern.net/doc/www/s3-us-west-2.amazonaws.com/d73fdc5ffa8627bce44dcda2fc012da638ffb158.pdf)) 等大型预训练语言模型与知识图谱中包含的知识相结合。由于预训练语言模型从大量的非结构化训练数据中获取知识，一种上升的研究趋势是将它们与结构化知识结合起来。知识图谱中的知识可以注入到语言模型的输入、架构、输出或其组合中。

## 使用知识图谱在 NLP 中的热门任务 📈

下图展示了在 NLP 中使用知识图谱的最受欢迎的任务。

![](../Images/35a9cd61c6cb49ed237845051447d341.png)

2013 年至 2021 年间最受欢迎任务的论文数量分布（图像作者提供）。

我们可以观察到，诸如关系提取或语义搜索等任务已经存在了一段时间，并且持续稳步增长。在我们的研究中，我们将这些任务作为指标之一，以得出关系提取或语义搜索等任务已经相对成熟的结论。相比之下，增强型语言模型和知识图谱嵌入任务仍然可以被认为相对不成熟。这可能是因为这些任务仍然相对年轻，且研究较少。上图显示，这两项任务从 2018 年开始研究数量急剧增加，并自那时起吸引了大量兴趣。

# 结论 💡

近年来，知识图谱在 NLP 研究中的重要性日益提升。自 2013 年首次发表相关文献以来，全球研究人员对从 NLP 角度研究知识图谱给予了越来越多的关注，特别是在过去五年中。为了提供这一成熟研究领域的概述，我们对知识图谱在 NLP 中的应用进行了多方面的调查。我们的发现显示，大量涉及知识图谱的 NLP 任务已在各个领域得到了研究。涉及使用实体提取和关系提取构建知识图谱的论文占所有工作的主要部分。应用的 NLP 任务，如 QA 和语义搜索，也有着强大的研究社区。近年来最突出的主题是增强型语言模型、QA 和知识图谱嵌入。

一些概述的任务仍然局限于研究领域，而另一些任务已经在许多实际应用中找到了应用。我们观察到，知识图谱的构建任务和对知识图谱的语义搜索是最广泛应用的任务。在自然语言处理任务中，问答系统和对话接口已经被应用到许多现实生活领域，通常以数字助手的形式出现。像知识图谱嵌入和增强语言模型这样的任务仍在研究中，在实际场景中尚未得到广泛应用。我们预期，随着增强语言模型和知识图谱嵌入的研究领域成熟，将会有更多的方法和工具被研究用于这些任务。

# 来源

[](https://aclanthology.org/2022.aacl-main.46/?source=post_page-----5fdb15abc2b3--------------------------------) [## 十年知识图谱在自然语言处理中的应用：一项调查

### Phillip Schneider, Tim Schopf, Juraj Vladika, Mikhail Galkin, Elena Simperl, Florian Matthes. 第2届会议论文集…

[aclanthology.org](https://aclanthology.org/2022.aacl-main.46/?source=post_page-----5fdb15abc2b3--------------------------------) [](https://github.com/sebischair/KG-in-NLP-survey?source=post_page-----5fdb15abc2b3--------------------------------) [## GitHub - sebischair/KG-in-NLP-survey: 本仓库包含了507篇被注释的论文…

### 本仓库包含了507篇被注释的论文，这些论文被纳入了研究：“十年知识图谱在自然语言处理中的应用”。

[github.com](https://github.com/sebischair/KG-in-NLP-survey?source=post_page-----5fdb15abc2b3--------------------------------)
