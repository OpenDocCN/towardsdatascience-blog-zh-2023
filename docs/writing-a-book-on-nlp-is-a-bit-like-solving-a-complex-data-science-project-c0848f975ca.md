# 编写关于自然语言处理的书籍有点像解决一个复杂的数据科学项目

> 原文：[`towardsdatascience.com/writing-a-book-on-nlp-is-a-bit-like-solving-a-complex-data-science-project-c0848f975ca`](https://towardsdatascience.com/writing-a-book-on-nlp-is-a-bit-like-solving-a-complex-data-science-project-c0848f975ca)

## 对刘易斯·坦斯特尔的访谈，他是《[**自然语言处理与变压器**](https://learning.oreilly.com/library/view/natural-language-processing/9781098136789/)》一书的合著者

[](https://pandeyparul.medium.com/?source=post_page-----c0848f975ca--------------------------------)![帕鲁尔·潘迪](https://pandeyparul.medium.com/?source=post_page-----c0848f975ca--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c0848f975ca--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----c0848f975ca--------------------------------) [Parul Pandey](https://pandeyparul.medium.com/?source=post_page-----c0848f975ca--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----c0848f975ca--------------------------------) ·7 分钟阅读·2023 年 2 月 6 日

--

*一系列访谈，突显数据科学领域作家的非凡工作及其写作历程。*

![](img/5fddda751df11beee48e146f3e6a365f.png)

图片由刘易斯·坦斯特尔提供

> “在小说中，语言和引发的感官体验很重要，而在技术写作中，内容和传达的信息才是重要的。”
> 
> ― **克里斯塔·范·兰**，[**《技术写作内幕指南》**](https://www.goodreads.com/work/quotes/20300600)

*最后编辑于 2023 年 2 月 6 日*

作为一名作家，我对揭示我们阅读的书籍背后的故事充满了浓厚的兴趣，特别是在机器学习领域。这些作家具有将人工智能的复杂性转化为既信息丰富又引人入胜的文字的非凡能力，实在是令人惊叹。我的目标是通过一系列访谈，把他们的故事展现出来，揭示一些在人工智能领域的知名作者的故事。

# 认识作者：刘易斯·坦斯特尔

**刘易斯·坦斯特尔** 是一位杰出的机器学习工程师，目前在 Hugging Face 工作。他在为初创企业和大型企业构建机器学习应用方面拥有丰富的经验，专注于自然语言处理、拓扑数据分析和时间序列等领域。拥有理论物理学博士学位的刘易斯曾有机会在澳大利亚、美国和瑞士等多个国家担任研究职位。他目前的工作集中在为自然语言处理社区开发创新工具，并赋予个人有效使用这些工具的知识和技能。

刘易斯是书籍《自然语言处理与 Transformers》的合著者之一，与[**Leandro von Werra**](https://twitter.com/lvwerra?s=20&t=XOZTW3iOxNZoAopPR5LRsw)和[**Thomas Wolf**](https://twitter.com/Thom_Wolf)**合作**。这本书是该领域最新进展的全面指南，是任何希望深入理解自然语言处理以及如何将其应用于现实世界问题的人的绝佳资源。

[](https://learning.oreilly.com/library/view/natural-language-processing/9781098136789/?source=post_page-----c0848f975ca--------------------------------) [## 自然语言处理与 Transformers, 修订版

### 自 2017 年引入以来，transformers 已迅速成为实现...

[learning.oreilly.com](https://learning.oreilly.com/library/view/natural-language-processing/9781098136789/?source=post_page-----c0848f975ca--------------------------------)

**问: 这本书的构思是如何产生的？**

*刘易斯:* 尽管我们在 2020 年开始了这本书，但它的起源故事实际上可以追溯到 2019 年，当时 Leandro 和我首次开始使用 Transformer 模型。当时，Jay Alammar 的精彩[博客文章](https://jalammar.github.io/)和 Sasha Rush 的[《注释 Transformer》](http://nlp.seas.harvard.edu/2018/04/03/attention.html)是理解这些模型如何工作的为数不多的书面资源。这些文章（并且仍然是！）非常有助于理解，但我们觉得在指导人们如何将 Transformer 应用于工业用例方面存在一个空白。因此，在 2020 年，我们有了一个有些冒失的想法，将我们从工作中学到的知识结合成一本书。我的妻子建议我们联系 Thomas 看他是否有兴趣成为合著者，令我们惊讶的是，他同意了！

**问: 能否为读者总结一下书中涵盖的主要内容？**

*刘易斯:* 正如标题所示，这本书是关于将 Transformer 模型应用于自然语言处理任务的。大多数章节围绕着你在行业中可能遇到的单一用例进行结构化。书中涵盖了文本分类、命名实体识别和问答等核心应用。我们从精彩的[fast.ai](http://fast.ai/)课程中汲取了很多灵感（这也是我开始深度学习的途径！），因此本书采用了动手实践的风格，强调通过代码解决现实世界的问题。在早期章节中，我们介绍了自注意力机制和迁移学习的概念，这些概念是 Transformer 成功的基础。

> 我对新作者的主要建议是找到可以深度批评你的想法和写作的合著者或同事。

书的后半部分深入探讨了更高级的主题，如为生产环境优化 Transformers 和处理标注数据稀少的场景（即每个数据科学家的噩梦 😃）。我最喜欢的章节之一是关于从头开始训练一个 GPT-2 规模模型，包括如何创建一个大规模语料库并在分布式基础设施上进行训练！书的结尾着眼于未来，通过突出一些涉及 Transformers 和其他模式（如图像和音频）的激动人心的最新进展来作结。

**问：你与 Leandro von Werra 和 Thomas Wolf 共同编写了这本书。与多位作者一起编写书籍是什么感觉？**

写一本关于 NLP 的书有点像解决一个复杂的数据科学项目。在各种挑战中，你需要为每个章节设计用例和故事，找到合适的数据和模型，并确保代码复杂性保持在最低限度，因为阅读长块代码一点也不有趣。而且就像任何数据科学项目一样，一些章节涉及到进行几十次实验。面对这些挑战，我发现与 Leandro 和 Thomas 一起写书是一个极其宝贵的经历！特别是，与能够头脑风暴或检查代码的合著者一起工作尤其有帮助。当然，多位作者面临的一个挑战是保持全书的写作风格一致，我们很幸运能有 O’Reilly 的优秀编辑帮助我们实现这一点。

**问：你认为这本书的目标读者是谁？**

我们写这本书是为了数据科学家和机器学习/软件工程师，他们可能听说过最近涉及 Transformers 的突破，但仍需要一个深入的指南来帮助他们将这些模型适应到自己的用例中。换句话说，就是像我自己大约 1-2 年前一样的人！我们设想这本书对行业从业者或希望从相关领域（例如，想要构建机器学习驱动应用的工程师）转向自然语言处理（NLP）的人会最有帮助。

**问：你认为怎样才能最大化利用这本书——先读后写代码，还是边读边写代码？**

我们实际上使用 Jupyter notebooks 和一个名为 [fastdoc](https://github.com/fastai/fastdoc) 的工具写了整本书，因此每一行代码都可以在我们提供的 [GitHub](https://github.com/nlp-with-transformers/notebooks) 附带的 notebooks 中执行。我建议将书的章节和附带的代码放在一起，这样你可以在阅读时快速实验输入和输出。我们还计划在书发布时举办一些直播活动，请关注 [书籍官网](https://nlp-with-transformers.github.io/website/) 以了解这些活动的时间。

**问：你会给刚开始写作的新作者什么建议？**

呵呵，我的第一条建议是问自己是否真的确定想写一本书。我的第二条建议是不要在刚刚有了宝宝之后……在疫情期间写书 😃

> 写一本关于 NLP 的书有点像解决一个复杂的数据科学项目。在各种挑战中，你需要为每一章设计用例和故事，找到合适的数据和模型，并确保代码复杂性保持在最低，因为阅读长篇代码一点也不有趣。

说正经的，我给新作者的主要建议是找到可以深入批评你想法和写作的合著者或同事。我们很幸运地有像 Aurélien Geron 和 Hamel Husain 这样的经验丰富的作者提供了详细的评论，他们的反馈显著提高了书籍质量。这种反馈非常宝贵，因为作为一个作者，你有时会忘记最初学习一个主题时哪些方面很难掌握。巧合的是，Aurélien 为我们的书写了前言！

**问：你最喜欢的书和作者是谁（技术或非技术领域）？**

哦，这是个棘手的问题！我是科幻小说的忠实读者，[**《三体》**](https://en.wikipedia.org/wiki/The_Three-Body_Problem_(novel)) **由刘慈欣** 编写，是我最喜欢的系列之一，因为它提供了对**费米悖论**的一个全新解决方案——即为什么没有可观察到的外星文明证据，尽管有大量类地行星的恒星？我也喜欢阅读关于物理学和计算机科学等科学领域历史的书籍和传记。我最近的一个最爱是 [**《未来之人》由 Ananyo Bhattacharya 编写**](https://www.amazon.in/Man-Future-Visionary-Life-Neumann/dp/0241398851)，这本书讲述了杰出的约翰·冯·诺依曼的生活。虽然作为物理学学生我遇到过冯·诺依曼的一些工作（他写了关于量子力学的杰作），但我没有完全意识到他对数学和科学领域的广泛贡献。这本书非常出色地描述了他的工作及其迷人的历史——强烈推荐！

👉 **你想与 Lewis 建立联系吗？请关注他的** [**Twitter**](https://twitter.com/_lewtun)**.**

👉 **阅读本系列的其他访谈：**

[](/dont-just-take-notes-turn-them-into-articles-and-share-them-with-others-72aa43b83e29?source=post_page-----c0848f975ca--------------------------------) [## 不要只是做笔记 — 把它们变成文章并与他人分享

### 一次与 Alexey Grigorev 的访谈，他是《机器学习书营》的作者。

[不要只是做笔记，把它们转化为文章并与他人分享](https://towardsdatascience.com/dont-just-take-notes-turn-them-into-articles-and-share-them-with-others-72aa43b83e29?source=post_page-----c0848f975ca--------------------------------) [](/you-do-not-become-better-by-employing-fancy-techniques-but-by-working-on-the-fundamentals-17d5c471c69c?source=post_page-----c0848f975ca--------------------------------) [## 你并不会通过使用花哨的技巧变得更好，而是通过专注于基础

### 与 Radek Osmulski 的访谈，他是《Meta Learning: How To Learn Deep Learning And Thrive In The…》一书的作者

[你并不会通过使用花哨的技巧变得更好，而是通过专注于基础](https://towardsdatascience.com/you-do-not-become-better-by-employing-fancy-techniques-but-by-working-on-the-fundamentals-17d5c471c69c?source=post_page-----c0848f975ca--------------------------------) [](/publishing-is-powerful-as-it-serves-as-a-catalyst-for-scope-and-writing-decisions-713306e8a0d?source=post_page-----c0848f975ca--------------------------------) [## 发布是强大的，因为它作为范围和写作决策的催化剂

### 与 Christoph Molnar 的访谈，他是《Interpretable Machine Learning》一书的作者

[发布是强大的，因为它作为范围和写作决策的催化剂](https://towardsdatascience.com/publishing-is-powerful-as-it-serves-as-a-catalyst-for-scope-and-writing-decisions-713306e8a0d?source=post_page-----c0848f975ca--------------------------------) ![](img/90c29813c4ef86e9865b1cc09aca3796.png)
