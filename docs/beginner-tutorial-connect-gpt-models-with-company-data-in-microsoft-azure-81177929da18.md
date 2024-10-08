# 初学者教程：在 Microsoft Azure 中将 GPT 模型与公司数据连接

> 原文：[`towardsdatascience.com/beginner-tutorial-connect-gpt-models-with-company-data-in-microsoft-azure-81177929da18`](https://towardsdatascience.com/beginner-tutorial-connect-gpt-models-with-company-data-in-microsoft-azure-81177929da18)

## 使用 OpenAI Studio、认知搜索和存储帐户

[](https://medium.com/@ebbeberge?source=post_page-----81177929da18--------------------------------)![Eirik Berge, PhD](https://medium.com/@ebbeberge?source=post_page-----81177929da18--------------------------------)[](https://towardsdatascience.com/?source=post_page-----81177929da18--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----81177929da18--------------------------------) [Eirik Berge, PhD](https://medium.com/@ebbeberge?source=post_page-----81177929da18--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----81177929da18--------------------------------) ·阅读时间 10 分钟·2023 年 10 月 15 日

--

![](img/d22b9b079dd194d6d903faa1cdd85800.png)

由 [Volodymyr Hryshchenko](https://unsplash.com/@lunarts?utm_source=medium&utm_medium=referral) 拍摄，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 概述

1.  介绍

1.  设置数据

1.  创建索引和部署模型

1.  其他考虑因素

1.  总结

# 介绍

在过去的一年里，关于 GPT 模型和生成式 AI 的炒作很多。虽然关于全面技术革命的承诺可能显得有些夸张，但确实**GPT 模型在许多方面令人印象深刻**。然而，GPT 模型的真正价值在于将它们与内部文档连接起来。这是为什么呢？🤔

当你使用 OpenAI 提供的普通 GPT 模型时，它们并不真正理解你公司的内部运作。如果你问它问题，它将**基于它最可能了解的其他公司的信息来回答**。当你想使用 GPT 模型来问如下问题时，这就是一个问题：

+   我必须遵循的内部程序步骤是什么？

+   我公司与特定客户之间的完整互动历史是什么？

+   如果我在特定软件或例程上遇到问题，我应该联系谁？

尝试用普通 GPT 模型问这些问题不会得到有价值的答案（试试看！）。但如果将 GPT 模型连接到你的内部数据，你就可以获得这些问题以及其他许多问题的有意义的答案。

**在本教程中，我想向你展示如何将 GPT 模型与 Microsoft Azure 中的内部公司数据连接起来。** 仅在过去几个月，这变得更简单了。我会慢慢演示资源的设置和必要的配置。这是一个初学者教程，所以如果你对 Microsoft Azure 非常熟悉，你可以快速浏览教程。

你需要在继续之前准备好两件事：

+   一个你有足够权限上传文档、创建资源等的 Microsoft Azure 租户。

+   在发布时，你的公司需要申请访问我们将要使用的 Azure OpenAI 资源。这可能会在未来某个时候取消，但目前这是必须的。申请后到获得访问权限的时间大约需要几天。

**注意：** 创建出色的 AI 助手的真正难点在于数据质量、正确定义项目范围、理解用户需求、用户测试、自动化数据摄取等。因此，不要以为创建一个优秀的 AI 助手很简单。只是设置基础设施很简单。

# 设置数据

一切从数据开始。第一步是将一些内部公司数据上传到 Azure。在我的示例中，我将使用以下文本，你也可以复制并使用：

```py
In company SeriousDataBusiness we always make sure to clean our 
desks before we leave the office.
```

将文本保存到名为`company_info.txt`的文本文件中，并存放在一个方便的位置。现在我们将前往 Microsoft Azure 并上传文本文档。在 Azure 市场上搜索**存储账户**资源：

![](img/59cff553ceaa3661729e6422137d0157.png)

在创建 Azure 资源时，有许多字段可以填写。对于存储账户，重要的字段有：

+   **订阅：** 你希望创建存储账户的[订阅](https://learn.microsoft.com/en-us/microsoft-365/enterprise/subscriptions-licenses-accounts-and-tenants-for-microsoft-cloud-offerings?view=o365-worldwide#subscriptions)。

+   **资源组：** 你希望在其中创建存储账户的[资源组](https://learn.microsoft.com/en-us/azure/azure-resource-manager/management/manage-resource-groups-portal#what-is-a-resource-group)。你也可以决定为本教程创建一个新的资源组。

+   **存储账户名称：** 在所有 Azure 账户中唯一的名称，长度在 3 到 24 个字符之间。只能包含小写字母和数字。

+   **区域：** 将承载数据的 Azure 区域。

+   **性能：** 选择*标准*足以用于测试。

+   **冗余：** 选择*本地冗余存储*足以用于测试。

一旦你点击了**审核**然后**创建**，应该会在你选择的资源组中等待一个存储账户，几分钟内就能看到。进入存储账户后，前往左侧边栏的`容器`：

![](img/344d1efa79db4becf1aa3297f652467d.png)

在这里，你可以创建一个新的容器，实际上它作为数据的命名空间。我把我的容器命名为 `newcontainer` 并进入了它。现在你可以在左上角看到一个上传按钮。点击上传，然后找到你之前保存的心爱的 `company_info.txt` 文件。

![](img/f26ec02f55750367e09e9a1ac90b85e0.png)

现在我们的数据在 Azure 中了。我们可以继续下一步 🔥

# 创建索引并部署模型

当我读一本食谱时，我经常查阅书后面的**索引**。索引可以快速告诉你哪个食谱在第几页。在忙碌的世界里，每次想做煎饼时翻阅整本书是不够的。

为什么我要告诉你这些？因为我们还要为我们在上一部分上传的内部数据创建索引！这将确保我们可以快速定位内部文档中的相关信息。这样，你就不需要在每个问题中都将所有数据发送给 GPT 模型。这不仅成本高，而且由于 GPT 模型的令牌限制，即使是中型数据源也无法实现。

我们需要一个**Azure Cognitive Search**资源。这是一个帮助我们自动索引文档的资源。和之前一样，前往 Azure 市场并找到 Cognitive Search 资源：

![](img/2585b2e773ceecb71a7979c183748a87.png)

在创建 Azure Cognitive Search 资源时，应该选择与存储账户相同的**订阅、资源组**和**位置**。给它一个独特的**服务名称**，并选择定价层 *Standard*。点击**Review**按钮，然后点击**Create**按钮继续。

完成后，我们实际上要创建另一个资源，即 `Azure OpenAI` 资源。原因是我们不会在 Cognitive Search 资源中创建索引，而是通过 Azure OpenAI 资源间接创建。这对于不需要大量调整索引的简单应用程序来说更为方便。

再次前往 Azure 市场并找到 Azure OpenAI 资源：

![](img/8f9d00a939a6df62ba311263c2c2ff49.png)

你需要选择与其他资源相同的订阅、资源组和区域。给它一个名称并选择定价层 `Standard SO`。点击进入 `Review and Submit` 部分，然后点击 `Create`。这是你完成教程所需的最后一个资源。等资源完成时，去喝杯咖啡或其他饮料吧。

进入 Azure OpenAI 资源后，你应该在 `Overview` 部分看到类似的内容：

![](img/c25f1111e77711815d8bb35ade4a4f5e.png)

点击 `Explore`，这会带你进入 `Azure OpenAI Studio`。在工作室中，你可以通过图形用户界面部署模型并连接内部数据。你现在应该看到类似的内容：

![](img/8058e32ee66eb1ca15e62d7d508c58c6.png)

首先让我们创建一个 GPT 模型的部署。前往左侧边栏的`Models`部分。这将显示你可以使用的可用模型。你看到的模型可能与你的不同，取决于你选择的区域。我将选择`gpt-35-turbo`模型并点击`Deploy`。如果你没有访问这个模型的权限，请选择另一个。

选择一个`Deployment name`并创建部署。如果你去左侧边栏的`Deployments`部分，你可以看到你的部署。接下来我们将前往左侧边栏的`Chat`部分，在这里我们将开始通过索引连接数据。

你应该会看到一个名为`Add you data (preview)`的选项卡，可以选择它：

![](img/265a10f0438fbeb54af4e8bafd12ef47.png)

当你阅读本教程时，这个功能可能已经不在预览模式中。选择`Add a data source`并选择`Azure Blob Storage`作为你的数据源。你需要输入的信息包括订阅、Azure Blob 存储资源、你放置文档`company_info.txt`的存储容器，以及我们创建的 Azure Cognitive Search 资源：

![](img/e9b21285a5f4036d954add22a87569f6.png)

输入一个索引名称，并将`Indexer schedule`选项保持为`Once`。这是根据潜在的新数据更新索引的频率。由于我们的数据不会改变，我们选择`Once`以简化操作。接受连接到 Azure Cognitive Search 帐户将产生使用费用的提示，然后继续。你可以在`Data management`下选择`Keyword`作为`Search type`：

![](img/990e9bd84e2f7bcf35708c80195edf58.png)

点击`Save and close`并等待索引完成。现在已部署的 GPT 模型可以访问你的内部数据了！你可以在`Chat session`中提问进行尝试：

![](img/543aa6e3a84e91421a3b43855f8c4d7f.png)

聊天机器人基于内部文档提供了正确的答案 😍

它提供了正确文档的参考，以便你可以检查源材料以确认。

还有一个叫做`View code`的按钮，你可以在这里查看用各种编程语言发出的请求。只要你包含列出的端点和访问密钥，就可以从任何地方发送这个请求。因此，你不局限于这里的操作环境，可以将其整合到你自己的应用程序中。

你现在已经成功将 GPT 模型与内部数据连接起来了！当然，在我们的教程中，内部数据并不太有趣。但你可以想象用比办公政策更重要的材料来做这件事。

# 其他注意事项

在这里，我想引导你去尝试一些进一步的内容。

## 系统消息

你还可以在 Chat 操作区指定系统消息：

![](img/b133c9c7f4e8b21bdb7f0d85d8c1f7e4.png)

这在其他设置中有时被称为`pre-prompt`。这是在用户实际提问之前每次发送的消息。目的是为 GPT 模型提供关于当前任务的上下文。它默认为类似`你是一个帮助人们查找信息的 AI 助手`的内容。

你可以更改系统消息以请求特定格式的响应，或更改回答的语气。随意尝试。

## 参数

你可以找到一个`Configuration`面板（它要么已经可见，要么你需要去`Show panels`并选择它）。它看起来像这样：

![](img/4afd09c8915f93029ce3abc9505d7e9c.png)

在这里你可以调整许多参数。也许最重要的是`Temperature`，它表示答案的确定性。低值表示高度确定，因此每次给出的答案大致相同。高值则相反，每次答案更为多样。高值通常使模型看起来更具创造性。

## 部署到 Web 应用程序

当你完成系统消息和参数的调整后，你可能想要将模型部署到一个 Web 应用程序。这可以通过 Azure OpenAI Studio 很容易地完成。只需点击`Deploy to`按钮，然后选择`A new web app...`

![](img/db3657491e2a397a50f56bd35ee094d7.png)

填写相关信息后，你可以从 Web 应用程序访问模型。这是使模型对其他人可用的一种方式。

# 总结

![](img/4a06c775ef80f575c5d7cdfe36d7a151.png)

图片由[斯宾塞·伯根](https://unsplash.com/@spencerbergen?utm_source=medium&utm_medium=referral)提供，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在本教程中，我展示了如何在 Azure 中将 GPT 模型与公司内部数据连接。再次强调，这只是获得出色 AI 助手的第一步。接下来的步骤需要在数据质量、索引优化、服务设计和自动化等领域的专业知识。但你现在有了一个最基本的设置，可以进一步开发 👋

如果你对人工智能或数据科学感兴趣，可以随时关注我或在[LinkedIn](https://www.linkedin.com/in/eirik-berge/)上联系我。你对将 GPT 模型连接到公司数据的经验如何？我很想听听你的看法 😃

**喜欢我的写作吗？** 查看我的其他帖子以获取更多内容：

+   [你作为数据科学家成功所需的软技能](https://medium.com/towards-data-science/the-soft-skills-you-need-to-succeed-as-a-data-scientist-ceac760230d3)

+   如何作为数据科学家编写高质量的 Python 代码

+   用美丽的类型提示现代化你的罪恶 Python 代码

+   在 Python 中可视化缺失值令人惊讶地简单

+   在 Python 中使用 PyOD 进行异常/离群值检测 🔥
