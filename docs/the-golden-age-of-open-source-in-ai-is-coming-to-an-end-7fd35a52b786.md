# 开源AI的黄金时代即将结束

> 原文：[https://towardsdatascience.com/the-golden-age-of-open-source-in-ai-is-coming-to-an-end-7fd35a52b786?source=collection_archive---------1-----------------------#2023-06-07](https://towardsdatascience.com/the-golden-age-of-open-source-in-ai-is-coming-to-an-end-7fd35a52b786?source=collection_archive---------1-----------------------#2023-06-07)

## 你不希望在你使用的模型的开源许可证中看到的缩写如NC、SA、GPL等

[](https://medium.com/@clemensm?source=post_page-----7fd35a52b786--------------------------------)[![克莱门斯·梅瓦尔德](../Images/c1c9277335dc2a8e238736be824f7fdd.png)](https://medium.com/@clemensm?source=post_page-----7fd35a52b786--------------------------------)[](https://towardsdatascience.com/?source=post_page-----7fd35a52b786--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----7fd35a52b786--------------------------------) [克莱门斯·梅瓦尔德](https://medium.com/@clemensm?source=post_page-----7fd35a52b786--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3214e56806b6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-golden-age-of-open-source-in-ai-is-coming-to-an-end-7fd35a52b786&user=Clemens+Mewald&userId=3214e56806b6&source=post_page-3214e56806b6----7fd35a52b786---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----7fd35a52b786--------------------------------) ·7分钟阅读·2023年6月7日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F7fd35a52b786&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-golden-age-of-open-source-in-ai-is-coming-to-an-end-7fd35a52b786&user=Clemens+Mewald&userId=3214e56806b6&source=-----7fd35a52b786---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F7fd35a52b786&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-golden-age-of-open-source-in-ai-is-coming-to-an-end-7fd35a52b786&source=-----7fd35a52b786---------------------bookmark_footer-----------)![](../Images/a5880e6b0a367be782b76b271fdc6be1.png)

图片由作者提供（修改自 [来源](https://pixabay.com/illustrations/hand-drawing-draw-notes-sketch-1515910/))

# 开源AI库和模型的（有偏见的）历史

我在 2015 年加入了 Google Brain 团队，正值 [TensorFlow 开源](https://ai.googleblog.com/2015/11/tensorflow-googles-latest-machine.html)。与普遍认知相反，TensorFlow 在当时并不是 Google 成功的秘密武器。只有少数研究人员使用了它，直到几年后，它才在实质上改变了 Alphabet 最重要的资产。

然而，TensorFlow 对开源社区的影响几乎是立竿见影的。它开启了一个由社区驱动的创新时代，这直接促进了过去几年 AI 进展的飞速发展。公平地说，TensorFlow 并不是第一个开源深度学习库（例如，[Caffe](https://caffe.berkeleyvision.org/) 于 2014 年发布），但它是第一个得到像 Google 这样的公司支持的信誉（以及开发者宣传预算）的库。

但 TensorFlow 只是一个库。关键是，你仍然需要提供自己的数据来实际训练预测**模型**。为了预测未来的房价，你需要一个包含历史房价的数据集，并使用 TensorFlow 来训练一个模型。最终得到的模型现在编码了你数据的综合知识。在 TensorFlow 开源几年后，Google 采取了另一个决定性的步骤，加速了向“全民免费”AI 的路径。2018 年 [开源 BERT 模型](https://ai.googleblog.com/2018/11/open-sourcing-bert-state-of-art-pre.html) 的决定帮助引发了大型语言模型的雪崩。没过多久，在 2019 年，OpenAI（当时仍是非营利组织）[开源了他们的 GPT2 模型](https://openai.com/research/gpt-2-1-5b-release)。就这样，开源训练模型成为了一种趋势。

从开源一个像 TensorFlow 这样的库到一个像 BERT 这样的全训练模型的升级不容小觑。TensorFlow 仅仅是一组指令，而 BERT 是将这些指令应用于大量数据的代价高昂的训练过程的结果。用一个强有力的类比来说：如果 TensorFlow 是一本关于人类繁殖的生物学教科书，那么 BERT 就是一个大学毕业生。有人阅读了这本教科书，应用了它的指令，并花费了大量时间和金钱来培养一个现在已经完全教育成才、准备进入职场（或研究生院）的成人。

> “如果 TensorFlow 是一本关于人类繁殖的生物学教科书，那么 BERT 就是一个大学毕业生。”

# 开源决策

我们是如何到达现在的局面的？我将那个时期许多开源决策归因于研究科学家的日益显著和相对谈判权力。研究人员更倾向于从事能够在权威期刊上发表论文的工作（如[NeurIPS](https://nips.cc/)），这些出版物若附带[开源代码](https://paperswithcode.com/)则更具相关性（和可信度）。这也是像苹果这样更为神秘的公司[难以吸引人才](https://hbr.org/2015/11/can-apple-attract-top-researchers-if-it-keeps-their-research-secret)的原因之一。允许公开讨论工作成果的研究人员在业界广受认可，他们的市场价值与他们在顶级期刊上的发表直接相关。由于顶尖的AI研究人员稀缺，[他们的年薪包超过$1M](https://www.nytimes.com/2018/04/19/technology/artificial-intelligence-salaries-openai.html)。

对这些开源决策并未进行很多经济上的审视，更不用说更重要的许可问题（稍后将详细讨论）。谷歌放弃BERT的知识产权价值是多少？谷歌的竞争对手将如何利用这些库和模型对付它们？值得庆幸的是，开源基础设施项目及其企业维护者提供了一个有用的案例研究，展示了公司如何逐渐认识到开源许可决策的影响。

# 开源的海洋变革

在TensorFlow崛起的同时，预示着开源AI未来的到来，企业软件经历了一场开源许可危机。主要是由于[多亏了AWS](https://www.nytimes.com/2019/12/15/technology/amazon-aws-cloud-competition.html)，它掌握了将开源基础设施项目转化为商业服务的技巧，许多开源项目将其许可协议从“允许”改为“Copyleft”或“ShareAlike”（SA）替代品。

并非所有开源项目都一样。允许性许可（如Apache 2.0或MIT）允许任何人将开源项目转化为商业服务。与之相对的“Copyleft”许可（如GPL），类似于创意共享的“ShareAlike”条款，是一种保护措施。如果AWS基于一个具有“Copyleft”许可的开源项目推出服务，那么AWS服务本身必须在相同许可下开源。

因此，部分是对竞争云服务的回应，开源项目的公司创作者和维护者，如[MongoDB](https://techcrunch.com/2018/10/16/mongodb-switches-up-its-open-source-license/)和[Redis](https://techcrunch.com/2019/02/21/redis-labs-changes-its-open-source-license-again/)，改变了它们的许可证，转向了更不可容忍的选择。这导致了[AWS](https://aws.amazon.com/blogs/opensource/setting-the-record-straight-aws-open-source/)与[这些公司](https://redis.com/blog/aws-vs-open-source/)之间围绕开源原则和价值的痛苦但有趣的争论，这种争论后来有所缓和。

请注意，这种许可证变更对开源生态系统产生了欺骗性的影响：虽然仍有许多新的开源项目被宣布，但关于这些项目可以和不可以做的许可证影响比大多数人意识到的要复杂得多。

# 在开源AI中的转变潮流

此时，你应该自问：如果开源基础设施项目的企业维护者意识到其他人正在收获更多的商业利益，而不是他们自己，那么同样的情况是否会发生在人工智能领域？对于持有它们创造的计算和数据的总体价值的开源AI模型来说，这不是更大的问题吗？答案是：是的，是的。

尽管围绕开源人工智能似乎存在一种罗宾汉式的运动，但数据显示情况正朝不同的方向发展。微软等大型企业正在将一些最受欢迎的模型的许可证从可容许转变为非商业（NC）许可证，Meta开始在所有最新的开源项目（如[MMS](https://ai.facebook.com/blog/multilingual-model-speech-recognition/)、[ImageBind](https://ai.facebook.com/blog/imagebind-six-modalities-binding-ai/)和[DINOv2](https://ai.facebook.com/blog/dino-v2-computer-vision-self-supervised-learning/)，都采用CC-BY-NC 4.0许可证，以及[LLAMA](https://ai.facebook.com/blog/large-language-model-llama-meta-ai/)采用GPL 3.0）。甚至像[斯坦福的Alpaca](https://github.com/tatsu-lab/stanford_alpaca)这样的大学项目，也仅限于非商业使用（继承自其使用数据集的不可容忍属性）。整个公司改变其业务模式，以保护其知识产权并摆脱开源作为其使命的一部分的义务 — 还记得一个名叫OpenAI的小型非营利组织[转变为](https://openai.com/blog/openai-lp)有限盈利的事件吗？请注意，GPT2是开源的，但GPT3.5或GPT4却没有？

更普遍地说，虽然不透明，但AI领域向更严格许可的趋势是显而易见的。下面是对[Hugging Face](https://huggingface.co/models)模型许可的分析。从2022年中期开始，允许性许可（如Apache、MIT或BSD）的比例持续下降，而不可许可的许可（如GPL）或限制性许可（如OpenRAIL）则变得越来越普遍。

![](../Images/859d31a8f4ec2bd22d3312453309373e.png)

来源：作者分析

更糟糕的是，最近对大型语言模型（LLMs）的狂热进一步混淆了情况。Hugging Face 维护了一个“[开放 LLM 排行榜](https://huggingface.co/spaces/HuggingFaceH4/open_llm_leaderboard)”旨在突出“*开源社区所取得的真正进展*”。公平地说，排行榜上的所有模型确实都是开源的。然而，仔细观察会发现几乎没有一个是商业用途许可的*。

![](../Images/5ecd1e36643248f20c368c81558f9ead.png)

来源：作者分析

**在这篇文章写作和发布之间，[*Falcon 模型*](https://falconllm.tii.ae/) 的许可已经更改为允许的 Apache 2.0 许可。总体观察仍然有效*。

如果说有什么值得注意的，那就是开放LLM排行榜突出显示了大型科技公司（LLaMA由Meta以非商业许可开源）的创新主导了所有其他开源努力。更大的问题是这些衍生模型在其许可方面不够透明。几乎没有明确声明其许可的模型，你必须自己研究才能发现这些模型及其所基于的数据不允许商业使用。

# 开源AI的未来

社区中有很多虚伪的标榜，主要来自善意的企业家和风险投资者，他们[希望](https://www.linkedin.com/posts/turck_this-week-in-ai-adobe-killed-all-the-generative-activity-7067856288155586560-FPNT?utm_source=share&utm_medium=member_desktop)有一个不被OpenAI、谷歌和少数其他公司主导的未来。为什么AI模型应该开源并不明显——它们代表了公司多年来开发的辛苦知识产权，涉及数十亿的计算、数据获取和人才投入。如果公司随便将所有东西免费赠送给别人，那就等于欺骗了他们的股东。

> “如果我能投资一个知识产权律师ETF，我会的。”

开源AI中向不可许可许可的趋势似乎很明确。然而，新闻的压倒性数量未能指出这些工作的累积好处几乎完全归属于学术界和爱好者。投资者和高管应更加意识到这些影响并加倍小心。我强烈感觉到大多数新兴LLM棉花行业的初创公司都是在非商业许可的技术基础上建立的。如果我能投资一个知识产权律师ETF，我会的。

我的预测是，AI的价值捕获（特别是对于最新一代的大型生成模型）将类似于其他需要大量资本投资和专业人才积累的创新，如云计算平台或操作系统。一些主要的玩家将会出现，为整个生态系统提供AI基础。虽然在这个基础上还有充足的创业公司层，但就像没有开源项目能取代AWS一样，我认为开源社区很难产生对OpenAI的GPT及其后续版本的严肃竞争者。

*本文中表达的观点仅代表我个人，不能代表我的雇主的观点。*

[*Clemens Mewald*](https://www.linkedin.com/in/clemensmewald/) *领导着Instabase的产品团队。之前，他在Databricks上参与了开源AI项目，如MLflow，在Google参与了TensorFlow。*
