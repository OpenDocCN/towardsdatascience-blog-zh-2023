# 利用 LLM 和 LangChain 实现供应链分析——一个由 GPT 提供支持的控制塔

> 原文：[https://towardsdatascience.com/leveraging-llms-with-langchain-for-supply-chain-analytics-a-control-tower-powered-by-gpt-21e19b33b5f0?source=collection_archive---------2-----------------------#2023-11-17](https://towardsdatascience.com/leveraging-llms-with-langchain-for-supply-chain-analytics-a-control-tower-powered-by-gpt-21e19b33b5f0?source=collection_archive---------2-----------------------#2023-11-17)

## 使用 LangChain SQL 代理构建一个自动化的供应链控制塔，并连接到运输管理系统的数据库。

[](https://s-saci95.medium.com/?source=post_page-----21e19b33b5f0--------------------------------)[![Samir Saci](../Images/722d1f56a3308f6527d82b5ab97064ec.png)](https://s-saci95.medium.com/?source=post_page-----21e19b33b5f0--------------------------------)[](https://towardsdatascience.com/?source=post_page-----21e19b33b5f0--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----21e19b33b5f0--------------------------------) [Samir Saci](https://s-saci95.medium.com/?source=post_page-----21e19b33b5f0--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fbb0f26d52754&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fleveraging-llms-with-langchain-for-supply-chain-analytics-a-control-tower-powered-by-gpt-21e19b33b5f0&user=Samir+Saci&userId=bb0f26d52754&source=post_page-bb0f26d52754----21e19b33b5f0---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----21e19b33b5f0--------------------------------) ·14 分钟阅读·2023年11月17日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F21e19b33b5f0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fleveraging-llms-with-langchain-for-supply-chain-analytics-a-control-tower-powered-by-gpt-21e19b33b5f0&user=Samir+Saci&userId=bb0f26d52754&source=-----21e19b33b5f0---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F21e19b33b5f0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fleveraging-llms-with-langchain-for-supply-chain-analytics-a-control-tower-powered-by-gpt-21e19b33b5f0&source=-----21e19b33b5f0---------------------bookmark_footer-----------)[![](../Images/c94e36d304d175293e5adb44a4c9e8b9.png)](https://samirsaci.com)

（图片来自作者）

供应链控制塔可以定义为一个集中式解决方案，提供可视化和监控功能，以高效管理端到端的供应链操作。

这个分析工具使供应链部门能够实时跟踪、理解和解决关键问题。

[![](../Images/729712dd7cf80890089d3c09c588b9fd.png)](https://samirsaci.com\)

使用Python构建的供应链控制塔 [[Link](/automated-supply-chain-control-tower-with-python-17dbf93a18d0)] —（图像由作者提供）

在[之前的文章](/automated-supply-chain-control-tower-with-python-17dbf93a18d0)中，我介绍了一种用于分析控制塔的解决方案（使用Python开发），能够自主生成事件报告。

然而，这种方法需要在**提供的指标和报告范围**上进行修订。

> 我们如何提升这个模型以提供更好的用户体验？

这一观察使我发现了像OpenAI的GPT这样的**大型语言模型（LLMs）**在根据每个用户请求提供量身定制分析方面的巨大潜力。

[![](../Images/54db742e8c2480e63562605a2614f757.png)](https://samirsaci.com)

解决方案的高级概念 —（图像由作者提供）

在这篇文章中，我将分享我掌握**Langchain与OpenAI的GPT模型**以及构建**终极供应链控制塔**的经历。

💌 免费新文章直接送到您的邮箱：[Newsletter](https://www.samirsaci.com/#/portal/signup)

📘 供应链分析的完整指南：[Analytics Cheat Sheet](https://bit.ly/supply-chain-cheat)

```py
SUMMARY
**I. LLMs with LangChain for Supply Chain Analytics**
An exploration of how LangChain and LLMs can revolutionize analytics 
in supply chain management.
**1\. Scenario: Distribution Process of a Fashion Retailer**
The complex distribution operations in a global fashion retail setting.
**2\. Setting the Stage for Experimentation**
Introduce the experimental setup to test a LangChain SQL agent. 
**3\. Experimental Protocol: Starting Simple**
Start with a straightforward query to assess the agent's basic capabilities.
**II. Experiments and Key Insights**
**1\. Test 1**
Simple Prompt without Template
2\. Test 2
Prompt Template with a Context
**3\. Test 3**
Prompt Template with an Improved Context
**4\. Test 4**
Advanced Analysis with an Improved Context
**5\. Final Test**
Create a Chain to Write an Email Report
**III. LLMs Shaping the Future of Supply Chain**
**1\. What about GPTs? "The Supply Chain Analyst" Agent**
I tried the new feature "GPTs" of ChatGPT with "The Supply Chain Analyt" Agent
**2\. A Simple 'Proof-of-Concept'**
We can use an agent to track shipments with TMS data
**3\. Continuing the Prototype Development**
Challenge the agent with complex analyses and more tables.
**4\. Exploring Broader Applications in Supply Chain**
LLMs can boost the user experience for Digital Twins, Network Optimization,
Business Intelligence, ESG Reporting and many other applications
```

# 使用LangChain进行供应链分析的LLMs

## 场景：时尚零售商的配送过程

想象一下你是一个拥有全球门店网络的国际服装集团的数据科学家。

你的项目涉及协助**配送规划经理**监控门店的补货过程。

[![](../Images/1b66dac9a66e28f98f25b6262eae4664.png)](https://samirsaci.com)

供应链网络 —（图像由作者提供）

她领导着一个管理全球门店库存的计划员团队。

这个过程很简单，当库存水平达到最低水平时。

+   计划员在[ERP](https://www.youtube.com/shorts/v0_R8P6MLQ0)中创建**补货订单**，包含如商品**数量**和[**请求的交货日期**](https://www.youtube.com/watch?v=qhLqu6M7lcA)的信息。

+   仓库的运营团队[**准备订单**](https://www.youtube.com/watch?v=XejgbF2m_8g)进行发货。

+   运输计划员[**组织送货到**](https://www.youtube.com/watch?v=PYkN24PMKd8)商店。

关键**绩效指标是**按时交货的订单百分比。

[![](../Images/896d17e04e0030cfdc25ff9e0652a024.png)](https://samirsaci.com)

配送链过程通过时间戳跟踪 —（图像由作者提供）

从订单创建到商店交货，数据库中记录了多个时间戳和布尔标志。

+   订单传输时间记录在**‘Transmission’** *如果这是在截止时间之后，‘Transmission_OnTime’会被设置为0。*

+   卡车装载时间记录在**‘Loading’**中。

    *如果这是在截止时间之后，‘Loading_OnTime’会被设置为0。*

+   卡车到达机场的时间记录在**‘Airport_Arrival’**中。

    *如果这是在截止时间之后，‘Airport_OnTime’会被设置为0。*

+   飞机降落在机场时，记录在‘**Airport_Arrival**’中

    *如果这是在截止时间之后，‘Airport_OnTime’将设置为0。*

+   卡车抵达城市由‘**City_Arrival**’记录

    *如果这是在商店开放时间之外，‘Store_Open’将设置为0。*

最重要的时间戳是‘**Delivery_Time**’。它与‘**Expected_Delivery_Time**’进行比较，以设置‘**On_Time_Delivery**’的值。

我在[之前的文章](/automated-supply-chain-control-tower-with-python-17dbf93a18d0)中提出的初始解决方案是一套视觉和报告，用于回答操作性问题。

> 问题 1：有多少批次运输延误了？

[![](../Images/0736a73e51fc917151e71604175a9abb.png)](https://samirsaci.com)

（作者提供的图像）

> 问题 2：目前运输在哪里？

[![](../Images/e902a1b97f3ccc06e5e16bc4acdb390d.png)](https://samirsaci.com)

（作者提供的图像）

设计这种描述性分析解决方案的主要困难在于**复杂性和完整性之间的平衡**。

+   如果你想回答所有潜在的操作性问题，你的报告很快就会变得非常难以使用。

+   为了保持报告简洁，您将不会涵盖全部操作范围。

我们正在接近传统[商业智能（BI）](/what-is-business-intelligence-bf1de730319c)工具的极限，这些工具依赖于视觉、表格和报告来回答操作性问题。

对我来说，报告的未来在于动态定制报告，根据每个用户的问题和上下文独特定制。

> 我们可以使用GPT模型来增强用户体验，通过为每个请求提供定制的输出吗？

这是我试图用Python开发的简单原型来弄清楚的事情。

## 为实验做准备

设置很简单：

+   一个名为‘shipments.db’的本地数据库，其中包含一个名为‘shipments’的表

+   Langchain版本0.0.325

+   一个用于查询GPT模型的OpenAI密钥

+   一个带有LangChain、SQLite和Pandas库的Python本地环境

[![](../Images/a1ce75ecb7c9a3a722eaf97330c6fe3a.png)](https://samirsaci.com)

解决方案的高层概述 — （作者提供的图像）

数据库包括时间戳和布尔标志，以及运输ID、目的地和订单金额。

因此，LangChain SQL代理（由OpenAI的GPT模型提供支持）可以访问数据库，编写SQL查询，并使用输出来回答用户的问题。

## 实验协议：从简单开始

我从一个简单的问题开始，因为我想感受代理的效果。

> “五月初七天内有多少批次的运输延误了？”

正确答案是6,816批次运输。

![](../Images/8a69292a51fbbfd64629e1c5a9f640d8.png)

代理的目标行为 — （作者提供的图像）

我希望看到代理创建一个SQL查询，统计从‘2021-05-01’到‘2021-05-07’期间所有运输的数量，其中布尔标志为**‘On_Time_Delivery’ = False**。

在接下来的部分，我们将探索与代理的**不同交互方法**并寻找提供准确答案的最有效方法。

💡 **关注我的Medium**，获取更多与🏭供应链分析、🌳可持续性和🕜生产力相关的文章。

# 实验与关键见解

现在一切准备就绪，我可以开始创建LangChain代理与数据库交互。

我使用的是`AgentType.ZERO_SHOT_REACT_DESCRIPTION`，这是一种设计用于“零-shot”学习环境的代理类型。

这个代理能够在没有任何特定训练的情况下回答查询。

## 测试1：简单提示而不使用模板

最初的测试涉及向代理提出直接问题。

> “五月前七天内有多少装运延迟？”

**[块1]:** 代理从包含唯一表格‘shipments’的数据库开始探索。

![](../Images/529f7321b43966f1755632ff7e437015.png)

[块1]: 数据库的发现 — (作者提供的图片)

代理正在链接问题中的**“延迟装运”**与数据库中的‘shipments’表。

这个初始块对我们实验的所有其他迭代将完全相同。

**[块2]:** 代理撰写查询并提供了错误答案。

![](../Images/d7cd0dced200d1ec20fad834b704b966.png)

[块2]: 查询数据库并提供答案 — (作者提供的图片)

[![](../Images/75a38e0e689212de695f4c34591ac49e.png)](https://samirsaci.com)

测试1：目标结果（左）/ 测试输出（右） — (作者提供的图片)

👍 一个好处是代理使用了正确的日期（订单日期）来筛选所在范围内的装运。

👎 然而，他错误地选择了定义延迟装运的标志。

我们可以接受这一点，因为我们并未明确定义何为延迟装运。

## 测试2：带有上下文的提示模板

我可以使用带有上下文的提示模板来改进答案。

我希望保持上下文的最小化，因为我们可能不总是知道用户会问什么。

```py
context_explanation = """
As a supply chain control tower manager, my role involves overseeing the logistics network and ensuring that shipments are processed efficiently and delivered on time. 
The 'shipments' table in our database is crucial for monitoring these processes. It contains several columns that track the progress and timeliness of each shipment throughout various stages:
- 'Order_Date' and 'Expected_Loading_Date' provide timestamps for when an order was placed and when it was expected to be loaded for departure.
- 'Expected_Delivery_Time' is a timpestamp defining when the shipment is expected to be delivered
- 'Loading_OnTime', 'Airport_OnTime', 'Landing_OnTime', 'Transmission_OnTime' are boolean values indicating whether the shipment was processed on time at various stages. If any of these are False, it implies a delay occurred, potentially causing the shipment to miss its cut-off time and be postponed to the next day.
- 'Store_Open' indicates if the truck arrived at the store before closing time. A False value here means the delivery must wait until the next day.
- 'On_Time_Delivery' is a critical indicator of our service level, showing whether a shipment arrived by the requested delivery time.
Understanding the data in these columns helps us identify bottlenecks in the shipment process and take corrective actions to improve our delivery performance.
"""
input_question = "How many shipments were delayed in the first seven days of May?"
```

**💡 观察:** 上下文是控制塔经理角色的高层演示和数据库内容。

**[块2]:** 代理撰写查询并提供了错误答案。

[![](../Images/21818392d6dbd610ff54372a49e22ed2.png)](https://samirsaci.com)

[块2]: 查询数据库并提供答案 — (作者提供的图片)

[![](../Images/9025232c2006bbe66e9dac7650483aea.png)](https://samirsaci.com)

测试2：目标结果（左）/ 测试输出（右） — (作者提供的图片)

👎 代理更好地理解了中间标志，但仍然错过了要点。

这个延迟装运的定义虽然不是不合逻辑，但却与操作实际不相符。

**💡 观察:** 装运即使有一个或多个标志为零，也可以按时进行。只有‘On_Time_Delivery’标志可以确定装运是否延迟。

[![](../Images/a0c8f7e133294b3b9fbca21a1902022a.png)](https://samirsaci.com)

与标志相关的截止时间定义 — (作者提供的图片)

🙋‍♀️ 为了公平起见，这不是一个容易猜测的定义。

因此，我可能应该在上下文中包含一个‘延迟发货’的明确定义。

## 测试 3: 具有改进上下文的提示模板

我通过增加一个额外的句子来改进了上下文。

> ‘如果‘On_Time_Delivery’为虚假，则视为延迟发货。’

正如预期的那样，结果很好

[![](../Images/73892320494329323acfc06026c2a52d.png)](https://samirsaci.com)

[Block 2]: 查询数据库并提供答案 — （图片作者提供）

👋 代理选择了正确的标志来定义延迟发货。

> 如果我们要求进行高级分析会怎样？

延迟可能是由于数据集中包含的不同标志所捕捉到的各种原因。

[![](../Images/559894c1d2f3f2b80e7cd899b0b75515.png)](https://samirsaci.com)

已交付的发货（上: 准时，下: 延迟）— （图片作者提供）

我们的控制塔团队希望获得**每个未按时交付的发货的原因代码**。

在这家公司中，原因代码由**所有虚假的中间标志列表**定义。

例如，如果发货延迟：

1.  ‘On_Time_Delivery’ 为虚假

1.  ‘Transmission_OnTime’ 和 ‘Loading_OnTime’ 为虚假。

1.  因素代码是**‘Transmission_OnTime, Loading_OnTime’**。

## 测试 4: 具有改进上下文的高级分析

让我们添加一个额外的声明

> ‘延迟发货的原因代码由所有对该发货为 0 的标志列表定义。’

因此，我可以向代理提出一个新问题：

> 提供 5 月前七天内延迟发货的总数及其按原因代码的拆分。

不幸的是，代理无法计算出正确的原因代码定义。

[![](../Images/862f261ced0d2019123452b29af1c157.png)](https://samirsaci.com)

[Block 2]: 查询数据库并提供答案 — （图片作者提供）

经过多次迭代，我发现**代理需要一些指导**。

因此，我修订了问题

> 请根据定义创建‘Reason_Code’列。然后，提供 5 月前七天内延迟发货的总数及其按原因代码的拆分。

[![](../Images/54b8a5439032ad185153e594676224db.png)](https://samirsaci.com)

[Block 2]: 查询数据库并提供答案 — （图片作者提供）

现在的输出符合原因的定义，并对延迟交付的根本原因进行了完整分析。

> 我们能否使用这个输出通过邮件发送报告？

作为最终练习，我想创建一个链条，让代理使用这个输出写一封邮件。

## 最终测试: 创建一个链条以编写邮件报告

使用 LangChain 创建链条涉及将多个代理组合在一起，以执行一系列任务，每个任务都使用前一个任务的输出。

+   代理 1: **SQL 查询代理** 该代理解释用户的问题，制定 SQL 查询，并从数据库中检索数据。

+   代理2：**电子邮件撰写代理** 此代理从SQL查询代理获取处理后的数据，并撰写一封连贯且信息丰富的电子邮件。

我们要求代理2为我（控制塔经理Samir Saci）向运营总监Jay Pity写一封电子邮件。

[![](../Images/35d80c8e7e0962fdfa90098f988eba91.png)](https://samirsaci.com)

[区块3]：使用代理1的输出撰写电子邮件 —（作者提供的图像）

**💡 观察：** 由于未知原因，代理将延迟发货数量按天分割。

[![](../Images/326c91788f69f9fa5c9fa4880aa159fe.png)](https://samirsaci.com)

[区块4] 电子邮件输出 —（作者提供的图像）

输出是一封总结查询结果的电子邮件。

+   在结束电子邮件之前，代理会进行额外的分析。

+   语调正式，并与物流运营管理的上下文相匹配。

输出可用于使用Python的SMTP库自动发送电子邮件。

这里是一个例子，

[](/automate-operational-reports-distribution-in-html-emails-using-python-c65c66fc99a6?source=post_page-----21e19b33b5f0--------------------------------) [## 使用Python自动化HTML电子邮件中的运营报告分发

### 使用Python在HTML电子邮件中自动分发供应链运营报告。

towardsdatascience.com](/automate-operational-reports-distribution-in-html-emails-using-python-c65c66fc99a6?source=post_page-----21e19b33b5f0--------------------------------)

> **我学到了什么？**

这个简单的LangChain SQL Agents实验教会了我…

+   代理并非无所不知。因此，必须在上下文中解释具体的业务定义。

+   即使有很好的上下文，代理也可能需要指导才能提供正确的输出。

+   可以链接多个代理以执行高级任务。

+   因为代理有时需要指导，我们可能需要培训用户以提示工程工作。

主要挑战是提供正确的上下文，以确保代理能够回答用户生成的所有问题。

**📝额外评论：** 我对自己使用Chat GPT-4感到惊讶，这对我使用其‘小弟’ GPT-3.5 Turbo改进提示模板的上下文非常有帮助。

# LLMs正在塑造供应链未来

## GPTs怎么样？

在掌握供应链分析产品的LLMs时，我还尝试了ChatGPT的新功能GPTs。

![](../Images/2e2f4456e681ba2620a3c95932a814f9.png)

“供应链分析师代理” —（作者提供的图像）

用户可以通过ChatGPT用户界面访问此代理。

+   [供应链分析师](https://bit.ly/the-supply-chain-analyst)

[![](../Images/e3c2f87fbaee9057ea80ba34d67e9df4.png)](https://bit.ly/the-supply-chain-analyst)

用户界面 [测试GPT: [链接](https://bit.ly/the-supply-chain-analyst)]

主意是创建一个配备核心分析模型（Python脚本）、上下文和一些提示指导的代理。

![](../Images/de465884ebfc2ff9a03aa301a0ac9021.png)

GPT简单架构 [测试这个GPT：[链接](https://bit.ly/the-supply-chain-analyst)]

所以用户可以互动询问

+   他们的销售进行帕累托或ABC分析

+   一个报告产品组合销售分布的电子邮件模板

你可以在这篇文章中找到更多细节和示例，

[](https://s-saci95.medium.com/create-gpts-to-automate-supply-chain-analytics-5b44dec8e0f8?source=post_page-----21e19b33b5f0--------------------------------) [## 创建GPTs以自动化供应链分析

### “供应链分析师”是一个自定义ChatGPT的“GPT”，利用销售数据执行帕累托和ABC分析。

[s-saci95.medium.com](https://s-saci95.medium.com/create-gpts-to-automate-supply-chain-analytics-5b44dec8e0f8?source=post_page-----21e19b33b5f0--------------------------------)

## 一个简单的“概念验证”

正如我刚刚开始这个激动人心的旅程，我积极寻求您对我在本文中分享的方法的评论和观察。

最初的结果承诺通过LLMs能力增强的“自助式”数据库迎来一个变革性未来。

[![](../Images/617264ea65e0cb22f5639d4ab117be3a.png)](https://samirsaci.com)

LangChain代理连接到多个数据产品 — （作者提供的图片）

这种解决方案，特别适用于实施数据网格的公司，可以通过响应式界面直接连接用户到数据产品，这些产品利用生成式人工智能的强大功能进行增强。

> 用户没有使用我们的仪表板。为什么？

它使用户能够通过自然语言进行复杂分析，打破我们当前基于仪表板的数据交互方式。

## 继续原型开发

这些初步测试的结论非常积极。

但在正式完成这个概念验证之前，我还有一些测试要进行。

+   丰富数据集，包括在途运输和取消订单

+   测试模型如何处理缺失数据

+   将代理连接到多个数据库，并测试它如何管理多个数据源以回答问题。

我只会在生产中部署它，进行用户验收测试，以了解用户可能会问什么问题 *(并监控代理的行为)*。

我将在未来的文章中分享我的未来实验。如果你感兴趣，欢迎在Medium上关注我。

## 探索供应链中更广泛的应用

作为供应链数据科学家，我的实验并不止步于此。

我迫切希望探索LLMs在供应链分析领域内的其他应用。

这些包括将LLMs与优化模型集成：

+   [👬📈 供应链数字孪生](/what-is-a-supply-chain-digital-twin-e7a8cd9aeb75)

    **应用：这个代理将帮助用户使用自然语言场景触发模拟。** *(用户可以问：“如果我们把中央仓库移到意大利会怎样？”)*

[](/what-is-a-supply-chain-digital-twin-e7a8cd9aeb75?source=post_page-----21e19b33b5f0--------------------------------) [## 什么是供应链数字孪生？

### 使用Python探索数字双胞胎：建模供应链网络、增强决策能力并优化操作。

towardsdatascience.com](/what-is-a-supply-chain-digital-twin-e7a8cd9aeb75?source=post_page-----21e19b33b5f0--------------------------------)

+   [🔗🍃 可持续供应链网络设计](/create-a-sustainable-supply-chain-optimization-web-app-20599b98cab6)

    **应用：用户可以通过使用自然语言来制定目标和约束，从而创建优化模型。** *(用户可以询问：“我希望创建一个满足法国市场需求并最小化CO2排放的工厂网络。”)*

[](/create-a-sustainable-supply-chain-optimization-web-app-20599b98cab6?source=post_page-----21e19b33b5f0--------------------------------) [## 创建可持续供应链优化Web应用程序

### 帮助你的组织结合可持续采购和供应链优化，以减少成本和环境影响…

towardsdatascience.com](/create-a-sustainable-supply-chain-optimization-web-app-20599b98cab6?source=post_page-----21e19b33b5f0--------------------------------)

+   [🏭🍃 可持续采购：选择最佳供应商](https://s-saci95.medium.com/what-is-sustainable-sourcing-46ad1fade14f)

    **应用：采购团队可以使用自然语言制定他们的绿色倡议，并查看对成本的影响。**

    *(用户可以询问：“我们希望估算仅选择碳中和工厂来采购此SKU的成本。”)*

[](https://s-saci95.medium.com/what-is-sustainable-sourcing-46ad1fade14f?source=post_page-----21e19b33b5f0--------------------------------) [## 什么是可持续采购？

### 你如何利用数据科学来选择最佳供应商，同时考虑可持续性和社会指标…

s-saci95.medium.com](https://s-saci95.medium.com/what-is-sustainable-sourcing-46ad1fade14f?source=post_page-----21e19b33b5f0--------------------------------)

我们还可以使用我们的代理来提高数据质量或支持用于战略报告的数据审核：

+   [📉📄 ESG报告：环境、社会和治理报告](/what-is-esg-reporting-d610535eed9c)

    应用：自动化审核用于编制报告的数据。

    *(审计员可以询问：“你能检索用于计算工厂XYZ能源消耗的公用事业账单吗？”)*

[](/what-is-esg-reporting-d610535eed9c?source=post_page-----21e19b33b5f0--------------------------------) [## 什么是ESG报告？

### 利用数据分析进行全面有效的公司环境、社会和治理报告

towardsdatascience.com](/what-is-esg-reporting-d610535eed9c?source=post_page-----21e19b33b5f0--------------------------------)

+   [📉✅ 数据质量是什么？](/what-is-data-quality-f2c0274a6404)

    应用：使用我们的代理来挑战或支持确保数据准确性、一致性和完整性的方法。

    *(用户可以问：去年送达但状态丢失的货物数量能分析吗？)*

[](/what-is-data-quality-f2c0274a6404?source=post_page-----21e19b33b5f0--------------------------------) [## 什么是数据质量?

### 探索确保供应链数据的准确性、一致性和完整性的方法论。

towardsdatascience.com](/what-is-data-quality-f2c0274a6404?source=post_page-----21e19b33b5f0--------------------------------)

每个领域都有巨大潜力，可以利用生成式AI在公司中部署“分析即服务”解决方案。

例如，你能通过智能代理改进这个网页应用的用户界面吗？

[![](../Images/3f16a0d0ce8a54c52315aca388846793.png)](https://cloud.viktor.ai/public/product-segmentation-abc-analysis)

ABC分析与帕累托图应用程序 — [[链接](https://cloud.viktor.ai/public/product-segmentation-abc-analysis)]

如果你分享这种热情，请在评论区提供建议！

# 关于我

让我们在[Linkedin](https://www.linkedin.com/in/samir-saci/)和[Twitter](https://twitter.com/Samir_Saci_)上互相关注，我是一名供应链工程师，利用数据分析来改善物流运营并降低成本。

如果你对数据分析和供应链感兴趣，请访问我的网站。

[](http://samirsaci.com/?source=post_page-----21e19b33b5f0--------------------------------) [## Samir Saci | 数据科学与生产力

### 一个专注于数据科学、个人生产力、自动化、运筹学和可持续发展的技术博客。

samirsaci.com](http://samirsaci.com/?source=post_page-----21e19b33b5f0--------------------------------)

💡 **关注我的Medium**获取更多与 🏭 供应链分析、🌳 可持续发展和 🕜 生产力相关的文章。
