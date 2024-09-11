# 使用 GPT-4 进行标签自助标注

> 原文：[https://towardsdatascience.com/bootstrapping-labels-with-gpt-4-8dc85ab5026d?source=collection_archive---------2-----------------------#2023-06-09](https://towardsdatascience.com/bootstrapping-labels-with-gpt-4-8dc85ab5026d?source=collection_archive---------2-----------------------#2023-06-09)

![](../Images/7364bb5c4883661737f7aa5783ca675d.png)

（来源：作者使用 DALL-E 生成的图像，由作者修改。）

## 一种具有成本效益的数据标注方法

[](https://jimmymwhitaker.medium.com/?source=post_page-----8dc85ab5026d--------------------------------)[![Jimmy Whitaker](../Images/7aa8c0d58dd1244ffca7ed9003cbb1d3.png)](https://jimmymwhitaker.medium.com/?source=post_page-----8dc85ab5026d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8dc85ab5026d--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----8dc85ab5026d--------------------------------) [Jimmy Whitaker](https://jimmymwhitaker.medium.com/?source=post_page-----8dc85ab5026d--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff40e6fd303f9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbootstrapping-labels-with-gpt-4-8dc85ab5026d&user=Jimmy+Whitaker&userId=f40e6fd303f9&source=post_page-f40e6fd303f9----8dc85ab5026d---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----8dc85ab5026d--------------------------------) ·9分钟阅读·2023年6月9日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F8dc85ab5026d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbootstrapping-labels-with-gpt-4-8dc85ab5026d&user=Jimmy+Whitaker&userId=f40e6fd303f9&source=-----8dc85ab5026d---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F8dc85ab5026d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbootstrapping-labels-with-gpt-4-8dc85ab5026d&source=-----8dc85ab5026d---------------------bookmark_footer-----------)

数据标注是机器学习项目中的关键组成部分。它建立在古老的格言“垃圾进，垃圾出”的基础上。标注涉及为训练和评估创建带注释的数据集。但这一过程可能既耗时又昂贵，特别是对于数据量巨大的项目。但如果我们可以利用大语言模型的进展来减少数据标注任务的成本和工作量呢？

GPT-4 是 OpenAI 开发的最先进的语言模型。它具有卓越的理解和生成类人文本的能力，并且在自然语言处理（NLP）社区及其他领域中带来了革命性的变化。在这篇博客文章中，我们将探讨如何利用 GPT-4 为各种任务引导标签。这可以显著减少标注过程中的时间和成本。我们将重点讨论情感分类，以展示提示工程如何使你能够使用 GPT-4 创建准确可靠的标签，并且这种技术还可以用于更强大的应用。

# 利用 GPT-4 的预测进行数据预标注

正如在写作中，编辑通常比创作原始作品的工作轻松。这就是为什么从预标注的数据开始比从空白状态开始更具吸引力。利用 GPT-4 作为预测引擎来预标注数据源于其理解上下文和生成类人文本的能力。因此，利用 GPT-4 减少数据标注所需的人工工作是非常理想的。这可能会带来成本节约，并使标注过程变得不那么单调。

那么我们怎么做呢？如果你使用过 GPT 模型，你可能对提示很熟悉。提示在模型开始生成输出之前设置了上下文，并且可以进行调整和工程（即提示工程），以帮助模型提供高度特定的结果。这意味着我们可以创建提示，让 GPT-4 生成看起来像模型预测的文本。对于我们的使用案例，我们将以一种引导模型生成期望输出格式的方式来设计我们的提示。

让我们以一个简单的情感分析示例为例。如果我们试图将给定的文本字符串分类为 `positive`（积极的）、`negative`（消极的）或 `neutral`（中立的），我们可以提供如下提示：

```py
"Classify the sentiment of the following text as 'positive', 'negative', or 'neutral': <input_text>"
```

一旦我们有了结构良好的提示，我们可以使用 OpenAI API 从 GPT-4 生成预测。以下是使用 Python 的一个示例：

```py
import openai
import re

openai.api_key = "<your_api_key>"

def get_sentiment(input_text):
    prompt = f"Respond in the json format: {{'response': sentiment_classification}}\nText: {input_text}\nSentiment (positive, neutral, negative):"
    response = openai.ChatCompletion.create(
        model="gpt-3.5-turbo",
        messages=[
            {"role": "user", "content": prompt}
        ],
        max_tokens=40,
        n=1,
        stop=None,
        temperature=0.5,
    )
    response_text =  response.choices[0].message['content'].strip()
    sentiment = re.search("negative|neutral|positive", response_text).group(0)
    # Add input_text back in for the result
    return {"text": input_text, "response": sentiment}
```

我们可以用一个示例来运行这个，以检查我们从 API 获得的输出。

```py
# Test single example
sample_text = "I had a terrible time at the party last night!"
sentiment = get_sentiment(sample_text)
print("Result\n",f"{sentiment}")
```

```py
Result:
{'text': 'I had a terrible time at the party last night!', 'response': 'negative'}
```

一旦我们对提示和结果感到满意，我们可以将其扩展到整个数据集。这里，我们假设一个文本文件，每行一个示例。

```py
import json

input_file_path = "input_texts.txt"
output_file_path = "output_responses.json"

with open(input_file_path, "r") as input_file, open(output_file_path, "w") as output_file:
    examples = []
    for line in input_file:
        text = line.strip()
        if text:
            examples.append(convert_ls_format(get_sentiment(text)))
    output_file.write(json.dumps(examples))
```

我们可以将带有预标注预测的数据导入 Label Studio，并让审核员验证或修正标签。这种方法显著减少了数据标注所需的人工工作，因为人工审核员只需要验证或修正模型生成的标签，而不是从头标注整个数据集。请查看我们的 [完整示例笔记本](https://gist.github.com/JimmyWhitaker/e2dbf7403f60489048287dc05ec420f6)。

请注意，在大多数情况下，OpenAI 被允许使用发送到其 API 的任何信息来进一步训练他们的模型。因此，如果我们不想更广泛地公开信息，就重要不要将受保护或私人数据发送到这些 API 进行标注。

# 在 Label Studio 中审查预标记数据

一旦我们的预标记数据准备好，我们将其导入到数据标注工具中，例如 Label Studio，以便进行审查。本节将指导你如何设置 Label Studio 项目、导入预标记数据和审查注释。

![](../Images/cb436b1216cf242646e5c6b89f62abb7.png)

图 1：在 Label Studio 中审查情感分类。（图片来源于作者，Label Studio 截图）

## 第一步：安装并启动 Label Studio

首先，你需要在机器上安装 Label Studio。你可以使用 pip 安装它：

```py
pip install label-studio
```

安装 Label Studio 后，通过运行以下命令启动它：

```py
label-studio
```

这将会在你默认的网页浏览器中打开 Label Studio。

## 第二步：创建新项目

点击“创建项目”，输入项目名称，例如“审查引导标签”。接下来，你需要定义标注配置。对于情感分析，我们可以使用文本 [Sentiment Analysis Text Classification](https://labelstud.io/templates/sentiment_analysis.html)。

这些模板是可配置的，因此如果我们想更改任何属性，非常简单。默认的标注配置如下所示。

```py
<View>
  <Header value="Choose text sentiment:"/>
  <Text name="my_text" value="$reviewText"/>
  <Choices name="sentiment" toName="my_text" choice="single" showInline="true">
    <Choice value="Positive"/>
    <Choice value="Negative"/>
    <Choice value="Neutral"/>
  </Choices>
</View>
```

点击“创建”以完成项目设置。

## 第三步：导入预标记数据

要导入预标记数据，请点击“导入”按钮。选择 json 文件并选择之前生成的预标记数据文件（例如，“output_responses.json”）。数据将与预填充的预测一起导入。

## 第四步：审查和更新标签

导入数据后，你可以审查模型生成的标签。注释界面将显示每个文本样本的预标记情感，审查员可以接受或更正建议的标签。

你可以通过让多个标注员审查每个示例来进一步提高质量。

通过利用 GPT-4 生成的标签作为起点，审查过程变得更加高效，审查员可以专注于验证或更正注释，而不是从头创建它们。

## 第五步：导出标记数据

一旦审查过程完成，你可以通过点击“数据管理器”标签中的“导出”按钮来导出标记的数据。选择所需的输出格式（例如 JSON、CSV 或 TSV），并保存标记的数据集以供进一步用于你的机器学习项目。

# 成本分析

我脑海中不断浮现的问题是：“这一天的费用到底是多少？”

*注意：以下显示的价格反映了作者在出版时的当前数据。未来价格可能会有所不同，或根据地理位置有所变化。*

对于语言模型，OpenAI根据请求中的令牌数量收费。令牌通常是查询中的单词数量，但特殊字符和表情符号有时也会被视为单个令牌。OpenAI的定价页面指出，“你可以将令牌视为单词的片段，其中1,000个令牌大约是750个单词。”有关令牌计数的更多信息，[请参见此页面](https://help.openai.com/en/articles/4936856-what-are-tokens-and-how-to-count-them)。

每个令牌的成本根据使用的模型不同而有所不同。例如，GPT-4 8K上下文模型的提示费用为$0.03/1K令牌，每个生成的令牌费用为$0.06/1K令牌，而GPT-3.5-turbo模型的费用为$0.002/1K令牌。

![](../Images/c57de6757c3495e22ac8e276ba4743b7.png)

OpenAI的令牌价格总结。（来源：[OpenAI论坛](https://community.openai.com/t/gpt4-and-gpt-3-5-turb-api-cost-comparison-and-understanding/106192)，图像由作者提供）

要估算预标记数据集的成本，我们可以使用一个简单的公式，考虑数据集中示例的数量、每令牌的提示和完成价格，以及每个示例的平均令牌数。

![](../Images/273980473542534d60bc3c4169530b21.png)

其中：

![](../Images/a51bc3523be67652d4ce34503a97a9df.png)

此外，我们可以通过以下方式计算数据集中令牌的总数：

![](../Images/8d1c6cb1efb7289bee820553bea2d4c4.png)

其中：

![](../Images/2eadd9c6d72e7a06d7acf3404a35b44d.png)

使用这个公式，我们可以通过将示例数量与提示费用和完成费用的总和相乘来估算预标记数据集的成本，并根据每个示例的平均令牌数进行调整。

例如，如果我们有一个包含1,000个示例的数据集，并且我们希望使用GPT-4进行情感分析的预标记，我们可以根据以下条件进行计算：每1K令牌的提示价格为$0.03，每1K令牌的完成价格为$0.06，提示长度为20令牌，每个示例的平均长度为80令牌，每个结果令牌的平均长度为3令牌，则预标记的总成本为：

![](../Images/649674d146b05fa6125f41409ff1fd76.png)

在这个例子中，使用GPT-4进行数据集的预标记将花费$3.18。注意：相同的数据集使用**GPT-3.5-turbo将花费大约$0.21**。

如果我们的预标记任务所需的专业知识较少，我们可能会希望使用一个不那么强大的模型以节省成本。通常，手动查看几个不同复杂程度的示例是值得的，以了解一个模型相对于另一个模型的准确性。有关模型的信息，请参见[OpenAI定价页面](https://openai.com/pricing)。

# 超越情感分析：标记任何NLP任务

![](../Images/2c64712f863815f74d4911ebd4f685df.png)

Label Studio中的命名实体识别截图。（图像由作者提供，截图来自Label Studio）

这种方法的优点在于它不仅限于情感分析。我们可以通过使用提示工程和引导GPT-4生成正确的输出，预先标注各种NLP任务的数据。以下是不同NLP任务的几个提示示例，所有示例都可以在Label Studio中进行审核和标注（所给示例是使用GPT-4生成的）。请记住，更复杂的任务可能需要更长的提示，这将计入你的令牌数，并因此增加成本。

## 总结

+   提示：

> 以JSON格式回应以下文本的总结：{‘summary’: summary}
> 
> 文本：我喜欢在晴天去公园。客户服务很糟糕；他们无礼且无助。我对新政策的变化既不开心也不难过。蛋糕很美味，摆盘也很棒。我对产品的体验非常糟糕；它在两天后坏了。
> 
> 总结：

响应：

```py
{'summary': 'Artificial intelligence is a rapidly developing technology that can learn from data and make decisions without being explicitly programmed. It has the potential to help solve some of the world's most pressing problems, but there are concerns about risks and ethical considerations such as bias and job displacement. It is important to consider AI's impact and ensure responsible and beneficial use.'}
```

## 分类法

提示：

> 以JSON格式回应以下文本的总结：{‘diagnosis’: diagnosis}，其中可能的诊断包括肺炎、急性支气管炎、哮喘加重、充血性心力衰竭、慢性阻塞性肺病（COPD）、肺栓塞、胸腔积液、间质性肺病或肺癌。
> 
> 文本：患者出现呼吸急促和持续性咳嗽。体检时在下肺区听到了裂纹音。胸部X光显示双侧浸润，与肺炎一致。患者开始接受抗生素治疗，并在48小时内症状有所改善。
> 
> 诊断：

响应：

```py
{'diagnosis': 'Pneumonia'}
```

## 命名实体识别（NER）

提示：

> 你是一个专业的NER标注员。只返回JSON格式。不要返回解释。以JSON格式返回以下句子的CoNLL格式。

响应：

```py
{
  "data": [
    {
      "word": "Harry",
      "entity": "B-PER"
    },
    {
      "word": "Potter",
      "entity": "I-PER"
    },
    {
      "word": "was",
      "entity": "O"
    },
    {
      "word": "a",
      "entity": "O"
    },
    {
      "word": "student",
      "entity": "O"
    },
    {
      "word": "at",
      "entity": "O"
    },
    {
      "word": "Hogwarts",
      "entity": "B-LOC"
    }
  ]
}
```

在我看来，没有什么能替代人工评估，但使用GPT模型给我们提供一个起点可能是一个巨大的优势。

# 结论

标注数据总是很困难，有时候，甚至得到一个起点也是巨大的优势。在这篇博客中，我们展示了如何使用OpenAI的GPT模型生成数据预测，以作为数据标注工作流的起点。这个过程可以显著减少人工工作量，并将标注员的注意力集中在为他们的工作提供更多价值上。查看资源以获取更多关于本博客中介绍的主题的信息。

## 资源

[完整示例笔记本](https://gist.github.com/JimmyWhitaker/e2dbf7403f60489048287dc05ec420f6) — 包含所有代码的笔记本，准备在Colab中运行

[Label Studio](https://labelstud.io/) — 开源数据标注工具

[OpenAI定价页面](https://openai.com/pricing) — 本文中的定价估算细节
