# 如何使用 Google 的 PaLM 2 API 与 Python

> 原文：[`towardsdatascience.com/how-to-use-google-palm-2-api-with-python-373bc564251c?source=collection_archive---------2-----------------------#2023-08-14`](https://towardsdatascience.com/how-to-use-google-palm-2-api-with-python-373bc564251c?source=collection_archive---------2-----------------------#2023-08-14)

## 自定义并将 Google 的 LLM 集成到你的应用程序中。

[](https://eliselandman.medium.com/?source=post_page-----373bc564251c--------------------------------)![Elise Landman](https://eliselandman.medium.com/?source=post_page-----373bc564251c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----373bc564251c--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----373bc564251c--------------------------------) [Elise Landman](https://eliselandman.medium.com/?source=post_page-----373bc564251c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fdbd14e538474&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-use-google-palm-2-api-with-python-373bc564251c&user=Elise+Landman&userId=dbd14e538474&source=post_page-dbd14e538474----373bc564251c---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----373bc564251c--------------------------------) ·11 分钟阅读·2023 年 8 月 14 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F373bc564251c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-use-google-palm-2-api-with-python-373bc564251c&user=Elise+Landman&userId=dbd14e538474&source=-----373bc564251c---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F373bc564251c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-use-google-palm-2-api-with-python-373bc564251c&source=-----373bc564251c---------------------bookmark_footer-----------)![](img/83e8ff5e480e20067392de2c2d57db3d.png)

图片由 Alexandre Debiève 提供，来源于 [Unsplash](https://unsplash.com/photos/FO7JIlwjOtU)。

生成型人工智能无处不在。我们看到越来越多的公司投资于这项**强大技术**，因为它的**潜力**变得越来越明显。正如 Gartner 所说：在不久的将来，[生成型人工智能]将成为竞争优势和差异化的关键。

> “在不久的将来，[生成型人工智能]将成为竞争优势和差异化的关键。”

不幸的是，开发生成性 AI 模型不仅是复杂的工程工作，而且通常是一个相当昂贵的项目。幸运的是，我们不必自己开发这些模型 — 我们可以重用已经为我们预先开发的：通过 APIs！所以，不要再等了 — 让我们直接深入了解如何通过**集成**生成性 AI 到我们的应用程序中来利用它。

对于本文，我们将查看**谷歌对 LLM 的回答**：**PaLM 2** API。PaLM 2 是谷歌最新版本的**Pathways 语言模型**，这是一个大型语言模型，使用的训练数据量约是他们在 2022 年发布的初始模型的五倍。

在本文中，我将介绍**一些代码示例**并展示如何认证到 Google Cloud，使用以及自定义**PaLM 2 APIs**，并使用 Python 3.11。

# 1 | 入门

PaLM 2 APIs 可以通过**Google Cloud**的 [**Vertex AI**](https://cloud.google.com/vertex-ai) 平台访问。因此，在我们进行任何 API 调用之前，我们需要设置我们的 Google Cloud 账户。你可以 [在这里注册](https://console.cloud.google.com/) 并获得 [$300 的免费额度](https://cloud.google.com/free/docs/free-cloud-features#free-trial) 来开始使用这些服务。

一旦你的账户和项目设置完成，我们可以继续 [创建一个服务账户](https://cloud.google.com/iam/docs/service-accounts-create)，我们将用它来认证到 Vertex AI APIs。我们使用服务账户，是因为我们可以通过仅给予它们特定的 IAM 权限来确保对 Google Cloud 资源的**访问控制**。对于我们的用例，我们将授予服务账户 `Vertex AI User` 角色。这可能对你的用例来说过于宽泛，所以我建议你检查 [可用的访问角色](https://cloud.google.com/vertex-ai/docs/general/access-control) 并选择适合你需求的角色。

在创建了服务账户并赋予其正确的权限后，我们可以继续 [生成一个服务账户密钥](https://cloud.google.com/iam/docs/keys-create-delete)。选择 JSON 作为密钥类型，并将文件保存在安全的位置。

很好 — 我们准备动手操作了！👏

# 2 | 认证到 Google Cloud

在这个示例中，我们将使用 [OAuth 2.0](https://oauth.net/2/) 进行认证，并借助我们在前一步生成的服务账户密钥请求访问令牌。

为了便利这个过程，我们可以使用 `[google-auth](https://pypi.org/project/google-auth/)` Python 库，如下面的代码示例所示：

使用 google-auth Python 库认证到 Google Cloud。

这个代码示例使用服务账户密钥文件“key.json”来请求和生成一个访问令牌，我们可以用它来访问 Google Cloud APIs。在获得访问令牌后，我们可以开始使用它调用 Vertex AI PaLM 2 APIs。

# 3 | 调用 PaLM 2 API

截至今天，Google Cloud 中提供了两种不同的 PaLM 2 模型：**PaLM 2 for Text**（即`[text-bison](https://cloud.google.com/vertex-ai/docs/generative-ai/text/text-overview)`）和**PaLM 2 for Chat**（即`[chat-bison](https://cloud.google.com/vertex-ai/docs/generative-ai/chat/chat-prompts)`）。文档建议对可以用一个回复完成的文本任务使用`text-bison`，对需要更多对话互动的文本任务使用`chat-bison`。

让我们从`text-bison`模型开始。对于这些示例，我们将使用 Python 的`[requests](https://pypi.org/project/requests/)`库来进行 API 调用。如果你愿意，也可以使用[Vertex AI SDK](https://cloud.google.com/python/docs/reference/aiplatform/latest/index.html)。

## PaLM 2 for Text：情感分析

PaLM 2 for Text 模型可用于各种**与文本相关的任务**：包括摘要、回答问题、情感分析等。它接受以下参数作为输入：

+   `prompt`：我们希望模型执行的任务的指令。

+   `temperature`：控制模型的“创造性”。如果我们希望模型在回复中更具开放性和创造性，则应提高温度。如果我们希望模型更具确定性，则温度应降低。值的范围在 0 到 1 之间。

+   `maxOutputTokens`：生成输出的 token 数量（1 个 token = 4 个字符）。值的范围在 1 到 1024 之间。

+   `topK`：改变模型选择生成输出的 token 的概率。在每个 token 选择步骤中，具有最高概率的`topK`个 token 被采样，然后进一步通过`topP`筛选。值越高，响应越随机。值的范围在 1 到 40 之间。

+   `topP`：改变模型选择生成输出的 token 的概率。token 的选择直到其概率总和等于`topP`。值越高，响应越随机。值的范围在 0 到 1 之间。

*有关参数的更多细节，请参见* [*此文档*](https://cloud.google.com/vertex-ai/docs/generative-ai/start/quickstarts/api-quickstart#parameter_definitions)*。*

在这个第一个例子中，我们将对一些**示例产品评论**进行情感分析：

```py
sentences = ["I have been using this product for a long time. Somehow the 
             product I received this time seems to be a fake one. It's 
             very thick and the smell is very chemical.",
             "A good book for for dog owners. The book dosen't just 
             focus on dog training itself, but also on nutrition and care. 
             The book contains all the information needed for beginner dog 
             owners.",
             "This lamp was better than I expected when it comes to its 
             quality. Nonetheless, the colours are not exactly as 
             displayed and it does not really fit with the curtains that I 
             purchased recently. Therefore I give this 3 out of 5 stars."]
```

我们给模型的指令（即提示）需要清楚地说明我们希望模型执行的任务，以及我们期望它生成的输出。在我们的例子中，我们要求它遍历`sentences`列表中的每个评论，并**告诉我们这些评论的情感**。我们还指示它将输出提供为 Python 列表。

```py
prompt = f"What is the sentiment of each of these reviews: {sentences}. 
         Output should be in a python list."
```

最后，我们需要定义**输入参数值**：

+   我们将`temperature`设置为`0`，因为对于这个任务，我们希望避免模型过于创意。通过降低温度，我们使得模型更有可能输出我们请求的确切结构。

+   我们将`256`设置为`maxOutputTokens`，因为它大约相当于 200 个单词，并且是适合我们任务的长度。

+   我们将`topK`设置为 40，因为这是模型的默认值。

+   我们将`topK`设置为 0.95，因为这是模型的默认值。

我们现在可以像进行任何其他 API 调用一样发出 API 调用，使用`[requests](https://pypi.org/project/requests/)`库，如下所示：

向 Google Cloud PaLM 2 for Text API 发出 API 调用。

我们可以通过`response.json()[“predictions”][0][“content”]`来获取响应的输出：

```py
"['negative', 'positive', 'positive']"
```

很棒！现在，让我们做完全相同的操作并尝试将`temperature`参数设置为`1.0`。这将意味着我们的模型输出将变得越来越富有创意。我们得到的输出如下：

```py
"The sentiment of each of these reviews is:
1\. Negative
2\. Positive
3\. Positive"
```

的确，我们可以看到输出比以前更具创意，并且没有正确遵循我们提到的“输出应该是一个 python 列表”的提示。因此，这是一个很好的教训，即**选择合适的参数值非常重要**，以从模型中获得期望的输出。

现在你可能会问自己：“*这是否意味着我不能确定模型输出是否符合正确的格式？*” 有多种机制可以设置以测试模型的输出并检查其是否符合你预期的格式。从理解并正确配置模型的输入参数，到优化你的提示（即提示工程），以及实施额外的静态测试以验证输出的结构。强烈建议应用这些技术，因为生成式 AI 可能会出错，我们希望在它们发生之前就解决这些问题。

## PaLM 2 for Text: 文本生成

在我们的下一个示例中，让我们要求 PaLM 2 为新客户生成**欢迎文本**，他们正在访问一个电子商务网站。我们想欢迎他们，并在首次购买时提供 20%的折扣码。对于这个用例，我们希望我们的模型提出一些更具创意的内容。因此，我们将`temperature`设置为`0.5`。

```py
discount_code = "2023-promo"

prompt = f"Can you write 2-3 sentences that can be displayed on an 
         e-commerce webpage if someone first visits it? It should contain 
         the code '{discount_code}' which gives the customer 20% discount 
         on their first purchase."
```

输出如下：

```py
"**Introducing our 20% off first purchase offer!**

Use the code **2023-promo** at checkout to save 20% on your first 
purchase. This offer is valid for a limited time only, so don't miss out!

**Shop now and start saving!**"
```

很好！它甚至以 markdown 格式给出了输出。让我们再次尝试将`temperature`调整为`1.0`。输出如下：

```py
"**20% OFF YOUR FIRST PURCHASE**

Use code 2023-promo at checkout to save 20% on your first purchase from 
our store. This offer is valid for new customers only and expires on 
March 31, 2023.

**Shop now and save big!**"
```

我们再次可以看到我们的模型相比之前的输出变得更加富有创意。它甚至提出了一个优惠过期日期，而我们并没有明确要求它这样做。

## PaLM 2 for Chat: 对话助手

在最后一个示例中，我们将测试专注于生成**对话体验**的 PaLM 2 for Chat API。我们将使用的模型称为`chat-bison`。它接受以下参数作为输入：

+   `messages`：包含与聊天机器人之间的消息和消息历史记录。

+   `context`：定义聊天机器人行为的“指南”。例如：聊天机器人的名称是什么，它的角色是什么，包含/排除的词汇等。

+   `examples`：示例输入和输出，展示了聊天机器人在对话中的响应方式。

+   以及`temperature`、`maxOutputTokens`、`topK`和`topP`，它们的作用与前一节中的`text-bison`模型相同。

*有关参数及其具体作用的更多详细信息，请参见* [*此文档*](https://cloud.google.com/vertex-ai/docs/generative-ai/model-reference/text-chat#generative-ai-text-chat-drest)*。*

我们来创建一个聊天机器人，作为在线园艺商店“**GardenWorld**”的**客户支持代表**。这个机器人应当能够回答有关植物和花卉类型、园艺工具等问题。我们希望机器人始终对客户友好和热情，并且应该用“Hoowdy gardener! 🌱”来问候客户，并激励客户注册新闻通讯以获得首次购买的 10%折扣码。

我们可以通过设置`context`和`examples`参数来定义这些，如下所示：

```py
context = "You're a customer assistance bot for an online gardening store 
          called GardenWorld and you want to help customers. You give 
          advice around gardening best practices, plant and flower types. 
          Always welcome the customer by telling them to subscribe to 
          the GardenWorld newsletter to get a 10% discount on their first 
          purchase."

examples = [{
           "input": {"content": "Hi!"},
           "output": {"content": "Hoowdy gardener! 🌱"}},
           {
           "input": {"content": "Hello"},
           "output": {"content": "Hoowdy gardener! 🌱"}
           },
           {
           "input": {"content": "Why should I choose GardenWorld?"},
           "output": {"content": "GardenWorld has been the award winning 
                                 supplier for best quality plants and 
                                 gardening equipment. With over 25 years 
                                 of experience, we continuously provide 
                                 the best in class service to our 
                                 customers."}}
           ]
```

我们将给模型三个示例，说明输出应是什么样的。当然，**我们提供的示例越多**，机器人可以**定制**的程度就越高，模型将能够更好地学习我们期望的内容。由于我们的用例非常简单，这三个示例应该能给机器人足够的信息来提供正确的答案结构。我们将`temperature`设置为`1.0`，`maxOutputTokens`设置为`256`，`topK`设置为`40`，`topP`设置为`0.95`。

现在，我们开始对话，先说“**Hi!**”。

```py
messages = [{
          "author": "user",
          "content": "Hi!",
           }]
```

然后我们可以进行 API 调用：

向 Google Cloud PaLM 2 进行聊天 API 调用。

几秒钟后，我们得到以下回复：

```py
"Hoowdy gardener! 🌱 How can I help you today? Don’t forget to subscribe 
to the GardenWorld newsletter to get a 10% discount on your first 
purchase!"
```

好的！我们的模型准确地完成了我们的要求：它使用了正确的问候语（甚至包括了表情符号🌱），并提到了 10%折扣码。

为了继续与机器人对话，我们需要相应地更新**对话历史**。我们可以定义两个简单的函数，在每次 API 调用后调用它们：

更新聊天机器人的消息历史的功能。

`update_history()`将最新的机器人回复添加到`messages`列表中。然后我们可以使用`send_message()`向机器人发送一个新问题“**我需要一株易于照料的漂亮室内植物。你有什么推荐？**”。

现在我们的`messages`变量如下所示，最新的消息位于列表末尾：

```py
[{"author": "user", 
  "content": "Hi!" },
 {"author": "1"
  "content": "Hoowdy gardener! 🌱 How can I help you today? Don’t forget 
             to subscribe to the GardenWorld newsletter to get a 10% 
             discount on your first purchase!" },
 {"author": "user",
  "content": "I need a nice houseplant that is easy to take care of. What 
             can you recommend?" }]
```

我们可以在下一次 API 调用中包含`messages`历史记录，得到的回复是：

```py
"A snake plant is a great choice for a houseplant because it is very low 
maintenance and can thrive in a variety of conditions. They are also very 
tolerant of neglect, so they are perfect for people who are busy or 
forgetful."
```

它建议我们选择“[蛇尾兰](https://www.almanac.com/plant/snake-plants)”，这是一种被认为易于照料的室内植物。让我们将这条消息也添加到`messages`历史记录中。

然后，我们提出最后一个问题“**你能建议一些替代方案吗？另外，我可以获得折扣吗？**”我们得到的回复是：

```py
"Sure! We offer a 10% discount to all first time customers who subscribe 
to our newsletter. A ZZ plant is another great option for a low-
maintenance houseplant. They are also very tolerant of neglect and can 
thrive in a variety of conditions.'
```

很好！如上所示，机器人建议了我们的折扣码，感谢`messages`历史记录，它记住了我们之前提出的问题，并建议了一个容易打理的植物“[ZZ plant](https://www.gardenersworld.com/how-to/grow-plants/how-to-grow-zz-plant-zamioculcas-zamiifolia/)”。

结果展示了我们如何**轻松**且**迅速**地让一个**自定义聊天机器人**运行起来：只需提供三个`示例`和一个高层次的`上下文`，机器人就能够**理解**我们的期望，并在对话中正确返回信息。想象一下如果我们提供数百甚至数千个额外的`示例`——潜力巨大！

# 4 | 结论

在本文中，我们看到利用 Google Cloud 的**PaLM 2 APIs**是多么**简单**，使用了`text-bison`和`chat-bison`模型。我们了解到如何使用 Python 的`[google-auth](https://pypi.org/project/google-auth/)`库进行远程**认证**，以及如何使用`[requests](https://pypi.org/project/requests/)`库调用这些 APIs。最后，我们看到如何通过调整和玩转输入参数来**定制**这些 APIs。

希望这篇文章对你有帮助，并给你一些灵感和关于如何开始使用 PaLM 2 APIs 的想法！ 🪴🤖

🔜🔜 在即将到来的**第二部分**中，我们将专注于**使用 PaLM 2 APIs 进行提示工程**，并探讨如何更好地优化输入提示和模型参数选择。

*反馈或问题？请随时在评论区联系我！* 💬

📚 想了解更多？查看 [Google Cloud 上的生成 AI](https://cloud.google.com/ai/generative-ai) 或尝试 Google Cloud Skills Boost 上免费的 [生成 AI 学习路径](https://www.cloudskillsboost.google/journeys/118)。

## 参考文献

[1] Gartner, [Gartner 专家回答您的企业生成 AI 顶级问题](https://www.gartner.com/en/topics/generative-ai)，访问日期：2023 年 8 月 13 日。

[2] CB Insights, [生成 AI 的现状：7 张图表](https://www.cbinsights.com/research/generative-ai-funding-top-startups-investors/)，2023 年 8 月 2 日。

[3] Google Cloud, [Vertex AI 上生成 AI 支持概览](https://cloud.google.com/vertex-ai/docs/generative-ai/learn/overview)，访问日期：2023 年 8 月 13 日。
