# 并非全是彩虹和阳光：ChatGPT 的阴暗面

> 原文：[https://towardsdatascience.com/not-all-rainbows-and-sunshine-the-darker-side-of-chatgpt-75917472b9c?source=collection_archive---------0-----------------------#2023-01-27](https://towardsdatascience.com/not-all-rainbows-and-sunshine-the-darker-side-of-chatgpt-75917472b9c?source=collection_archive---------0-----------------------#2023-01-27)

## 第1部分：大型语言模型的风险和伦理问题

[](https://mary-reagan.medium.com/?source=post_page-----75917472b9c--------------------------------)[![Mary Reagan PhD](../Images/e3e149519419faf20a50f44600378755.png)](https://mary-reagan.medium.com/?source=post_page-----75917472b9c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----75917472b9c--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----75917472b9c--------------------------------) [Mary Reagan PhD](https://mary-reagan.medium.com/?source=post_page-----75917472b9c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4a596f4380a0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnot-all-rainbows-and-sunshine-the-darker-side-of-chatgpt-75917472b9c&user=Mary+Reagan+PhD&userId=4a596f4380a0&source=post_page-4a596f4380a0----75917472b9c---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----75917472b9c--------------------------------) · 9 分钟阅读 · 2023年1月27日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F75917472b9c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnot-all-rainbows-and-sunshine-the-darker-side-of-chatgpt-75917472b9c&source=-----75917472b9c---------------------bookmark_footer-----------)![](../Images/352cf5e838507e7a22b65e9a6aea2a22.png)

[图片](https://drive.google.com/file/d/1tPFnutb_CTri72gX8RuJAVOLWtPYRXAh/view?usp=share_link) 由 Fiddler AI 提供，已获许可

如果你还没听说过 ChatGPT，你一定是躲在一块非常大的石头下。这款病毒式聊天机器人被用于自然语言处理任务，如文本生成，已经引起了广泛关注。其背后的公司 OpenAI 最近在谈判以获得 290 亿美元的估值¹，微软可能很快会再投资 100 亿美元²。

ChatGPT是一个自回归语言模型，使用深度学习生成文本。它通过在各种领域提供详细的回答让用户感到惊讶。它的回答非常有说服力，以至于很难判断这些回答是否由人类撰写。ChatGPT建立在OpenAI的GPT-3系列大型语言模型（LLMs）基础上，于2022年11月30日推出。它是最大的LLM之一，可以写出优美的文章和诗歌，生成可用的代码，以及根据文本描述生成图表和网站，所有这些都不需要或几乎不需要监督。ChatGPT的回答如此出色，它显示出有可能成为无处不在的Google搜索引擎的潜在对手。

大型语言模型是……嗯……***庞大***的。它们在大量的文本数据上进行训练，这些数据可以达到PB级，并且具有数十亿个参数。最终的多层神经网络通常有几个TB大。围绕ChatGPT和其他LLM的炒作和媒体关注是可以理解的——它们确实是人类智慧的杰出成果，有时会以新兴行为让这些模型的开发者感到惊讶。例如，通过在提示的开头使用某些“魔法”短语，如“让我们一步一步思考”，可以改善GPT-3的回答。这些新兴行为表明了模型的巨大复杂性以及当前的可解释性缺失，甚至让开发者思考这些模型是否具有意识。

# 大型语言模型的幽灵

尽管有许多积极的宣传和炒作，但[负责任的人工智能](https://www.fiddler.ai/blog/responsible-ai-by-design)社区中的一些人发出了强烈的警告。值得注意的是，在2021年，负责人工智能领域的著名研究员Timit Gebru发表了一篇论文，警告了与LLM相关的许多伦理问题，这也导致她被谷歌解雇。这些警告涉及广泛的问题：缺乏可解释性、抄袭、隐私、偏见、模型鲁棒性以及其环境影响。让我们稍微探讨一下这些话题。

# 信任与缺乏可解释性：

深度学习模型，尤其是LLM，已经变得如此庞大和不透明，以至于即使是模型的开发者也常常无法理解为什么他们的模型会做出某些预测。这种缺乏可解释性是一个重大问题，特别是在用户希望了解模型生成特定输出的原因和方式的情况下。

在轻松的风格下，我们的首席执行官Krishna Gade使用了ChatGPT创作了一首关于可解释人工智能的诗歌，风格模仿了**约翰·济慈**，坦率地说，我认为效果相当不错。

![](../Images/7c8dff325094a5cb5f589745e90a4bf0.png)

克里希纳正确指出，关于模型如何得出输出的透明度不足。对于LLMs生成的作品，缺乏关于输出所依赖数据来源的透明度意味着ChatGPT提供的答案无法被正确引用，因此用户无法验证或信任其输出⁹。这导致ChatGPT创建的答案在像Stack Overflow¹⁰这样的论坛上被禁止。

当使用像OpenAI的嵌入模型¹¹这样的工具时，透明度和理解模型如何得出输出变得尤为重要，因为这些模型本质上包含了一层模糊性，或者在模型用于高风险决策的情况下。例如，如果有人使用ChatGPT获取急救指示，用户需要知道回应是可靠的、准确的，并且来源可信。虽然存在各种事后方法来解释模型的选择，但这些解释在模型部署时往往被忽视。

这种缺乏透明度和可信度的后果在假新闻和虚假信息泛滥的时代尤其令人担忧，因为LLMs可能会被微调以传播虚假信息并威胁政治稳定。虽然Open AI正在研究各种方法来识别其模型的输出，并计划嵌入加密标签以对输出进行水印¹²，这些负责任的AI解决方案的速度仍然不足，可能也不够充分。

这引发了关于……的问题。

# 剽窃：

精心制作的ChatGPT文章的来源难以追踪自然引发了关于剽窃的讨论。但这是否*真的*是个问题？作者认为不是。在ChatGPT出现之前，学生们已经可以利用写作服务¹³，且一直有少数学生决心作弊。但对ChatGPT使所有孩子变成无脑的剽窃者的担忧已成为许多教育者的关注重点，并导致一些学区禁止使用ChatGPT¹⁴。

关于剽窃的讨论掩盖了与LLMs相关的更大、更重要的伦理问题。鉴于这一话题的广泛关注，我不得不提及它。

# 隐私：

如果大型语言模型用于处理敏感数据，则面临数据隐私泄露的风险。训练集来源于各种数据，有时包括个人身份信息¹⁵ — 姓名、电子邮件地址¹⁶、电话号码、地址、医疗信息 — 因此，可能会出现在模型的输出中。虽然这是任何使用敏感数据训练的模型都会面临的问题，但考虑到LLMs的训练集规模庞大，这个问题可能影响到很多人。

# 潜在偏见：

如前所述，这些模型在大量的数据语料库上进行训练。当数据训练集如此庞大时，它们变得非常难以审计，因此固有地存在风险⁵。这个[数据包含社会和历史偏见](/understanding-bias-and-fairness-in-ai-systems-6f7fbfe267f3)¹⁷，因此任何在这些数据上训练的模型都可能会再现这些偏见，除非采取适当的保护措施。许多流行的语言模型被发现含有偏见，这可能导致偏见思想的传播增加，并对某些群体造成持续伤害。GPT-3 被发现表现出常见的性别刻板印象¹⁸，将女性与家庭和外貌联系起来，并将她们描述为比男性角色更没有权力。令人遗憾的是，它还将穆斯林与暴力联系在一起¹⁹，其中三分之二对包含“穆斯林”一词的提示的回应中都包含了暴力的参考。很可能存在更多的偏见关联尚未被发现。

值得注意的是，微软的聊天机器人在 2016 年迅速变成了最糟糕的网络恶搞者的模仿者²⁰，吐露种族主义、性别歧视和其他辱骂性语言。尽管 ChatGPT 设有过滤器以尝试避免最糟糕的此类语言，但它可能并非万无一失。OpenAI 为人工标注员支付费用，以标记最具攻击性和令人不安的数据，但其合作公司因每小时仅支付 2 美元而受到批评，工人报告称遭受了深刻的心理伤害²¹。

# 模型的鲁棒性和安全性：

由于大语言模型（LLMs）是预先训练的，并随后根据特定任务进行微调，这会导致一系列问题和安全风险。值得注意的是，LLMs 缺乏提供不确定性估计的能力²²。没有了解模型的置信度（或不确定性），我们很难判断何时可以信任模型的输出，何时需要保持怀疑态度²³。这影响了它们在微调到新任务时的表现以及避免过拟合的能力。可解释的不确定性估计有潜力提高模型预测的鲁棒性。

模型安全性是一个迫在眉睫的问题，原因在于 LLM 的父模型在微调步骤之前的普遍性。因此，模型可能成为单点故障和攻击的主要目标，这会影响从原始模型派生的任何应用。此外，由于缺乏监督训练，LLMs 可能会受到数据投毒的威胁²⁵，这可能导致恶意言论被注入以针对特定公司、群体或个人。

LLM 的训练语料库是通过爬取互联网上各种语言和主题来源创建的，然而它们仅仅反映了最有可能接触并频繁使用互联网的人群。因此，AI 生成的语言趋于同质化，并且通常反映了最富有的社区和国家的实践⁶。对于不在训练数据中的语言，LLMs 更容易失败，需要更多的研究来解决与分布外数据相关的问题。

# 环境影响与可持续性：

一篇由 Strubell 和合作者于 2019 年发表的论文概述了 LLM 训练生命周期的巨大碳足迹²⁴ ²⁶，其中，训练一个拥有 2.13 亿参数的神经架构搜索模型被估算产生的碳排放量是普通汽车生命周期排放量的五倍以上。考虑到 GPT-3 拥有 175 *亿* 个参数，而下一代 GPT-4 传闻拥有 100 *万亿* 个参数，这在面临气候变化带来的日益严重的恐怖和破坏的世界中是一个重要的方面。

# 现在怎么办？

任何新技术都会带来优势和劣势。我已经概述了许多与大语言模型（LLMs）相关的问题，但我想强调的是，我也对这些模型为我们每个人带来的新可能性和承诺感到兴奋。社会有责任采取适当的保障措施，并明智地使用这项新技术。任何在公众中使用或公开的模型都需要进行监控、解释，并定期审计模型的偏差。在第2部分中，我将概述对 AI/ML 从业者、企业和政府机构的建议，说明如何解决特定于 LLMs 的一些问题。

**参考文献：**

1.  [ChatGPT 创始人与投资者谈论以 290 亿美元估值出售股份](https://www.wsj.com/articles/chatgpt-creator-openai-is-in-talks-for-tender-offer-that-would-value-it-at-29-billion-11672949279)，华尔街日报，2023年。

1.  Todd Bishop, [微软计划通过潜在的 100 亿美元投资和新的整合来巩固与 OpenAI 的关系](https://www.geekwire.com/2023/microsoft-looks-to-tighten-ties-with-openai-through-potential-10b-investment-and-new-integrations/)，GeekWire，2023年。

1.  Parmy Olson, [ChatGPT 应该让 Google 和 Alphabet 感到担忧。为什么要搜索，当你可以问 AI？](https://www.bloomberg.com/opinion/articles/2022-12-07/chatgpt-should-worry-google-and-alphabet-why-search-when-you-can-ask-ai)，彭博社，2022年。

1.  Subbarao Kambhampati, [改变人工智能研究的性质](https://cacm.acm.org/magazines/2022/9/263799-changing-the-nature-of-ai-research/fulltext)，ACM 通信，2022年。

1.  Roger Montti, [什么是 Google LaMDA，为什么有人相信它具有意识？](https://www.searchenginejournal.com/google-lamda-sentient/454820/)，搜索引擎期刊，2022年。

1.  艾米莉·M·本德，蒂姆尼特·戈布鲁，安吉丽娜·麦克米伦-梅杰，施玛尔格·施密切，[关于随机鹦鹉的危险：语言模型会不会过大？](https://dl.acm.org/doi/10.1145/3442188.3445922) FAccT 2021。

1.  蒂姆尼特·戈布鲁，[有效利他主义推动一种危险的“AI安全”品牌](https://www.wired.com/story/effective-altruism-artificial-intelligence-sam-bankman-fried/)，Wired，2022年。

1.  克里希纳·加德，[https://www.linkedin.com/feed/update/urn:li:activity:7005573991251804160/](https://www.linkedin.com/feed/update/urn:li:activity:7005573991251804160/)

1.  奇拉格·沙，艾米莉·本德，[情境化搜索](https://dl.acm.org/doi/abs/10.1145/3498366.3505816)，CHIIR 2022。

1.  [为什么发布GPT和ChatGPT生成的回答目前不可接受](https://stackoverflow.com/help/gpt-policy)，Stack Overflow，2022年。

1.  [https://openai.com/blog/new-and-improved-embedding-model/](https://openai.com/blog/new-and-improved-embedding-model/)

1.  凯尔·维格斯，[OpenAI尝试给AI文本加水印遇到的限制](https://techcrunch.com/2022/12/10/openais-attempts-to-watermark-ai-text-hit-limits/)，TechCrunch，2022年。

1.  [7个最佳大学论文写作服务：评论和排名](https://www.globenewswire.com/news-release/2021/03/27/2200368/0/en/7-Best-College-Essay-Writing-Services-Reviews-Rankings.html)，GlobeNewswire，2021年。

1.  卡尔汉·罗森布拉特，[ChatGPT被禁止在纽约市公立学校的设备和网络上使用](https://www.nbcnews.com/tech/tech-news/new-york-city-public-schools-ban-chatgpt-devices-networks-rcna64446)，NBC新闻，2023年。

1.  尼古拉斯·卡尔尼，弗洛里安·特拉默，埃里克·沃勒斯，马修·贾吉尔斯基，阿里尔·赫伯特-沃斯，凯瑟琳·李，亚当·罗伯茨，汤姆·布朗，道恩·宋，乌尔法尔·厄尔林森，阿丽娜·奥普雷亚，科林·拉费尔，[从大型语言模型中提取训练数据](https://arxiv.org/abs/2012.07805)，USENIX安全研讨会，2021年。

1.  马丁·安德森，[从预训练自然语言模型中检索现实世界的电子邮件地址](https://www.unite.ai/retrieving-real-world-email-addresses-from-pretrained-natural-language-models/)，Unite.AI，2022年。

1.  玛丽·里根，[理解AI系统中的偏见和公平性](https://www.fiddler.ai/blog/ai-explained-understanding-bias-and-fairness-in-ai-systems)，Fiddler AI博客，2021年。

1.  李·露西，戴维·班曼，[GPT-3生成故事中的性别和表现偏见](https://aclanthology.org/2021.nuse-1.5.pdf)，ACL叙事理解研讨会，2021年。

1.  安德鲁·迈尔斯，[剔除流行语言模型GPT-3中的反穆斯林偏见](https://hai.stanford.edu/news/rooting-out-anti-muslim-bias-popular-language-model-gpt-3)，斯坦福HAI新闻，2021年。

1.  詹姆斯·文森特，[推特让微软的AI聊天机器人在不到一天的时间里变成了一个种族主义者](https://www.theverge.com/2016/3/24/11297050/tay-microsoft-chatbot-racist)，The Verge，2016年。

1.  比利·佩里戈，[OpenAI在肯尼亚以每小时不到2美元的工资雇佣工人来减少ChatGPT的毒性](https://time.com/6247678/openai-chatgpt-kenya-workers/)，《时代》杂志，2023年。

1.  Karthik Abinav Sankararaman, Sinong Wang, Han Fang, [BayesFormer：具有不确定性估计的 Transformer](https://arxiv.org/abs/2206.00826)，arxiv，2022年。

1.  Andrew Ng, [ChatGPT 疯狂！加密混乱削减了AI安全资金，Alexa 讲睡前故事](https://www.deeplearning.ai/the-batch/issue-174/)，《The Batch — Deeplearning.ai 通讯》，2022年。

1.  Emma Strubell, Ananya Ganesh, Andrew McCallum, [深度学习在 NLP 中的能源和政策考虑](https://arxiv.org/abs/1906.02243)，ACL 2019年。

1.  Eric Wallace, Tony Z. Zhao, Shi Feng, Sameer Singh, [对 NLP 模型的隐蔽数据污染攻击](https://arxiv.org/abs/2010.12563)，NAACL 2021年。

1.  Karen Hao, [训练单一AI模型可能排放相当于五辆汽车使用寿命的碳](https://www.technologyreview.com/2019/06/06/239031/training-a-single-ai-model-can-emit-as-much-carbon-as-five-cars-in-their-lifetimes/)，《麻省理工学院技术评论》，2019年。
