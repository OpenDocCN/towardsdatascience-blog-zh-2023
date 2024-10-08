# PyCon 珍品：精选 PyCon DE 2023 中卓越讲座的精选集

> 原文：[`towardsdatascience.com/pycon-gems-a-curated-selection-of-exceptional-talks-from-pycon-de-2023-84461368df18`](https://towardsdatascience.com/pycon-gems-a-curated-selection-of-exceptional-talks-from-pycon-de-2023-84461368df18)

![](img/3b400df4d0a02d493062a5da3a1a77eb.png)

图片由作者提供。

## 单独的 LLM 不是未来。

[](https://medium.com/@mary.newhauser?source=post_page-----84461368df18--------------------------------)![Mary Newhauser](https://medium.com/@mary.newhauser?source=post_page-----84461368df18--------------------------------)[](https://towardsdatascience.com/?source=post_page-----84461368df18--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----84461368df18--------------------------------) [Mary Newhauser](https://medium.com/@mary.newhauser?source=post_page-----84461368df18--------------------------------)

·发布于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----84461368df18--------------------------------) ·8 分钟阅读·2023 年 5 月 2 日

--

在 2023 年 4 月中旬的柏林 PyCon DE 2023 大会上，空气中的兴奋和紧张气氛十分明显，会议室座无虚席，观众排队却被拒之门外。几个月前 ChatGPT 的发布引发了 AI 狂潮，激发了一场创新和合作的浪潮，致力于开发第一个完全开源的最先进指令跟随大型语言模型。在会议的三天里，开源世界宣布了[LLaVA](https://llava-vl.github.io/)、[StableLM](https://stability.ai/blog/stability-ai-launches-the-first-of-its-stablelm-suite-of-language-models)和[RedPajama](https://www.together.xyz/blog/redpajama)数据集的发布。

如果我用一句话总结[PyCon DE 2023](https://2023.pycon.de/)，那就是：“单独的 LLM 不是未来。”

总结[Label Studio](https://labelstud.io/)的 Erin Mikail Staples 和[Explosion](https://explosion.ai/)的 Ines Montani 的讲座，LLM 在与特定任务的数据一起使用时在下游任务中表现更好。此外，与会者中最普遍的讨论话题是 OpenAI 的[侵入性数据收集政策](https://www.wired.com/story/italy-ban-chatgpt-privacy-gdpr/)，这些政策禁止许多公司甚至整个行业将[ChatGPT](https://openai.com/blog/chatgpt)和[GPT-4](https://openai.com/product/gpt-4)用于商业目的。

> 单独的 LLM 不是未来。

本文的目的是概述我在今年 PyCon DE 上最喜欢的演讲。以下是我最喜欢的五个演讲的总结，并包含了程序描述、幻灯片和代码的链接（如果有的话）。大会上的所有演讲都被录制下来，并将在上传后完全公开访问。

## [从人类反馈中改进机器学习](https://github.com/heartexlabs/RLHF)

(Erin Mikail Sharples, [Label Studio](https://labelstud.io/))

训练于巨大数据集上的模型，例如 ChatGPT，会在下游任务中引入互联网规模的偏见。虽然提示工程，即迭代选择和设计提示以引发生成语言模型的期望响应，是一种流行的方法，但它只是适应了模型的已知局限性。幸运的是，还有更好的替代方案来解决 LLM 中的偏见问题。

在这次演讲中，[从人类反馈中进行强化学习](https://openai.com/research/learning-from-human-preferences)（RLHF）是明星话题。RLHF 是一种模型通过迭代从人类提供的反馈中学习，以提高模型性能的过程。RLHF 使你能更精确地控制 LLM，将模型输出与特定需求和使用场景对齐，同时减少 LLM 相关的偏见。Label Studio 是一个开源数据标注平台，具有用户友好的界面和 Python 客户端，允许你将 RLHF 集成到自己的机器学习工作流中。RLHF 不仅提高了下游任务的准确性，还增加了真实性，减少了毒性，成本极低。

我特别喜欢这次演讲，因为它揭示了 RLHF 的概念，这是一种在 ChatGPT 开发中发挥了关键作用的方法。此外，Label Studio 展示了 RLHF 是一个强大而实用的开源工具，可以轻松地添加到你当前的工作流中。

GitHub: [heartexlabs/RLHF](https://github.com/heartexlabs/RLHF)

Notebook: [RLHF_with_Custom_Datasets.ipynb](https://github.com/heartexlabs/RLHF/blob/master/tutorials/RLHF_with_Custom_Datasets.ipynb)

[](https://labelstud.io/?source=post_page-----84461368df18--------------------------------) [## 开源数据标注 | Label Studio

### 最灵活的数据注释工具。可以快速安装。构建自定义 UI 或使用预构建的标注模板……

labelstud.io](https://labelstud.io/?source=post_page-----84461368df18--------------------------------)

## [将 GPT-3 纳入实用的 NLP 工作流](https://pretalx.com/pyconde-pydata-berlin-2023/talk/77MWVW/)

(Ines Montani, [Explosion](https://explosion.ai/))

当我第一次尝试 ChatGPT 时，我真的怀疑开源 NLP 库如何与 OpenAI 的强大相比。Ines Montani 认为 LLM 是对现有机器学习工作流的补充，而不是替代。

Explosion 发布了一个配方库，使用户能够利用 OpenAI 模型的强大功能，并结合通过其企业注释工具 Prodigy 收集的人工反馈。这个流程是这样的：

1.  给 GPT-3.5（ChatGPT 的基础模型）设置任务。

1.  检索响应并将其视为零-shot 或少-shot 分类。

1.  让人工决策者标记响应是否准确。

1.  使用生成的注释来训练或评估你的任务特定模型。

如果我仍然需要说服自己 RLHF 是未来的最终方向，这次讲座做到了。在我上面提到的讲座中，Ines 展示了将人工反馈纳入 NLP 工作流比单独使用 LLMs 可以获得更好的下游任务性能。鉴于我自己使用 ChatGPT 的经验，我对这些说法毫不怀疑。虽然我发现它在执行广泛任务时表现良好，但我绝对不会毫无保留地信任 ChatGPT 或 GPT-4 处理需要专业知识的敏感任务。

[幻灯片](https://speakerdeck.com/inesmontani/incorporating-llms-into-practical-nlp-workflows)

GitHub: [explosion/prodigy-openai-recipes](https://github.com/explosion/prodigy-openai-recipes)

[## Explosion · spaCy、Prodigy 及其他 AI 和 NLP 开发工具的制造商](https://explosion.ai/?source=post_page-----84461368df18--------------------------------)

### Explosion 是一家专注于人工智能和自然语言处理开发工具的软件公司……

[Explosion.ai](https://explosion.ai/?source=post_page-----84461368df18--------------------------------)

## [文本风格迁移的方法：文本解毒案例](https://pretalx.com/pyconde-pydata-berlin-2023/talk/UECWHD/)

（达琳娜·德门捷娃，慕尼黑工业大学）

GitHub: [dardem/text_detoxification](https://github.com/dardem/text_detoxification)

发表文献：[ParaDetox: 利用平行数据进行解毒](https://aclanthology.org/2022.acl-long.469.pdf)

互联网的全球普及为个人提供了一个与不断增长的观众分享信息、想法和观点的平台。2020 年的[研究](https://www.nature.com/articles/s41599-020-00550-7)甚至发现，Facebook 的 Feed 推荐算法优先推送煽动性内容，因为这些内容通常会增加用户在平台上的参与度。尽管仇恨言论和有毒文本检测已成为许多研究的主题，但对实际解毒此类文本的工作却较少。

在这个讲座中，Daryna 介绍了 ParaDetox，这是一个新颖的管道和一组平行数据集，这些数据集基于平行的有毒和去毒化数据集进行训练，并使用文本风格转换（TST）来去毒化有毒文本。将其视为一个 seq2seq 文本生成任务，ParaDetox 的第一步是策划有毒文本和去毒化文本的数据集对。这些平行数据集随后用于训练一个语言模型，该模型可以自动去毒化文本输入。目前，ParaDetox 模型可以去毒化俄语和英语文本，及用于训练模型的平行数据集，均托管在 [HuggingFace Hub](https://huggingface.co/s-nlp)。

在生成文本模型（如 ChatGPT）被公众广泛使用之前，我们只需担心互联网中由人类产生的有害文本。然而，现在我们需要担心人类和机器生成的有毒、有害和仇恨的文本。ParaDetox 还使用了一种创新的方法，通过使用平行语料库来解决这个古老的问题。这种方法是另一个强大的例子，展示了如何利用 LLMs *和* 人类输入来创建有效的下游任务解决方案。

[](https://github.com/s-nlp/paradetox?source=post_page-----84461368df18--------------------------------) [## GitHub - s-nlp/paradetox: 论文“ParaDetox: 使用平行语料库进行去毒化”的数据和信息…

### 这个仓库包含了有关 Paradetox 数据集的信息——这是第一个用于去毒化任务的平行语料库……

[github.com](https://github.com/s-nlp/paradetox?source=post_page-----84461368df18--------------------------------)

## [在浏览器中使用 PyScript 实现可操作的机器学习](https://pretalx.com/pyconde-pydata-berlin-2023/talk/9Q38VT/)

（Valerio Maggio, [Anaconda](https://www.anaconda.com/))

如果你习惯于主要使用 Jupyter 笔记本进行端到端的数据科学项目，你可能会对部署第一个 Web 应用感到害怕。PyScript 旨在改变这种情况，通过提供一个简单的框架，让所有技能水平的编码者都能创建动态的 Python Web 应用。根据 Valerio 的说法，*“你可以在浏览器中编写 Python 代码，无需任何安装。”*

PyScript 基于 [Pyodide](https://github.com/pyodide/pyodide)，它在浏览器中提供了完整的 PyData Stack（减去一些不支持的模块）。与 PHP 不同，PyScript 是一种客户端技术，这意味着不需要服务器或任何形式的安装。它可以用于共享互动仪表板、可视化数据和创建客户端 Python Web 应用。

尽管 PyScript 应用程序可能不如使用 Streamlit 或 Gradio 开发的应用程序那么先进，但它们为数据科学家提供了一个用户友好的机会，让他们熟悉并增强对 Web 应用部署的信心。作为一个对非 Python 或 R 编程语言感到羞愧过敏的人，PyScript 让我着迷于“部署就像‘部署’一个 HTML 文件一样简单”。

[幻灯片](https://speakerdeck.com/leriomaggio/actionable-machine-learning-in-the-browser-with-pyscript)

GitHub: [pyscript/pyscript](https://github.com/pyscript/pyscript)

[](https://pyscript.net/?source=post_page-----84461368df18--------------------------------) [## Pyscript.net

### 难道不酷吗……在你的浏览器中运行 Python…… | print('现在你可以了！') | | | 点击这里查看示例……

pyscript.net](https://pyscript.net/?source=post_page-----84461368df18--------------------------------)

## [我们如何管理？现实中的数据团队](https://pretalx.com/pyconde-pydata-berlin-2023/talk/MBH7GB/)

(Noa Tamir)

尽管谈论管理显然不如谈论最新的 SOTA 机器学习模型、包或平台那样性感，但这场主题演讲受到了很好的关注，原因也很充分。虽然数据科学家这一角色已经存在了 15 年，但我们的大多数会议讨论的是过程和平台的管理，而忽视了对人员管理的关注。

在这次演讲中，Noa 解释了数据驱动工作的概率性，这意味着它很难管理，并且需要与非数据驱动工作不同的管理技术。新机器学习技术和科技的快速发展给数据团队的管理者带来了独特的挑战。随着这一领域的演变，数据驱动团队的角色也在变化。我们很难理解职位名称和描述中的细微差别，这使得招聘合适的人才更加困难，也可能对员工的工作满意度和职业发展产生负面影响。

优秀的管理者可以通过建立共同的理解和与现有及潜在员工沟通数据角色的具体定义来减轻这些后果。管理者还可以通过帮助员工发展成为专才或通才来支持他们，这两者都为数据团队带来了价值。

Noa 的演讲简洁地描述了数据团队面临的挑战，既诚实又具有验证性。他们为数据团队管理者提供了实际的建议，同时强调了我们工作在一个新的、快速变化的领域中，我们都还在学习。

[幻灯片](https://speakerdeck.com/noatamir/how-are-we-managing-data-teams-management-irl)

PyCon DE 2023 既充满了动力又令人不知所措，确实是一场出色的会议。除了挑选引人入胜且相关的演讲和工作坊外，组织者还很好地营造了一个安全包容的氛围。

今年，我很高兴地听到，尽管围绕 LLMs 有很多炒作，但人类在创造有价值的数据解决方案方面仍然发挥着重要作用。很难想象一年来人工智能的状态会是什么样，但我知道的一件事是，我将在 2024 年再次回到柏林参加 PyCon DE。

*附言：我会在会议记录发布后尽快附上访问链接。*

*如果你想跟上最新的数据科学趋势、技术和软件包，可以考虑成为 Medium 会员。你将获得对《Towards Data Science》等文章和博客的无限访问，并且你将支持我的写作。（我从每个会员订阅中赚取少量佣金）。*

[## 通过我的推荐链接加入 Medium - Mary Newhauser](https://medium.com/@mary.newhauser/membership?source=post_page-----84461368df18--------------------------------)

### 每月$5 即可访问无限 Medium 文章 🤗 你的会员费直接支持 Mary Newhauser 和…

[Medium 会员](https://medium.com/@mary.newhauser/membership?source=post_page-----84461368df18--------------------------------)

# 想要联系一下？

+   📖 在[Medium](https://medium.com/@mary.newhauser)上关注我

+   💌 [订阅](https://medium.com/@mary.newhauser/subscribe)以便在我发布新内容时收到邮件

+   🖌️ 查看我的生成式 AI[博客](https://www.gptechblog.com/)

+   🔗 看看我的[作品集](https://www.datascienceportfol.io/marynewhauser)

+   👩‍🏫 我还是一名数据科学[教练](https://www.datajump.co/)！

# 我还写过：

[## GPT-4 与 ChatGPT：训练、性能、能力和局限性的探讨](https://towardsdatascience.com/gpt-4-vs-chatgpt-an-exploration-of-training-performance-capabilities-and-limitations-35c990c133c5?source=post_page-----84461368df18--------------------------------)

### GPT-4 是一次改进，但要调整你的期望。

[## 最终的 Pandas 代码清理参考](https://towardsdatascience.com/gpt-4-vs-chatgpt-an-exploration-of-training-performance-capabilities-and-limitations-35c990c133c5?source=post_page-----84461368df18--------------------------------) [## 清洁文本数据的干净方法

### 清洁文本数据的干净方法。

[## 2023 年从数据分析师转型为数据科学家的路径](https://towardsdatascience.com/the-ultimate-reference-for-clean-pandas-code-413df676e63c?source=post_page-----84461368df18--------------------------------) [## 从数据分析师到数据科学家的转型

### 从数据分析师转型为数据科学家的技能和资源。

[## GPT-4 与 ChatGPT 的比较：训练、性能、能力和局限性探讨](https://towardsdatascience.com/making-the-jump-from-data-analyst-to-data-scientist-in-2023-74e2cf7fc139?source=post_page-----84461368df18--------------------------------)

# 参考资料

(1) H. Liu, C. Li, Q. Wu, Y. J. Lee，[视觉指令调优](https://llava-vl.github.io/) (2023)。

(2) Stability AI，[Stability AI 推出首款 StableLM 语言模型套件](https://stability.ai/blog/stability-ai-launches-the-first-of-its-stablelm-suite-of-language-models) (2023)。

(3) 一起，[RedPajama，一个创建领先开源模型的项目，首先通过重现 LLaMA 训练数据集的 1.2 万亿个标记开始](https://www.together.xyz/blog/redpajama) (2023)。

(4) PYSV E.V., [PyCon DE & PyData Berlin 2023](https://2023.pycon.de/)（2023）。

(5) M. Burgess，[ChatGPT 面临严重的隐私问题](https://www.wired.com/story/italy-ban-chatgpt-privacy-gdpr/)（2023）。

(6) OpenAI，[介绍 ChatGPT](https://openai.com/blog/chatgpt)（2023）。

(7) OpenAI，[GPT-4 是 OpenAI 最先进的系统，能够产生更安全、更有用的回应](https://openai.com/product/gpt-4)（2023）。

(8) OpenAI，[从人类偏好中学习](https://openai.com/research/learning-from-human-preferences)（2023）。

(9) Heartex Labs，[heartexlabs/RLHF](https://github.com/heartexlabs/RLHF)（2023）。

(10) Heartex Labs，[使用自定义数据集实现 RLHF](https://github.com/heartexlabs/RLHF/blob/master/tutorials/RLHF_with_Custom_Datasets.ipynb)（2023）。

(11) I. Montani，[将 LLMs 融入实际 NLP 工作流程](https://speakerdeck.com/inesmontani/incorporating-llms-into-practical-nlp-workflows)（2023）。

(12) Explosion，[explosion/prodigy-openai-recipes](https://github.com/explosion/prodigy-openai-recipes)（2023）。

(13) D. Dementieva，[dardem/text_detoxification](https://github.com/dardem/text_detoxification)（2023）。

(14) V. Logacheva1, D. Dementieva1, S. Ustyantsev, D. Moskovskiy, D. Dale, I. Krotova, N. Semenov, 和 A. Panchenko，[ParaDetox：使用平行数据进行去毒化](https://aclanthology.org/2022.acl-long.469.pdf)（2022）。

(15) L. Munn，[设计中的愤怒：有毒沟通与技术架构](https://www.nature.com/articles/s41599-020-00550-7)（2020）。

(16) V. Maggio，[使用 PyScript 在浏览器中进行可操作的机器学习](https://speakerdeck.com/leriomaggio/actionable-machine-learning-in-the-browser-with-pyscript)（2023）。

(17) PyScript，[pyscript/pyscript](https://github.com/pyscript/pyscript)（2023）。

(18) N. Tamir，[我们如何管理？数据团队管理现实](https://speakerdeck.com/noatamir/how-are-we-managing-data-teams-management-irl)（2023）。
