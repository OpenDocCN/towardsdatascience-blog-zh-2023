# 反对企业 LLMs 的案例

> 原文：[https://towardsdatascience.com/the-case-against-enterprise-llms-43795a0db501?source=collection_archive---------2-----------------------#2023-04-29](https://towardsdatascience.com/the-case-against-enterprise-llms-43795a0db501?source=collection_archive---------2-----------------------#2023-04-29)

## 观点

## 一种清醒的视角，说明为什么无聊可能是最好的，甚至对于AI来说也是如此

[](https://medium.com/@lsci?source=post_page-----43795a0db501--------------------------------)[![Mathieu Lemay](../Images/39db2877c94829bef1d6642daf3ccecb.png)](https://medium.com/@lsci?source=post_page-----43795a0db501--------------------------------)[](https://towardsdatascience.com/?source=post_page-----43795a0db501--------------------------------)[![数据科学之路](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----43795a0db501--------------------------------) [Mathieu Lemay](https://medium.com/@lsci?source=post_page-----43795a0db501--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff84a70d8f74&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-case-against-enterprise-llms-43795a0db501&user=Mathieu+Lemay&userId=f84a70d8f74&source=post_page-f84a70d8f74----43795a0db501---------------------post_header-----------) 发表在 [数据科学之路](https://towardsdatascience.com/?source=post_page-----43795a0db501--------------------------------) ·6分钟阅读·2023年4月29日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F43795a0db501&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-case-against-enterprise-llms-43795a0db501&user=Mathieu+Lemay&userId=f84a70d8f74&source=-----43795a0db501---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F43795a0db501&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-case-against-enterprise-llms-43795a0db501&source=-----43795a0db501---------------------bookmark_footer-----------)

在过去的几周里，我们收到了大量来自客户和合作伙伴的定制 LLM 请求。这种兴奋感，虽然是合理的，但更多是基于技术新闻的轰炸，而不是获取根本的企业优势。

尽管 LLMs 在概念上并不比大多数基于变换器的训练管道差，但在企业环境中微调和顺利操作需要更复杂的机制。我们已经测试和部署给客户的所有模型都很好，但它们没有像抛光的商业产品那样的光泽，这对业务负责人来说是个问题。

别误解我的意思：LLMs 是自 GPU 加速训练以来 AI 中最棒的东西，但它们应该是最后的手段，而不是在企业 AI 领域中首次尝试的工具。

![](../Images/82f7477b2193751b3997054be184aa65.png)

图片来自 [Pixabay](https://www.pexels.com/photo/abandoned-broken-cabinets-demolished-206829/)  见于 Pexels.com

“软件变慢的速度比硬件变快的速度快”是计算机科学中的一个古老谚语。软件容易膨胀，因为通常添加模块比删除模块要容易。

在 AI 方面，从机器学习模型到深度神经网络，再到 LSTM、预训练 CNNs 以及现在的 Transformers，模型的增长遵循了类似的路径。虽然使用顶级技术是合理的，但在某些情况下，如果总拥有成本超过任何已识别的收益，渐近收益就会变成递减回报。

在与客户的对话中，一个诡异的熟悉模式正在重新出现：那些今天推动 ChatGPT 克隆的高管，几年前对聊天机器人也是态度坚决的。这是一个容易取得可证明胜利的手段，而无需将其与企业价值关联或考虑其正当性。他们对于“你只是不看到价值”的宗教式坚持，充其量只是懒惰的陈词滥调，最坏的情况则是一个资源浪费变成企业负担。

在这些部署中出现的许多问题并不在于项目的理由或可行性，而是在于为什么从人类开发的最复杂技术中选择作为改善下一个季度业绩的有效首选的正当性。

AI 不是魔法粉；它在与数据、上下文和推断有效性的关系中创造价值。希望和祈祷通常不是应对技术债务的有效策略。美国足球比赛的胜利来自于战略和执行，而不是“绝望传球”。

# 我的偏见

作为一名工程师，我关注的是部署的可行性、总拥有成本以及性价比。我们的客户信任我们成为新技术及其在客户组织中的应用的透明裁决者。部署风险需要与商业回报和投资回报率进行评估。仅仅因为一项技术流行就实施它，通常会引发许多警报，这也是有道理的。

我们对 LLMs 并没有（无论是伦理的还是经济的）问题——我们热爱技术。我们有给 [预训练模型做罗夏测试](https://medium.com/towards-data-science/rorschach-tests-for-deep-learning-image-classifiers-68c019fcc9a9) 的记录，构建了一个 [星座交易机器人](/silly-stock-trading-on-onepanel-io-gpus-51cde1772bd1)，甚至创建了一个带有迪斯科球的紧急聚会按钮。技术很酷。

然而，我必须坚持，如果你不知道你的问题是什么，技术将无法解决你的问题。更糟糕的是，与 LLMs 相关的工程挑战意味着每个典型项目风险现在都是交付的生存危机。

## 成功的样子

在我们经营[AI咨询公司](https://www.lemay.ai/)的这些年里，AI采纳成功的最大驱动因素是明确的成功衡量标准。这意味着：

+   业务背景已经通过KPI明确；

+   项目的需求已经确定；

+   项目的交付已经设定了目标。

质量管理系统，作为比较您的机器学习项目的自然尺度，需要在已建立的需求与验证/确认之间保持可追溯性，而新的管理技术则要求进行目标/关键结果（OKR）任务分配。围绕AI部署的期望也应根据可测量和客观的成功指标进行评估。

## 未提供理由的成本

尤其是对于LLM，近年来AI推动的项目经历了自愿项目和必要项目之间的推拉力量。自愿项目有趣，并在传说中的饮水机旁成为有趣的故事；必要项目则是完成任务的单色西装。你在市场不确定性时更愿意拥有哪一种？

> 优秀的AI，就像优秀的设计一样，应当是隐形的，而非焦点。

# 在LLM之前的众多选择

在考虑生成型AI之前，较老的基于变压器的模型和流程能够在不花费大量资金的情况下获得相同的商业结果。

大多数用例是知识库、历史分析和洞察生成，因此让我们看看可以找到哪些替代方法。

## 导航你的数据

在过去几年里，有两项技术使智能文本搜索变得轻而易举：句子嵌入和向量数据库。

自从最后几种词嵌入或子词嵌入技术以来，句子（或文档）嵌入确实成为了一个区分因素。对词序的意识（得益于位置编码）在细微差别中创造了更多的理解，并具有令人难以置信的复杂性导航。复杂的句子结构，甚至文档，可以可靠地向量化、聚类和比较。

向量数据库，许多是舒适的开源（例如[Vald](https://vald.vdaas.org/)和[Weaviate](https://weaviate.io/)），已经包含了开箱即用的自我优化和近似最近邻搜索。

在这个简单模型的商业背景下，应用数量令人眼花缭乱：你现在拥有一个迷你搜索引擎，可以检索与您最新提案最相似的历史RFP句子，甚至可以查找和组织合同所需的相关文件。

这种方法的优势在于，你能够避免LLM的幻觉：排名结果首先提供了上下文价值，意味着你不需要深入研究前几个答案之外。要么你面前直接有一个答案，要么就没有。尽管这不如提示性反应的舒缓步调令人放心，信息却极为精准，即使没有有价值的结果，也可以体现出团队内部状态的迹象。

*注：你甚至不需要VD来从相似搜索中获取价值。在多核系统上，带有余弦相似度的平面文件实际上足够快速，在企业环境中可用。如果你想亲自尝试，请将所有文档转换为Markdown，并在标题之间分割文本。恭喜，你现在有了你的小型搜索引擎。*

在LLMs之前（这有点名不副实；它似乎包含了生成文本的一切和所有），NLP世界充斥着各种应用和明确定义的解决方案模式。（查看[Papers With Code](https://paperswithcode.com/area/natural-language-processing)的NLP部分作为例子。）

构建一个句子或文档分类器仍然是组织数据的一种可靠方法；其中不乏强制组织认识其半空数据库的数据清理过程。

# 基础数据卫生到底发生了什么？

我无法再强调了：数据将解决你的AI问题；AI不会解决你的数据问题。

我曾[详细写过](https://medium.com/towards-data-science/how-does-artificial-intelligence-create-value-bec14c785b40)关于数据与AI的关系以及价值创造，AI不仅生成见解，还有助于清理布满蜘蛛网的数字档案。在商业环境中，由于流程和历史文化，产生惯性是很自然的。这种战争迷雾源于个人之间关系的巨大数量，以实现愿景和使命的成功。

然而，在这些过程中，预计会发现不完整的表单和缺失的报告。精力不应该用来跳过这些问题，而应该用来正确地归档它们。

![](../Images/7782eda6beb3468270470ac46d0d8344.png)

图片由[G.C.](https://pixabay.com/users/garten-gg-201217/)提供，来自[Pixabay](https://pixabay.com/)。

# 然而，LLMs 是未来。

关键的要点是，对于刚开始采用AI的公司而言，大多数情况下，先稳扎稳打比冒进更为明智。成功的项目交付能够节省资金，是一种全面稳固的策略。

LLMs有着超凡的能力，可以处理复杂的思想，并在几秒钟内干净地总结它们，但大多数新闻文章只引用最佳案例，而不是达到这一点所需的总体努力量。就像社交媒体一样，现实往往是欺骗性的。如果看起来毫不费力，那可能并非如此。

我并不是在反对企业级LLM；我在反对将企业级LLM作为首个AI项目。

# 您可能会喜欢的其他文章

+   [我们如何赢得第一个政府AI项目](https://medium.com/towards-data-science/how-we-won-our-first-government-ai-project-8c67e58c22f0)

+   [解读MLOps的商业考虑](https://medium.com/towards-data-science/interpreting-the-business-considerations-of-mlops-f32613c4bcb4)

+   [PyTorch与TensorFlow在基于Transformer的NLP应用中的比较](/pytorch-vs-tensorflow-for-transformer-based-nlp-applications-b851bdbf229a)

+   [用于批处理的MLOps：在GPU上运行Airflow](/mlops-for-batch-processing-running-airflow-on-gpus-dc94367869c6)

+   [数据集偏差：制度化歧视还是充分透明？](/dataset-biases-institutionalized-discrimination-or-adequate-transparency-ae4119e2a65c)

*如果您对本文或我们的AI咨询有额外的问题，请随时通过* [***LinkedIn***](https://www.linkedin.com/in/mnlemay/)*或* [***电子邮件***](mailto:matt@lemay.ai)*联系我们。*

-Matt.
