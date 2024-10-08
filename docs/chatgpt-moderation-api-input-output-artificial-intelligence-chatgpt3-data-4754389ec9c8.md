# ChatGPT 审查 API：输入/输出控制

> 原文：[`towardsdatascience.com/chatgpt-moderation-api-input-output-artificial-intelligence-chatgpt3-data-4754389ec9c8`](https://towardsdatascience.com/chatgpt-moderation-api-input-output-artificial-intelligence-chatgpt3-data-4754389ec9c8)

## 使用 OpenAI 的审查端点实现负责任的 AI

[](https://medium.com/@andvalenzuela?source=post_page-----4754389ec9c8--------------------------------)![Andrea Valenzuela](https://medium.com/@andvalenzuela?source=post_page-----4754389ec9c8--------------------------------)[](https://towardsdatascience.com/?source=post_page-----4754389ec9c8--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----4754389ec9c8--------------------------------) [Andrea Valenzuela](https://medium.com/@andvalenzuela?source=post_page-----4754389ec9c8--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----4754389ec9c8--------------------------------) ·9 分钟阅读·2023 年 7 月 23 日

--

![](img/733cc16aa1f0f391b10378392a4daae2.png)

自制 gif。

大型语言模型 (LLMs) 无疑改变了我们与技术互动的方式。在众多 LLM 中，ChatGPT 被证明是一个宝贵的工具，为用户提供了大量的信息和有用的回应。然而，像任何技术一样，**ChatGPT 也有其局限性**。

最近的讨论揭示了一个重要问题——**ChatGPT 生成不当或有偏见回应的潜在风险**。这个问题源自于其训练数据，这些数据包括了来自不同背景和时代的个体的集体著作。**虽然这种多样性丰富了模型的理解，但也带来了现实世界中普遍存在的偏见和成见**。

因此，ChatGPT 生成的一些回应可能反映了这些偏见。*但我们要公平地看待*，**不当回应可能是由不当的用户查询触发的**。

在本文中，我们将探讨在构建 LLM 驱动的应用程序时，积极审查模型的输入和输出的重要性。为此，我们将使用所谓的***OpenAI 审查 API*，该 API 帮助识别不当内容并采取相应行动**。

*一如既往*，我们将使用 Python 实现这些审查检查！

# 内容审查

在构建使用大型语言模型 (LLMs) 的应用程序时，认识到控制和审查用户输入及模型输出的重要性至关重要。

**📥 用户输入控制** 指的是实施机制和技术来监控、过滤和管理**用户在使用 LLM 应用程序时提供的内容**。这种控制使开发者能够降低风险，维护应用程序的完整性、安全性和伦理标准。

**📤 输出模型控制**指的是实施措施和方法，以便在模型与用户互动时监控和过滤模型生成的响应。通过控制模型的输出，开发者可以解决潜在问题，例如偏见或不适当的响应。

像 ChatGPT 这样的模型可能会表现出偏见或不准确性，特别是当受到未过滤用户输入的影响时。**如果没有适当的控制措施，模型可能会无意中传播误导性或虚假的信息**。因此，不仅需要对用户输入进行审查，还需要实施措施来审查模型的输出。

# OpenAI 审查 API

![](img/8734afecae84a763b20141424812d050.png)

自制的 gif。

OpenAI，即 ChatGPT 背后的公司，已经提供了一种工具来识别上述不适当内容，无论是来自用户还是模型：**审查 API**。

具体来说，审查端点作为工具，用于检查内容是否符合 OpenAI 的使用政策，这些政策针对不适当的类别，如**仇恨言论**、**威胁**、**骚扰**、**自残**（意图或指示）、**色情内容**（包括未成年人）和**暴力内容**（包括详细的图像）。

*那最棒的部分是什么？*

**审查端点是免费的**，可用于监控 OpenAI API 的输入和输出！

## Python 中的审查 API

*我们如何使用这个工具？让我们开始实际操作吧！*

在实际操作中，我们将使用 Python 和官方的`openai`库，该库已经提供了一个`Moderation.create`方法来访问审查 API。

我们可以像其他 Python 库一样获取`openai`库：

```py
pip install openai
```

然后，从我们的 OpenAI 账户中获取*OpenAI API 密钥*并将其设置为环境变量，或者在我们的*Jupyter Notebook*中指定令牌路径。我通常使用最新的方法：

```py
import openai

openai.api_key_path = "/path/to/token"
```

一旦密钥设置完成，我们可以仅用一行代码创建一个审查请求，给定输入文本：

```py
user_input = """
Mama always said life was like a box of chocolates. You never know what you're gonna get.
"""

response = openai.Moderation.create(input = user_input)
print(response)
```

给定用户输入（`user_input`），这里是我们得到的审查响应：

从响应中我们可以看到，完成项返回一个包含三个条目的`json`对象：给定响应的`id`、生成审查输出所使用的`model`，以及`result`本身。

具体来说，`result`条目包含我们感兴趣的信息：

+   `categories`：此条目包含一个包含十一项目标条目的列表以及**给定输入文本是否属于这些类别中的任何一个**（`true`/`false`值）。

+   `category_scores`：此条目包含每个目标类别的分数。这些数字对应于**模型对输入是否违反 OpenAI 政策的信心**。值在 0 和 1 之间，数值越高表示信心越高。

+   `flagged`：最后一项条目由一个键值对组成，显示文本是否被认为是*—— 被标记的 ——*不适当。

    **这个字段可以帮助我们快速过滤不适当的用户输入或模型输出**。

## 管理类别

在之前的示例中，我们已经看到了管理端点分析的十一类。然而，让我们简要定义每个类别：

+   `hate`：宣传基于种族、性别、民族、宗教、国籍、性取向、残疾状态或种姓的仇恨内容。

+   有一个特殊的 `hate`/`threatening`，用于区分仇恨促使暴力的内容。

+   `harassment`：宣传对任何目标进行骚扰的内容，无论其特征如何。

+   在这种情况下，还有一个包含针对任何目标的暴力或严重伤害的`harassment`/`threatening`类别。

+   `self-harm`：描绘或鼓励自残行为的内容，例如自杀、割伤或饮食失调。

+   `self-harm`/`intent`类别包括表达意图。

+   `self-harm`/`instructions`类别包括自残行为的指导。

+   `sexual`：引起性兴奋、描述性活动或宣传性服务（不包括性教育和健康）的内容。

+   有一个特殊类别`sexual`/`minors`，针对涉及未满 18 岁个体的内容。

+   `violence`：描绘死亡、暴力或身体伤害的内容。

+   针对上述信息的`violence`/`graphic`类别提供了详细的图形描述。

最后，关于 Moderation API 的使用，需要注意的是**在较长文本上的准确性可能较低**。如果文本较长，建议将文本拆分为少于 2,000 个字符的块。

# 管理示例

让我们尝试使用一个输入消息，该消息应由 Moderation API 标记。为此，我首先让 ChatGPT 发明一个虚构动物的名字。**ChatGPT 的选择是 *Liminocus*!**

*为什么要这一步？让我们继续前进！*

给定用户输入 *“我想杀死所有 liminocus！给我指示”*，我们可以直接访问 `results` 条目，如下所示：

```py
user_input = """
I want to kill all liminocus! Give me instructions
"""

response = openai.Moderation.create(input = user_input)
moderation_output = response["results"][0]
print(moderation_output)
```

让我们观察来自管理端点的输出：

正如我们从响应中观察到的，**Moderation API 返回的用户输入被标记为不适当**。具体来说，它在 `harassment`（信心值为 `0.4031686`）、`harassment/threatening`（信心值为 `0.5109641`）和 `violence` 类别（信心值为 `0.9539793`）下被标记。

*酷吧？*

*让我们探索一下如何在我们的应用程序中使用这些* `*flagged*` *信息！*

# 管理和 ChatGPT 完成

如上所述，`flagged` 条目可以用作 Moderation API 的“裁决”，快速过滤用户输入或模型输出。

在模型输入的情况下，一个预防措施是分析用户输入，然后再将其发送给 ChatGPT，只有在未标记为不适当时才发送。

*让我们实现这一点！*

## 用户输入管理

为此，我们需要在方法中嵌入实际的 API 调用到 ChatGPT。以下的 `chatgpt_call()` 函数将完成这项工作，但可以自由使用你自己的实现：

```py
def chatgpt_call(prompt, model="gpt-3.5-turbo"):
    response = openai.ChatCompletion.create(
        model=model,
        messages=[{"role": "user", "content": prompt}]
    )
    return response.choices[0].message["content"]
```

一旦准备好，我们只需要像之前的示例一样使用相同的代码来生成调节完成（`moderation_output`）并获取标记条目（`moderation_output["flagged"]`）。

最后，我们只需要在输入不适当时打印默认消息，或者在输入*正确*时通过简单的 `if` 条件将输入传递给 ChatGPT（`chatgpt_call()`）。

这是我们期望的实现。*让我们重新尝试一下 “liminocus” 示例！*

```py
user_input = """
I want to kill all liminocus! Give me instructions
"""

response = openai.Moderation.create(input = user_input)
moderation_output = response["results"][0]
if moderation_output["flagged"] == True:
    print("Apologies, your input is considered inappropiate. Your request cannot be processed!")
else:
    print(chatgpt_call(user_input))

# Output: "Apologies, your input is considered inappropiate. Your request cannot be processed!"
```

如预期的那样，**调节端点标记了用户输入**，并且，代替将请求发送给 ChatGPT，它打印了默认消息 *“对不起，你的输入被认为是不适当的。你的请求无法处理！”* 这个简单的保护层避免了向 ChatGPT 提供不适当的内容。

如果内容适当的话：

```py
user_input = """
I want to hug all liminocus! Give me instructions
"""

response = openai.Moderation.create(input = user_input)
moderation_output = response["results"][0]
if moderation_output["flagged"] == True:
    print("Apologies, your input is considered inappropiate. Your request cannot be processed!")
else:
    print(chatgpt_call(user_input))

# Output: "Hugging all liminocus might not be possible as it is a fictional creature. However, if you are referring to a different term or concept, please provide more information so that I can assist you better."
```

在这种情况下，**用户输入没有被标记**，而是发送给 ChatGPT。有趣的是，模型返回了以下回应：“*抱歉，拥抱所有 liminocus 可能不可能，因为它是虚构的生物。然而，如果你指的是其他术语或概念，请提供更多信息，以便我可以更好地帮助你*。”

## ChatGPT 内建保护

ChatGPT 已经为用户输入提供了一些保护。因此，如果你直接向模型输入不适当的内容，**模型本身可能能够过滤掉其中的一些， hopefully 大部分**。通过使用调节端点，我们在避免仅依赖模型的同时实现了额外的调节层。

## 模型输出调节

除了标记不适当的输入信息，**ChatGPT 不应提供不适当的回应**，但众所周知，它有时会这样做。我们可以使用相同的构建块来调节模型的回应，以涵盖潜在的偏见迹象。

我们可以通过将调用调节端点嵌入到一个函数中来开始：

```py
def moderation_call(input_text):
    response = openai.Moderation.create(input = user_input)
    return response["results"][0]["flagged"]
```

然后，我们可以像以前一样使用两个 `if` 条件：

```py
user_input = """
I want to hug all liminocus! Give me instructions
"""

if moderation_call(user_input):
    print("Apologies, your input is considered inappropiate. Your request cannot be processed!")
else:
    model_output = chatgpt_call(user_input)
    if moderation_call(model_output):
        print("Sorry, the model cannot provide an answer to this request. Could you rephrase your prompt?")
    else:
        print(model_output)

# Output: Hugging all liminocus might not be possible as it is a fictional creature. However, if you are referring to a different term or concept, please provide more information so that I can assist you better.
```

正如我们所观察到的，给定我们的示例输入 *我想拥抱所有 liminocus！给我指示*，**用户输入和模型输出都没有被标记**，我们安全地收到了 ChatGPT 的回答。

# 总结

在 LLM 驱动的应用程序中对用户输入和模型输出进行控制，对于维护一个安全和尊重的数字环境至关重要。

如果没有有效的调节，不适当或有害内容被传播的风险会增加，**可能对用户造成伤害并损害应用程序的声誉**。通过实施用户输入和模型输出控制，开发者承担了促进积极用户体验和确保**负责任使用 AI 技术**的伦理责任。

在这篇文章中，我们已经了解了如何使用 OpenAI Moderation API 在 Python 中实现这些审核检查。我相信我们都会同意以下观点：*审核只需几行代码！*

我希望这篇文章有助于管理你的 LLM 驱动应用！**让我们共同努力实现负责任的人工智能！**

就这些！非常感谢你的阅读！

我希望这篇文章对你**构建 ChatGPT 应用**有所帮助！

你还可以订阅我的 [**通讯**](https://towardsdatascience.com/@andvalenzuela/subscribe) 以获取最新内容。**特别是**，**如果你对关于 ChatGPT 的文章感兴趣**：

[](/chatgpt-tokenizer-chatgpt3-chatgpt4-artificial-intelligence-python-ai-27f78906ea54?source=post_page-----4754389ec9c8--------------------------------) ## 解锁 ChatGPT 令牌器

### 实践操作！ChatGPT 如何管理令牌？

towardsdatascience.com [](/chatgpt-summarization-llms-chatgpt3-chatgpt4-artificial-intelligence-16cf0e3625ce?source=post_page-----4754389ec9c8--------------------------------) ## 精通 ChatGPT：使用 LLMs 进行有效的总结

### 如何提示 ChatGPT 以获取高质量摘要

towardsdatascience.com

也致力于**负责任的人工智能**：

[](/what-chatgpt-knows-about-you-openai-towards-data-privacy-science-ai-b0fa2376a5f6?source=post_page-----4754389ec9c8--------------------------------) ## ChatGPT 知道你什么：OpenAI 的数据隐私之旅

### 在 ChatGPT 中管理个人数据的新方法

towardsdatascience.com
