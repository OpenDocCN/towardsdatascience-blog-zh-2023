# 如何使用 OpenAI 的代码解释器来分析数据

> 原文：[`towardsdatascience.com/how-to-use-openais-code-interpreter-to-analyze-data-45f5b2c57c1e`](https://towardsdatascience.com/how-to-use-openais-code-interpreter-to-analyze-data-45f5b2c57c1e)

## 在 AI 的帮助下探索 SaaS 收入倍数

[](https://michaeltauberg.medium.com/?source=post_page-----45f5b2c57c1e--------------------------------)![Michael Tauberg](https://michaeltauberg.medium.com/?source=post_page-----45f5b2c57c1e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----45f5b2c57c1e--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----45f5b2c57c1e--------------------------------) [Michael Tauberg](https://michaeltauberg.medium.com/?source=post_page-----45f5b2c57c1e--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----45f5b2c57c1e--------------------------------) ·阅读时间 6 分钟·2023 年 7 月 17 日

--

![](img/4d4890c28cee22c5291208b1f30cb9b9.png)

由 OpenAI Dall-E 工具生成的照片

我已经暂时搁置了新的数据分析文章。投资回报率似乎太低了。收集数据、清洗数据和编写精细的绘图代码需要很长时间，并且涉及大量枯燥的工作。

但时代变了！我看到 OpenAI 终于发布了他们的[代码解释器工具](https://openai.com/blog/chatgpt-plugins)，这可能解放了我，使我不再需要做枯燥和重复的数据任务。让我们看看 AI 如何在一个我拖延了几个月的项目上帮助我——对我的故事《SaaS 收入倍数、利率与 R 建模》的后续跟进

# 获取数据

这是代码解释器目前无法帮助的一个领域。我不得不自己出门收集数据，来源如下：

+   [Macrotrends](https://www.macrotrends.net/stocks/charts/CRM/salesforce/price-sales)上的股票价格与销售数据

+   [圣路易斯联邦储备银行](https://fred.stlouisfed.org/series/fedfunds)页面上的联邦基金利率

+   [财政部](https://home.treasury.gov/policy-issues/financing-the-government/interest-rate-statistics)网站上的国债收益率数据

没有 AI 的帮助，我还编写了 Python 代码来解析和合并这些来源到一个 CSV 文件中。在这里，我也应该使用 AI 工具。例如，ChatGPT 在生成用于数据处理的 Python pandas 代码方面表现出色。

无论如何，这里是我们将输入到机器人朋友中的数据快照。来自 10 多年和近 100 家公司中的 2000 多个价格/销售数据点。

[typhon/saasrevenuewinterestrates | 工作区 | data.world](https://data.world/typhon/saasrevenuewinterestrates/workspace/file?filename=min_df.csv)

![](img/9c2ee1236c4cd751754d4dc07a77f79b.png)

# 启用解释器

如果你像我一样为 ChatGPT 付费，那么你应该可以访问代码解释器。只需前往设置启用此 Beta 功能。

![](img/472ab96d462b46fee99d5edb82ccd732.png)

# 加载数据

启用解释器后，我们现在可以上传我们的 CSV 文件，并询问解释器关于它的问题。还有一个新的“展示工作”标签，可以让我们查看解释器生成的 Python 代码。例如：

![](img/046934a3b39f7e48c9af3e1f724a83ff.png)![](img/d9db28d6fff26e7aac490c51654745df.png)

解释器能够弄清楚数据集是什么，并正确解释列名。特别是，`3 Mo`、`6 Mo`、`1 Yr`、`2 Yr`、`3 Yr`、`5 Yr`、`7 Yr`、`10 Yr`、`20 Yr`、`30 Yr` 列确实是美国国债收益率数据，而 FedFundsRate 是联邦储备银行支付给银行的利率。它还弄清楚了 growth_rolling_avg 是一个增长率指标（在我们公司的收入情况下）。

# 基本分析

你可以像和 ChatGPT 交流一样与代码解释器对话。在这里，我问了探索性数据分析的第一个问题之一，即是否存在任何有趣的相关性？

![](img/e6cea46eb0b714239be9edaa5fe060c1.png)

解释器不仅生成了良好的代码，它提供的描述也大体上是正确的。

![](img/aef1dc03d217827d7f32632805cf7435.png)

代码解释器甚至识别了上述相关性的可能***原因***。例如，较新上市的公司通常具有更高的 P/S 比率，而更高的 M2 货币供应量会导致更高的股票价格。

工具没有对价格/销售与价格本身之间的最奇怪的相关性进行推测。我的猜测是，这反映了 P/S 比率逐渐上升的长期趋势。Adobe 是一个很好的例子，突出了这一点。

![](img/26532ce131bae50fb9dfde412b8b804b.png)

# 绘图

让我们用新的 AI 朋友来测试一个理论，即最近上市的公司拥有更高的价格/销售比率。

![](img/ece4869e6c7da2f2b27724663c897ad1.png)

哎呀，我们遇到了一些问题。我可以理解为什么这个东西还在 Beta 阶段。

![](img/637be665414ff4a644c6602a66ead93b.png)

幸运的是，在开始新的会话并重新上传后，解释器表现完美。

![](img/29e77956cd612751d77d8f796c7044a0.png)

我们得到了一张很好的图形，显示了最极端的 P/S 数字出现在 2021 年，而且这些数字主要涉及最近上市的公司。

我们还可以看到生成此图的代码：

![](img/c35024ab39419b43be5e1af95880dcf0.png)

我个人认为，使用“色调”来着色 ipo_year 点有点令人困惑，因为年份是一个离散变量。让我们看看是否能说服解释器做得更好。

尽管有我的拼写错误，但它完成了工作。

![](img/676aa63f4a3e7bfa96931a964e7f6bf4.png)

接下来，让我们看看代码解释器能否在我不直接询问的情况下弄清楚我想要什么。它能决定哪种图表最佳吗？它能找出轴上使用的正确变量吗？

![](img/09ee0a65dd2598d1189f5a723253d17a.png)

太棒了！正是我想要的。唯一缺少的是趋势线，工具在我要求后轻松添加了。

![](img/683d7a3f1ff98ffc17a09554d3bbfd83.png)

正如预期的那样，一旦 30 年期收益率超过 2.5%，数据中的疯狂 P/S 倍数就消失了。

# 高级分析

到目前为止，代码解释器成功地完成了初级数据分析师的工作。现在让我们看看它能否处理真正的数据科学家工作。

![](img/c11b046f9b651c95dcc739e6a1b0836e.png)

幸运的是我的拼写错误无法阻止代码解释器

好的，这变得有趣了。代码解释器可以帮助我们生成一个不错的线性回归模型。当然，我们必须对机器保持警惕。它在我上一个提示上过度索引，只使用了 30 年期收益率作为输入参数。正如预期的那样，这样一个简单（单变量）的模型误差范围很大。

![](img/1030af0c6b7233cde13b5f031c02e39c.png)

不过经过一点劝说，它扩展了线性模型以包含这些特征（与价格销售比高度相关）

![](img/00cf03c8e5bcef3c702a918c61d05e0e.png)

现在我们的结果看起来好多了。

![](img/414f9e8b9e0e743da6292d6083265f75.png)

当然，我们在使用 ps_prev（上一季度的价格销售比）时有点作弊。凭借这些信息、增长率和货币条件的数据，代码解释器的模型可以在大约 2 个单位内预测价格/销售比。不算差，但也不算好。

公平地说，这并不是代码解释器的错。数据集太小且波动太大，无法进行准确的 P/S 预测。我尝试过的更复杂模型效果也不比这好，但代码解释器得出这个结论的速度快得多。

# 最终思考

OpenAI 的新代码解释器工具确实很棒。最重要的是它***快速***。你可以探索数据，进行简单分析，并利用自动生成的代码进行更精细的分析。总的来说，我强烈推荐这个工具来加快你的工作进度。

然而，使用 AI 工具时特别需要谨慎。它们不一定理解你的目标或你所工作的领域。它们会犯错误。在数据分析项目中，总是容易自找麻烦。代码解释器使得这一点比以往任何时候都容易。特别是初级分析师不应完全信任工具的输出，应该尝试直接与生成的代码互动。

尽管如此，经验丰富的数据分析师应能通过这些 AI 工具显著提高工作效率。随着我们摆脱繁琐的工作，也许我们会有时间解决新问题，*更深入*地探究我们周围的数字世界。
