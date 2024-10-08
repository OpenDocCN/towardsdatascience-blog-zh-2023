# ChatGPT 的电力消耗

> 原文：[`towardsdatascience.com/chatgpts-electricity-consumption-7873483feac4`](https://towardsdatascience.com/chatgpts-electricity-consumption-7873483feac4)

## 观点

## ChatGPT 在 2023 年 1 月的电力消耗可能相当于 175,000 人的用电量。

[](https://kaspergroesludvigsen.medium.com/?source=post_page-----7873483feac4--------------------------------)![Kasper Groes Albin Ludvigsen](https://kaspergroesludvigsen.medium.com/?source=post_page-----7873483feac4--------------------------------)[](https://towardsdatascience.com/?source=post_page-----7873483feac4--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----7873483feac4--------------------------------) [Kasper Groes Albin Ludvigsen](https://kaspergroesludvigsen.medium.com/?source=post_page-----7873483feac4--------------------------------)

·发布于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----7873483feac4--------------------------------) ·阅读时长 7 分钟·2023 年 3 月 1 日

--

![](img/ae36dcab4194bc416080d1f1c5eae69b.png)

图片由[NASA](https://unsplash.com/@nasa?utm_source=medium&utm_medium=referral)提供，发布在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

我最近写了一篇文章，其中我估算了 ChatGPT 的每日碳足迹约为 24 kgCO2e。由于当时关于 ChatGPT 用户群体的信息很少，我的估算是基于 ChatGPT 服务运行在 16 个 A100 GPU 上的假设。根据最近的报告估算，ChatGPT 在 1 月份有 5.9 亿次访问[1]，这很可能意味着 ChatGPT 需要更多的 GPU 来服务其用户。

[](https://kaspergroesludvigsen.medium.com/membership?source=post_page-----7873483feac4--------------------------------) [## 使用我的推荐链接加入 Medium - Kasper Groes Albin Ludvigsen

### 作为 Medium 会员，您的一部分会员费将用于支持您阅读的作者，您可以全面访问每个故事…

[kaspergroesludvigsen.medium.com](https://kaspergroesludvigsen.medium.com/membership?source=post_page-----7873483feac4--------------------------------)

由此也自然推测，ChatGPT 可能部署在多个地理位置。这使得估算 ChatGPT 的总每日碳足迹非常困难，因为我们需要确切知道每个地区运行了多少个 GPU，以将每个地区的电力碳强度纳入碳足迹估算中。

另一方面，估算 ChatGPT 的电力消耗原则上更简单，因为我们无需知道 ChatGPT 运行的地理区域。下面我将解释如何估算 ChatGPT 的能源消耗，并特别给出对 ChatGPT 在 2023 年 1 月的电力使用的估算。范围限于 2023 年 1 月，因为我们有该月份的 ChatGPT 流量估算。

[](https://kaspergroesludvigsen.medium.com/chatgpts-electricity-consumption-pt-ii-225e7e43f22b?source=post_page-----7873483feac4--------------------------------) [## ChatGPT 的电力消耗，第二部分

### ChatGPT 的成本估算表明，ChatGPT 的月电力消耗在 1.1M 到 23M KWh 之间

kaspergroesludvigsen.medium.com](https://kaspergroesludvigsen.medium.com/chatgpts-electricity-consumption-pt-ii-225e7e43f22b?source=post_page-----7873483feac4--------------------------------) [](/the-carbon-footprint-of-chatgpt-66932314627d?source=post_page-----7873483feac4--------------------------------) ## ChatGPT 的碳足迹

### 本文试图估算名为 ChatGPT 的流行 OpenAI 聊天机器人所产生的碳足迹

towardsdatascience.com

# 估算 ChatGPT 的电力消耗

下面是如何估算 ChatGPT 的电力消耗：

1.  估算 ChatGPT 每次查询的电力消耗

1.  估算给定时间段内 ChatGPT 的总查询数

1.  将这两个数值相乘

为了计算 ChatGPT 电力消耗可能的范围，我将为每个第 1 和第 2 点定义 3 个不同的值。让我们首先来看第 1 点。

## 步骤 1：ChatGPT 每次查询的电力消耗

BLOOM 是一个与 ChatGPT 基础语言模型 GPT-3 大小相似的语言模型。

BLOOM 曾在 18 天的时间里消耗了 914 KWh 的电力，这期间它在 16 台 Nvidia A100 40 GB GPU 上运行，每小时处理平均 558 个请求，总共 230,768 个查询。查询未批处理。914 KWh 计算了 CPU、RAM 和 GPU 的使用[2]。

因此，BLOOM 在该期间的电力消耗为每次查询 0.00396 KWh。

让我们考虑 BLOOM 的电力使用可能作为 ChatGPT 电力使用的良好代理。

首先，BLOOM 有 176b 个参数，而 GPT-3（ChatGPT 的基础模型）有 175b 个参数，因此在其他条件相同的情况下，它们的能耗应相当相似。

现在，有几个因素表明 ChatGPT 每次查询的电力消耗可能低于 BLOOM。例如，ChatGPT 每小时接收到的请求远多于 BLOOM，这可能导致每次查询的能耗降低。下图（图 1）绘制了 BLOOM 的电力使用情况与请求数量的关系，我们可以看到，即使在没有请求的情况下，BLOOM 的空闲电力消耗为 0.3 KWh，而在 1000 个请求时，其电力消耗也只是 200 个请求时的 5 倍不到。

![](img/f09db435a80c6b832d8e23621a535fa3.png)

图 1: “GCP 实例使用的能源数量（在 y 轴）与实例在 10 分钟间隔内接收到的请求数量（在 x 轴）之间的关系。可以看到，即使在这一时间段内实例接收到零请求（图的左下方），能源消耗仍约为 0.28 kWh。”（图和文字由 [2] 第 6 页提供）

尽管测量 BLOOM 电力消耗的论文作者表示*“由于推理请求是不可预测的，因此使用批处理和填充等技术优化 GPU 内存 […] 是不可能的”*（[2] 第 5 页），我们可能预期 ChatGPT 会经历一个足够高的请求速率，使得批处理成为可行。我猜测，查询的批处理会导致每次查询的能耗降低。

一个可能增加 ChatGPT 每次查询能耗的因素是 ChatGPT 平均生成的词数更多，但我们没有任何信息可以让我们对这一点进行估算。

为了谨慎起见，我将在以下估算中假设 ChatGPT 的电力消耗最多与 BLOOM 相同，因此我将根据以下假设对 ChatGPT 的电力消耗进行估算：

1.  ChatGPT 每次查询的电力消耗与 BLOOM 相同，即 0.00396 KWh

1.  ChatGPT 每次查询的电力消耗是 BLOOM 的 75%，即 0.00297 KWh

1.  ChatGPT 每次查询的电力消耗是 BLOOM 的 50%，即 0.00198 KWh

[如何估算和减少机器学习模型的碳足迹](https://towardsdatascience.com/how-to-estimate-and-reduce-the-carbon-footprint-of-machine-learning-models-49f24510880?source=post_page-----7873483feac4--------------------------------)

### 有两种简单的方法来估算机器学习模型的碳足迹，以及 17 个减少碳足迹的想法

[如何估算和减少机器学习模型的碳足迹](https://towardsdatascience.com/how-to-estimate-and-reduce-the-carbon-footprint-of-machine-learning-models-49f24510880?source=post_page-----7873483feac4--------------------------------)

## 第 2 步：发送到 ChatGPT 的查询次数

现在我们已经有了一些关于 ChatGPT 每次查询的电力消耗的估算，我们需要估算 ChatGPT 在一月份接收到的查询次数。

估计 ChatGPT 在 2023 年 1 月有 590 万次访问 [1]。每次访问产生了多少查询？我们不知道，但我们可以做一些假设。我预计每次访问至少会产生 1 个查询，这是最低假设。然后，我将做一个每次访问 5 次和 10 次查询的场景。从一些人的经验来看，他们在日常任务中广泛使用 ChatGPT，所以我认为这些数字是合理的。

1.  每次访问的查询数 = 1，因此总查询数 = 590 百万

1.  每次访问的查询数 = 5，因此总查询数 = 29.5 亿

1.  每次访问的查询数 = 10，因此总查询数 = 59 亿

## # 3：将电力消耗估算与查询次数结合

上述内容概述了有关 ChatGPT 每次查询电力使用的三种不同假设和有关总查询次数的三种不同假设。

这给出了 9 个独特的假设集，我将其称为“场景”。

下表显示了每个场景的估算电力消耗。

表格显示，ChatGPT 在 2023 年 1 月的电力消耗估计在 1,168,200 KWh 和 23,364,000 KWh 之间。

[这是](https://docs.google.com/spreadsheets/d/166ZVGLeQwry7mwtY2aQ0TQhKeRI2e4XNB_edCWi-HPA/edit?usp=sharing)一个在线电子表格，如果你想检查我的计算。

![](img/21c652434c155f4f92b88c957d40c56e.png)

表 1：ChatGPT 在 2023 年 1 月在不同能源效率和总查询场景下的估算电力消耗。表格由 Kasper Groes Albin Ludvigsen 编制，基于 Luccioni 等人，2022 [2]

# ChatGPT 可能使用的电力超过了 175,000 名丹麦人

让我们将表 1 中的数字放在背景中来看。

平均丹麦人的年电力消耗为 1,600 KWh [3]。

如果将年度消耗平均分布在全年，那么每位丹麦人在一月份大约使用了 133.33 KWh 的电力。

因此，当我们将 ChatGPT 估算的电力消耗除以 133.3 时，可以看到 ChatGPT 的估算消耗相当于 8,762 至 175,234 名丹麦人每月的电力消耗。

从另一个角度来看，你可以用 1,168,200 KWh 的电力（1,168,200 KWh / (7 / 1000) / 24 / 365）运行一个标准的 7w 灯泡约 19,050 年。

训练 ChatGPT 的基础语言模型 GPT-3 估计消耗了 1,287 MWh [4]。

因此，在 2023 年 1 月 ChatGPT 电力消耗的估算范围的低端，训练 GPT-3 和运行 ChatGPT 一个月所需的能量大致相同。在范围的高端，运行 ChatGPT 所需的能量是训练所需的 18 倍。

# 结论

虽然处理单个查询需要的资源微不足道，但当像 ChatGPT 这样的产品扩展时，所需的资源（例如电力）会累计成显著的数量。搜索引擎 Bing 现在已经整合了 ChatGPT [5]，这可能会显著增加 ChatGPT 需要处理的查询数量，从而也增加电力消耗。在我看来，这需要讨论是否将大型机器学习模型整合到现有产品中，能够将用户体验提升到一个值得大规模能源消耗的水平。

此外，除非这里提出的估计值过高，否则本文显示，运行一个基于机器学习的产品在生产中可能比训练模型要消耗更多的能源——特别是当产品有较长的生命周期和/或大量用户时。在讨论人工智能的环境影响以及思考如何减少机器学习模型的生命周期碳足迹时，应考虑到这一点。

我必须强调的是，本文提出的估计值存在很大不确定性，因为关于 ChatGPT 如何运行的信息公开得非常有限。在最理想的情况下，OpenAI 和 Microsoft 以及其他科技公司会公开其产品的能耗和碳足迹。但在我们等待这一点发生的同时，我希望我的估计能够引发讨论，并激励他人提出更好的估计。

附注：ChatGPT 电力消耗的 saga 继续在 [这里](https://kaspergroesludvigsen.medium.com/chatgpts-electricity-consumption-pt-ii-225e7e43f22b)。

就这些！希望你喜欢这篇文章 🤞

我很想听听你的意见：

1) 你对分析的看法以及

2) 你是否认为 ChatGPT 的电力消耗是值得的。

关注以获取更多与可持续数据科学相关的帖子。我还会写关于时间序列预测的内容，例如 这里 或 这里。

同样，确保查看 [丹麦数据科学社区](https://ddsc.io/) 的 [可持续数据科学](https://github.com/Dansk-Data-Science-Community/sustainable-data-science) 指南，以获取更多关于可持续数据科学和机器学习环境影响的资源。

也可以随时在 [LinkedIn](https://www.linkedin.com/in/kaspergroesludvigsen/) 上与我联系。

# 参考文献

[1] [`www.theguardian.com/technology/2023/feb/02/chatgpt-100-million-users-open-ai-fastest-growing-app`](https://www.theguardian.com/technology/2023/feb/02/chatgpt-100-million-users-open-ai-fastest-growing-app)

[2] [`arxiv.org/pdf/2211.02001.pdf`](https://arxiv.org/pdf/2211.02001.pdf)

[3] [`www.gasel.dk/vaerd-at-vide/bruger-du-mere-el-end-gennemsnittet#:~:text=If%C3%B8lge%20Dansk%20Energi%20er%20et,dog%20v%C3%A6re%20en%20smule%20mindre`](https://www.gasel.dk/vaerd-at-vide/bruger-du-mere-el-end-gennemsnittet#:~:text=If%C3%B8lge%20Dansk%20Energi%20er%20et,dog%20v%C3%A6re%20en%20smule%20mindre)

[4] [`arxiv.org/ftp/arxiv/papers/2204/2204.05149.pdf`](https://arxiv.org/ftp/arxiv/papers/2204/2204.05149.pdf)

[5] [`www.digitaltrends.com/computing/how-to-use-microsoft-chatgpt-bing-edge/`](https://www.digitaltrends.com/computing/how-to-use-microsoft-chatgpt-bing-edge/)
