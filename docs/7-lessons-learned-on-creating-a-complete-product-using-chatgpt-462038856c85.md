# 使用 ChatGPT 创建完整产品的7个经验教训

> 原文：[https://towardsdatascience.com/7-lessons-learned-on-creating-a-complete-product-using-chatgpt-462038856c85?source=collection_archive---------2-----------------------#2023-08-05](https://towardsdatascience.com/7-lessons-learned-on-creating-a-complete-product-using-chatgpt-462038856c85?source=collection_archive---------2-----------------------#2023-08-05)

## ChatGPT 的编码能力使得在短时间内完成整个产品变得非常简单——前提是你知道如何正确使用它

[](https://shakedzy.medium.com/?source=post_page-----462038856c85--------------------------------)[![Shaked Zychlinski 🎗️](../Images/4d050b916bccab64df3c02236b3129eb.png)](https://shakedzy.medium.com/?source=post_page-----462038856c85--------------------------------)[](https://towardsdatascience.com/?source=post_page-----462038856c85--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----462038856c85--------------------------------) [Shaked Zychlinski 🎗️](https://shakedzy.medium.com/?source=post_page-----462038856c85--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F43218078e688&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F7-lessons-learned-on-creating-a-complete-product-using-chatgpt-462038856c85&user=Shaked+Zychlinski+%F0%9F%8E%97%EF%B8%8F&userId=43218078e688&source=post_page-43218078e688----462038856c85---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----462038856c85--------------------------------) ·9分钟阅读·2023年8月5日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F462038856c85&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F7-lessons-learned-on-creating-a-complete-product-using-chatgpt-462038856c85&user=Shaked+Zychlinski+%F0%9F%8E%97%EF%B8%8F&userId=43218078e688&source=-----462038856c85---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F462038856c85&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F7-lessons-learned-on-creating-a-complete-product-using-chatgpt-462038856c85&source=-----462038856c85---------------------bookmark_footer-----------)![](../Images/d5f9f954d8d215c025a78ee8ac90a7f1.png)

通过 StableDiffusion 生成

不久前，我与您分享了如何[从 ChatGPT 创建我自己的法语导师](/how-i-coded-my-own-private-french-tutor-out-of-chatgpt-16b3e15007bb)（顺便说一下，它是[开源的](https://github.com/shakedzy/companion)）。我描述了如何设计应用（尤其是它的后端）以及如何连接和配置不同的基于 AI 的服务。但有一件事我基本上跳过了，那就是如何创建应用的前端。你看，我不是前端程序员，我对 JavaScript 的了解仅限于知道需要将其放在*<script></script>* 标签内。

但我心中设想的应用需要一个动态的用户界面。这意味着 HTML、JavaScript 和 CSS——但我完全不知道如何开始编写这些代码。

我知道的是我*想要*它看起来的样子。我心中有设计构想，我知道如果我知道如何编码的话，我会怎么做。因此，我决定采用一种新的和相当激进的方法——我会让 ChatGPT 为我编写代码。在那时，我已经有了向 ChatGPT 请求代码相关请求的经验，但从未尝试过如此复杂的任务。

好吧，当你读到这些文字时，你知道它成功了——我仅仅通过指示一个 LLM（大型语言模型）我想看到什么，就创建了一个完整的应用。我真的想再写一遍，只是为了确保我们都明白发生了什么：*一个算法仅仅通过我用简单英语解释，就编写了整个应用*。😲

尽管如此，这个过程并不像听起来那么简单——因此我想借此机会分享一些我在使用 ChatGPT 生成复杂代码时学到的技巧。

## 1\. 亲自设计

LLMs 是创建代码和内容的强大工具，但它们不*思考*——它们只能执行请求（或者至少它们会尝试）。这意味着需要你来进行思考，特别是设计。在开始向生成模型发送请求之前，确保你知道最终产品应该是什么样子的。

关于这点——你需要自行研究什么技术栈最适合你。由于你需要将复杂的应用拆分为步骤（参见下方第2点），LLM 无法预见最终产品的样子，可能会使用次优的库或服务。

例如，ChatGPT 为我生成的第一个 UI 基于*tkinter*，它创建的是一个实际应用而不是网页 UI。这使得创建动态 UI 变得更加复杂（而且现在不太标准）。另一个尝试是基于*steamlit*，它使得创建非复杂 UI 非常简单，但同样没有设计用于复杂请求（例如：*“仅在用户消息旁边添加播放录音按钮，但仅在用户录制了音频时”*）。在我的情况下，最终决定使用*Flask* 是最优的选择。

## 2\. 将其拆分为任务并从简单的开始

如果你要求 ChatGPT 一次性编写整个产品的代码，很有可能会得到一段有问题的代码。尽管它“聪明”，但不要期望它能够一次性关注所有细节。将你的设计拆分成任务和阶段，从比较简单的开始，然后逐步增加。

比如，这里是我最终设计的聊天界面，即我最初设计和计划的界面：

![](../Images/bb4cf3a47f2797d501e732b465059387.png)

聊天机器人界面

你可以看到界面上有各种按钮和功能，而我最初对 ChatGPT 的提示是：

```py
Write a Python web UI for a chatbot application. The text box where 
the user enters his prompt is located at the bottom of the screen, and 
all previous messages are kept on screen
```

没有特别的按钮，没有消息旁的个人头像，没有其他特别之处。只是一个简单的聊天界面，这将是我构建的核心。这个提示生成了 4 个文件：

+   一个作为后端的 Python 文件（使用 Flask）

+   一个 HTML 文件

+   一个 JavaScript 文件（使用 jQuery）

+   一个 CSS 文件

一旦我有了这些，我就可以开始使产品变得更复杂。

你可能会觉得我自相矛盾 —— 说要将应用拆分成小步骤，但又承认我的第一次提示生成了四个文件。每个对 ChatGPT 的请求之间，在完成任务所需的代码量与其非标准和特定性的权衡。要求生成整个聊天界面会得到比较通用的东西，但需要大量代码。要求“*在导师消息旁边添加一个翻译按钮*”，并且确保它位于消息气泡的右侧，始终在垂直中心，并且在 *播放声音* 按钮上方，这样的请求非常具体，因此它将是一个单独的请求。

## 3\. 仔细解释你真正想要的

每个对你产品的请求和添加都可能涉及到多个文件的更改（每个文件可能有不止一个更改）。这意味着每次请求时都会创建新的变量、函数和端点，并且会从不同的位置引用它们。 ChatGPT 会为这些提供名称，并尽力提供有意义的名称 —— 但前提是你要解释清楚上下文。

比如，如果你想在你的产品中添加一个“保存”按钮，最好像这样提问：

```py
Add a "Save Session" button to the left of the text box. It should have 
a floppy-disk icon. Once clicked, all messages on the UI will be saved to 
a JSON file named "saved_session.json"
```

而不是这样的缺乏上下文的提示：

```py
Add a button to the left of the text box wth a floppy-disk icon. Once 
clicked, all messages on the UI will be saved to a JSON file.
```

倾向于丰富上下文的提示会产生更好的命名约定。

## 4\. 非常清楚地知道你要问什么

这是我遇到的一个真正的问题，之前没预见到：我希望 UI 显示来自我的法语导师的生成文本，并且是实时流式显示，类似于 ChatGPT 的效果。我使用的 Python API（OpenAI *ChatCompletion* API）返回了一个 Python *Generator*，这需要被消费并打印到屏幕上。因此，我问 ChatGPT：

```py
Write a JavaScript function that consumes the generator and updates the 
message text one item at a time
```

我不知道的是——因为我从未写过任何严肃的 JavaScript 代码——我要求的东西是不可能的；JavaScript 无法处理 Python 生成器。结果是 ChatGPT 给了我各种奇怪且完全无用的解决方案，因为它试图完全按照我的要求——修改 JavaScript 代码。

你必须记住，ChatGPT 尝试完全按照你的要求来满足你的请求，只要这些请求不违反其指导方针。在那时我真正需要的是它*告诉我*我要求的东西很愚蠢，但这并不是它的工作方式。

这个问题只有在*我发现*自己在要求不可能的东西（老办法——Google 和 StackOverflow）之后才得到解决，并将我的提示更改为类似以下内容：

```py
Given the response generator, add functionality to consume the generator 
and updates the message text one item at a time 
```

这导致对 JavaScript *和* Python 文件进行了修改，从而实现了预期的结果。

![](../Images/38122452e132858f6d754376a14f10a9.png)

使用 StableDiffusion 生成

## 5\. LLM 无法还原其代码（以及如何还原）

虽然 ChatGPT 在编写代码方面表现出色，但它仍然只是一个语言模型，它在还原自己修改的内容方面做得不好——尤其是当你要求它还原并回到两三次之前的提示时。与 LLMs 进行分阶段生成代码时，我强烈建议始终保留你满意的最后一个工作版本的代码副本；这样，如果 ChatGPT 添加的新代码出现问题且无法修复，你可以轻松将代码还原到最后一个正常工作的版本。

但有一个问题——因为如果你恢复了你的代码，你还*需要*恢复 ChatGPT，以确保它确切知道你代码现在的样子。最好的方法是启动一个新会话，并用类似下面的提示开始：

```py
I'm building a chatbot application. Here is my code so far:

HTML:
```

你的 HTML 代码

```py

JavaScript:
```

你的 JavaScript 代码

```py

CSS:
```

你的 CSS 代码

```py

Python:
```

你的 Python 代码

```py

Add a "Save Session" button to the left of the text box. It should have 
a floppy-disk icon. Once clicked, all messages on the UI will be saved to 
a JSON file named "saved_session.json"
```

（你也可以将文件上传到 ChatGPT 的代码解释器，那时还不可用）。如果提示过长无法作为单条消息发送，将其拆分为两部分。在这两条消息之间，点击*“停止生成”*，以防止机器人插入不必要的文本。

## 6\. 不要太长时间地与之对抗

使用 ChatGPT 编码的一个酷炫之处是，如果它写出的代码有问题，或者代码没有按预期执行，你可以直接发送错误信息，它会相应地修复代码。

但这并不总是发生。有时 ChatGPT 无法修复错误，或者反而引入了另一个错误。我们会将新的错误信息发送给它，并再次要求它修复。如果这种情况发生超过两三次，代码有可能会变得非常破碎或过度修改，从而完全无法工作。如果你达到了这一点，请停止，恢复（见上文），并重新表述你的请求。

## 7\. 学习如何编写提示

尽管 ChatGPT 的核心在于你可以使用日常语言与其互动，但正确编写提示可以对结果产生巨大影响。我真的推荐花时间学习如何做到这一点。例如，这个由 OpenAI 和 DeepLearning.AI 提供的[免费课程](https://www.deeplearning.ai/short-courses/chatgpt-prompt-engineering-for-developers/)是必修的，特别是关于如何将指令、代码和示例结合在一个提示中的课程。

你可以学到的最重要的事情之一是确保在提示中自由文本和代码之间有明显的区别。因此，避免这样做：

```py
Here's a Python function: 
def func(x): 
  return x*2
Change it so it'll return the root of the absolute value of the input if 
it's negative.
```

这样写：

```py
Here's a Python function: 
```

def func(x):

return x*2

```py
Change it so it'll return the root of the absolute value of the input if 
it's negative.
```

此外，如果可能的话，提供输入输出示例。这是向 LLM 解释它应该做什么的最佳方法，因为它消除了你请求中的任何模糊性（*如果输入是正数，模型应该返回什么？保持 x*2 还是什么都不做？*）：

```py
Here's a Python function: 
```

def func(x):

return x*2

```py
Change it so it'll return the root of the absolute value of the input if 
it's negative.

Examples:
Input: 2, Output: 4
Input: -9, Output: 3
```

## 额外提示：选择合适的 LLM

记住，“ChatGPT”是一个网络产品的名称，而不是模型本身。免费版让你使用 GPT-3.5，而付费版则包括 GPT-4，它在编码任务中表现显著更好。新的代码解释器也使其表现更好，因为它可以*运行和测试*其代码。

即使你决定选择另一个 LLM 来合作，确保你选择的那个在编码任务中表现良好。否则，这些提示将毫无帮助。

在总结这一切时，我想最重要的是认识到与 LLMs 沟通时*每个词都很重要*。LLMs 不会思考，也不能真正理解我们想要什么，除非明确以*它们需要的方式*向它们解释，因为——谢天谢地——它们还不是人类（还没？），它们只是工具。就像每一个工具一样——如果你不知道怎么使用它，你就无法完成任何工作。我确实希望你能在下一个项目中找到这些提示的用处！

![](../Images/72db9dae8fd870a11c548f15b52a7292.png)

使用 StableDiffusion 生成
