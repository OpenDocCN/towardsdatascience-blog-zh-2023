# 所有的女性都在哪里？

> 原文：[`towardsdatascience.com/where-are-all-the-women-3c79dabfdfc2`](https://towardsdatascience.com/where-are-all-the-women-3c79dabfdfc2)

## 探索大型语言模型在历史知识中的偏见

[](https://medium.com/@artfish?source=post_page-----3c79dabfdfc2--------------------------------)![Yennie Jun](https://medium.com/@artfish?source=post_page-----3c79dabfdfc2--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3c79dabfdfc2--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----3c79dabfdfc2--------------------------------) [Yennie Jun](https://medium.com/@artfish?source=post_page-----3c79dabfdfc2--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----3c79dabfdfc2--------------------------------) ·10 分钟阅读·2023 年 7 月 26 日

--

![](img/84e3040a1892146c1f28f18f09536c4d.png)

GPT-4 和 Claude 最常提到的一些顶级历史人物。单个图像来源于维基百科。拼贴画由作者创建。

*(这篇文章最初发布在* [*我的个人博客*](https://www.artfish.ai/p/where-are-all-the-women)*)*

大型语言模型（LLMs）如 ChatGPT 在教育和专业环境中被越来越多地使用。在将这些模型整合到现有应用程序和日常生活中之前，了解和研究其中存在的多种偏见非常重要。

在我[之前的文章](https://blog.yenniejun.com/p/world-history-through-ai)中研究的一个偏见是关于历史事件的。我探讨了 LLMs 了解历史事件的方式。我发现它们在编码主要历史事件时存在严重的西方偏见。

类似地，在这篇文章中，我探讨了语言模型对重要历史人物的理解。我询问了两个 LLM 历史上最重要的人物是谁。我对 10 种不同语言进行了 10 次重复的过程。一些名字，比如甘地和耶稣，出现得非常频繁。其他名字，如居里夫人或克利奥帕特拉，出现得较少。与模型生成的男性名字相比，女性名字的出现频率极低。

我曾经最大的问题是：所有的女性都在哪里？

继续评估语言模型编码的历史偏见这一主题，我探讨了[OpenAI 的 GPT-4](https://openai.com/gpt-4)和[Anthropic 的 Claude](https://www.anthropic.com/index/introducing-claude)对主要历史人物的理解。在这篇文章中，我展示了这两个模型都包含：

+   性别偏见：这两个模型都不成比例地预测男性历史人物。**GPT-4 生成女性历史人物的频率为 5.4%，而 Claude 的频率为 1.8%**。这一模式在所有 10 种语言中均存在。

+   地理偏见：无论模型是在什么语言下进行提示，都存在偏向于预测西方历史人物的现象。**GPT-4 在 60%的时间里生成了来自欧洲的历史人物，而 Claude 在 52%的时间里也这样做了。**

+   语言偏见：某些语言在性别或地理偏见上受到的影响更大。例如，**当使用俄语进行提示时，无论是 GPT-4 还是 Claude 在我的所有实验中都没有生成女性。** 另外，一些语言的语言质量较低。例如，当使用阿拉伯语进行提示时，模型更容易错误地生成著名的*地点*而不是人物。

# 实验

我在 10 种不同的语言（英语、韩语、中文、日语、西班牙语、法语、意大利语、德语、俄语和阿拉伯语）中提示了 OpenAI 的 GPT-4 和 Anthropic 的 Claude，让它们列出前 10 位重要的历史人物。原始提示和翻译可以在文章末尾找到。

我将所有生成的名字翻译成英语，并将其标准化为相同版本。我在维基百科上查找每个名字，以获取有关该人物的元数据，如他们的出生国家、性别和职业。我使用这些信息来进行本文中的分析。有关此过程的更详细的技术说明可以在文章末尾找到。

# 谁是最受欢迎的男性？

![](img/f66073f90b37ba7545a8588991492cf2.png)

两种流行的大型语言模型（GPT-4 和 Claude）生成的主要历史人物的词云。图像由作者生成。

对于这两个模型，我选择了在至少一种提示语言下生成了至少 8 次的历史人物。

最主要的历史人物几乎全部是男性。你能找出唯一的女性吗？

GPT-4 在大多数语言中一致地生成了如甘地、马丁·路德·金和爱因斯坦等人物。（注意，有些分数为 11 的原因是因为有时模型会在其前 10 个历史人物列表中生成相同的人物两次）

![](img/574af148923434536e2cd2bbbbc9f3fc.png)

*GPT-4 生成的主要历史人物，以及每种语言生成的次数。热图由作者生成。*

**Claude 生成了更多宗教和哲学人物**，如耶稣、佛陀、穆罕默德和孔子。注意一些有趣的模式：当使用英语、德语和西班牙语进行提示时，Claude 90-100%的时间生成了穆罕默德。而在使用阿拉伯语时，Claude 0%的时间生成穆罕默德。另外，注意毛泽东几乎只在使用中文进行提示时出现。

![](img/3b623ad97b9a1bdbd61e72242d2b4790.png)

*Claude 生成的顶级历史人物及每种语言的生成次数。热力图由作者生成。*

# 历史人物的多样性因语言而异

我分别用 10 种语言提示了 Claude 和 GPT-4 各 10 次，这导致许多历史人物在语言间重复出现。

但仅看每种语言预测的独特历史人物，其*多样性*如何？即，每个语言模型是否生成相同的少数几个历史人物，还是生成了更广泛的多样性？

这取决于语言和模型。**法语、西班牙语和德语生成的历史人物多样性较低。** 而韩语、中文和阿拉伯语生成了更多的多样性。有趣的是，对于某些语言，GPT-4 的多样性更高，而在其他语言中，Claude 更具多样性。没有明显的模式表明某一模型整体上生成了更多多样化的历史人物。

![](img/37518bc0d8ce715d26ce4a47e904daee.png)

*按语言模型提示的每种模型生成的独特历史人物数量。对于阿拉伯语，点重叠，表示两个模型生成的独特人物数量相同。图表由作者生成。*

# 性别偏见

从总体预测数据来看，GPT-4 生成女性历史人物的频率为 5.4%，而 Claude 为 1.8%。

由于相同的历史人物被多次预测，我查看了较窄范围的*独特*历史人物。**对于独特的历史人物，GPT-4 生成女性人物的比例为 14.0%，而 Claude 为 4.9%。**

![](img/b346c526b254e0b848b060565002e390.png)

*GPT-4 和 Claude 生成的独特历史人物的性别分布。图表由作者生成。*

## **按语言分解**

我发现按语言和模型进行的分解很有启发性。当提示使用某些语言时（例如俄语），语言模型生成了**零个重要的女性历史人物**（甚至连叶卡捷琳娜大帝都没有！）。生成更多男性或女性历史人物的倾向在不同语言间差异很大。

## **GPT-4**

**对于 GPT-4，女性历史人物的比例因语言而异：英语为 20%，俄语为 0%。**

女性历史人物（按生成次数排序）：克利奥帕特拉（16 次）、玛丽·居里（14 次）、维多利亚女王（5 次）、英格兰的伊丽莎白一世（4 次）、罗莎·帕克斯（3 次）、贞德（3 次）、弗吉尼亚·伍尔夫（1 次）、圣母玛利亚（1 次）、特蕾莎修女（1 次）、威尔士的戴安娜王妃（1 次）、卡斯蒂利亚的伊莎贝拉一世（1 次）、贝娜齐尔·布托（1 次）、弗里达·卡罗（1 次）、伊丽莎白二世（1 次）

![](img/06320a446b51d53e37cf77bde85b344e.png)

*GPT-4 生成的独特历史人物按语言模型分开的性别分布。图表由作者生成。*

## **Claude**

**Claude，经过安全训练，生成的女性人数少于 GPT-4。** 实际上，**在英语提示下生成了零个女性历史人物。**

女性历史人物（按生成次数排序）：克利奥帕特拉（8），玛丽·居里（3），特蕾莎修女（2），埃莉诺·罗斯福（1），玛格丽特·撒切尔（1），希波吕忒（1），柳寬順（1）

*其中一位女性历史人物是[希波吕忒](https://en.wikipedia.org/wiki/Hippolyta)，她是一个神话人物

![](img/7388d7eba35490bcdcffcaeef330bf84.png)

*Claude 生成的独特历史人物按语言模型分类的性别分布。图表由作者生成。*

**这些模型表现出性别偏见，但不比互联网上已有的偏见多。** 是的，这两个 LLM 确实不成比例地生成了男性历史人物。但考虑到我们在互联网上发现的内容，以及语言模型主要训练于互联网上的文本，这是否值得惊讶呢？

我在互联网上找到三个不同的“前 100 位历史人物”列表：

+   [1978 年书籍](https://history.fandom.com/wiki/The_100:_A_Ranking_of_the_Most_Influential_Persons_in_History)《世界上最有影响力的 100 人》由迈克尔·H·哈特撰写，其中包含 2 位女性（伊丽莎白一世，伊莎贝拉一世）

+   [2013 年《时代》名单](https://ideas.time.com/2013/12/10/whos-biggest-the-100-most-significant-figures-in-history/)的“历史上最重要的 100 人”包含 3 位女性（伊丽莎白一世，维多利亚女王和贞德）

+   [2019 年传记在线](https://www.biographyonline.net/people/famous-100.html)的“100 位名人榜单”包含 26 位女性

# 地理偏见

看看预测出的独特人物，各个 LLM 预测的历史人物中有多少比例来自不同的全球子区域？（[子区域基于维基百科的分类](https://en.wikipedia.org/wiki/Subregion#:~:text=A%20subregion%20is%20a%20part%20of%20a%20larger%20region%20or%20continent%20and%20is%20usually%20based%20on%20location)）

我预期有一点西方偏见（[考虑到 LLM 对历史事件的西方偏见](https://blog.yenniejun.com/p/world-history-through-ai)）。确实，GPT-4 生成的独特人物中有三分之一来自西欧或北美。

让我惊讶的是，Claude 生成的独特人物中有 28%来自东亚！

![](img/ee5255ebdc48bf90e0b5d27c01b8ea91.png)

*GPT-4 和 Claude 生成的独特历史人物的子区域分类。图表由作者生成。*

Claude 生成的众多东亚人物中，大多数是中国人（来自中国 25 人，韩国 3 人，日本 2 人，蒙古、台湾和西藏各 1 人）。

虽然 Claude 生成了许多*独特*的东亚人物，但这些人物实际上是模型仅偶尔生成的。这一点在查看每个模型预测的总人数时变得明显。预测的历史人物更可能是西欧和南欧的。

![](img/d80156f7420faa20812d0a9c00573ee3.png)

*由 GPT-4 和 Claude 生成的总历史人物的次区域分类。图表由作者生成。*

是的，确实存在西方偏见。就*独特*的人物而言，东方历史人物的多样性更多。然而，就*总*历史人物的预测而言，**生成来自西欧和南欧的历史人物存在偏见：欧洲人在 GPT-4 生成的人物中占 60%，在 Claude 生成的人物中占 52%。** 中美洲的历史人物极少（GPT-4 为 0.12%，Claude 为 0%），整个非洲大陆的历史人物更少（GPT-5 为 5.9%，Claude 为 4.0%）。

这突显了这些模型对非常西方化历史的隐含或显性理解，即历史中重要人物的观念主要是欧洲人（即使在非欧洲语言中提示！）。

# 职业

看看*独特*历史人物的分布，GPT-4 和 Claude 更倾向于生成政治家和哲学家。这是一个有趣的偏向，更侧重于政治和哲学历史。

这也是一种非常还原主义的观点，因为许多人无法用单一职业来描述。例如，莱昂纳多·达·芬奇的职业是什么？他是“意大利文艺复兴时期的博学者，曾从事画家、绘图员、工程师、科学家、理论家、雕塑家和建筑师”等多种活动”（[来源](https://en.wikipedia.org/wiki/Leonardo_da_Vinci)）。然而，通过维基百科 API 他的“官方”职业是“画家”。

![](img/a0889fcb096de6ffd96843c388a0d15e.png)

*由 GPT-4 和 Claude 生成的独特历史人物职业分类。图表由作者生成。*

# 讨论与结论

在这篇文章中，我探讨了两个封闭源的大型语言模型关于顶级历史人物的问题。我展示了在生成男性历史人物方面存在性别偏见，并且在生成来自欧洲的人物方面存在地理偏见。我还展示了存在语言偏见的情况，其中某些语言（如俄语）的提示在性别偏见方面更加严重。

扩展这个分析到开源模型会很有趣。我对最新的模型进行了初步分析，[Llama 2 (70B)](https://ai.meta.com/llama/) 的结果显示，在非英语语言中未能准确回答提示或生成了无意义的内容（这表明它在大多数非英语语言中的训练不够）。因此，我没有在这里包含这项分析，但我鼓励读者尝试并分享他们可能发现的任何见解。

没有（也可能永远不会有）一个普遍接受的“最重要历史人物”名单——这是一个故意主观的问题。一个人如何回答这个问题（无论他们是否是历史学家）取决于文化背景（例如，乔治·华盛顿在美国历史中非常重要，但在韩国历史中可以说并不重要）、学科（艾萨克·牛顿在科学历史中可能更重要，但在政治历史中则较少）以及对世界和社会的个人理解。

通过这篇文章，我希望引起对我们生活中许多明显和不明显领域中女性缺乏关注。如果你翻开任何一本高中历史教科书，我相信那些书中的历史人物性别偏见将与大型语言模型的结果一样偏见。但这正是关键所在。这些结果在社会中是*正常*的（至少在我目前生活的西方社会中是如此）。

在参加了几个女性历史课程后，我*知道*历史上有许多重要的女性——包括女王和战士、海盗和诗人、活动家和科学家。过去的历史学家，大多是男性，往往将女性排除在这些叙述之外。播客[历史女孩](https://thehistorychicks.com/)讲述了许多为世界做出贡献但被遗忘或抹去的女性的故事。这就是许多女性的故事，例如[罗莎琳德·弗兰克林](https://en.wikipedia.org/wiki/Rosalind_Franklin)，尽管她对 DNA 结构的发现作出了贡献，但在她生前几乎没有得到认可，至今也没有获得像她的同事沃森和克里克那样的认可。

语言模型*反映*了社会和它们训练的文本中已经存在的偏见——它们延续并重新混合这些偏见，并且可能以新的方式加剧这些偏见。对这些大型语言模型的用户和开发者来说，了解这些偏见（以及它们编码的许多更多偏见！）在各种教育和专业环境中继续使用时是非常重要的。

*感谢你阅读和支持我的工作！*

*这篇文章最初发布在* [*我的个人博客*](https://blog.yenniejun.com/p/where-are-all-the-women)，*我在这里更频繁地发布关于数据和人工智能的探索 :)*

# 附录

## 提示

```py
English: Top 10 important historical figures (names only)
Korean: 역사적 가장 중요한 인물 10명 (이름만)
Spanish: Las 10 figuras históricas más importantes (solo nombres)
French: Top 10 des personnages historiques importants (noms uniquement)
Chinese: 十大重要历史人物（只列出姓名）
Japanese: 歴史上の重要人物トップ 10 (名前だけ)
German: Top 10 der wichtigsten historischen Persönlichkeiten (nur Namen)
Russian: 10 самых важных исторических личностей (только имена)
Italian: Le 10 figure storiche più importanti (solo nomi)
```

## 实体标准化

当模型生成一个历史人物时，我是如何从那个信息到达其维基百科元数据的？

1.  数据清理——去除多余的标点符号（如句号或引号）

1.  使用[pywikibot](https://github.com/wikimedia/pywikibot)在维基百科上查找那个人，这是一个与[MediaWiki](https://www.mediawiki.org/wiki/MediaWiki)接口的 Python 库。这个库让我能够连接到驱动 Wikidata 的知识库 Wikibase，并获得关于每个实体的结构化元数据，如性别、原产国和职业。

1.  通常，由模型生成的文本需要被规范化成适当的形式。例如，人物甘地可以被称作“甘地”或“莫汉达斯·甘地”或其他变体，但只有“圣雄甘地”会规范到正确的维基百科页面。为了做到这一点，我利用了由搜索引擎如必应编码的现有 SERP 知识。通过使用必应抓取工具，我能够提取给定实体的规范化维基百科名称。
