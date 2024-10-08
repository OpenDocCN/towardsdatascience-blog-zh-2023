# 被禁书籍：美国书籍禁令的数据分析

> 原文：[`towardsdatascience.com/the-forbidden-pages-a-data-analysis-of-book-bans-in-the-us-e03a22fb0fa8`](https://towardsdatascience.com/the-forbidden-pages-a-data-analysis-of-book-bans-in-the-us-e03a22fb0fa8)

## 关于书籍、禁令、审查以及一点 AI

[](https://medium.com/@artfish?source=post_page-----e03a22fb0fa8--------------------------------)![Yennie Jun](https://medium.com/@artfish?source=post_page-----e03a22fb0fa8--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e03a22fb0fa8--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----e03a22fb0fa8--------------------------------) [Yennie Jun](https://medium.com/@artfish?source=post_page-----e03a22fb0fa8--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e03a22fb0fa8--------------------------------) ·阅读时间 17 分钟·2023 年 3 月 6 日

--

![](img/e34e733f6515806141a573a835d66751.png)

一位女性站在空中，俯瞰从天而降的书籍堆。使用 Stable Diffusion 生成

*转发自我博客上的原文:* [*🎨 art fish intelligence 🐡*](https://www.artfish.ai/p/the-forbidden-pages-a-data-analysis)

在过去几年中，美国各州被禁书籍的数量不断增加——[预计这一趋势在 2023 年会加速](https://www.theguardian.com/us-news/2022/dec/24/us-book-bans-streak-of-extremism)。最近在[德克萨斯州](https://www.texastribune.org/2022/09/19/texas-book-bans/)、[犹他州](https://www.kuer.org/education/2022-11-29/these-are-the-22-books-removed-from-utahs-alpine-school-district)和[佛罗里达州](https://www.buzzfeednews.com/article/juliareinstein/florida-school-book-bans-teachers-confusion)的书籍禁令导致数百本书被撤下课堂和图书馆的书架。这些被标记为“敏感材料”的书籍中，很多是关于 LGBTQ+人士和有色人种的书籍。

在这篇文章中，我分析了最近在美国被禁的书籍和作者：

+   我使用了 2021-2022 年被禁书籍的数据集，并结合了来自 Google Books 的元数据，以研究这些书籍的主要主题。我发现被禁的书籍不仅包括关于 LGBTQ+和性别的书籍，还包括关于黑人历史和女性科学家的书籍。许多被禁书籍是近期（2000 年后）出版的，并针对年轻读者。

+   我研究了撰写顶级被禁书籍的作者类型。这些作者中的大多数认同为女性、非二元性别和/或有色人种。

+   我选取了被禁书籍中最常见的书籍，并询问大型语言模型（如 GPT-3）是否会推荐这些书籍给不同年龄段的课堂。

[大多数美国人都反对书籍禁令](https://www.cbsnews.com/news/book-bans-opinion-poll-2022-02-22/)。作为一名终身的多产读者，书籍禁令的话题对我而言非常重要。禁书是禁思想、禁观念和禁想象的第一步；这些禁令违反了美国引以为傲的民主自由表达价值观。在这篇文章中，我希望分享一些关于最近被禁和目前在美国各地学区被禁的书籍、作者和主题的见解。

![](img/a4c4441f17743b47f045fd7deea0e83c.png)

由 PEN 创建的按州被禁书籍。图源于他们的文章，[《在美国被禁》](https://pen.org/report/banned-usa-growing-movement-to-censor-books-in-schools/)。

# 数据：2021-2022 年被禁图书数据集

## 被禁图书数据集

[PEN America 的学校图书禁令索引数据集](https://docs.google.com/spreadsheets/d/1hTs_PB7KuTMBtNMESFEGuK-0abzhNxVv4tgpI5-iKe8/edit#gid=1263265416) 包含了在 **2021 年 7 月 1 日至 2022 年 6 月 30 日** 期间，在美国各地的学校图书馆和教室被禁的书籍。该数据集由 [PEN America](https://pen.org/about-us/) 编制，PEN America 是一个致力于通过推动文学和人权捍卫和庆祝自由表达的非营利组织。该数据集在获得 PEN 许可的情况下使用。

PEN America 在他们的文章中详细描述了数据集，[《在美国被禁：对学校图书审查运动的增长》](https://pen.org/report/banned-usa-growing-movement-to-censor-books-in-schools/)，其中还包括对最近被禁图书的全面概述以及被禁书籍索引的收集和整理方式。有关数据集的更多细节请见本文末尾。

**被禁图书数据集包含了 1146 位独特作者和 1649 本独特书籍。**

## 使用 Google Books 进行数据增强

PEN 数据集包括了书名、作者、以及书籍被禁的州和学区等信息。我使用了[Google Books API](https://developers.google.com/books)来为每本书增加额外的元数据，如书籍描述、成熟度评级、页数、出版年份和类别标签，如体裁和推荐年龄层。

## 数据说明

被禁书籍数据集并未完全捕捉到 2021-2022 年美国所有被禁书籍，因为有关书籍禁令的详细报告并不总是可用或易于追踪。然而，它是最近书籍禁令的一个不错的近似值。此外，虽然 Google Books 在查找书籍并获取其元数据方面表现良好，但它也并不完美，有时会返回不正确的元数据（例如，返回的是新版本的出版日期而非原版书籍的出版日期）。由于我没有时间逐一检查所有 1649 本书的结果是否正确，因此本文的分析中包含了一些错误。

## GPT-3 实验

大型语言模型如 GPT-3（及其变体 ChatGPT 和 Bing Chat）最近引起了公众的关注。这些模型虽然强大且令人印象深刻，但仍然在很大程度上是“[黑箱](https://hdsr.mitpress.mit.edu/pub/f9kuryi8/release/8)”——我们对其行为的理解仍然很有限。鉴于这些模型越来越多地被整合到我们在线生活的各个方面，如[心理健康聊天机器人](https://www.nbcnews.com/tech/internet/chatgpt-ai-experiment-mental-health-tech-app-koko-rcna65110)、[社交媒体](https://www.theverge.com/2023/2/27/23614959/snapchat-my-ai-chatbot-chatgpt-openai-plus-subscription)、[Quora](https://quorablog.quora.com/Poe-1)、[数字学习工具](https://quizlet.com/labs/qchat)、[搜索](https://blogs.microsoft.com/blog/2023/02/07/reinventing-search-with-a-new-ai-powered-microsoft-bing-and-edge-your-copilot-for-the-web/)甚至是新的[Windows 11 更新](https://www.computerworld.com/article/3688953/new-windows-11-update-puts-bing-chat-front-and-center.html)，这些模型控制（或至少影响）了我们越来越多的人类决策过程，了解它们在不同情况下的响应是非常重要的。例如，未来某种形式的 AI（也许是 ChatGPT 的变体）决定我们在个别学生课程计划、课堂大纲和图书馆书架上看到的书籍和主题，这并不是一个不切实际的想法。

为了更好地理解这些大型语言模型，我询问了 GPT-3 是否会根据书名和描述禁止最常被禁的书籍。我使用了 OpenAI 的 GPT-3.5（`davinci-003`）API 来进行每一个查询。我运行了每个查询 5 次以考虑随机性（就像我在其他所有文章中做的那样）。我还在[微软的 Bing 聊天机器人](https://blogs.microsoft.com/blog/2023/02/22/the-new-bing-preview-experience-arrives-on-bing-and-edge-mobile-apps-introducing-bing-now-in-skype/)（基于更强大的 GPT-3 版本）上运行了每个查询 1 次，因为它没有 API。

# 认识这些书籍

各州在所有学区中禁用了多少本独特的书籍？[德克萨斯州](https://www.texastribune.org/2022/09/19/texas-book-bans/)位居首位——确实，德克萨斯州的一切都更大，包括不允许进入其课堂和图书馆书架的书籍数量。[宾夕法尼亚州](https://www.penncapital-star.com/civil-rights-social-justice/meet-moms-for-liberty-the-group-leading-lgbtq-book-bans-in-pa-schools/)、[佛罗里达州](https://www.buzzfeednews.com/article/juliareinstein/florida-school-book-bans-teachers-confusion)和[田纳西州](https://tn.chalkbeat.org/2022/4/28/23047535/book-ban-tennessee-textbook-commission-legislation-age-appropriate)在过去一年中也禁用了数百本书籍。该数据集是在犹他州及其他可能的州近期书籍禁令之前创建的，因此这些数据可能不反映最新的书籍禁令状态。

![](img/40fb980847f2bd814b460d66951c2e7b.png)

每个州被禁书籍的独特数量。图形由作者制作。

以下图表显示了按每本书在唯一学区中的禁令数量排序的被禁书籍。我在文章末尾包括了这些书籍的一些描述。*Gender Queer*，在所有州和学区中被禁的最多的书籍，是一本关于非二元性别的回忆录。

![](img/06555725d41df5cf9181649797c7d0dd.png)

按每本书在唯一学区中的禁令数量排序的被禁书籍。图形由作者制作。

## 更多的儿童书籍和虚构书籍被禁

大多数被禁书籍是为年轻读者编写的：47%为青少年，30%为年轻成人。将近 70%的书籍是虚构作品。

按虚构、非虚构、青少年和成人类别列出了前五本被禁书籍，这些信息在本文末尾。

![](img/40e9edaa4df9b9c65f3a943fbe32c001.png)

按年龄组和类型的被禁书籍比例。图形由作者制作。

## 2000 年后出版的书籍被禁的情况更多

95%的被禁书籍是在 2000 年后出版的。

![](img/3c6c6ae1d559eb018e1613babf0f9846.png)

直方图显示了书籍出版年份的分布。图形由作者制作。

较老的书籍被禁的情况较少。出版于 2000 年之前的被禁书籍包括：托尼·莫里森的《最蓝的眼睛》和《宠儿》，阿特·斯比格曼的《鼠族》，库尔特·冯内古特的《屠场五号》，这些书籍我都读过并非常喜欢。（事实上，托尼·莫里森是我最喜欢的作家之一。）

## 较短的书籍被禁

书籍的页数各不相同，平均为 223 页，中位数为 241 页。相比之下，普通小说的页数约为 250-350 页。近四分之一的书籍（26%）少于 100 页——很可能是儿童书籍或早期读物。

![](img/815b62be64432c9a6f7f7096992e1f84.png)

直方图显示了被禁书籍的页数分布。图形由作者制作。

最短的被禁书籍为 16 页长：《Cow on the Town: Practicing the Ow Sound》，作者为**伊莎贝拉·加西亚**。关于牛的主题，38 页长的《14 Cows for America》，作者为**卡门·阿格拉·迪迪**，也被禁了。[也许了解牛会冒犯**罗恩·德桑蒂斯**](https://www.mahablog.com/2023/02/07/desantis-protects-school-children-from-cow-lit/)。

# 认识作者

以下图表显示了与前 20 名被禁书籍（在所有学区中）的前 20 名作者对应的图表。在图中，我用黄色突出显示所有认同为白人男性的作者。蓝色条形代表认同为女性或非二元性别和/或是有色人种的作者。

![](img/c767a5d234b8b0d5b344698c54b65033.png)

每位作者在各个学区被禁的数量。白人男性作者用黄色标记。图表由作者制作。

这为什么重要？图书行业由[白人主导](https://www.nytimes.com/interactive/2020/12/11/opinion/culture/diversity-publishing-industry.html)（2018 年，11%的书籍由有色人种创作）和[历史上由男性主导](https://press.princeton.edu/ideas/women-in-the-gentlemans-career-of-publishing)（想想所有[女性作者](https://jocolibrary.bibliocommons.com/list/share/1216390437/1717126439)曾不得不[以男性笔名写作](https://artsandculture.google.com/story/women-who-wrote-under-male-pen-names/tgUBfDz_O3goIw?hl=en)）。通过不成比例地禁止由传统上被低估的作者（如女性、非二元性别者和有色人种）创作的书籍，他们的故事可能不会在图书馆和教室书架上得到展现。

# 被禁书籍的共同话题是什么？

我采集了所有独特书籍的描述，以确定被禁书籍的主要类别。我从 PEN 文章（以及在进行探索性数据分析时扫描标题）中了解到，许多被禁书籍围绕性别、跨性别身份和 LGBTQ+故事展开。

我使用了[主题建模](https://en.wikipedia.org/wiki/Topic_model)，这是一种机器学习技术，通过聚类词汇统计确定书籍描述中的主题。我使用了[BERTopic](https://maartengr.github.io/BERTopic/index.html)，这是一个基于大语言模型 BERT 生成的嵌入进行主题建模的优秀 Python 库。

![](img/d71c912b334338d81eec722148e5b3a9.png)

被禁书籍描述中的热门话题。图表由作者制作。

这些是从书籍描述中出现的一些主要话题。虽然性别、性取向和变性的话题较为普遍，但也有关于种族主义和黑人历史的书籍被禁（与最近关于[在公立学校禁用批判性种族理论的运动](https://www.brookings.edu/blog/fixgov/2021/07/02/why-are-states-banning-critical-race-theory/)一致），还有关于堕胎和女性在科学与数学领域的书籍。每个话题中的书籍示例：

**女性/科学话题**

+   《不准女孩入内：勇敢女性伪装成男人追求爱情、自由与冒险的故事》（Hughes）

+   《25 位最先想到这些的女性》（Sherman）

+   《艾达·拜伦·洛夫莱斯与思考机器》（Wallmark）

**堕胎/最高法院话题**

+   《堕胎辩论》（Farrell）

+   《罗诉韦德：使堕胎合法的里程碑最高法院判决的未述故事》（Faux）。

**穆斯林/头巾话题**

+   《在我头巾下》（Khan）

+   《骄傲：活出我的美国梦》（Muhammad）

# GPT-3 在给出描述时对禁书更为保守

基于以上数据分析，我想看看 GPT-3 对于最被禁的（因此更具争议的，由社会确定）书籍会有什么反应。我从数据集中选取了五本最被禁的书籍的子集（按禁用该书籍的学区数量排名）。然后，我问 GPT-3 是否会以两种不同的方式禁用一本书：

+   基于书名和作者：*你会禁止{年龄}岁孩子的课堂上使用{作者}的{书籍}吗？*

+   基于书籍描述（通过 Google Books 获得）。描述*没有*包含作者姓名或书名：*根据以下书籍描述，你会禁止这本书在{年龄}岁孩子的课堂上使用吗？{描述}*

例如，“《最蓝的眼睛》”的描述如下：“一个 11 岁的黑人女孩在一个偏爱金发碧眼孩子的美国中祈祷她的眼睛变蓝：这样她会更美丽，人们会注视她，她的世界会有所不同。”

## GPT-3 通常反对书籍禁用

在实验中选择的五本书中，GPT-3 无论在任何年龄组的课堂上，还是在使用书名或书籍描述时，都会强烈反对禁用这些书籍。

![](img/29dcfab592f9aa005e78c928944e8456.png)

GPT-3 对于是否会根据书名和作者/描述禁用某个书籍的不同年龄组的反应（是或否）。图表由作者创建。

唯一的例外是“性别酷儿”的描述，仅针对某些年龄组。这将在下一部分探讨。

我促使 GPT-3 也提供对其决定的解释。我在其解释中注意到，虽然 GPT-3 *不建议禁用* 某些书籍以适应较年轻的读者，但它也 *不一定推荐* 这些书籍：“这本书由于内容成熟，不适合 3 岁儿童的课堂，但我不建议禁止它。”

> GPT-3: (不禁止) “由于书中涉及的成熟主题，这本书不适合 3 岁的课堂，但我不建议禁止它。提供多样化的书籍以代表各种经历是重要的，但这本书更适合年长的读者。”
> 
> GPT-3: (不禁止) “这本书超出了 7 岁孩子的典型阅读水平，包含的内容可能不适合这个年龄段的孩子。然而，禁止这本书并不合适，因为它为边缘声音提供了重要的代表性。相反，老师应该考虑这本书是否可以在课堂上用于以适合学生发展的成熟方式讨论重要主题。”

GPT-3 的回答*仅*推荐禁止《性别怪客》，*仅*根据其描述，*仅*针对年幼的年龄段。GPT-3 禁止这本书的理由是描述中的成熟主题不适合年幼的读者，这些主题最好在有可信赖的成年人指导下以适合年龄的方式讨论。

> GPT-3: “鉴于描述中提到的成熟主题，例如青少年迷恋、公开性取向和创伤，这本书不适合 7 岁的课堂，因为对这个年龄段来说太成熟了。”
> 
> GPT-3: “这本书包含不适合 9 岁孩子的成熟内容和语言。书中讨论的主题，如迷恋、性取向、公开性取向和创伤性医疗经历，应该在有可信赖的成年人指导下以适合年龄的方式讨论。”

## GPT-3 更可能不推荐而不是直接禁止书籍

由于 GPT-3 几乎总是建议不禁止书籍，我想知道它会如何推荐书籍。我在改变措辞后重复了上述实验。不是问 GPT-3 是否会直接禁止一本书，而是问它是否会*推荐*一本书（再次，根据书名/作者以及描述）给不同年龄段的读者。结果差异很大。

![](img/85f86e1abb917c5e6a9563c8aa8743d8.png)

GPT-3 对于是否推荐一本书给不同年龄段的回答（是或否），基于书名和作者/描述。图表由作者制作。

对于大多数书籍，GPT-3 的生成结果表明，它会推荐给年长的读者，而不推荐给年幼的读者，依据是成熟度水平。它的推理与我询问是否会禁止时非常相似。通常，它会推荐 11-15 岁的人阅读书籍。

给定书名和作者以及描述，GPT-3 的回答类似——*除了*“《最蓝的眼睛》”。GPT-3 的回答表明，无论是针对年轻读者还是年长读者，它都不会推荐“《最蓝的眼睛》”。

查看 GPT-3 对其回答的解释提供了更多关于其回答方式的见解，但并没有明确说明。一般来说，GPT-3 似乎有些困惑，它提到《蓝眼睛》的描述可能“对 21 岁年轻人的课堂来说过于成熟和强烈”，同时也表示这本书“聚焦于一个 11 岁的孩子，可能对这个年龄段来说过于年轻”。

> GPT-3: 这本书可能对 21 岁年轻人来说过于成熟和强烈。种族主义和暴力的主题可能对他们来说难以处理和理解。
> 
> GPT-3: 这本书可能涉及敏感话题，如种族主义和自尊，这些对 21 岁年轻人来说可能过于成熟。此外，故事可能对这个班级不感兴趣，因为它集中于一个 11 岁的孩子，这对这个年龄段来说可能过于年轻。
> 
> GPT-3: 这本书可能是一个有意义且感人的故事，但似乎不适合 21 岁年轻人的课堂。该书似乎更适合比这个年龄段更年轻的观众。

## 微软 Bing Chat 和 ChatGPT 在被禁书推荐方面的反应类似。

我问了微软 Bing Chat 和 ChatGPT（GPT-3 的两个变体）相同的问题，这些问题在前面的部分中已经由 GPT-3 回答。考虑到这些大型语言模型都是彼此的变体，两者的回答与 GPT-3 类似也就不足为奇了。输出结果有一些细微的差异——例如，Bing Chat 对禁令或不推荐书籍的理由比其他语言模型更具细致性。有趣的是，所有这些大型语言模型在不推荐某些书籍给特定年龄段的同时，并未推荐直接禁止书籍。

文章末尾提供了完整的输出。

**注意：** 我尚未有机会在新[ChatGPT API](https://openai.com/blog/introducing-chatgpt-and-whisper-apis)上重复这些实验，该 API 在本文撰写后发布。

# 结论

美国有着[长期的图书禁令历史](https://www.nationalgeographic.com/culture/article/history-of-book-bans-in-the-united-states)，从清教徒对任何带有性感暗示的书籍的禁令，到内战后对种族主义和奴隶制的禁令。那些挑战“关于美国历史或社会规范的长期叙事”的书籍常常成为禁令的目标，包括华尔特·惠特曼的《草叶集》和哈丽雅特·比彻·斯托的《汤姆叔叔的小屋》等经典美国文学作品。

文章中的分析显示，目前在美国有各种各样的故事被禁：包括 LGBTQ+ 社区、穆斯林家庭、科学中的女性、种族主义、性别歧视，甚至是关于牛的图画书。孩子们成长过程中读的书籍（以及成人继续阅读的书籍）对他们的信念体系和对世界的理解有着巨大的影响。

重要的是要记住，“你会推荐一本书吗？”和“你会禁一本书吗？”之间存在差距。一个短语突出了不同年龄段的书籍，而另一个则完全不允许它们存在。这一差异在 GPT-3 的回应中也很明显：尽管它会或不会向不同年龄段推荐某些书籍，但它拒绝（在大多数情况下）推荐 *禁* 书籍，无论哪个年龄层。也许，这是一种我们可以采用的类似方法——为不同年龄段推荐书籍，而不是完全从书架上移除它们。

![](img/5a2110e4db90004c23428f764c87e0d7.png)

这是一个聚会吗？这是焚书吗？这是一个仪式吗？那些是书吗？它们在燃烧吗？那些人吗？他们在跳舞吗？他们在惊慌吗？他们在庆祝吗？使用 Stable Diffusion 生成

*本文转载自我的博客原文:* [*🎨 art fish intelligence 🐡*](https://blog.yenniejun.com/p/the-forbidden-pages-a-data-analysis) *… 如果你喜欢这篇文章，欢迎订阅我的博客以阅读更多类似的内容！*

# 附录

## 关于 PEN 的被禁书单

PEN America [定义](https://pen.org/banned-in-the-usa/#what) 书籍禁令为“基于书籍内容并因家长或社区挑战、行政决定或应立法者或其他政府官员直接或威胁性行动的结果而采取的任何措施，导致原本可用的书籍被完全移除，或书籍的访问被限制或减少。”

如他们在 [网站](https://pen.org/banned-in-the-usa/#what) 上提到的，学校书籍禁令索引并不是所有被禁书籍的全面清单，因为“有关书籍禁令的详细报告有时不可用”，而“书籍的安静移除或限制”并不总是可追踪的。这个列表旨在统计根据上述定义被禁的书籍数量。

在 [《美国的禁书：日益增长的学校书籍审查运动》](https://pen.org/report/banned-usa-growing-movement-to-censor-books-in-schools/) 中，PEN 的作者描述了数据集和书籍禁令，他们进行了基本分析，展示了每个州被禁书籍的数量以及禁令内容的主题（在本文中部分重复）。他们的主要发现之一如下（将在本文后续讨论）：

> *“在 2021-22 学年，从最初在学校层面的活动挑战和移除书籍，发展成为由地方、州和国家团体推动的全面社会和政治运动。这些团体针对的绝大多数书籍包含 LGBTQ+角色或有色人种角色，和/或涉及美国历史中的种族与种族主义、LGBTQ+身份或性教育。”*

事实上，[PEN 的代表指出](https://www.news-press.com/story/news/education/2022/09/26/school-book-bans-florida-2nd-highest-nation-pen-america-report-banned/10428021002/)被选择禁止的书籍通常不是由于儿童或家长的个人投诉，而是由于对“哪些书籍有 LGBTQ 内容……即使只是一本有同性恋跨种族情侣插图的书”的 Google 搜索结果。

## 最受禁忌书籍的简短描述

+   《性别怪异》：“一本自传……包括青少年暗恋的羞辱与困惑，挣扎于如何向家庭和社会出柜，与朋友们通过色情同性恋同人小说建立联系，以及面对创伤和基础性侵犯的宫颈抹片检查”

+   《所有男孩都不是蓝色》：“从五岁时被恶霸踢掉牙齿的记忆，到与慈爱的祖母一起去跳蚤市场，再到他的第一次性关系，这本年轻成人的回忆录编织了黑人同性恋男孩所面临的考验和胜利”

+   《黑暗中的光》：“这部小说讲述了 1930 年代德克萨斯州新伦敦，一名墨西哥裔美国少女与一名非裔美国少年之间的爱情故事，一直到 1937 年的新伦敦学校爆炸事件。”

+   《最蓝的眼睛》：“一个 11 岁的黑人女孩在一个钟爱金发碧眼儿童的美国中祈祷她的眼睛能变成蓝色：这样她就会变得美丽，人们会注视她，她的世界会有所不同。”

+   《草坪男孩》：“一本半自传式的成长小说，讲述了一个年轻的墨西哥裔美国人自童年起经历困境，现在正经历自我发现的阶段。”

## 最受禁忌的五本小说、非小说、青少年和成人书籍

```py
Top Banned Fiction Books
------------------------------
Gender Queer: A Memoir (Maia Kobabe)
Out of Darkness (Ashley Hope Pérez)
The Bluest Eye (Toni Morrison)
Lawn Boy (Jonathan Evison)
The Hate U Give (Angie Thomas)Top Banned Nonfiction Books
------------------------------
All Boys Aren't Blue (George M. Johnson)
Beyond Magenta: Transgender Teens Speak Out (Susan Kuklin)
This Book Is Gay (Juno Dawson)
Beloved (Toni Morrison)
Fun Home: A Family Tragicomic (Alison Bechdel)Top Banned Juvenile Books
------------------------------
Out of Darkness (Ashley Hope Pérez)
Me and Earl and the Dying Girl (Jesse Andrews)
Crank (Crank Series) (Ellen Hopkins)
This One Summer (Mariko Tamaki)
Drama: A Graphic Novel (Raina Telgemeier) Top Banned Adult Books
------------------------------
Gender Queer: A Memoir (Maia Kobabe)
The Bluest Eye (Toni Morrison)
Lawn Boy (Jonathan Evison)
The Kite Runner (Khaled Hosseini)
Beloved (Toni Morrison)
```

## 询问 ChatGPT 和 Bing Chat 是否会向不同年龄段推荐被禁止的书籍

我并不是在编造，当我说*ChatGPT 和 Bing Chat**对每个问题的回应完全相同*时。好吧，几乎如此。他们对我问题的“是/否”部分的回答完全一致。

![](img/b762d599a7b5b8faf8722f22241cc04b.png)

ChatGPT 和 Bing 在是否会因不同年龄段对一本书的描述而禁止该书的回应（是或否）。图表由作者制作。

与 GPT-3 相比，ChatGPT/Bing Chat 更倾向于向 11-13 岁的青少年推荐书籍（相较于 13-15 岁的青少年）。然而，我认为更有趣的是查看每个聊天机器人对是否会向特定年龄段推荐书籍的解释。

我对 ChatGPT 和 Bing Chat 使用了相同的提示。我使用了《最蓝的眼睛》的描述来展示这个例子。

> 以下是一本书的描述。制作一个 Markdown 表格，一列用于 3、5、7、9、11、13、15、17、19、21 岁。写出你是否会推荐这本书（是）或不推荐（否），用于填满对应年龄的课堂。再写一列简短的理由说明为什么或为什么不推荐。描述：{Description}

![](img/d227c0fc583baafa113e35a708474195.png)

ChatGPT 对于《最蓝的眼睛》一书的推荐输出

ChatGPT 的理由有些过于简单。它重复说“这本书的种族主义和自我厌恶的主题可能对多个年龄段来说过于成熟”，突然改变主意，认为适合 11 岁的孩子。

另一方面，Bing Chat 提供了非常深入的理由（实际上我对此感到相当惊讶！）。至少在这一点上，Bing Chat 似乎更能意识到用户请求的细微差别。然而，请对此持保留态度，因为我没有严格测试这个推测，这只是一个单一的观察。

![](img/d6cc0023183c7ea90d32d83967131fea.png)

Bing Chat 对于《最蓝的眼睛》一书的推荐输出

再次说明——这篇文章及其分析是在 ChatGPT API 发布之前进行的！未来使用 ChatGPT 进行的实验将很有趣，看看它们与此处分享的结果有何不同 :)

*这篇文章转载自我博客上的原文：* [*🎨 art fish intelligence 🐡*](https://blog.yenniejun.com/p/the-forbidden-pages-a-data-analysis)*。请给予原博客一些关注和支持！*
