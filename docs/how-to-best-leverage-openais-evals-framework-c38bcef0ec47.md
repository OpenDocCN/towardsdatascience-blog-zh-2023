# 如何最佳利用 OpenAI 的 Evals 框架

> 原文：[`towardsdatascience.com/how-to-best-leverage-openais-evals-framework-c38bcef0ec47?source=collection_archive---------2-----------------------#2023-05-23`](https://towardsdatascience.com/how-to-best-leverage-openais-evals-framework-c38bcef0ec47?source=collection_archive---------2-----------------------#2023-05-23)

![](img/91f397160d3039365854c9a5bc606c06.png)

图片由作者授权用于商业用途

## 使用 OpenAI Evals 评估 LLM 的关键

[](https://aparnadhinak.medium.com/?source=post_page-----c38bcef0ec47--------------------------------)![Aparna Dhinakaran](https://aparnadhinak.medium.com/?source=post_page-----c38bcef0ec47--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c38bcef0ec47--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----c38bcef0ec47--------------------------------) [Aparna Dhinakaran](https://aparnadhinak.medium.com/?source=post_page-----c38bcef0ec47--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff32f85889f3a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-best-leverage-openais-evals-framework-c38bcef0ec47&user=Aparna+Dhinakaran&userId=f32f85889f3a&source=post_page-f32f85889f3a----c38bcef0ec47---------------------post_header-----------) 发布于 [数据科学前沿](https://towardsdatascience.com/?source=post_page-----c38bcef0ec47--------------------------------) ·6 分钟阅读·2023 年 5 月 23 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc38bcef0ec47&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-best-leverage-openais-evals-framework-c38bcef0ec47&user=Aparna+Dhinakaran&userId=f32f85889f3a&source=-----c38bcef0ec47---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc38bcef0ec47&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-best-leverage-openais-evals-framework-c38bcef0ec47&source=-----c38bcef0ec47---------------------bookmark_footer-----------)

*本博客由* [*Trevor LaViale*](https://www.linkedin.com/in/trevor-laviale/) *共同撰写*

根据近期的一项[调查](https://arize.com/blog/survey-massive-retooling-around-large-language-models-underway/)，近一半（43%）的数据科学和工程团队计划在未来一年内部署大型语言模型（LLMs）。LLMs 的时代确实已经来临；然而，评估这些模型往往具有挑战性，研究人员需要开发可靠的方法来比较不同模型的性能。

几个月前，OpenAI 开源了他们的框架，用于根据一系列基准评估 LLM。这一框架在 OpenAI 内部使用，以确保他们的新版本模型表现适当。OpenAI 的 Eval 框架是一个工具，旨在帮助研究人员和从业者评估他们的 LLM，并将其与其他先进的模型进行比较。

# Eval 框架是如何工作的？

现在你可能会想，“哇，这似乎是一个评估 LLM 的非常有用的工具，但什么是评估，我该如何使用它？”让我们深入了解细节吧！

“评估”指的是用于衡量语言模型在特定领域（例如问题回答或情感分析）表现的具体任务。这些评估通常是标准化的基准，用于比较不同的语言模型。Eval 框架提供了一个标准化的接口，用于运行这些评估并收集结果。

从本质上讲，评估是一个数据集和一个在 YAML 文件中定义的评估类。下面展示了一个评估示例（该示例取自 [Github 存储库](https://github.com/openai/evals)）：

让我们解析一下以上内容的意思：

+   test-match: 这是评估的名称

+   id: 这是 *test-match* 的别名的评估全名

+   description: 评估的描述。

+   metrics: 评估的指标。

+   class: 指定评估的模块/类路径。

+   args: 任何你想传递给类构造函数的内容。

+   samples_jsonl: 指向样本所在的位置，在本例中是 test_match/samples.jsonl

为了让你了解样本的样子，这些是 ***test_match/samples.jsonl*** 文件中的样本：

在 JSONL 文件中（每行一个独特的 JSON 对象的 JSON 文件），我们可以看到评估的样本。每个 JSON 对象代表模型需要完成的任务，并且在评估中计作 1 个数据点。有关 JSONL 文件的更多示例，可以访问 [registry/data/README.md](https://github.com/openai/evals/blob/main/evals/registry/data/README.md) 在 Eval Github 存储库中。

在下面的部分中，我们将介绍如何运行 test-match 评估。

# 我如何运行评估？

我们可以通过一个简单的命令来运行上述评估：

在这里我们使用 oaieval CLI 来运行这个评估。我们指定了完成函数的名称（gpt-3.5-turbo）和评估的名称（test-match）。就这么简单！*我们将在下面的部分中更深入地探讨完成函数和如何构建你的评估。* 运行该命令后，你将看到精度的最终报告打印到控制台，以及一个包含完整报告的临时文件的文件路径。这表明使用这个框架快速评估 LLM 是多么简单。接下来，让我们学习如何构建自己的评估，而不是使用已经在注册表中的评估。

# 我如何构建自己的评估？

在本节中，我们将介绍如何从现有模板构建评估，并解释完成函数以及如何构建自己的函数。

# 构建评估

## 构建样本

在这里，我们将介绍如何使用[现有模板](https://github.com/openai/evals/blob/main/docs/eval-templates.md)来加快工作速度来构建自定义评估。（如果你想构建一个完全自定义的评估，[这里有一个 README](https://github.com/openai/evals/blob/main/docs/custom-eval.md)来自 Eval GitHub 存储库。）

构建评估的第一步是构造样本。样本需要根据你选择的模板包含某些字段。每个样本需要包含一个“input”字段，代表提示，建议以[聊天格式](https://platform.openai.com/docs/guides/chat/introduction)指定。其他字段取决于你选择的评估模板。例如，假设我们使用*Match*模板。在这种情况下，我需要在聊天格式中指定“input”字段和“ideal”字段。它可能如下所示：

这告诉系统尽可能简洁地完成短语，我们提供的短语是“ABC AI 是一家专注于 ML 的公司”，预期的答案是“可观测性”。

如果你的样本在不同于 JSONL 的文件格式中，OpenAI 提供了一个 CLI 来将这些样本转换为 JSONL 文件。你可以使用以下代码由[评估存储库](https://github.com/openai/evals/blob/main/docs/build-eval.md#formatting-your-data)提供来完成这个任务：

很好，我们的样本在一个 JSONL 文件中！下一步是注册我们的评估。

## 注册你的评估

要注册评估，我们需要将一个文件添加到*evals/registry/evals/<eval_name>.yaml*。该文件的格式与上面的示例*test-match*评估格式相同。它需要包含评估名称、ID、可选描述、指标、类别和指定样本文件位置的参数。一旦我们注册了评估，就可以像运行 test-match 评估一样运行它。这就是设置你自己的评估所需的全部内容！

# 构建完成函数

## 什么是完成函数？

在 *如何运行评估？* 部分，我们简要提到你需要指定一个完成函数来运行 oaieval 命令。首先，让我们了解什么是完成。完成是模型对提示的输出。例如，如果我们给模型的提示是“告诉我加州的首府是什么”，我们期望完成是“萨克拉门托”。然而，有些提示可能需要访问互联网或其他操作来帮助模型准确回答问题，这就是完成函数的作用。完成函数允许你定义模型可能需要执行的这些操作。**oaieval** 命令中的完成函数参数可以是 CompletionFn URLs，或者是 OpenAI API 中模型的名称或注册表中的键。有关完成函数的更多信息，请参见 [此处](https://github.com/openai/evals/blob/main/docs/completion-fns.md)。

## 我如何构建自己的完成函数？

在这一部分，我们将讨论如何构建自己的完成函数。为了使你的完成函数与所有评估兼容，它需要实现几个接口。这些接口基本上只是标准化评估的输入和输出。如果你想了解更多有关这些接口的信息，请查看 [有关完成函数协议的文档](https://github.com/openai/evals/blob/main/docs/completion-fn-protocol.md)。

一旦你的完成函数实现完毕，你需要像注册我们的评估一样注册它。注册完成函数使其可以在 **oaieval** CLI 中使用。下面是一个来自 [Evals 仓库](https://github.com/openai/evals/blob/main/evals/registry/completion_fns/cot.yaml) 的注册示例：

让我们详细解析以上内容：

**cot/gpt-3.5-turbo**：这是 oaieval 将使用的完成函数的全名

**class**：这是完成函数实现的类路径

**args**：初始化时传递给你的完成函数的参数

**-> cot_completion_fn**：这是传递给 ChainOfThoughtCompletionFn 类的参数

# 使用 Eval 框架的优势

Eval 框架为研究人员和从业者提供了若干好处。

**标准化评估指标和基准**：Eval 框架提供了一套标准化的评估指标，研究人员可以用来比较他们模型的性能。这使得研究人员能够将他们的模型与其他最先进的模型在相同的基准上进行比较。

**易于使用**：Eval 框架旨在易于使用。你可以使用现有的模板快速构建自己的评估，并仅需几行代码即可开始使用，如上所示。

**灵活**：Eval 框架具有灵活性，可以用于评估模型在广泛的任务和不同基准上的表现。

**开源**：Eval Framework 是开源的，这意味着研究人员/从业者可以根据他们的特定需求使用和修改它。此外，任何人都可以为[openai/evals Github 仓库](https://github.com/openai/evals)做贡献，这将有助于众包更多的基准测试，供社区共享。

## 结论

OpenEvals 是一个强大的工具，用于[评估 LLMs](https://arize.com/blog-course/evals-openai-simplifying-llm-evaluation/)，为研究人员和从业者提供了一套标准化的评估指标和任务，以将他们的模型与其他最先进的模型进行比较。考虑到它的好处，这个框架很可能在未来成为评估和比较模型性能的有用工具，因为 LLMs 将不断改进。祝评估愉快！
