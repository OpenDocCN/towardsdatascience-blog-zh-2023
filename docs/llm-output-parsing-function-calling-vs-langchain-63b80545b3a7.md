# LLM 输出解析：函数调用与 LangChain

> 原文：[`towardsdatascience.com/llm-output-parsing-function-calling-vs-langchain-63b80545b3a7?source=collection_archive---------2-----------------------#2023-09-21`](https://towardsdatascience.com/llm-output-parsing-function-calling-vs-langchain-63b80545b3a7?source=collection_archive---------2-----------------------#2023-09-21)

## 如何使用 Open AI API 和 LangChain 函数调用一致地解析 LLM 的输出：评估这些方法的优缺点

[](https://gabrielcassimiro17.medium.com/?source=post_page-----63b80545b3a7--------------------------------)![Gabriel Cassimiro](https://gabrielcassimiro17.medium.com/?source=post_page-----63b80545b3a7--------------------------------)[](https://towardsdatascience.com/?source=post_page-----63b80545b3a7--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----63b80545b3a7--------------------------------) [Gabriel Cassimiro](https://gabrielcassimiro17.medium.com/?source=post_page-----63b80545b3a7--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3692fb93d7e5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fllm-output-parsing-function-calling-vs-langchain-63b80545b3a7&user=Gabriel+Cassimiro&userId=3692fb93d7e5&source=post_page-3692fb93d7e5----63b80545b3a7---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----63b80545b3a7--------------------------------) ·11 分钟阅读·2023 年 9 月 21 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F63b80545b3a7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fllm-output-parsing-function-calling-vs-langchain-63b80545b3a7&user=Gabriel+Cassimiro&userId=3692fb93d7e5&source=-----63b80545b3a7---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F63b80545b3a7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fllm-output-parsing-function-calling-vs-langchain-63b80545b3a7&source=-----63b80545b3a7---------------------bookmark_footer-----------)![](img/affa0a341f50bd362761ff3898004072.png)

图片由 [Victor Barrios](https://unsplash.com/pt-br/@thevictorbarrios?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/pt-br/fotografias/yjygDnvRuaI?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

使用 LLMs 创建工具需要多个组件，例如向量数据库、链、代理、文档分割器和许多其他新工具。

然而，最关键的组件之一是 LLM 输出解析。如果你不能从 LLM 接收到结构化的响应，你将很难处理生成的内容。当我们希望一次调用 LLM 输出多个信息时，这一点变得尤为明显。

让我们用一个假设的场景来说明这个问题：

我们希望 LLM 从一次调用中输出 **ingredients** 和 **steps** 来制作特定的食谱。但我们希望将这两项内容分别用于系统的两个不同部分。

```py
import openai

recipe = 'Fish and chips'
query = f"""What is the recipe for {recipe}? 
Return the ingredients list and steps separately."""

response = openai.ChatCompletion.create(
    model="gpt-3.5-turbo-0613",
    messages=[{"role": "user", "content": query}])

response_message = response["choices"][0]["message"]
print(response_message['content'])
```

这将返回如下内容：

```py
Ingredients for fish and chips:
- 1 pound white fish fillets (such as cod or haddock)
- 1 cup all-purpose flour
- 1 teaspoon baking powder
- 1 teaspoon salt
- 1/2 teaspoon black pepper
- 1 cup cold beer
- Vegetable oil, for frying
- 4 large russet potatoes
- Salt, to taste

Steps to make fish and chips:

1\. Preheat the oven to 200°C (400°F).
2\. Peel the potatoes and cut them into thick, uniform strips. Rinse the potato strips in cold water to remove excess starch. Pat them dry using a clean kitchen towel.
3\. In a large pot or deep fryer, heat vegetable oil to 175°C (350°F). Ensure there is enough oil to completely submerge the potatoes and fish.
4\. In a mixing bowl, combine the flour, baking powder, salt, and black pepper. Whisk in the cold beer gradually until a smooth batter forms. Set the batter aside.
5\. Take the dried potato strips and fry them in batches for about 5-6 minutes or until golden brown. Remove the fries using a slotted spoon and place them on a paper towel-lined dish to drain excess oil. Keep them warm in the preheated oven.
6\. Dip each fish fillet into the prepared batter, ensuring it is well coated. Let any excess batter drip off before carefully placing the fillet into the hot oil.
7\. Fry the fish fillets for 4-5 minutes on each side or until they turn golden brown and become crispy. Remove them from the oil using a slotted spoon and place them on a paper towel-lined dish to drain excess oil.
8\. Season the fish and chips with salt while they are still hot.
9\. Serve the fish and chips hot with tartar sauce, malt vinegar, or ketchup as desired.

Enjoy your homemade fish and chips!
```

这是一个庞大的字符串，解析起来会很困难，因为 LLM 可能会返回稍有不同的结构，从而破坏你编写的代码。你可以争辩说，在提示中始终要求返回“Ingredients:” 和 “Steps:” 可以解决问题，你说得没错。这可能有效，但你仍然需要手动处理字符串，并且要应对可能的变化和虚构。

# 解决方案

我们可以用几种方法解决这个问题。上述提到了一种，但还有几种经过测试的方法可能更好。在本文中，我将展示两个选项：

1.  Open AI 函数调用；

1.  LangChain 输出解析器。

## Open AI 函数调用

这是我尝试过的一种方法，并且给出了最一致的结果。我们利用 Open AI API 的函数调用能力，以便模型将响应作为结构化的 JSON 返回。

这个功能的目标是提供给 LLM 通过 JSON 输入调用外部函数的能力。模型经过微调，以理解何时需要使用特定函数。一个例子是当前天气的函数。如果你问 GPT 当前的天气，它不能告诉你，但你可以提供一个做这件事的函数并传递给 GPT，这样它就会知道可以根据输入访问它。

如果你想深入了解这个功能，可以查看 [Open AI 的公告](https://openai.com/blog/function-calling-and-other-api-updates)，以及一篇 很棒的文章。

那么让我们在代码中看看这个在我们面临的问题下会是什么样子。让我们分解一下代码：

```py
functions = [
    {
        "name": "return_recipe",
        "description": "Return the recipe asked",
        "parameters": {
            "type": "object",
            "properties": {
                "ingredients": {
                    "type": "string",
                    "description": "The ingredients list."
                },
                "steps": {
                    "type": "string",
                    "description": "The recipe steps."
                },
            },
            },
            "required": ["ingredients","steps"],
        }
]
```

我们需要做的第一件事是声明将对 LLM 可用的函数。我们必须给它一个名称和描述，以便模型理解何时应使用该函数。在这里，我们告诉它这个函数是用来返回请求的食谱的。

然后我们进入参数设置。首先，我们说明它的类型为对象，其可以使用的属性有“ingredients”和“steps”。这两个属性也都有一个描述和一个类型，以指导 LLM 输出。最后，我们指定哪些属性是调用函数时必需的（这意味着我们可以有可选字段，LLM 可以决定是否使用它们）。

现在让我们在一次调用 LLM 中使用这个：

```py
import openai

recipe = 'Fish and chips'
query = f"What is the recipe for {recipe}? Return the ingredients list and steps separately."

response = openai.ChatCompletion.create(
    model="gpt-3.5-turbo-0613",
    messages=[{"role": "user", "content": query}],
    functions=functions,
    function_call={'name':'return_recipe'}
)
response_message = response["choices"][0]["message"]

print(response_message)
print(response_message['function_call']['arguments'])
```

这里我们开始通过格式化一个可能是变量输入（食谱）的基础提示来创建对 API 的查询。然后，我们使用“gpt-3.5-turbo-0613”声明我们的 API 调用，我们将查询传递到 messages 参数中，然后传递我们的函数。

关于我们的函数有两个参数。第一个是我们传递的对象列表，格式如上所示，包含模型可以访问的函数。第二个参数“function_call”我们指定模型应该如何使用这些函数。有三种选项：

1.  “Auto” -> 模型在用户响应或函数调用之间进行决定；

1.  “none” -> 模型不调用函数，而是返回用户响应；

1.  {“name”: “my_function_name”} -> 指定一个函数名称强制模型使用它。

你可以在[这里](https://platform.openai.com/docs/api-reference/chat/create)找到官方文档。

在我们的案例中，作为输出解析我们使用了后者：

```py
function_call={'name':'return_recipe'}
```

现在我们可以查看我们的响应。我们得到的响应（在此过滤器[“choices”][0][“message”]之后）是：

```py
{
  "role": "assistant",
  "content": null,
  "function_call": {
    "name": "return_recipe",
    "arguments": "{\n  \"ingredients\": \"For the fish:\\n- 1 lb white fish fillets\\n- 1 cup all-purpose flour\\n- 1 tsp baking powder\\n- 1 tsp salt\\n- 1/2 tsp black pepper\\n- 1 cup cold water\\n- Vegetable oil, for frying\\nFor the chips:\\n- 4 large potatoes\\n- Vegetable oil, for frying\\n- Salt, to taste\",\n  \"steps\": \"1\. Start by preparing the fish. In a shallow dish, combine the flour, baking powder, salt, and black pepper.\\n2\. Gradually whisk in the cold water until the batter is smooth.\\n3\. Heat vegetable oil in a large frying pan or deep fryer.\\n4\. Dip the fish fillets into the batter, coating them evenly.\\n5\. Gently place the coated fillets into the hot oil and fry for 4-5 minutes on each side, or until golden brown and crispy.\\n6\. Remove the fried fish from the oil and place them on a paper towel-lined plate to drain any excess oil.\\n7\. For the chips, peel the potatoes and cut them into thick chips.\\n8\. Heat vegetable oil in a deep fryer or large pan.\\n9\. Fry the chips in batches until golden and crisp.\\n10\. Remove the chips from the oil and place them on a paper towel-lined plate to drain any excess oil.\\n11\. Season the chips with salt.\\n12\. Serve the fish and chips together, and enjoy!\"\n}"
  }
}
```

如果我们进一步解析到“function_call”，我们可以看到我们预期的结构化响应：

```py
{
  "ingredients": "For the fish:\n- 1 lb white fish fillets\n- 1 cup all-purpose flour\n- 1 tsp baking powder\n- 1 tsp salt\n- 1/2 tsp black pepper\n- 1 cup cold water\n- Vegetable oil, for frying\nFor the chips:\n- 4 large potatoes\n- Vegetable oil, for frying\n- Salt, to taste",
  "steps": "1\. Start by preparing the fish. In a shallow dish, combine the flour, baking powder, salt, and black pepper.\n2\. Gradually whisk in the cold water until the batter is smooth.\n3\. Heat vegetable oil in a large frying pan or deep fryer.\n4\. Dip the fish fillets into the batter, coating them evenly.\n5\. Gently place the coated fillets into the hot oil and fry for 4-5 minutes on each side, or until golden brown and crispy.\n6\. Remove the fried fish from the oil and place them on a paper towel-lined plate to drain any excess oil.\n7\. For the chips, peel the potatoes and cut them into thick chips.\n8\. Heat vegetable oil in a deep fryer or large pan.\n9\. Fry the chips in batches until golden and crisp.\n10\. Remove the chips from the oil and place them on a paper towel-lined plate to drain any excess oil.\n11\. Season the chips with salt.\n12\. Serve the fish and chips together, and enjoy!"
}
```

## 函数调用的结论

可以直接从 Open AI API 使用函数调用的功能。这允许我们每次调用 LLM 时都得到具有相同键的字典格式响应。

使用起来相当直接，你只需声明函数对象，指定名称、描述和关注于你的任务的属性，同时在描述中指定这应该是模型的响应。此外，当调用 API 时，我们可以强制模型使用我们的函数，从而使其更加一致。

这种方法的主要缺点是并不是所有的 LLM 模型和 API 都支持。所以，如果我们想使用 Google PaLM API，我们将不得不使用另一种方法。

# LangChain 输出解析器

我们可以选择一个与模型无关的替代方案，就是使用 LangChain。

首先，什么是 LangChain？

> LangChain 是一个用于开发由语言模型驱动的应用程序的框架。

这是 LangChain 的官方定义。这个框架最近创建，并已经作为构建由 LLM 驱动的工具的行业标准。

它有一个非常适合我们用例的功能，称为“输出解析器”。在这个模块中，可以创建多个对象来返回和解析来自 LLM 调用的不同类型的格式。实现的方式是，首先声明格式，并将其传递到 LLM 的提示中。然后，使用之前创建的对象来解析响应。

让我们分解代码：

```py
from langchain.prompts import ChatPromptTemplate
from langchain.output_parsers import ResponseSchema, StructuredOutputParser
from langchain.llms import GooglePalm, OpenAI

ingredients = ResponseSchema(
        name="ingredients",
        description="The ingredients from recipe, as a unique string.",
    )
steps = ResponseSchema(
        name="steps",
        description="The steps to prepare the recipe, as a unique string.",
    )

output_parser = StructuredOutputParser.from_response_schemas(
    [ingredients, steps]
)

response_format = output_parser.get_format_instructions()
print(response_format)

prompt = ChatPromptTemplate.from_template("What is the recipe for {recipe}? Return the ingredients list and steps separately. \n {format_instructions}")
```

我们首先要做的是创建我们的响应模式，这将作为解析器的输入。我们为成分和步骤各创建一个，每个都包含一个名称，这将是字典的键，以及一个描述，这将指导 LLM 如何回应。

然后我们从这些响应模式中创建我们的结构化输出解析器。有多种方法可以做到这一点，使用不同风格的解析器。了解更多信息，请查看[这里](https://python.langchain.com/docs/modules/model_io/output_parsers/)。

最后，我们获取我们的格式说明，并定义我们的提示，将食谱名称和格式说明作为输入。格式说明如下：

```py
"""
The output should be a markdown code snippet formatted in the following schema, including the leading and trailing "```json" 和 "```py":

```json

{

"ingredients": string  // 食谱中的成分，作为一个独特的字符串。

"steps": string  // 准备食谱的步骤，作为一个独特的字符串。

}

"""

```py

Now what we have left is just calling the API. Here I will demonstrate both the Open AI API and with Google PaLM API.

```

llm_openai = OpenAI()

llm_palm = GooglePalm()

recipe = '炸鱼薯条'

formated_prompt = prompt.format(**{"recipe":recipe, "format_instructions":output_parser.get_format_instructions()})

response_palm = llm_palm(formated_prompt)

response_openai = llm_openai(formated_prompt)

print("PaLM:")

print(response_palm)

print(output_parser.parse(response_palm))

print("Open AI:")

print(response_openai)

print(output_parser.parse(response_openai))

```py

As you can see it is really easy to change between models. The whole structure defined before can be used in the exact same way for any models supported by LangChain. We used also the same parser for both models.

This generated the following output:

```

# PaLM:

{

'ingredients': '''- 1 杯通用面粉\n

- 1 茶匙发酵粉\n

- 1/2 茶匙盐\n

- 1/2 杯冷水\n

- 1 个鸡蛋\n

- 1 磅白鱼片，如鳕鱼或鳕鱼\n

- 用于炸制的植物油\n- 1 杯塔塔酱\n

- 1/2 杯麦芽醋\n- 柠檬角''',

'steps': '''1\. 在一个大碗中，搅拌面粉、发酵粉和盐。\n

2\. 在另一个碗中，搅拌鸡蛋和水。\n

3\. 将鱼片浸入鸡蛋混合物中，然后在面粉混合物中裹上。\n

4\. 在深炸锅或大煎锅中加热油至 375 华氏度（190 摄氏度）。\n

5\. 将鱼片两面煎 3-5 分钟，直到金黄酥脆并完全熟透。\n

6\. 在纸巾上沥干鱼片。\n

7\. 立即将鱼片与塔塔酱、麦芽醋和柠檬角一起上桌。

'''

}

# Open AI

{

'ingredients': '1 ½ 磅鳕鱼片，切成 4 块，

2 杯通用面粉，

2 茶匙发酵粉，

1 茶匙盐，

1 茶匙新鲜磨碎的黑胡椒，

½ 茶匙大蒜粉，

1 杯啤酒（或水），

用于炸制的植物油，

塔塔酱，用于搭配',

'steps': '1\. 预热烤箱至 400°F (200°C)，并在烤盘上铺上烘焙纸。

2\. 在一个中等大小的碗中，将面粉、发酵粉、盐、胡椒粉和大蒜粉混合在一起。

3\. 倒入啤酒，搅拌至形成浓稠的面糊。

4\. 将鳕鱼浸入面糊中，确保四面都裹上。

5\. 在一个大锅或煎锅中加热约 2 英寸（5 厘米）的油，火力中高。

6\. 将鳕鱼两面煎 3 到 4 分钟，直到金黄酥脆。

7\. 将鳕鱼转移到准备好的烤盘上，烘烤 5 到 7 分钟。

8\. 热食，配以塔塔酱。'

}

```

## 结论：LangChain 输出解析

这种方法也非常好，其主要特点是灵活性。我们创建了一些结构，如响应模式、输出解析器和提示模板，这些结构可以轻松组合在一起，并与不同的模型一起使用。另一个好处是支持多种输出格式。

主要的缺点来自通过提示传递格式指令。这可能会导致随机错误和幻觉。一个真实的例子是，在这个具体的案例中，我需要在响应模式的描述中指定“作为唯一字符串”。如果我没有这样指定，模型会返回一个包含步骤和指令的字符串列表，这会导致输出解析器的解析错误。

# 结论

使用输出解析器来处理 LLM 驱动的应用有多种方法。然而，根据问题的不同，你的选择可能会有所变化。对我而言，我喜欢遵循这个思路：

我总是使用输出解析器，即使我只有一个来自 LLM 的输出。这使我能够控制和指定我的输出。如果我使用的是 Open AI，Function Calling 是我的选择，因为它具有最好的控制能力，并能避免生产应用中的随机错误。然而，如果我使用的是不同的 LLM 或需要不同的输出格式，我的选择是 LangChain，但需要对输出进行大量测试，以便构建最少错误的提示。

感谢阅读。

完整的代码可以在 [这里](https://github.com/gabrielcassimiro17/llm-output-parsing) 找到。

如果你喜欢这些内容并想要支持我，可以请我喝杯咖啡：

[](https://www.buymeacoffee.com/cassimiro?source=post_page-----63b80545b3a7--------------------------------) [## 加布里埃尔·卡西米罗是一个数据科学家，向社区分享免费内容

### 我喜欢支持创作者！

www.buymeacoffee.com](https://www.buymeacoffee.com/cassimiro?source=post_page-----63b80545b3a7--------------------------------)

这里有一些你可能感兴趣的其他文章：

[](/async-calls-for-chains-with-langchain-3818c16062ed?source=post_page-----63b80545b3a7--------------------------------) ## 使用 Langchain 进行链条的异步调用

### 如何使 Langchain 链条与异步调用 LLM 一起工作，从而加快运行顺序长时间任务的速度…

towardsdatascience.com [](/solving-unity-environment-with-deep-reinforcement-learning-836dc181ee3b?source=post_page-----63b80545b3a7--------------------------------) ## 使用深度强化学习解决 Unity 环境问题

### 使用 PyTorch 实现深度强化学习代理的端到端项目代码。

towardsdatascience.com
