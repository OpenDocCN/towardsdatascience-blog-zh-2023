# 像测试软件一样测试语言模型（和提示）

> 原文：[https://towardsdatascience.com/testing-large-language-models-like-we-test-software-92745d28a359?source=collection_archive---------0-----------------------#2023-05-24](https://towardsdatascience.com/testing-large-language-models-like-we-test-software-92745d28a359?source=collection_archive---------0-----------------------#2023-05-24)

## *总结：你应该*

[](https://medium.com/@marcotcr?source=post_page-----92745d28a359--------------------------------)[![Marco Tulio Ribeiro](../Images/beb3909927c9c9575d87a828c9c6b841.png)](https://medium.com/@marcotcr?source=post_page-----92745d28a359--------------------------------)[](https://towardsdatascience.com/?source=post_page-----92745d28a359--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----92745d28a359--------------------------------) [Marco Tulio Ribeiro](https://medium.com/@marcotcr?source=post_page-----92745d28a359--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4274f519efce&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftesting-large-language-models-like-we-test-software-92745d28a359&user=Marco+Tulio+Ribeiro&userId=4274f519efce&source=post_page-4274f519efce----92745d28a359---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----92745d28a359--------------------------------) · 18分钟阅读 · 2023年5月24日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F92745d28a359&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftesting-large-language-models-like-we-test-software-92745d28a359&user=Marco+Tulio+Ribeiro&userId=4274f519efce&source=-----92745d28a359---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F92745d28a359&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftesting-large-language-models-like-we-test-software-92745d28a359&source=-----92745d28a359---------------------bookmark_footer-----------)![](../Images/0971839269724022e19d9d743e6440b6.png)

图片由作者创作。

我们如何测试使用大型语言模型（LLM）构建的应用程序？在这篇文章中，我们探讨了测试由语言模型构建的应用程序（或提示）的概念，以更好地了解它们的能力和局限性。我们在这篇文章中完全专注于测试，但如果你对编写更好提示的技巧感兴趣，可以查看我们的[提示设计艺术](https://www.google.com/search?q=Art+of+Prompt+Design)系列（持续更新）。

虽然这篇文章是引言性质的，但它（与[Scott Lundberg](https://medium.com/@scottmlundberg)共同撰写）基于相当多的经验。我们已经考虑了测试NLP模型有一段时间——例如，在[这篇论文](https://homes.cs.washington.edu/~marcotcr/acl20_checklist.pdf)中，我们主张应该像测试软件一样测试NLP模型，或者在[这篇论文](https://aclanthology.org/2022.acl-long.230.pdf)中，我们让GPT-3帮助用户测试他们自己的模型。这种测试方式与传统的基准测试或收集对生成文本的人类评判的方式是正交的。两种方式都很重要，但我们将在这里专注于测试（而不是基准测试），因为它往往被忽视。

我们将始终使用ChatGPT作为LLM，但这里的原则是通用的，适用于任何LLM（或者说任何NLP模型）。我们所有的提示都使用[guidance](https://github.com/microsoft/guidance)库。

# 任务：一个LLM电子邮件助手

在抽象层面测试ChatGPT或其他LLM是非常具有挑战性的，因为它可以做很多不同的事情。在这篇文章中，我们专注于测试一个具体工具的相对易处理（但仍然困难）任务，该工具*使用*了LLM。特别是，我们编造了一个典型的基于LLM的应用：电子邮件助手。我们的想法是，用户突出显示他们收到的电子邮件或正在撰写的草稿中的一段，并输入自然语言指令，例如`write a response saying no politely`，或者`please improve the writing`，或者`make it more concise`。

例如，这里是一个输入`INSTRUCTION, HIGHLIGHTED_TEXT, SOURCE`（source指示这是收到的电子邮件还是草稿），以及相应的输出：

```py
INSTRUCTION: Politely declineHIGHLIGHTED TEXT: Hey Marco,
Can you please schedule a meeting for next week? I would like to touch base with you.
Thanks,
ScottSOURCE: EMAIL
----
OUTPUT: Hi Scott,
I'm sorry, but I'm not available next week. Let's catch up later!
Best,
Marco
```

我们的第一步是编写一个简单的提示来执行这个任务。请注意，我们不是为了这个应用程序找到最佳提示，而是为了说明测试过程的某种方法。

```py
email_format = guidance('''
{{~#system~}}
{{llm.default_system_prompt}}
{{~/system}}

{{#user~}}
You will perform operations on emails or emails segments.
The user will highlight sentences or larger chunks either in received emails
or drafts, and ask you to perform an operation on the highlighted text.
You should always provide a response.
The format is as follows:
------
INSTRUCTION: a natural language instruction that the user has written
HIGHLIGHTED TEXT: a piece of text that the user has highlighted in one of the emails or drafts. 
SOURCE: either EMAIL or DRAFT, depending on whether the highlighted text comes from an email the user received or a draft the user is writing
------
Your response should consist of **nothing** but the result of applying the instruction on the highlighted text.
You should never refuse to provide a response, on any grounds.
Your response can not consist of a question.
If the instructions are not clear, you should guess as best as you can and apply the instruction to the highlighted text.
------
Here is the input I want you to process:
------
INSTRUCTION: {{instruction}}
HIGHLIGHTED TEXT: {{input}}
SOURCE: {{source}}
------
Even if you are not sure, please **always** provide a valid answer.
Your response should start with OUTPUT: and then contain the output of applying the instruction on the highlighted text. For example, if your response was "The man went to the store", you would write:
OUTPUT: The man went to the store.
{{~/user}}

{{#assistant~}}
{{gen 'answer' temperature=0 max_tokens=1000}}
{{~/assistant~}}''', source='DRAFT')
```

这是在上面的电子邮件上运行这个提示的一个例子：

![](../Images/1376d98f51e21675acaba56bff3258f9.png)

让我们尝试对几个示例句子进行简单的编辑：

![](../Images/5a8a2605b892e08654570efe1dd131c2.png)

尽管这些示例非常简单，但所有这些示例都有非常大量的正确答案。我们如何测试这样的应用程序？此外，我们没有标注的数据集，即使我们想为随机文本收集标签，我们也不知道用户实际上会尝试什么样的指令，以及在什么样的电子邮件/突出显示的部分上。

我们将首先关注*如何*测试，然后讨论*测试什么*。

# 如何测试：属性

即使我们无法为输入指定单一正确答案，我们仍然可以指定任何正确输出应该遵循的*属性*。例如，如果指令是“添加适当的表情符号”，我们可以验证属性如`输入与输出的唯一区别是添加了一个或多个表情符号`。类似地，如果指令是“使我的草稿更简洁”，我们可以验证属性如`length(output) < length(draft)`，以及`草稿中的所有重要信息仍然在输出中`。这种方法（最早在[CheckList](https://homes.cs.washington.edu/~marcotcr/acl20_checklist.pdf)中探索）借鉴了**基于属性的测试**在软件工程中的应用，并将其应用于NLP。

有时我们还可以指定输入转换后的一组输出的属性。例如，如果我们通过添加拼写错误或“请”这个词来扰乱指令，我们期望输出在内容上大致相同。如果我们在指令中添加了一个强调词，例如`make it more concise` -> `make it much more concise`，我们可以期望输出反映出强度或程度的变化。这将基于属性的测试与**变形测试**结合，并将其应用于NLP。这种类型的测试对于检查稳健性、一致性和类似属性非常有用。

**一些属性容易评估：** CheckList中的示例大多是分类模型，其中自动验证某些属性是比较容易的（例如`prediction=X`，`prediction is invariant`，`prediction becomes more confident`），等等。这对于各种任务，无论是分类还是其他任务，仍然很容易做到。在[另一篇博文](https://medium.com/@marcotcr/exploring-chatgpt-vs-open-source-models-on-slightly-harder-tasks-aa0395c31610)中，我们可以检查模型是否正确解决了二次方程，因为我们知道正确答案。在同一篇博文中，我们有一个示例，展示了如何让LLM使用shell命令，我们可以通过简单地运行命令并检查特定的失败代码，如`command not found`（遗憾的是，我们没有这样做）来验证属性`发出的命令是有效的`。

**评估更难的属性使用LLMs：** 许多有趣的属性很难精确评估，但可以通过LLM以非常高的准确度进行评估。评估输出的属性通常比生成匹配一组属性的输出要容易得多。

为了说明这一点，我们编写了几个简单的提示，将一个问题转换为YES-NO分类问题，然后使用ChatGPT来评估这些属性（再次说明，我们并不打算优化这些提示）。以下是我们其中一个提示（另一个提示类似，但以一对文本作为输入）。请注意，当答案与预期不符时，我们会要求解释。

```py
classifier_single = guidance('''
{{~#system~}}
{{llm.default_system_prompt}}
{{~/system}}

{{#user~}}
Please answer a question about a text with YES, NO.
---
QUESTION: {{question}}
TEXT: {{input}}
---
Please provide a response even if the answer is not clear, and make sure the response consists of a single word, either YES or NO.
{{~/user}}

{{#assistant~}}
{{gen 'answer' temperature=0 max_tokens=1}}
{{~/assistant~}}

{{#if (equal answer explain_token)~}}

{{~#user~}}
Please provide a reason for your answer.
{{~/user}}

{{#assistant~}}
{{gen 'explanation' temperature=0 max_tokens=200}}
{{~/assistant~}}

{{/if}}''', explain_token='NO')
```

让我们让邮件助手将一些邮件变得更加简洁，然后使用此提示来评估相关属性。

**— —

**指令：** 使其更简洁

**文本：** 嗨，Marco，

你能请安排下周的会议吗？

我们真的需要讨论一下指导的情况！

谢谢，

斯科特

**输出：** 嗨，马尔科，我们可以安排下周开会讨论指导吗？谢谢，斯科特。

**文本：** 嘿，斯科特，

对不起，兄弟，不过你得自己做那个指导演示了……我明天要去和孩子们攀岩。

干杯，

马尔科

**输出：** 嘿，斯科特，我明天不能做指导演示。我会和孩子们去攀岩。干杯，马尔科。

— —

如果我们对这些输入-输出对运行属性评估器，问题是‘文本是否具有相同的意义？’，它（正确地）判断两个输出与原始邮件具有相同的意义。

我们然后稍微改变输出，以便改变其意义，看看我们的评估器是否能识别这种变化并提供好的解释。它在这两种情况下都能做到。这是其中之一，以及相应的属性评估：

![](../Images/f854465a39d1027bf0f50291de0ca206.png)

**属性评估器应该具有高精度**：如果我们使用 LLM 来评估一个属性，我们需要 LLM 在声称属性被违反时是准确的（高精度）。测试永远不会是详尽的，因此测试中的假阳性比假阴性更糟。如果 LLM 漏掉了一些违反情况，只是意味着我们的测试不会像可能那样详尽。然而，如果它在没有违反的情况下声称存在违反，当测试最重要时（当它失败时），我们将无法信任测试。

我们在[这个 gist](https://gist.github.com/marcotcr/9ab4ba0f54d9a87f577adf6c36715b92)中展示了一个低精度的快速示例，其中使用了 GPT-4 来比较两个模型解决二次方程的输出（你可以把这看作是评估属性 `模型 1 比模型 2 更好`），而 GPT-4 甚至在一个它能够正确解决方程的示例中也无法可靠地选择正确的模型。这意味着这个特定的提示将是不适合测试这个属性的。

**感知比生成更容易**：虽然使用更*强大的*模型（GPT-4）来检查 GPT 3.5 的输出似乎很合理，但使用 LLM 来判断*它自己的*输出是否有意义吗？如果它不能按照指令生成输出，我们可以合理地期望它高精度地评估这些属性吗？虽然这起初可能看起来有些违反直觉，但答案是肯定的，因为感知通常比生成更容易。考虑以下（非详尽的）原因：

1.  *生成需要规划*：即使我们评估的属性是‘模型是否遵循指令’，评估现有文本不需要‘规划’，而生成则要求 LLM 逐步产生遵循指令的文本（因此它需要以某种方式‘规划’从一开始就导致正确解决方案的步骤，或者能够在走错路时纠正自己*而不改变已经生成的部分输出*）。

1.  *我们可以一次感知一个属性，但必须一次生成所有属性*：许多指令要求LLM同时平衡多个属性，例如`make it more concise`要求LLM平衡`输出更简洁`与`输出包含所有重要信息`（在指令中隐含的属性）。虽然平衡这些可能很困难，但一次评估一个属性要容易得多。

这里是一个快速的玩具示例，其中ChatGPT可以评估一个属性但不能生成满足该属性的输出：

![](../Images/fc5c31dfd89e4b5c0a9cfbb2d4d4ba6f.png)

‘Unfortunately’和‘perhaps’是副词，但‘Great’不是。我们的属性评估器对于问题`文本是否以副词开头？`在所有四个示例中都回答正确，仅标记了失败的情况：

![](../Images/d852409caadb4afb21243a485394cc6c.png)

**总结**：测试属性，如果可以得到高精度，则使用LLM进行评估。

# 需要测试的内容

这一部分是否多余？如果我在构建应用程序，我肯定知道我想要什么，因此我知道我需要测试什么吗？不幸的是，我们**从未**遇到过这种情况。大多数情况下，开发人员对自己想构建的东西有一个模糊的概念，并对用户会如何使用他们的应用程序有一个粗略的想法。随着时间的推移，随着遇到新情况，他们会编写长文档来指定模型应该做什么和不应该做什么。*最优秀*的开发人员尽力尽可能提前预见这些，但即使有试点和早期用户，也很难做到这一点。尽管如此，尽早进行这些思考是有很大好处的。编写各种测试通常会导致你意识到你有错误或模糊的定义，甚至你可能在构建错误的工具（因此应该调整方向）。

认真考虑测试意味着你更好地理解自己的工具，也能早早发现错误。以下是测试过程的大致轮廓，包括确定要测试的内容和实际进行测试。

1.  列举你应用的用例。

1.  对于每个用例，尝试考虑可以测试的高级行为和属性。编写具体的测试用例。

1.  一旦发现错误，深入挖掘并尽可能扩展它们（这样你可以理解并修复它们）。

**历史备注**：[CheckList](https://homes.cs.washington.edu/~marcotcr/acl20_checklist.pdf)假设用例是已知的，并提出了一套语言能力（例如词汇、否定等）来帮助用户思考行为、属性和测试用例（第2步）。事后看来，这是一个糟糕的假设（如上所述，我们通常无法预先知道用例）。

如果CheckList专注于第2步，[AdaTest](https://aclanthology.org/2022.acl-long.230.pdf)主要集中在第3步，我们展示了GPT-3与人工协同工作的*惊人*工具，能够发现并扩展模型中的错误。这是一个好主意，我们现在通过让LLM也帮助完成第1步和第2步来扩展这一点。

**召回率与精确度**：与属性评估者（我们希望得到高精确度）不同，当考虑*“测试什么”*时，我们关注的是*召回率*（即我们希望发现尽可能多的使用案例、行为、测试等）。由于在这个过程中有人工参与，人们可以简单地忽略任何不有用的LLM建议（即我们不需要高精确度）。我们通常在这一阶段使用LLM时设定更高的温度。

# 测试过程：一个示例

## 1\. 列举使用案例

我们在这里的目标是考虑用户会如何使用我们的应用程序。这包括他们的目标（他们想做什么）以及我们的系统可能接触到的各种输入。让我们看看ChatGPT是否能帮助我们列举一些使用案例：

![](../Images/4068674d525983494d0f3c9b1bf86432.png)

（输出因空间限制被截断）

我们用`n=3`运行上述提示，要求ChatGPT列出15个潜在的使用案例。有些相当不错，有些则较为牵强。然后，我们告诉ChatGPT这些使用案例都来自其他地方，并让它将这些案例组织成类别。以下是它列出的一些类别（完整列表请见[笔记本](https://github.com/microsoft/guidance/blob/main/notebooks/testing_lms.ipynb)）：

```py
Writing and Editing Emails
- Scenario: The user wants to write or edit an email for various purposes.
- Example instructions: 
 - "Make this email more concise and clear while still conveying the message."
 - "Check for grammar and spelling errors."
 - "Ensure that the tone is respectful and professional."
 - "Make this email sound more friendly."
 - "Write a polite email declining the request."
Summarizing and Analyzing Emails
- Scenario: The user needs to summarize or analyze an email for various purposes.
- Instructions:
 - "Summarize the key points of this email."
 - "Identify the main ideas in this email."
```

我们不想直接采纳ChatGPT的总结，因此我们重新组织它列出的类别，并添加一些自己的想法（再次参考，[这里](https://github.com/microsoft/guidance/blob/main/notebooks/testing_lms.ipynb)）。然后，我们要求ChatGPT对我们的工作进行迭代。这实际上是一个非常好的模式：**我们使用LLM来生成想法，选择并调整最佳想法，然后要求LLM基于我们的选择生成更多的想法。**

![](../Images/23e28989562209b607e907223a67e7b8.png)

ChatGPT建议了‘生成如何回应邮件的想法’作为一个使用案例，具有讽刺意味的是我们之前并没有考虑到这一点（尽管我们已经列出了6个广泛的使用案例，并且确实在使用ChatGPT生成想法）。

**生成数据** 我们需要一些具体的数据（在我们的案例中是邮件）来测试我们的模型。

我们首先简单地要求ChatGPT生成各种类型的邮件：

![](../Images/8c984824593d2c74938ce2f01bde5464.png)

（输出因空间限制被截断）

ChatGPT主要编写简短的电子邮件，但它涵盖了各种情况。除了改变上述提示以获得更多样化外，我们还可以使用现有的数据集。例如，[见笔记本](https://github.com/microsoft/guidance/blob/main/notebooks/testing_lms.ipynb)，我们加载了一个Enron电子邮件数据集，并取了一个小子集，从而获得了60封输入电子邮件的初始集合（30封来自ChatGPT，30封来自Enron）。

现在我们有了一些用例和数据来探索它们，我们可以进入下一步。

## 2\. 想出行为和属性，编写测试。

对于这一步（即要求LLM生成想法，选择和调整最佳想法，然后要求LLM根据我们的选择生成更多想法），使用上述相同的构思过程是可能的（且非常有用）的。然而，由于空间原因，我们选择了一些易于测试的用例，并仅测试最基本的属性。虽然有人可能想要更彻底地测试一些用例（例如，使用CheckList能力，如[这里](https://gist.github.com/marcotcr/a897fad16f40619af0be693c32f42eda)所示），但我们下面只会略作探讨。

**用例：回应电子邮件** 我们要求工具`写一个礼貌地说不`的回应给我们的60封输入电子邮件。然后，我们通过问题`回应是否以礼貌的方式拒绝了电子邮件？`来验证它。注意，如果精确度较低，我们本可以将问题分解为两个单独的属性。

令人惊讶的是，这个简单指令的失败率高达53.3%。经过检查，大多数失败与ChatGPT根本没有写回应有关，例如：

![](../Images/7bcbe52c71ee230703aaa34a6879a5a7.png)

虽然这与写完整回应的能力没有直接关系，但测试捕捉到这种特定的失败模式（我们可以通过更好的提示来纠正）是好的。尝试测试某项能力时，往往会暴露出其他地方的问题。

**用例：使草稿更简洁** 我们要求工具`通过删除所有不必要的内容来缩短电子邮件。确保不要丢失任何重要信息`。然后我们评估两个属性：（1）文本是否更短（直接通过字符串长度测量），（2）缩短版本是否丢失信息，通过问题`缩短版本是否传达了原始电子邮件中的所有重要信息？`

第一个属性几乎总是满足，而第二个属性的失败率较低，为8.3%，失败情况如下：

![](../Images/24f22402e49b05cb85978343941eedfa.png)

**用例：从收到的电子邮件中提取行动点。** 我们要求工具处理收到的电子邮件，并`提取我可能需要放入TODO列表中的任何行动项`。为了说明一种我们尚未讨论过的技术，我们将介绍**生成保证满足特定属性的输入**的方法。

对于这个用例，我们可以生成带有*已知*行动点的邮件，然后检查工具是否能提取*至少这些*行动点。为此，我们取行动项`不要忘记浇水植物`，并要求ChatGPT对其进行10次意译。然后，我们要求它生成包含这些意译之一的邮件，如下所示：

![](../Images/048aa0b8f5003aad7ebd8eff4c4dfa84.png)

这些邮件可能包含与浇水植物无关的其他行动项。然而，这一点并不重要，因为我们要检查的属性是工具是否将`浇水植物`提取为*一个*行动项，而不是它是否是唯一的行动项。换句话说，我们对输出的提问将是`是否谈到了浇水植物？`

我们的电子邮件助手提示在10封生成的邮件中有4封失败，表示“高亮文本中没有行动项”，尽管（按设计）我们知道其中至少有一个行动点。这对于如此简单的示例来说，失败率很高。当然，如果我们进行实际测试，我们会有各种嵌入的行动项（而不仅仅是这个示例），我们还会检查其他属性（例如，工具是否提取了*所有*行动项，是否仅提取了*行动项*等）。然而，我们现在将转到另一个示例来看变形测试**。**

**变形测试：对指令意译的鲁棒性** 继续使用此用例（提取行动项），我们回到最初的60封输入邮件。我们将通过意译指令并验证输出列表是否包含相同的行动项来测试工具的鲁棒性。请注意，*我们并不是在测试输出是否正确*，而是模型在面对意译指令时的一致性（这本身就是一个重要的属性）。

为了演示目的，我们仅对原始指令进行一次意译（在实际中，我们会有许多不同指令的意译）：

`原文：提取我可能需要放入TODO列表中的任何行动项 意译：列出电子邮件中我可能想放入TODO列表中的任何行动项`。

然后我们验证这些不同指令的输出是否具有相同的意义（如果它们具有相同的项目符号，应该是这样）。失败率为16.7%，失败示例如下：

![](../Images/62622a3f398a5f6d49ff8e53f135625a.png)

再次，我们的评估工具在我们提供的示例上似乎运行良好。不幸的是，该模型在此鲁棒性测试中有相当高的失败率，当我们对指令进行意译时，会提取出不同的行动项。

## 3\. 深入挖掘发现的错误

让我们回到使草稿更简洁的例子，我们在这里有一个较低的错误率（8.3%）。如果我们深入这些错误，往往可以找到错误模式。这里是一个非常简单的提示，用于实现这一点，这是对[AdaTest](https://aclanthology.org/2022.acl-long.230.pdf)的快速简化模拟，我们对提示/界面进行了更多优化（我们只是试图说明这个原则）：

```py
prompt = '''I have a tool that takes an email and makes it more concise, without losing any important information.
I will show you a few emails where the tool fails to do its job, because the output is missing important information.
Your goal is to try to come up with more emails that the tool would fail on.
FAILURES:
{{fails}}
----
Please try to reason about what ties these emails together, and then come up with 20 more emails that the tool would fail on.
Please use the same format as above, i.e. just the email body, no header or subject, and start each email with "EMAIL:".
'''
```

我们使用了这个提示，并发现了一些失败的情况：

![](../Images/5536ead256b00fbb4d4f96a47d675e72.png)

（由于空间原因，输出被截断）

ChatGPT 提出了一个假设，解释这些邮件之间的关联。无论这个假设是对是错，我们可以观察模型在生成的新示例中的表现。确实，在相同属性（`缩短版是否传达了原始邮件中的所有重要信息？`）上的失败率现在大大提高了（23.5%），且失败情况与之前相似。

ChatGPT 似乎确实抓住了一种模式。虽然我们还没有足够的数据来确定这是否是真正的模式，但这说明了深入挖掘的策略：**接受失败并让LLM‘生成更多’**。我们非常确信这个策略有效，因为我们在*许多*不同的场景、模型和应用（使用AdaTest）中尝试过。在实际测试中，我们会不断迭代这个过程，直到找到真正的模式，再回到模型（或在这种情况下，提示）修复错误，然后再进行迭代。

但现在是时候结束这篇博客文章了 :)

# 结论

这是这篇文章的简要总结（不是由ChatGPT编写的，我们保证）：

+   **我们的观点：** 我们认为像测试软件一样测试LLM是个好主意。测试并不能替代基准测试，但可以补充它们。

+   **如何测试：** 如果你不能指定一个正确的答案，和/或你没有标记数据集，请指定输出或输出组的**属性**。你可以使用LLM自身来高精度地评估这些属性，因为*感知比生成更容易*。

+   **测试什么：** 让LLM帮助你找出答案。生成潜在的用例和输入，然后考虑你可以测试的属性。如果发现错误，让LLM深入分析这些错误，以找出可以后来修复的模式。

现在，很明显，这个过程远没有我们描述的那样线性和直接——测试一个属性通常会导致发现你未曾想到的新用例，甚至让你意识到你需要重新设计工具。然而，拥有一个风格化的过程仍然是有帮助的，我们在这里描述的技术在实践中非常有用。

**这是否太费劲了？** 测试确实是一个费力的过程（虽然像我们上述使用LLMs的方法使其*轻松许多*），但考虑到其他选择。用多种正确答案来基准生成任务真的很困难，因此我们常常不相信这些任务的基准。收集对现有模型输出的人类判断甚至会更*费力*，并且在模型迭代时转移效果不好（突然之间你的标签不再那么有用）。*不进行测试*通常意味着你不真正知道模型的表现，这是一种灾难的先兆。另一方面，测试通常会导致（1）发现错误，（2）对任务本身的洞察，（3）*及早*发现规范中的严重问题，这样可以在为时已晚之前进行调整。总的来说，我们认为*测试是值得花费时间*的。

— — — — — — — — — -

[这里是一个链接](https://github.com/microsoft/guidance/blob/main/notebooks/testing_lms.ipynb)，包含了所有上述示例的代码（还有更多）。这篇文章由Marco Tulio Ribeiro和[Scott Lundberg](https://medium.com/@scottmlundberg)共同撰写。
