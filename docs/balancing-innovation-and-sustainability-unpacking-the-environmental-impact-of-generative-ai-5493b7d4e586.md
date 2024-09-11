# 平衡创新与可持续性：揭示生成AI的环境影响

> 原文：[https://towardsdatascience.com/balancing-innovation-and-sustainability-unpacking-the-environmental-impact-of-generative-ai-5493b7d4e586?source=collection_archive---------7-----------------------#2023-10-04](https://towardsdatascience.com/balancing-innovation-and-sustainability-unpacking-the-environmental-impact-of-generative-ai-5493b7d4e586?source=collection_archive---------7-----------------------#2023-10-04)

## 语言模型碳足迹的深入探讨及可持续解决方案。

[](https://medium.com/@charlet.jeremie?source=post_page-----5493b7d4e586--------------------------------)[![Jeremie Charlet](../Images/38a553e40b2ccb320398a433d31a05f0.png)](https://medium.com/@charlet.jeremie?source=post_page-----5493b7d4e586--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5493b7d4e586--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----5493b7d4e586--------------------------------) [Jeremie Charlet](https://medium.com/@charlet.jeremie?source=post_page-----5493b7d4e586--------------------------------)

·

[跟进](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F11a086eb7d52&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbalancing-innovation-and-sustainability-unpacking-the-environmental-impact-of-generative-ai-5493b7d4e586&user=Jeremie+Charlet&userId=11a086eb7d52&source=post_page-11a086eb7d52----5493b7d4e586---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----5493b7d4e586--------------------------------) ·7 分钟阅读·2023年10月4日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F5493b7d4e586&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbalancing-innovation-and-sustainability-unpacking-the-environmental-impact-of-generative-ai-5493b7d4e586&user=Jeremie+Charlet&userId=11a086eb7d52&source=-----5493b7d4e586---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5493b7d4e586&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbalancing-innovation-and-sustainability-unpacking-the-environmental-impact-of-generative-ai-5493b7d4e586&source=-----5493b7d4e586---------------------bookmark_footer-----------)![](../Images/16f6adcc50ce220f7bc5d55d9f3b1a8a.png)

由[Pixabay](https://pixabay.com//?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=7850192)的[Kohji Asakawa](https://pixabay.com/users/deltaworks-37465/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=7850192)贡献。

法国协会[Data for Good](https://dataforgood.fr/)发布了一份[白皮书](https://dataforgood.fr/iagenerative)，探讨了生成型人工智能的社会和环境问题。我特别关注语言模型的环境影响，因为这一点比伦理方面的讨论少一些。以下是我的主要收获：

# TL;DR

+   **背景**：**全球领导者承诺到** [**2050年将我们的排放减少** 至远低于2°C](https://unfccc.int/process-and-meetings/the-paris-agreement)。这意味着我们需要在2020年至2030年间减少43%的排放（为了限制升温至1.5°C，[请参见IPCC报告的C.1.1节](https://www.ipcc.ch/report/ar6/wg3/downloads/report/IPCC_AR6_WGIII_FullReport.pdf)）。然而，在数字领域，排放量并未减少，反而从[2](https://en.reset.org/our-digital-carbon-footprint-environmental-impact-living-life-online-12272019/)增加至[7](https://theshiftproject.org/en/article/unsustainable-use-online-video/)%每年。

+   **GPT-3的训练产生了多达2200吨的二氧化碳当量**——相当于1600次从巴黎到纽约的往返航班。

+   **拥有1300万用户的ChatGPT每月使用量相当于10000吨的二氧化碳**。如果每个人今天都使用它，它将占法国/英国个人年度碳足迹的0.1%，到2050年则占目标碳足迹的0.5%。

+   **ChatGPT+的影响，依赖于GPT-4，可能是其10到100倍**，这将使我们的年度碳足迹增加10%……或者达到目标碳足迹的50%。

+   **有许多方法可以减少使用这些模型的影响**：合理使用它们，并选择具有 proven 环境绩效的云服务。

![](../Images/e0873e974e9b1c532f9212d9b1a40394.png)

英国公民的年度碳足迹

# 背景

要评估任何事物的环境影响，我们可以估算其碳足迹：它衡量个人、组织或产品直接和间接造成的温室气体排放总量，表示为二氧化碳（CO2e）当量。

从总体上来看，英国的平均年度碳足迹约为每人8-13吨，[法国](https://2tonnes-compagnie.gitbook.io/v1-guide-technique-de-2tonnes/calcul-de-lempreinte-carbone-moyenne-par-2tonnes/empreinte-carbone-moyenne-francaise)为21吨，美国为21吨，全球为6吨](https://www.nature.com/articles/s41893-022-00955-z)。我将考虑10吨作为我们当前的碳足迹。

一些例子（附来源）：

+   从巴黎到纽约的[往返航班](https://dataforgood.fr/iagenerative/)相当于1.4吨的二氧化碳当量。

    [航空占英国公民年度平均碳足迹的 10%](https://www.carbonindependent.org/23.html)（英国每年航空排放总量为 8000 万吨 CO2，大约等于英国总温室气体（GHG）年度排放 7700 万吨的 10%）。

+   每天观看 3 小时视频的年总碳排放为：

    (0.4kWh * 2) * 3h * 0.26kgCO2eq/kWh * 365 days = 每年 228kgCO2eq

    或大约为英国公民年度平均碳足迹的 2%。

    根据以下假设和我的简单计算：

    [30 分钟在线视频的能耗](https://theshiftproject.org/wp-content/uploads/2020/06/2020-06_Did-TSP-overestimate-the-carbon-footprint-of-online-video_EN.pdf) = 0.4 kW

    [每人每天视频流量](https://senalnews.com/en/research/time-spent-streaming-increases-in-uk-germany-france-spain-and-italy) = 3 小时

    [英国的碳强度](https://ourworldindata.org/grapher/carbon-intensity-electricity) = 0.26kgCO2eq/kWh

为了将全球温度升高控制在 2 度以下，[我们](https://en.wikipedia.org/wiki/Under2_Coalition) [应该](https://bonpote.com/en/how-can-you-estimate-your-carbon-footprint/) [目标](https://www.ecologie.gouv.fr/sites/default/files/Projet%20strategie%20nationale%20bas%20carbone.pdf) 到 2050 年将我们的全球碳足迹减少到每人 2 吨。

减少 80% 或 90% 的排放还有很多工作要做，[对数字服务的持续需求增长](https://post.parliament.uk/research-briefings/post-pn-0677/) 超过了效率提升，这无济于事。生成性 AI 如何适应这个方程式，我们能做些什么来将我们的数字进步与环境目标对齐？

# 训练影响：

![](../Images/dc4e9f041f1901c0bdf191d677d9e6b0.png)

归功于 Victor Freitas 的 [Unsplash](https://unsplash.com/photos/hOuJYX2K5DA)

在 [训练](https://www.xilinx.com/applications/ai-inference/difference-between-deep-learning-training-and-inference.html) 阶段，我们向语言模型提供一些精选数据，以便它们从中学习并能够回答我们的请求。

研究分析了两个大型语言模型：

1\. 开源 Bloom

2\. OpenAI 的专有 GPT-3

主要发现：

- Bloom 的碳足迹：最初估计为 30 吨，经过综合分析后修订为 120 吨。

- GPT-3 的碳足迹：推算为 2200 吨，相当于 1600 次巴黎——纽约的往返航班。

一个常见的观点是，这些模型的高训练成本是可以接受的，因为它们被许多用户广泛使用。

# 推理影响

![](../Images/b383f2eecf8f5dd8e1f32428fac79585.png)

归功于 [Fitsum Admasu](https://unsplash.com/@fitmasu) 的 [Unsplash](https://unsplash.com/photos/oGv9xIl7DkY)

[机器学习中的推理](https://www.xilinx.com/applications/ai-inference/difference-between-deep-learning-training-and-inference.html)是指我们使用训练好的模型对实时数据进行预测。我们现在正在观察运行ChatGPT的影响。

基于ChatGPT有1300万活跃用户平均每人发起15个请求的假设，每月碳足迹为1万吨CO2。

对我而言，关键的经验教训是，这比训练的影响要大得多。

对于一个用户来说，年度碳足迹的增加量是12个月 * 10000吨 / 1300万用户 = 每年每用户9公斤CO2eq，相当于当前平均年度碳足迹的0.1%或我们目标足迹的0.5%。

但如果那个人使用带有GPT-4的ChatGPT+呢？GPT-4的足迹是GPT-3的10到100倍。这种足迹相当于100公斤到1吨额外的CO2e，占法国公民碳足迹的高达10%——如果你尽力减少它，甚至会是两倍。如果我们考虑到2050年的目标足迹，这就代表了50%！

这太糟糕了。

如果有一天，你生活中的每一个应用互动都向语言模型发起请求，会怎么样？这是个令人恐惧的想法。

好消息是，广泛使用gpt4 API的成本非常高，我们无法让用户每天发起15个请求，除非他们愿意支付每月100美元以上的订阅费用，而我正在开发的产品（一个冥想个人助手）的目标市场并不愿意支付这个费用。这不仅仅是小型企业无法承担的：谷歌和微软也无法承受用GPT-4这种规模的模型来替代他们的搜索引擎，这将使他们的查询成本增加100倍。

# 推荐

推荐如下：

+   **保持清醒**：用ChatGPT-4替代整个IT项目可能很诱人，但我们可以质疑项目的实际效用、使用语言模型的真实需求，并将其使用限制在真正需要的具体场景中。例如，尽可能使用比GPT-4小得多的模型。在使用（它）ChatGPT+之前要三思。

+   **优化训练和使用**：在这一点上，技术众多，持续发展，数据科学家应当已经在使用它们……以降低成本。这些技术主要包括减少基础设施的使用，从而减少电力消耗，进而减少碳排放。本质上，我们只有在必要时才训练模型；如果确实需要训练，我们会规划以避免资源浪费。并且我们使用符合需求的最小模型。

+   **根据能源的碳足迹选择最佳国家来托管你的服务器**。这里有法国的骄傲：我们主要依赖核能的碳足迹是美国的7倍低。然而，如果你们都开始在这里托管你们的语言模型，我们可能会从亲爱的邻国进口煤炭能源🔥。

+   **根据环境表现选择顶级云服务**（这些数据有时是公开的；否则有工具来测量/估算，比如[https://mlco2.github.io/impact/](https://mlco2.github.io/impact/)）——偏好那些使用服务器时间较长的云服务（然而超级大规模提供商通常不会将硬件使用超过4年），以及共享程度高的数据中心。

# 你是否有兴趣减少你的影响？

无论你是个人还是公司，资源和专家都可以帮助你走上可持续发展的道路。

**在个人层面**：

- 如果你想要**评估你的碳足迹**，网上有许多工具。个人而言，测量我的碳足迹让我大开眼界，促使我探索如何做出积极的影响。如果你在英国，请查看[https://footprint.wwf.org.uk/](https://footprint.wwf.org.uk/)

- 要**快速了解气候变化背后的基础科学**，可以参加3小时的课程：[https://climatefresk.org/](https://climatefresk.org/)

- 要**调查你可以采取的行动并估算它能减少多少足迹**，可以参加另一个3小时的研讨会：[https://en.2tonnes.org/](https://en.2tonnes.org/)

**在企业层面**：

许多公司正在探索这些问题，以下是他们可以做的事情：

+   **教育员工**（如上面建议的研讨会），

+   **执行审计** **并测量他们的碳足迹**，

+   **制定策略以改善其ESG**（环境、社会和公司治理）**评分**。

我得知这项出色的研究是通过一些我最近认识的[优秀](https://www.linkedin.com/in/marine-loisel-58b14818/) [人士](https://www.linkedin.com/in/benoitdurand350ppm/)了解到的，他们来自[Toovalu](https://toovalu.com/en)和[Wavestone](https://www.wavestone.com/en/capabilities/sustainability/)。请查看他们的工作！

如果你发现我的估算有错误或想要添加你的观点，请评论并分享你是否觉得这篇文章有趣。

🙌 **感谢**你抽出时间阅读这篇文章，希望对你有所启发！非常感谢Thibaut、Léo、Benoit和Diane对这篇文章的宝贵反馈和补充 🙏。

如果你想了解生成式AI和负责任的机器学习的最新动态，请关注我在[Linkedin](https://www.linkedin.com/in/jeremiecharlet/) 👋。
