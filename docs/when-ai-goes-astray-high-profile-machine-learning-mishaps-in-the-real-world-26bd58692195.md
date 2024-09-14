# 当 AI 走错路时：现实世界中的高-profile 机器学习失误

> 原文：[`towardsdatascience.com/when-ai-goes-astray-high-profile-machine-learning-mishaps-in-the-real-world-26bd58692195`](https://towardsdatascience.com/when-ai-goes-astray-high-profile-machine-learning-mishaps-in-the-real-world-26bd58692195)

## 一次对引起全球关注的臭名昭著的机器学习失误和失败的巡礼

[](https://kennethleungty.medium.com/?source=post_page-----26bd58692195--------------------------------)![Kenneth Leung](https://kennethleungty.medium.com/?source=post_page-----26bd58692195--------------------------------)[](https://towardsdatascience.com/?source=post_page-----26bd58692195--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----26bd58692195--------------------------------) [Kenneth Leung](https://kennethleungty.medium.com/?source=post_page-----26bd58692195--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----26bd58692195--------------------------------) ·阅读时间 6 分钟·2023 年 8 月 19 日

--

![](img/09242ff40162d09e9b281e8a1f92d26b.png)

照片由 [NEOM](https://unsplash.com/@neom?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，[Unsplash](https://unsplash.com/photos/AdkJ-LgpTrE?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

人工智能 (AI) 和机器学习的变革潜力经常成为新闻头条，报告了它在医疗、金融等各个领域的积极影响。

然而，没有任何技术是免疫于失误的。虽然成功的故事描绘了机器学习的奇妙能力，但同样重要的是强调其陷阱，以便理解其影响的全貌。

在本文中，我们探讨了许多高-profile 机器学习失误，以便为未来更有信息化的实施提供借鉴。

# 目录

特别是，我们将查看以下每个类别中的一个值得注意的案例：

> ***(1)*** *经典机器学习****(2)*** *计算机视觉****(3)*** *预测****(4)*** *图像生成****(5)*** *自然语言处理****(6)*** *推荐系统*

***可以在以下 GitHub 仓库 Failed-ML 中找到高-profile 机器学习失误的全面汇编：***

[](https://github.com/kennethleungty/Failed-ML?source=post_page-----26bd58692195--------------------------------) [## GitHub — kennethleungty/Failed-ML: 高-profile 现实世界案例汇编

github.com](https://github.com/kennethleungty/Failed-ML?source=post_page-----26bd58692195--------------------------------)

# *(1) 经典机器学习*

## 标题

**亚马逊 AI 招聘系统：** 亚马逊的人工智能驱动的自动招聘系统在出现对女性候选人的歧视证据后被取消。

## 细节

亚马逊开发了一款由人工智能驱动的招聘工具，以从十年的简历中识别顶尖候选人。然而，由于科技行业主要由男性主导，该系统对女性申请者表现出了偏见。

例如，它开始降级包含“女性”一词的简历或来自两所女性专用学院的毕业生的简历，同时偏爱某些术语（例如“执行”），这些术语在男性简历中出现频率更高。

亚马逊尝试纠正这些偏见，但在消除歧视性倾向方面面临挑战。因此，他们在 2017 年初终止了该项目，强调该系统从未用于实际候选人评估。

## 关键课程

训练数据中的偏见可能会在机器学习模型中延续，并导致人工智能系统中出现意想不到和歧视性的结果，这强调了多样化和具有代表性的数据集的重要性。

## [报告链接](https://finance.yahoo.com/news/amazon-reportedly-killed-ai-recruitment-100042269.html?source=post_page-----26bd58692195--------------------------------)

[## 亚马逊据报取消了一个 AI 招聘系统](https://finance.yahoo.com/news/amazon-reportedly-killed-ai-recruitment-100042269.html?source=post_page-----26bd58692195--------------------------------)

[详细信息](https://finance.yahoo.com/news/amazon-reportedly-killed-ai-recruitment-100042269.html?source=post_page-----26bd58692195--------------------------------)

# (2) 计算机视觉

## 标题

**谷歌糖尿病视网膜病检测 AI：** 该视网膜扫描工具在现实环境中的表现远不如在控制实验中的表现，问题包括由于扫描质量差而拒绝的扫描和因间歇性互联网连接导致的上传图像到云端的延迟。

## 细节

谷歌健康在泰国的 11 个诊所安装了一套深度学习系统，用于识别糖尿病患者眼部疾病的迹象。在实验室测试中，该系统能够以超过 90%的准确率从眼部扫描中识别糖尿病视网膜病，并在不到 10 分钟内给出结果。

然而，当实际部署时，由于图像质量差，AI 系统常常无法给出结果，拒绝了超过五分之一的图像。

当护士认为被拒绝的扫描没有显示出疾病迹象，或当他们不得不重拍和编辑 AI 系统拒绝的图像时，他们感到非常沮丧。几家诊所的糟糕互联网连接也导致了将图像上传到云端处理的延迟。

## 关键课程

重要的是将 AI 工具量身定制以适应现实环境的具体条件和限制，包括图像质量和离线可用性等因素。

## 参考资料

[## 谷歌的医疗 AI 在实验室中的准确性非常高，但现实生活却完全不同。](https://www.technologyreview.com/2020/04/27/1000658/google-medical-ai-accurate-lab-real-life-clinic-covid-diabetes-retina-disease/?source=post_page-----26bd58692195--------------------------------)

[www.technologyreview.com](https://www.technologyreview.com/2020/04/27/1000658/google-medical-ai-accurate-lab-real-life-clinic-covid-diabetes-retina-disease/?source=post_page-----26bd58692195--------------------------------)

# (3) 预测

## 标题

**Zillow 即时购买（iBuying）算法**：由于其机器学习模型对房产估价的价格过高估计，Zillow 在房屋翻转业务中遭受了重大损失。

## 详细信息

像 Zillow 这样的房地产公司一直在使用 iBuying 商业模式，即直接从卖家那里购买房屋，然后在进行一些小修小补后重新挂牌出售。

Zillow 购买任何房屋的第一个步骤是“Zestimate”——基于一个在数百万次房屋估价上训练的模型的机器学习辅助的未来市场价值估算，并依赖于如房屋状况和邮政编码等数据。

然而，Zillow 的系统误判了未来的房价，导致他们向房主提出了许多高于市场的报价，特别是在 COVID-19 大流行引起的房地产波动期间。

这种误判最终导致了 Zillow 的即时购买操作的关闭，预计损失达到 3.8 亿美元。

## 关键教训

持续的模型监控、评估和再训练至关重要，以确保捕捉到新事件引起的数据漂移（导致测试数据分布的变化），以便进行最新的预测。

## 参考

[## 为什么 Zillow 无法使算法房价定价成功](https://www.wired.com/story/zillow-ibuyer-real-estate/?source=post_page-----26bd58692195--------------------------------)

[www.wired.com](https://www.wired.com/story/zillow-ibuyer-real-estate/?source=post_page-----26bd58692195--------------------------------)

# (4) 图像生成

## 标题

**Stable Diffusion（文本到图像模型）**：Stable Diffusion 在生成的数千张与职位名称和犯罪相关的图像中表现出种族和性别偏见。

## 详细信息

彭博社对 5000 多张使用 Stable Diffusion 生成的图像进行分析，发现了显著的种族和性别偏见。

使用 Fitzpatrick 皮肤分级标准进行分类，他们发现高薪职位的图像主要展示了肤色较浅的主体，而肤色较深则与“快餐工人”等低薪职业相关联。

同样，性别分析显示，每描绘一名女性的图像几乎会有三张图像描绘男性，大多数高薪职位的代表都是肤色较浅的男性。

Stable Diffusion 从 LAION-5B 获取数据，这是全球最大的公开可用图像-文本数据集，来自各种在线网站，包括暴力、仇恨符号等的描绘。

## 关键课程

审计用于机器学习的数据至关重要。例如，如果描绘放大刻板印象的图像通过增强训练数据回流到未来的模型中，下一代文本到图像模型可能会变得更加有偏见，形成偏见的滚雪球效应。

## 参考

[`www.bloomberg.com/graphics/2023-generative-ai-bias`](https://www.bloomberg.com/graphics/2023-generative-ai-bias/)

# (5) 自然语言处理

## 标题

**ChatGPT 引用：** 一名律师使用流行的聊天机器人 ChatGPT 来补充他的研究发现，但提供了完全虚构的不存在的过去案例。

## 细节

当一名名为史蒂文·施瓦茨的律师使用 ChatGPT 来协助准备与航空公司过失有关的伤害诉讼的法院文件时，事情很快出了岔子。

他提交的简报中包含了几项所谓相关的法院裁决引用，但航空公司的律师和主审法官都无法找到这些引用的裁决。

从事法律工作三十年的施瓦茨承认使用 ChatGPT 进行法律研究，但对其可能产生虚假内容一无所知。他甚至要求 ChatGPT 验证案件的有效性，而 ChatGPT 错误地确认了这些案件的存在。

## 关键课程

仅仅依赖像 ChatGPT 这样的生成模型的输出而不进行人工验证，可能导致重大不准确，突显了人类监督（即“人类在环”系统）和与可信来源交叉验证的必要性。

## 参考

[](https://www.nytimes.com/2023/05/27/nyregion/avianca-airline-lawsuit-chatgpt.html?source=post_page-----26bd58692195--------------------------------) [## 当你的律师使用 ChatGPT 时会发生什么

www.nytimes.com](https://www.nytimes.com/2023/05/27/nyregion/avianca-airline-lawsuit-chatgpt.html?source=post_page-----26bd58692195--------------------------------)

# (6) 推荐系统

## 标题

**IBM Watson 在肿瘤学中的应用：** IBM 的 Watson 据称为癌症患者提供了大量不安全和不正确的治疗建议。

## 细节

曾被视为癌症研究的未来，IBM 的 Watson 超级计算机据报导已对癌症治疗提出了不安全的建议。

一个显著的例子是，它建议给一位重度出血的癌症患者用一种可能加重出血的药物。然而，这一建议被指出是理论性的，并未应用于实际患者。

根本问题源于输入 Watson 的数据的性质。IBM 的研究人员一直在输入假设性或“合成”案例，以使系统能够在更广泛的患者场景上进行训练。

然而，这也意味着许多建议主要基于提供数据的少数医生的治疗偏好，而不是从真实患者案例中得出的见解。

## 关键课程

训练数据的质量和代表性在机器学习中至关重要，尤其是在医疗等关键应用中，以避免偏见和潜在的有害结果。

## 参考资料

[](https://www.theverge.com/2018/7/26/17619382/ibms-watson-cancer-ai-healthcare-science?source=post_page-----26bd58692195--------------------------------) [## IBM 的 Watson 提供了不安全的癌症治疗建议

[www.theverge.com](https://www.theverge.com/2018/7/26/17619382/ibms-watson-cancer-ai-healthcare-science?source=post_page-----26bd58692195--------------------------------)

# 总结

虽然机器学习带来了许多好处，但我们必须记住它并不完美，正如本文中的众多实际错误所示。我们必须从这些错误中学习，以便未来更好地利用 AI 和机器学习。

查看[**这个 GitHub 仓库**](https://github.com/kennethleungty/Failed-ML)，获取完整的机器学习错误汇编。

# 在你离开之前

欢迎**加入我的数据科学探索之旅！** 关注我的[Medium](https://kennethleungty.medium.com/)页面，并访问我的[GitHub](https://github.com/kennethleungty)，以获取更多引人入胜和实用的内容。与此同时，祝你在探索机器学习中的成功与失误时玩得愉快！

[](/running-llama-2-on-cpu-inference-for-document-q-a-3d636037a3d8?source=post_page-----26bd58692195--------------------------------) ## 在本地 CPU 上运行 Llama 2 进行文档问答

### 清晰解释如何使用 LLama 2、C Transformers、GGML 在 CPU 上运行量化开源 LLM 应用程序……

[towardsdatascience.com [](/micro-macro-weighted-averages-of-f1-score-clearly-explained-b603420b292f?source=post_page-----26bd58692195--------------------------------) ## 微平均、宏平均和加权平均 F1 分数，清晰解释

### 理解 F1 分数的微平均、宏平均和加权平均背后的概念……

[towardsdatascience.com [](https://medium.com/geekculture/how-to-create-clickable-table-of-contents-for-your-medium-posts-e81e22f83142?source=post_page-----26bd58692195--------------------------------) [## 为你的 Medium 帖子创建可点击的目录

### 创建动态目录的简单技巧，允许读者轻松滚动导航

[medium.com](https://medium.com/geekculture/how-to-create-clickable-table-of-contents-for-your-medium-posts-e81e22f83142?source=post_page-----26bd58692195--------------------------------)
