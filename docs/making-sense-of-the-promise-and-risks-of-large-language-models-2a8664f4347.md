# 理解大型语言模型的承诺（及风险）

> 原文：[https://towardsdatascience.com/making-sense-of-the-promise-and-risks-of-large-language-models-2a8664f4347?source=collection_archive---------7-----------------------#2023-04-27](https://towardsdatascience.com/making-sense-of-the-promise-and-risks-of-large-language-models-2a8664f4347?source=collection_archive---------7-----------------------#2023-04-27)

[](https://towardsdatascience.medium.com/?source=post_page-----2a8664f4347--------------------------------)[![TDS Editors](../Images/4b2d1beaf4f6dcf024ffa6535de3b794.png)](https://towardsdatascience.medium.com/?source=post_page-----2a8664f4347--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2a8664f4347--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----2a8664f4347--------------------------------) [TDS Editors](https://towardsdatascience.medium.com/?source=post_page-----2a8664f4347--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7e12c71dfa81&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmaking-sense-of-the-promise-and-risks-of-large-language-models-2a8664f4347&user=TDS+Editors&userId=7e12c71dfa81&source=post_page-7e12c71dfa81----2a8664f4347---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----2a8664f4347--------------------------------) ·发送至 [通讯](/newsletter?source=post_page-----2a8664f4347--------------------------------) ·4分钟阅读·2023年4月27日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F2a8664f4347&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmaking-sense-of-the-promise-and-risks-of-large-language-models-2a8664f4347&source=-----2a8664f4347---------------------bookmark_footer-----------)

尽管ChatGPT和类似工具在最近几个月中占据了我们大量的关注，但大型语言模型（LLMs）——这些支撑着我们无休止分享、截图，偶尔摇头的聊天输出的基础设施——却一直处于（相对）默默无闻的状态。

从某种程度上说，这很有道理：当你在笔记本电脑上观看节目时，你专注于情节和角色，而不是使那些移动像素成为可能的电网。如果你现在正在吃饼干（就像…一些TDS编辑，可能如此），你可能在思考你正在享受的风味和口感，而不是，比如说，关于小麦农民或可可豆的历史。

然而，作为数据专业人员，我们只有通过深入和细致地理解LLM才能受益。当然，这有一个职业角度，但也有一种更模糊的享受，即了解某些复杂而神秘的事物在幕后实际是如何运作的。为了给你提供帮助，我们汇集了一系列优秀的文章，探讨了LLM的过去、现在和未来——并讨论了它们的惊人能力和非同小可的局限性。请享用！

+   [**后ChatGPT时代的机器学习研究**](/closed-ai-models-make-bad-baselines-4bf6e47c9e6a)。一些最普遍的LLM（如OpenAI的GPT模型）的内部运作仍被企业严格保密。这对那些未来项目很可能会遇到专有壁垒的NLP研究人员意味着什么？[安娜·罗杰斯](https://medium.com/u/201bcd64e17?source=post_page-----2a8664f4347--------------------------------)提出了一个有力的观点：“那些不开放且不合理可重复的东西不能被视为必要的基线。”

+   [**评估最近在对话AI方面的进展的影响**](/have-machines-just-made-an-evolutionary-leap-to-speak-in-human-language-319237593aa4)。LLM的规模和力量使得人机互动在短短几个月内取得了巨大的飞跃。[加迪·辛格](https://medium.com/u/51de1f48d0b?source=post_page-----2a8664f4347--------------------------------)反思了我们如何走到这里——以及在机器开始以类人方式交流之前，还缺少什么。

+   [**欢迎来到一个新的伦理（和实际）困境**](/a-pathway-towards-responsible-ai-generated-content-6c915e8155f9)。LLM需要大量的训练数据，这已成为一个主要的风险因素。这些模型目前面临着对可能存在的偏见、私人或有害文本以及未经授权的版权材料的严密审查。在此期间，用户该如何做？[凌娟·吕](https://medium.com/u/ca2f89d83dfb?source=post_page-----2a8664f4347--------------------------------)的首篇TDS文章对我们在负责任地生产AI生成内容时应了解的风险和危险进行了深思熟虑的概述。

![](../Images/c79b0c68fc328f976d2f61d1e14f7021.png)

由[利比·佩纳](https://unsplash.com/@libby_penner?utm_source=medium&utm_medium=referral)拍摄，照片来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

+   [**LLM驱动工具的广阔视野**](/a-gentle-intro-to-chaining-llms-agents-and-utils-via-langchain-16cd385fca81)。仅几个月前，LangChain库还未引起大多数从业者的注意。现在不同了：它已经成为许多想利用LLMs构建新应用程序的人的首选资源。[Varshita Sher博士](https://medium.com/u/f8ca36def59?source=post_page-----2a8664f4347--------------------------------)的最新深度剖析是该库核心构建模块的有益、实用介绍。

+   [**使用LLMs识别漂移和检测异常**](/applying-large-language-models-to-tabular-data-to-identify-drift-54c9fa59255f)。随着使用ChatGPT生成生硬诗歌的新奇感逐渐消退，新的用例继续出现 —— 其中许多可能会简化数据科学工作流程。举例来说，[Aparna Dhinakaran](https://medium.com/u/f32f85889f3a?source=post_page-----2a8664f4347--------------------------------)，Jason Lopatecki和Christopher Brown的最新文章，概述了使用LLM嵌入进行异常和漂移检测的有前景方法。

+   [**额外阅读：深入一层**](/the-map-of-transformers-e14952226398)。如果语言模型（LLMs）使用户界面应用如ChatGPT成为可能，那么转换器神经网络是首先使LLMs成为可能的架构。要理解这个至关重要（且常常复杂）的主题，探索[Soran Ghaderi](https://medium.com/u/d2b75b0bb761?source=post_page-----2a8664f4347--------------------------------)详细的“地图”，回顾过去和现在的转换器研究。

准备继续你用饼干激励的阅读狂欢？以下是几篇你应该看看的杰出文章：

+   我们很高兴分享[Michael Bronstein](https://medium.com/u/7b1129ddd572?source=post_page-----2a8664f4347--------------------------------)和Emanuele Rossi的[最新研究，探索机器学习和博弈论的交集](/learning-network-games-29970aee44bb)。

+   三年后，她的病毒式帖子宣布了仪表板的消亡，[泰勒·布朗洛](https://medium.com/u/cdc63fa2a06e?source=post_page-----2a8664f4347--------------------------------)带来了对这些常受诟病但不可避免的工具更新和更加微妙的视角。

+   当前的商业格局如何影响数据团队？对于[Barr Moses](https://medium.com/u/2818bac48708?source=post_page-----2a8664f4347--------------------------------)，数据科学家缩小他们的工作与核心业务需求之间的差距至关重要[/the-next-big-crisis-for-data-teams-58ac2bd856e8]。

+   深度学习专家们，这篇文章适合你们：[Shashank Prasanna](https://medium.com/u/e0c596ca35b5?source=post_page-----2a8664f4347--------------------------------)的最新作品是对推动PyTorch 2.0的编译器技术的[便捷、耐心的入门指南](/how-pytorch-2-0-accelerates-deep-learning-with-operator-fusion-and-cpu-gpu-code-generation-35132a85bd26)。

感谢你本周花时间与我们相伴！如果你喜欢在 TDS 阅读的文章，可以考虑 [成为 Medium 会员](https://bit.ly/tds-membership) —— 如果你是符合条件国家的学生，不要错过 [享受会员大幅折扣的机会](https://blog.medium.com/new-student-discounts-cc10e964495b)。

直到下一个变量，

TDS 编辑部
