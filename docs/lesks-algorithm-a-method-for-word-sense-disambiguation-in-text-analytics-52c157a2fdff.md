# Lesk 算法：一种用于文本分析中的词义消歧方法

> 原文：[https://towardsdatascience.com/lesks-algorithm-a-method-for-word-sense-disambiguation-in-text-analytics-52c157a2fdff?source=collection_archive---------4-----------------------#2023-03-24](https://towardsdatascience.com/lesks-algorithm-a-method-for-word-sense-disambiguation-in-text-analytics-52c157a2fdff?source=collection_archive---------4-----------------------#2023-03-24)

[![Duncan W.](../Images/25c776723a90149efd5f090c67ba078a.png)](https://duncan-w.medium.com/?source=post_page-----52c157a2fdff--------------------------------) [![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----52c157a2fdff--------------------------------) [Duncan W.](https://duncan-w.medium.com/?source=post_page-----52c157a2fdff--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6de39228b050&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flesks-algorithm-a-method-for-word-sense-disambiguation-in-text-analytics-52c157a2fdff&user=Duncan+W.&userId=6de39228b050&source=post_page-6de39228b050----52c157a2fdff---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----52c157a2fdff--------------------------------) · 9 分钟阅读 · 2023年3月24日 [投票](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F52c157a2fdff&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flesks-algorithm-a-method-for-word-sense-disambiguation-in-text-analytics-52c157a2fdff&user=Duncan+W.&userId=6de39228b050&source=-----52c157a2fdff---------------------clap_footer-----------)

--

![](../Images/fa65b7a30182111f54b8dfa0b8ab9a0f.png) 

照片由 [Amador Loureiro](https://unsplash.com/@amadorloureiro?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在英语中，超过38%的词汇是**多义的**，这个术语指的是一个词可能有多个定义或“意义”（Edmonds, 2006）。例如，单词“set”——它可以用作名词、动词和形容词——具有数十种独特的定义，使其成为英语词典中记录的最多义的词之一。因此，如果我们要求某人“请把餐具套放在桌子上”，我们如何本能地知道“set”这个词的两个用法之间的区别？答案全在于上下文。人脑的神经网络执行语义处理、存储和检索，使我们在**词义消歧（WSD）**方面具有天然的天赋。这意味着我们能够在给定的上下文中根据最合理的意思来确定一个多义词的含义。因此，自然语言也随之发展，反映出这种进行复杂上下文关联的天赋能力。

# NLP中的词义消歧

尽管我们的大脑在这些游戏中能够取得很大成功，但具有多个意义的词在**自然语言处理（NLP）**中带来了显著的问题，NLP是人工智能的一个分支，研究计算机解释和“理解”人类语言的能力。虽然一个词的意义依赖于其上下文，但计算机很难捕捉到词的上下文，因为它被隐喻、修饰语、句子否定以及其他无数的语言复杂性所复杂化，这使得机器难以学习。

由于词义消歧在NLP的实际应用中具有重要价值，近年来出现了几种词义消歧的方法。目前的机器学习方法包括监督学习方法，通过使用一组手动标记的词汇来训练分类算法。然而，这些训练集可能成本高昂、获取时间长且不完美，因为即使是人类标注者也常常只有70-95%的时间达成一致（Edmonds, 2006）。鉴于所需的手动努力，未监督学习方法也被使用，其中许多方法旨在基于某种相似上下文的度量来聚类词汇。

# **Lesk算法：一种简单的词义消歧方法**

也许最早且至今仍最常用的词义消歧方法之一是 **Lesk 算法**，由 Michael E. Lesk 于 1986 年提出。Lesk 算法基于这样的理念：在文本中一起出现的词是某种程度上相关的，而这些词的关系和相应的上下文可以通过感兴趣的词的定义以及周围使用的其他词的定义提取。Lesk 算法在现代技术出现之前就已经发展，旨在通过找到在词典定义中重叠最多的一对匹配词典“意义”（即同义词）来消歧感兴趣词的含义——通常出现在短语或句子中。

在 Lesk 使用的示例中，他引用了“pine”和“cone”这两个词，并指出这些词从《牛津英语词典》中返回以下定义：

![](../Images/58ef1f6a46ae1584f75086e10e0ab903.png)

Lesk 在 1986 年提出的语义匹配示例。

简而言之，Lesk 算法计算了一个感兴趣的词与其上下文窗口中所有词的词典定义之间的重叠次数。然后，它选择与重叠次数最多的词对应的定义，排除停用词（如“the”、“a”、“and”），并将其推断为该词的“意义”。如果我们考虑“pine”作为感兴趣的词，而“cone”是其上下文窗口中的唯一词，比较“pine”和“cone”的词典定义会发现“常绿”是这两个词最常见的“意义”。因此，我们可以合理推断感兴趣的词“pine”指的是一种常绿树，而不是它的另一种定义。

# **优点和缺点**

Lesk 算法有许多优点，其中最主要的是其简单性使其易于实现，适用于各种不同的上下文，因此易于推广。Lesk 指出，该算法不依赖于全局信息，这意味着因为相同的词在文本中可能被多次引用但每次含义不同，词义仅由其上下文窗口中的直接支持词汇集合推导，而不是整个文本。除此之外，Lesk 算法是非句法性的，这意味着该方法不依赖于词语的排列或句子的结构，因为所有的关联仅通过词典定义进行。这使得该算法与基于句法的文本分析解决方案配合使用时具有很好的效果。例如，一个基于词性标注的文本标记工具可能能够识别“mole”作为名词的用法，但不能准确区分动物（mole）、皮肤肿块（mole）和计量单位（mole），因为这三者都是名词。在这种情况下，Lesk 算法的优势得以显现。

尽管简单且功能强大，Lesk 原始算法的最大缺点是性能——Lesk 提出的准确率仅为约 50-70%，并且在与带有语义标记的文本进行实验验证时表现得更低（Viveros-Jiménez, 2013）。该算法还显著存在低召回率的问题，即由于词典定义之间完全没有重叠或多个定义有相同数量的重叠，无法为许多单词提供对应的上下文定义。此外，Lesk 留下了一些未解答的问题，包括最适用的词典是什么，所有匹配的术语是否应被平等对待或按词典定义的长度加权，以及单词的上下文窗口应有多宽（Lesk 提出约 10 个单词，但建议这个范围是相当灵活的）。

# **前进：Lesk 的最新进展**

尽管 Lesk 的方法中留有一些问题和不足，但已经进行了重要的努力来在他原始工作的基础上进行扩展。2002年，研究人员提出了一种将 Lesk 的基于词典的算法适配到词汇数据库 WordNet 的方法（Banerjee & Pedersen, 2002）。与词典中按字母顺序排列单词和定义不同，**WordNet** 是一个在线数据库，按语义排列单词，创建名词、动词、形容词和副词的数据库组。具有同义关系的单词被归入称为“同义词集合”（synset）的关系中，而多义词可以通过其出现在多个同义词集合中被识别，因为每个同义词集合代表一个单词的不同定义或“意义”。WordNet 还考虑了词汇关系，例如下义词（hyponymy，即属于更广泛类别的词，如郁金香是花，花是植物）和转喻（metonymy，即用与某物密切相关的名称来代指该物，如“王冠”指代君主制），因此能够捕捉到比原始 Lesk 算法更广泛的词汇定义和关系。WordNet 的结构允许更有针对性地搜索相关词汇，因为如果已知感兴趣单词的词性，随后搜索的关系和同义词集合将仅限于该词性的范围内。

Lesk 算法的低召回率问题也一直是改进的领域。2013 年，研究人员发表了一篇论文，比较了几次实验，在这些实验中，他们调整了考虑的词的上下文窗口的大小，以期提高算法的性能（Viveros-Jiménez 等人，2013）。提出了一项新建议，即在定义上下文窗口的大小时，只考虑有至少一个重叠的四个词，并且在词义匹配中排除目标词自身。由于词典定义通常会在示例句子中包含感兴趣的词，例如“即松树是一种常绿树”，对于词“pine”，这可能会导致对“pine”的虚假重叠，从而使所有词义中最匹配的词仍为“pine”而不是“evergreen”。研究人员发现，当结合这两种方法时，Lesk 算法的精确度和召回率显著提高，相较于简单选择 4 个词的上下文窗口。

# 在 Python 中应用 Lesk 算法

以下是一个简单的例子，展示了如何实现 Lesk 算法，使用 **pywsd** 包在 Python 中进行词义消歧。以下函数是 Adapted Lesk 算法的实现，该算法由 Banerjee 和 Pederson 于 2002 年描述，使用了 WordNet 实现的 Lesk。

![](../Images/810020a07dcb984252dbd9214b64bd8e.png)

Adapted Lesk 算法在 Python 中的实现示例

我们可以看到，对于词“bank”，Lesk 算法能够推断出在上下文中，一个“bank”的意思是指金融机构，而另一个意思是指一种倾斜的土地。同样，在进行这种关联时，句法结构并不重要，而 Lesk 算法的成功依赖于上下文线索的存在，例如提到河岸时的“river”和“fish”，以及提到金融银行时的“deposit”和“money”。通过这个例子可以看出，Lesk 对句子结构具有鲁棒性，因此可以用于从各种非结构化文本数据类型中提取词义，无论是经典小说的摘录还是 Twitter 评论。然而，由于 Lesk 算法对上下文窗口的大小敏感，这种数据结构限制可能最限制其应用。因此，数据必须被结构化并分词成适当大小的窗口，以提供足够的上下文信息，但仍能捕捉到感兴趣词的真正含义。由于 Lesk 算法可能最适用于短语和具有上下文信息的相关术语组，它可以应用于查询和检索类型的场景，例如自动聊天机器人或搜索框，在这些场景中输入一些上下文信息，进行解释，然后返回响应或结果。

尽管 Lesk 算法及类似的词义消歧方法直观上简单，但它们作为当前开发直观日常工具（需要词义消歧）的基础性垫脚石发挥了重要作用。特别是，这些算法背后的直观理念可以实际应用于开发新的算法，以提升搜索引擎在全球数十亿用户中使用时的信息检索相关性。毕竟，当我们搜索一个模糊的术语时，我们本能地知道需要包含一个“上下文窗口”，以增加准确命中的可能性。

那么为什么呢？因为上下文就是一切。

*感谢阅读！如果你想分享任何想法，请随时在 LinkedIn 上联系我*！

[https://www.linkedin.com/in/duncan-w/](https://www.linkedin.com/in/duncan-w/)

# **资源**

Banerjee, S. & Pedersen, T. (2002). 使用 WordNet 的适应型 Lesk 算法进行词义消歧。计算语言学与智能文本处理。2276\. 136–145\. 10.1007/3−540−45715−111.

Edmonds, P. (2006). 消歧义，词汇的。语言与语言学百科全书（第二版），Elsevier，第 607–623 页。

Lesk, M. (1986). 使用机器可读词典的自动词义消歧。第 5 届年度国际系统文档会议论文集 — SIGDOC

Liling Tan. 2014\. Pywsd: Python 实现的词义消歧（WSD）

技术 [software]。取自 [https : //github.com/alvations/pywsd](https : //github.com/alvations/pywsd)

SAP Conversational AI. (2015年11月20日)。从上下文到用户理解。取自 [https : //medium.com/@SAPCAI/from−context−to−user−understanding−a692b11d95aa](https : //medium.com/@SAPCAI/from−context−to−user−understanding−a692b11d95aa)

Viveros-Jiménez F., Gelbukh A., Sidorov G. (2013) 对简化的 Lesk 算法在词义消歧中的简单窗口选择策略。载：Castro F., Gelbukh A., González M. (编) 人工智能及其应用的进展。MICAI 2013\. 计算机科学讲义，第 8265 卷\. Springer, Berlin, Heidelberg. [https : //doi.org/10.1007/978−3−642−45114−017](https : //doi.org/10.1007/978−3−642−45114−017)
