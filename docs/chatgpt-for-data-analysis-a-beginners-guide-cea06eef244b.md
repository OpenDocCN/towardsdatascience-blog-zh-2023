# ChatGPT 数据分析——初学者指南

> 原文：[`towardsdatascience.com/chatgpt-for-data-analysis-a-beginners-guide-cea06eef244b`](https://towardsdatascience.com/chatgpt-for-data-analysis-a-beginners-guide-cea06eef244b)

## 一个关于使用 ChatGPT 进行数据分析的完整教程。

[](https://natassha6789.medium.com/?source=post_page-----cea06eef244b--------------------------------)![Natassha Selvaraj](https://natassha6789.medium.com/?source=post_page-----cea06eef244b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----cea06eef244b--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----cea06eef244b--------------------------------) [Natassha Selvaraj](https://natassha6789.medium.com/?source=post_page-----cea06eef244b--------------------------------)

·发布于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----cea06eef244b--------------------------------) ·阅读时间 12 分钟·2023 年 12 月 23 日

--

![](img/73b3342ef34c0d6455741626f3b0bd02.png)

图片由[Myriam Jessier](https://unsplash.com/@mjessier?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)在[Unsplash](https://unsplash.com/photos/person-using-macbook-pro-on-black-table-eveI7MOcSmw?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)上拍摄。

数据分析是一个耗时的任务。

这需要对复杂的 Excel 公式有一定了解，并具备一些编程技能。

在分析数据时，我曾花费数小时调试代码，并在网上教程中寻找所需的结果。

直到最近！

ChatGPT 是数据分析领域的一个变革者。

即使你不了解 Excel，也不会写一行代码，ChatGPT 依然能将初级数据分析师的能力呈现在你面前。

你需要做的只是用简单的英语问 ChatGPT 一个问题。

模型将利用其自然语言处理能力来分析你的数据并解决问题。

在本教程中，我将向你展示如何将 ChatGPT 转变为你自己的个人数据分析师。

要跟随这个教程，你必须订阅[ChatGPT Plus](https://openai.com/blog/chatgpt-plus)。

欲观看视频版，请点击[这里](https://youtu.be/m6XS9Txvf-Q?si=SDGLBvA0YEZkQ60T)。

点击[这里](https://youtu.be/m6XS9Txvf-Q?si=SDGLBvA0YEZkQ60T)

我认识一位私人教练（我们叫他詹姆斯），他最近开始经营自己的健身公司。

由于预算限制，詹姆斯需要自己管理几乎所有业务方面的工作。

这包括但不限于公司的运营、财务、营销、客户获取和战略。

最近，詹姆斯开始使用 ChatGPT 来做**基于客户数据的业务决策**。

你看，尽管詹姆斯对健身如数家珍，但他没有处理电子表格和编程语言的经验。

过去，如果他想执行任何类型的数据分析任务，詹姆斯将有两个选择：

1.  雇佣那些擅长理解原始数据和分析数据的专业人员。

1.  自己花费无数小时学习这些技能。

无论如何，他都得花费大量时间和金钱。

现在，得益于 ChatGPT 及其引入的专用数据分析功能，詹姆斯可以在几分钟内分析数据，将他的业务提升到一个新水平。

在这篇文章中，我们将站在詹姆斯的角度，分析他健身公司的交易数据。我们将使用 ChatGPT 揭示数据驱动的建议，并探索如何利用这些洞察力来改善销售。

# 高级数据分析

ChatGPT 内置的数据分析插件可以分析你上传的文档，并在几秒钟内生成响应。

这个功能于 2023 年 7 月 6 日由 OpenAI 发布，目前仅对 ChatGPT Plus 订阅用户开放。

这个插件最近更名为“*高级数据分析*”，以更好地与其数据分析能力对齐，这是该功能的主要商业用途。

当你使用这个数据分析功能并向 ChatGPT 提供指令时，模型会将你的提示翻译成 Python 代码，运行这些代码，并给出所需的结果。

这相当于你自己编写代码来完成任务——一个需要人类经过多年学习和实践才能掌握的过程。

像詹姆斯这样的中小企业主将从这个插件中大大受益，因为他缺乏处理客户数据的技术技能。

这里是一个描述这个数据分析功能如何工作的视觉图：

![](img/7cbfabb98d91703e2171f56d31f463d4.png)

图片由作者提供

# 第一步：探索性数据分析

让我们使用这个插件来分析一些数据。

你可以在[这个](https://github.com/Natassha/Data-Analysis-with-ChatGPT)链接中找到本次分析的数据集——文件名为“James Transaction Dataset”。（*本次分析的数据集由作者创建*）。

本文档包含了过去一个月中与詹姆斯健身公司相关的交易数据。

在这个 Excel 文件中，你可以找到两个与詹姆斯健身公司相关的信息工作表——“电子商务销售”和“健身服务”

第一个工作表“电子商务销售”如下所示：

![](img/d71163a935832cdb2c600bdc8abc3bd2.png)

图片由作者提供

它包含了每个客户购买的产品、每项商品的价格、折扣百分比和总花费。

我们将首先关注这个工作表。

让我们通过点击界面左下角的回形针图标，将 Excel 文件上传到 ChatGPT（确保选择 GPT-4 而不是 GPT-3.5）：

![](img/63c1d35d0b7ec951ebdb27df6e0068ad.png)

图片由作者提供

文件上传后，只需让 ChatGPT 描述数据集中存在的列：

```py
Can you describe the columns present in this dataset?
```

仅需几秒钟，你应该会看到如下的回应：

![](img/0ba9d220f085676ab4d69be50d8f280f.png)

作者提供的图片

要查看 ChatGPT 生成的代码，你可以点击模型回应末尾的蓝色图标。

如果你知道 Python 编程，你可以复制并粘贴这段代码，自己运行。

当我有大量数据需要分析时，我倾向于这样做，因为 ChatGPT 一次只能处理 10 个文件。

在处理数据时，我通常发现自己需要分析数百个甚至数千个文件。在这种情况下，我只需复制 ChatGPT 生成的代码，并编写一个循环来遍历所有文件。

回到手头的任务，请注意，ChatGPT 只提供了第一个工作表的详细信息，尽管 Excel 文件中有两个工作表。

这是因为聊天机器人通常默认生成代码以读取第一个工作表，假设这是最相关的，除非另有说明。

如果你想让它分析第二个工作表，你需要明确说明这一点。

现在让我们询问 ChatGPT 有关客户从詹姆斯那里购买的独特产品的信息：

```py
What are the unique products available in this worksheet?
```

ChatGPT 分析工作表以识别所有列出的独特产品，并生成以下回应：

![](img/e48b2ab7c841dfcedeb14a9225a9259b.png)

作者提供的图片

我们立刻看到詹姆斯在他的电子商务商店中只销售了 5 种产品。

现在让我们询问 ChatGPT 交易的数量，以了解过去一个月人们从詹姆斯公司购买了多少次：

```py
How many transactions are captured in the dataset?
```

以下是 ChatGPT 对上述提示的回应：

![](img/53f88d7bbdf81cea3315f3ca71bdf3c1.png)

作者提供的图片

现在我们对数据集有了基本了解，让我们继续对其进行一些简单的计算。

# 第 2 步：数据汇总

让我们开始询问 ChatGPT 计算詹姆斯在数据集中所有交易的总销售额：

```py
What is the total sales amount from all the transactions present in this worksheet?
```

以下是 ChatGPT 对我们的提示的回应：

![](img/d958b2a0dce0fb4298f7d5624c289bb3.png)

作者提供的图片

请注意，即使没有告知 ChatGPT 我们希望分析的具体列，它也识别了正确的列，“总金额”，并计算了总和。

这展示了它在理解上下文和将普通文本需求转化为准确结果方面的卓越能力。

# 第 3 步：生成数据驱动的洞察

现在我们已经对数据集进行了基本计算，让我们更进一步，请 ChatGPT 提供可以帮助改善詹姆斯商业策略的洞察。

作为销售人员，了解哪个产品销售最好，以及产品价格和折扣是否对购买数量有影响是很重要的。

为了改善他的商业策略，詹姆斯想知道两件事：

1.  *他的哪些产品是畅销品？*

1.  *人们的购买决策是否受到价格和折扣的影响？*

让我们将这些问题合并成一个单一的提示：

```py
From the purchase details sheet:

Can you identify which product has been purchased the most in terms of quantity?
What is the average selling price of each product?
What is the average discount given for each product?
Based on the data, is there a relationship between the product’s price or discount amount and the number of purchases? Do lower prices mean more sales?
```

下面是 ChatGPT 对每部分提示的回应：

1.  ***你能找出哪个产品在数量上被购买得最多吗？***

![](img/5831eb58732c7b91d49b58eab29322fc.png)

作者提供的图片

ChatGPT 发现运动自行车在购买数量方面是最受欢迎的产品。

***2\. 每个产品的平均售价是多少？***

聊天机器人进行了一些计算，并列出了每个产品的平均价格：

![](img/5fae04ed4e84c0447b2eec5d6ccb1762.png)

作者提供的图片

仅凭这一点，我们可以看出这些产品的售价大致相同。

被购买最多的运动自行车并不是最便宜的。

这表明更低的价格不一定会转化为更多的销售。

至少对于这个项目来说，购买数量与产品价格之间没有简单的关系。

我想指出的是，这种分析需要中级的 Excel 或编程知识，因为用户需要对数据进行分组或创建某种数据透视表，然后汇总以计算平均值。

然而，通过 ChatGPT，我们在几秒钟内就获得了这个见解，无需编写任何代码或 Excel 公式。

***3\. 每个产品的平均折扣是多少？***

下面是 ChatGPT 列出的每个产品的平均折扣：

![](img/6ab7c3fe73cf36ff70aaf1479a540873.png)

作者提供的图片

再次强调，运动自行车的折扣低于其他产品的折扣。

这意味着更高的折扣并不总是会转化为更多的销售，尤其是对于这个产品来说。

为了进一步分析折扣与购买之间的关系，让我们继续查看 ChatGPT 对下一个问题的回应。

***4\. 根据数据，产品的平均售价或平均折扣金额与购买数量之间是否存在关系？更低的价格是否意味着更多的销售？***

![](img/9598c7a3b54e3ede53ad6fb9776c7247.png)

作者提供的图片

ChatGPT 告诉我们，要理解定价和折扣对销售的影响，我们需要进行**相关性分析**。

相关性分析是一种量化两个变量之间关系强度的技术。

我们不会深入探讨这项技术背后的机制，因为这超出了这篇博客的范围。

但从本质上讲，相关性可以告诉我们价格的上涨是否对应于更高的销售量，如果是的话，涨幅是多少。

这项统计技术通常使用专业软件或编程工具来执行，这意味着传统上，没有数据分析或统计背景的人可能会发现执行和解释相关性分析具有挑战性。

然而，借助 ChatGPT，你可以直接要求它为你进行分析，并用简单的术语解释结果，使得像这样的复杂分析任务对每个人都更易于接受。

如果你愿意，可以阅读 ChatGPT 对相关性分析的解释，但我将跳过这些，直接进入总结部分。

![](img/4af315ebc0f87f2e0ebb7913c6267eef.png)

作者提供的图片

ChatGPT 已经得出结论，根据相关性分析的结果，产品价格与销售折扣之间的关系并不强，这表明这些因素不是销售的主要驱动因素。

这意味着从 James 处购买健身器材的人并没有真正受到价格或折扣的影响。还有其他因素，比如产品兴趣，促使他们从他那里购买。

作为一个企业主或决策者，这种反馈是非常宝贵的，因为它可以帮助你重新思考定价策略，并激励客户进行更多购买。

# 第四步：数据可视化

现在，让我们继续使用 ChatGPT 创建一些图表。

我们将使用此文件中的第二个工作表来完成此任务：

![](img/b23d991c5a34d3a181369ca3dec34101.png)

作者提供的图片

这个工作表包含了所有 James 客户所参加的健身课程和培训课程的信息。

让我们先让 ChatGPT 描述一下这个表格中的列：

```py
Can you describe the columns present in the second worksheet?
```

![](img/b7f38805f551a984162b23a962ed4357.png)

作者提供的图片

ChatGPT 列出了数据集中存在的列，并给出了每列的描述。

现在，回想一下 James 并不是一个高度技术化的人。

他并不知道在这个数据集中具体该可视化什么。

他所知道的就是他想利用以前客户互动中发现的趋势来增加未来的销售。

他可以简单地告诉 ChatGPT 这些信息，然后让聊天机器人生成视觉创意：

```py
This worksheet comprises transaction information for my fitness company for the past year. 
What charts can I ask you to create if I'd like to learn more about my 
customer behavior?
```

![](img/d6e3aab7e85274c26f1d430ca1edce33.png)

作者提供的图片

ChatGPT 生成了各种各样的视觉创意，如月度销售趋势、平均购买价值和客户购买频率。

为了本教程的目的，我们选择两项内容进行可视化。

首先，让我们让 ChatGPT 可视化**时间上的销售趋势**，并基于这些趋势提出改善销售的建议：

```py
Can you visualize sales trends by month? Based on these trends, provide 
detailed insights on key trends and generate actionable recommendations on 
how to improve future sales.
```

针对上述提示，ChatGPT 生成了以下条形图来可视化销售信息：

![](img/e2f201c22a3419043b586f59d056c3ae.png)

作者提供的图片

一开始，我们就看到“总销售额”在四月、五月和十二月出现了峰值。

根据从可视化中获得的见解，ChatGPT 还生成了一些建议，说明 James 如何提高健身公司的未来销售额。

![](img/3b029917989f262cf7432fcf700c2b4d.png)

作者提供的图片

首先，聊天机器人建议 James 调查为什么四月和十二月的销售额较高。

如果这些波动可以归因于特价优惠或季节性促销，那么这表明我们应在不同的时间段内复制这一策略。

然后，它还建议詹姆斯在销售较低的月份推出特别活动或优惠。

我想退一步指出，优秀的数据分析师的角色是**用数据回答正确的问题**。

ChatGPT 完全依靠自身完成了这一任务，没有任何指导。

它能够读取数据集，发现需要回答的问题类型，甚至生成改进詹姆斯商业策略的建议。

我将留给你去查看聊天机器人生成的其余建议。

让我们继续下一个可视化。

我们现在将查看每项服务的销售数量，以了解詹姆斯的哪些服务在客户中最受欢迎：

```py
Can you visualize the amount of total sales by service? Based on these trends, provide 
detailed insights on key trends and generate actionable recommendations on 
how to improve future sales.
```

这是 ChatGPT 针对上述提示生成的图表：

![](img/610f2ac8dae33b56bb4fafd84327e76b.png)

图片来源：作者

看起来“核心力量训练”是最受欢迎的课程，其次是“健康指导”。

作为一个后续问题，值得研究一下季节性趋势等方面。例如，人们在夏季是否购买更多的特定服务？

这些见解可以用于制定个性化的目标策略，以提高客户获取和保留率。

我将留给你去提问并进一步探索数据集，但现在，让我们看看 ChatGPT 基于上述图表的建议：

![](img/c765faa19f39eec976c77ba68a2a813f.png)

图片来源：作者

ChatGPT 提出的第一个建议是**关注高需求服务**。由于客户似乎喜欢这些服务，聊天机器人建议詹姆斯为这些课程创建更多的时间段和不同的级别。

接下来，它建议詹姆斯**创建套餐**，将表现最佳的服务与不太受欢迎的服务结合起来。

还有一些额外的建议，例如创建客户调查、修订销售不佳的课程，并进行一些交叉推广。

虽然这些建议目前可能看起来过于通用，但当将多个数据集的见解结合起来时，ChatGPT 的建议将变得更为有力。

例如，如果詹姆斯上传了包含课程结构和时间安排的文档，以及客户调查数据集，ChatGPT 可以通过根据用户的可用性和兴趣对他们进行细分，从而生成量身定制的营销策略。

# 这里是 ChatGPT 在数据分析方面表现出色的原因

像 ChatGPT 这样的 LLM 最大的优势在于它们能够识别数据集中人类可能忽略的复杂关系。

由于 GPT 模型经过大量文本数据的训练，它们见过各种主题中无数的模式和上下文，从而使这些模型能够更好地识别新数据集中类似的模式。

此外，他们的技术熟练程度和语言能力使这些模型能够处理大规模的数据集，并解释人类语言的细微差别，从而有效地弥合原始数据与易读见解之间的差距。

我希望这篇文章能帮助你更好地理解 ChatGPT 的高级数据分析插件如何被用来处理数据。

如果你想了解更多关于使用 ChatGPT 进行数据分析和自动化相关任务的内容，可以阅读[我关于 ChatGPT Plus 的书](http://mng.bz/A8qe)。

*注意：本文包含附属链接。*
