# 语义网发生了什么？

> 原文：[`towardsdatascience.com/what-happened-to-the-semantic-web-cbaaf547a09f`](https://towardsdatascience.com/what-happened-to-the-semantic-web-cbaaf547a09f)

## 结果并没有如预期那样发展。

[](https://rafebrena.medium.com/?source=post_page-----cbaaf547a09f--------------------------------)![Rafe Brena, Ph.D.](https://rafebrena.medium.com/?source=post_page-----cbaaf547a09f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----cbaaf547a09f--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----cbaaf547a09f--------------------------------) [Rafe Brena, Ph.D.](https://rafebrena.medium.com/?source=post_page-----cbaaf547a09f--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----cbaaf547a09f--------------------------------) ·7 分钟阅读·2023 年 8 月 3 日

--

![](img/63d2901780c006da477b9820dcf7735c.png)

塞尔·蒂姆·伯纳斯-李设想的由稳定扩散生成的网络

还记得语义网吗？可能不记得了。大约二十年前，它曾风靡一时。但如今，关于它的提及已经很少了。

在开始时怀揣对新网络的大胆愿景（见下文），语义网逐渐失去了动力，逐渐被边缘化。然而，关于为什么会这样，从未有过“清算”——直到现在，语义网的愿景必须考虑到颠覆性和炫目的生成式人工智能技术的存在。

在这篇文章中，我将探讨为什么语义网尽管失去了最初的光彩，但仍然是网络拼图中的一个重要部分，即便与新的生成式人工智能共存，以及为什么数据科学专业人士也应该了解它。

但首先，让我们来看看创立语义网的那个人。

# 网络背后的男人

[蒂姆·伯纳斯-李](https://en.wikipedia.org/wiki/Tim_Berners-Lee)（TimBL）在 1999 年提出了语义网（SW）的概念，当时他在 [万维网联盟（W3C）](https://www.w3.org/)工作。这个想法是创建一个机器可以处理的数据网络，使它们能够理解信息的含义并在不同数据片段之间建立联系。

我们必须理解，语义网对蒂姆·伯纳斯-李来说是第二次尝试。1999 年，他已经是全球（不是开玩笑）知名人物，因为他早已创造了万维网。

在 1990 年，当他作为访问学者在瑞士的 CERN 时，他向他的老板迈克·森德尔提出了将网络变为现实的想法，后者认为这个提案“模糊但令人兴奋。” TimBL 说他几乎没有发明什么，因为超链接、互联网协议和许多其他元素已经存在。然而，没有人采取将它们全部整合起来的步骤。

正如 TimBL 所说，

> *我只需将超文本的理念与 TCP 和 DNS 的理念连接起来——瞧！——于是出现了万维网。*

人类的一小步……

第一个构建的网页仍然可以在这个地址访问——没有花哨的格式，只是在顶部写着“**第一个网站的首页。**”

现在你对 TimBL 有了了解……以及为什么在他的第二个提案，即语义网提案，几乎在世纪之交时发布时，他会被非常严肃地对待。

# 语义网的愿景

语义网被视为互联网发展的下一步，互联网直到那时还专注于将文档链接在一起。通过语义网，重点将转向连接数据，创建一个更智能和互联的网络。

语义网的愿景是使机器能够理解网络上信息的含义，并利用这种理解提供更智能和个性化的服务。在世纪之交时，显而易见的是，网络不仅仅是供人类使用的，但信息在网页中的存储方式完全无法自动化处理信息。根据 TimBL 的说法，传统网络正在成为网络处理自动化的瓶颈。

这一愿景非常吸引人：与传统的网络不同，传统网络的页面设计用于向人类展示信息，自描述的网页将使得自动化处理内容成为可能。因此，彻底改造我们所知的互联网是非常有意义的。正如他对网络所做的那样，TimBL 呼吁开始新的革命。

我当时在场，见证了这一运动的兴起——不仅仅是观察：我的几位研究生也在从事语义网相关的研究。

# 实现语义网的技术

但语义网不仅仅是一个理念——为使其成为现实，开发了一系列技术和标准，这些内容在以下几点中有所介绍：

+   **资源描述框架（RDF）**：用于在网络上表示信息的数据模型。RDF 使用 URI 来标识资源，使用谓词来描述资源之间的关系。

+   **Web 本体语言（OWL）**：一种用于描述 RDF 数据含义的表达性语言。OWL 可用于定义资源之间的复杂关系以及定义在层次结构中组织的资源类。

+   **SPARQL**：一种用于 RDF 数据的查询语言。SPARQL 可以用来从 RDF 数据存储中提取信息。

+   **语义网服务**：一种使用语义标记来描述服务能力以及它所消耗和产生的数据的网络服务。

一些重要的语义网发展，例如 DBpedia，一种按照上述标准组织的结构化信息的维基百科，已经开展。它包含超过 2.28 亿个实体，当我们不习惯谈论像大型语言模型中的节点数这样的大量数据时，这个数字显得非常庞大（见下文）。

DBpedia 类似于 Google 的“知识图谱”，它被用来解决搜索查询。Google 的知识图谱是一个概念和名称的网络，表现为 RDF 三元组的集合。我不能给你更多的信息，因为这主要是 Google 的内部信息。我相信知识图谱对 Google 提供了相对于竞争对手的优势。

例如，我相信你已经注意到，Google 有时会检索到关键词没有出现但与查询相关的页面。这是因为“*语义搜索*”。这种搜索超越了简单的关键词匹配，通过结合与查询相关的知识图谱中的术语来进行。语义搜索考虑了同义词和其他概念关系，以丰富搜索结果。

即使不完全了解每项技术，我们也可以看到，从技术角度来看，几乎所有的要素都已经存在。那么，为什么这次缺少了“哇哦！”的时刻？为什么 SW 花了这么长时间才显现？

# 生成式 AI 的崛起

SW 认为需要机器可读的格式来理解存储在 Web 上的信息的含义。TimBL 认为人类可读的信息对机器来说不可用。

而且时间不长。

直到 2022 年 11 月，ChatGPT 在抓取了数百万个网页文档后，才开始能够回答人类提出的问题，尽管不太可靠。

然后情况发生了快速而戏剧性的变化。ChatGPT 和其他聊天机器人的令人印象深刻的对话能力表明，存储在 Web 上的信息可以被机器“[理解](https://medium.com/generative-ai/i-started-to-think-chatgpt-might-actually-understand-after-all-6a7eec0209a0)”。进一步地，它可以回答问题并向人类建议行动。

谁会想到这一点？我没想到，Tim Berners-Lee 也没想到。

生成式 AI 很快超越了语义网（SW）。它的能力吸引了开发者、研究人员和企业的关注。焦点从创建结构化的 SW 转向利用 AI 的力量生成内容。

# 会有复兴吗？

一旦 SW 的核心假设（即机器无法理解网页）被证明是错误的，它的未来变得不确定。许多人认为，SW 会变得过时，走向像渡渡鸟一样的灭绝。

不要那么快。

首先，像 RDF、SPARQL 和语义网服务这样的具体 SW 技术将会长期存在，因为它们是现有基础设施的一部分。事实上，[它们是 Amazon Web Services 提供的一部分](https://docs.aws.amazon.com/neptune/latest/userguide/get-started-graph-sparql.html)。

此外，我认为，无论是在 Google 还是其他地方，知识图谱都将补充生成式 AI 解决方案。

让我给你一个具体的例子。

从今年春天开始，我与同事讨论了 Bing 如何使用知识图谱回答问题（是的，微软也有它的知识图谱版本）。然后我看到了一张 Bing 的技术图解，变得清楚它们是如何做到的。

事情是这样的：

当查询提交给 Bing 时，他们使用它来检索知识图谱的相关部分。结果类似于补充原始用户查询的一组陈述。这种“增强”的查询随后作为 ChatGPT 或其他大型语言模型的提示。就是这样。

从知识图谱获取的信息相比于没有它的聊天机器人有优势：主要的优势在于它由可靠的信息组成，而不是由 LLM 单独生成的虚假信息、幻想或其他虚假信息。

还有一个令人垂涎的可能性没有人提及：AI 可以用于注释常规网页，并使用 SW 标准使其自我描述。

你知道，拖慢 SW 的一个障碍是为每个特定网站采用它的成本。SW 从未成为许多公司的优先事项，所以他们避免将资源用于将页面改造为 SW 标准。

但有了 AI，转变普通网页为 SW 网页可能会变得便宜。实际上，聊天机器人特别擅长遵循格式：我自己验证过这一点。因此，AI 可能会去除 SW 采用的主要障碍，前提是对其仍有兴趣。

# 结束语

我认为知识图谱以及许多与 SW 相关的技术最终将保持有用。

讽刺的是，生成性 AI 起初看似会给 SW 最后一击，但最终可能会与其形成一种共生关系，使其再次变得相关。它们正在合作开发诸如新型 Web 搜索的生成性 AI 应用，未来可能会继续合作。

所以，回答“SW 会有复苏吗？”这个问题的答案是响亮的“是的”，但不是按照 TimBL 最初的设想，即全面改造 Web，而主要作为对其他系统的支持，确实少了些光彩，但同样有用。

让我们面对现实：TimBL 对语义网的原始愿景永远不会成为现实。那是一个（或看似）极好的想法，个人来说，我曾经坚信不疑地投入其中。但是技术并不促进优秀的想法，而是方便的想法。

那么，让我们结束于新的语义网愿景：

*“愿沉闷、乏味、朴实、真实的语义网长命百岁，它将作为闪亮、花哨、迷人的人工智能的支持角色。”*
