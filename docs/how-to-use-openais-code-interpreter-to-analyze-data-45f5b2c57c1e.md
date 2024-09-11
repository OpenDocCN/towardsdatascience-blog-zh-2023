# 如何使用 OpenAI 的代码解释器进行数据分析

> 原文：[`towardsdatascience.com/how-to-use-openais-code-interpreter-to-analyze-data-45f5b2c57c1e?source=collection_archive---------4-----------------------#2023-07-17`](https://towardsdatascience.com/how-to-use-openais-code-interpreter-to-analyze-data-45f5b2c57c1e?source=collection_archive---------4-----------------------#2023-07-17)

## 利用 AI 探索 SaaS 收入倍数

[](https://michaeltauberg.medium.com/?source=post_page-----45f5b2c57c1e--------------------------------)![Michael Tauberg](https://michaeltauberg.medium.com/?source=post_page-----45f5b2c57c1e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----45f5b2c57c1e--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----45f5b2c57c1e--------------------------------) [Michael Tauberg](https://michaeltauberg.medium.com/?source=post_page-----45f5b2c57c1e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fa4d3e7fca7af&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-use-openais-code-interpreter-to-analyze-data-45f5b2c57c1e&user=Michael+Tauberg&userId=a4d3e7fca7af&source=post_page-a4d3e7fca7af----45f5b2c57c1e---------------------post_header-----------) 发布在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----45f5b2c57c1e--------------------------------) ·6 分钟阅读·2023 年 7 月 17 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F45f5b2c57c1e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-use-openais-code-interpreter-to-analyze-data-45f5b2c57c1e&user=Michael+Tauberg&userId=a4d3e7fca7af&source=-----45f5b2c57c1e---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F45f5b2c57c1e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-use-openais-code-interpreter-to-analyze-data-45f5b2c57c1e&source=-----45f5b2c57c1e---------------------bookmark_footer-----------)![](img/4d4890c28cee22c5291208b1f30cb9b9.png)

由 OpenAI Dall-E 工具生成的图片

我已经搁置了写新的数据分析文章有一段时间了。投资回报似乎过低。收集数据、清理数据和编写精细化的绘图代码需要很长时间，而且涉及大量繁琐的工作。

但是时代已经改变！我看到 OpenAI 最终发布了他们的[代码解释器工具](https://openai.com/blog/chatgpt-plugins)，这有可能让我摆脱枯燥重复的数据任务。让我们看看 AI 如何在我拖延了几个月的项目中帮助我——这是我故事的后续《SaaS 收入倍数、利率及 R 中的建模》。

# 获取数据

这是代码解释器尚无法提供帮助的领域。我不得不亲自出去，从以下来源收集数据：

+   [Macrotrends](https://www.macrotrends.net/stocks/charts/CRM/salesforce/price-sales)上的股票价格与销售比率

+   来自[圣路易斯联邦储备银行](https://fred.stlouisfed.org/series/fedfunds)页面的联邦基金利率

+   来自[财政部](https://home.treasury.gov/policy-issues/financing-the-government/interest-rate-statistics)网站的国债收益率数据

在没有 AI 帮助的情况下，我还编写了 python 代码来解析和组合这些数据源成一个 csv 文件。在这方面，我也应该使用 AI 工具。例如，ChatGPT 在生成用于数据处理的 python pandas 代码方面表现出色。
