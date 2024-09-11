# GPT-4 带来的 AI 新视角

> 原文：[`towardsdatascience.com/what-gpt-4-brings-to-the-ai-table-74e392a32ac3?source=collection_archive---------9-----------------------#2023-04-14`](https://towardsdatascience.com/what-gpt-4-brings-to-the-ai-table-74e392a32ac3?source=collection_archive---------9-----------------------#2023-04-14)

## 自然语言处理

## 一种语言模型及更多内容

[](https://medium.com/@doziesixtus?source=post_page-----74e392a32ac3--------------------------------)![Dozie](https://medium.com/@doziesixtus?source=post_page-----74e392a32ac3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----74e392a32ac3--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----74e392a32ac3--------------------------------) [Dozie](https://medium.com/@doziesixtus?source=post_page-----74e392a32ac3--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fcbd576093dd8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-gpt-4-brings-to-the-ai-table-74e392a32ac3&user=Dozie&userId=cbd576093dd8&source=post_page-cbd576093dd8----74e392a32ac3---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----74e392a32ac3--------------------------------) ·7 分钟阅读·2023 年 4 月 14 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F74e392a32ac3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-gpt-4-brings-to-the-ai-table-74e392a32ac3&user=Dozie&userId=cbd576093dd8&source=-----74e392a32ac3---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F74e392a32ac3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-gpt-4-brings-to-the-ai-table-74e392a32ac3&source=-----74e392a32ac3---------------------bookmark_footer-----------)![](img/0e0c0b42cb53b2761b7503bab87e66ba.png)

图片来自[Unsplash](https://unsplash.com/photos/Snqm29dhfOk)

期待已久的最新生成预训练变换器（GPT）模型终于发布了。OpenAI 的 GPT 模型第四版在前几版的基础上有了一些改进，并且新增了一些扩展功能。像前几代模型一样，GPT-4 使用**半监督训练**进行训练和微调。GPT 模型中使用的半监督训练是通过两个步骤完成的：无监督生成预训练和有监督的判别微调。这些训练步骤帮助绕过了其他语言模型因标注数据不良而面临的语言理解障碍。

## GPT-4 如何走到今天

OpenAI 于 2023 年 3 月 14 日发布了 GPT-4，距初次推出 GPT-1 已近五年。每次新版本发布时，这些模型的速度、理解能力和推理能力都有所提高。这些改进在很大程度上归因于训练过程中使用的数据量、模型的稳健性以及计算设备的新进展。GPT-1 在训练期间只能访问 4.5GB 的 BookCorpus 文本。GPT-1 模型的参数大小为 1.17 亿——相较于发布时存在的其他语言模型，这已经非常庞大。GPT-1 在它经过微调的不同任务中表现优异。这些任务包括自然语言推断、问答、语义相似性和分类任务。

对于那些仍对模型是否会超越 GPT-1 感到不确定的人来说，GPT-2 发布时的数字让他们大吃一惊。GPT-2 的参数大小和用于训练的文本大小大约是 GPT-1 的十倍。GPT-2 的尺寸并不是唯一的新增加内容。与 GPT-1 相比，OpenAI 去除了对特定任务进行额外微调步骤的需求。使用了少量样本学习，以确保 GPT-2 能够为单词赋予意义和上下文，而无需多次遇到这些单词。

就像 GPT-2 一样，GPT-3 和其他后续语言模型不需要针对特定任务进行额外的微调。GPT-3 的 1750 亿参数模型是在 570GB 的文本上进行训练的，这些文本来源于 Common Crawl、Web Text、英文维基百科以及一些书籍。GPT-3 的语言理解和推理能力非常深刻，进一步的改进导致了 ChatGPT 的开发，这是一个互动对话 API。OpenAI 开发了 ChatGPT，以便为用户提供一个基于网页的对话环境，使用户可以亲身体验扩展版 GPT-3 的能力，通过让语言模型根据用户的输入进行对话和响应。用户可以提出问题或请求有关模型训练范围内任何主题的详细信息。OpenAI 进一步规定了其模型能够提供的信息的范围。在涉及犯罪、武器、成人内容等的提示中，答案会特别小心。

## GPT-4 的令人兴奋的特性

每一次 GPT 的新版本发布都带来了一系列在过去看似不可能的功能。ChatGPT 以其推理和理解能力给用户留下了深刻印象。用户能够获得关于任何主题的准确回应，只要这些主题是 ChatGPT 训练文本的一部分。但也有案例显示，ChatGPT 在回应发生在模型训练后事件的查询时遇到了困难。理解新主题的难度应是预期中的，因为 NLP 模型是对文本的再现，并尝试将时间和空间中的实体映射到期望的上下文中。因此，只有在其训练数据集中存在的主题才能被回忆起，对新主题进行概括将是相当雄心勃勃的。

GPT-3 模型的推理不仅相对有限，而且是单模态的。该模型只能处理文本序列。最新发布的 GPT 在前一版本的基础上进行了改进。由于其更高的推理水平，GPT-4 模型可以更好地估计句子的上下文，并根据该上下文进行一般理解。根据对新模型能力的初步了解，其他新特性如下：

+   字数限制的增加，限制大小为 25,000 字，而 ChatGPT 的限制为 3,000 字。GPT-4 具有更大的上下文窗口，大小为 8,129 和 32,768 个标记，而 GPT-3 为 4,096 和 2,049 个标记。

+   推理和理解的改进。文本理解得更好，并对文本进行更好的推理。

+   GPT-4 是多模态的。它接受文本输入以及图像。GPT-4 能够识别和理解图像的内容，并以人类水平的准确性从图像中做出逻辑推断。

+   由 GPT-4 生成的文本更难被标记为机器生成文本。这些文本更具人类生成的特点，并利用诸如表情符号等句子特征，使文本感觉更个人化，并注入一些情感。

+   最后，我想特别提到 GPT-4 附带的新动态标志。该标志展示了该模型的多变性以及其潜在应用场景的活力。我认为这个标志可能是赋予模型的最佳身份之一。

## 真实与虚构

GPT-4 的大小可视化表示

在等待 GPT-4 发布期间的某个时间点，这张图片曾在 Twitter 上流传。这张图片是 GPT-4 规模的传闻大小的视觉表现。与 ChatGPT 使用的参数大小相比，这张图片显示了新模型参数的显著增加。虽然这张图片传达的表现可能听起来是突破性的，但这可能并非完全真实。甚至 OpenAI 的 CEO 也驳斥了关于模型大小的传闻。关于训练多模式语言模型的架构和模型参数大小的官方文档尚未发布。我们无法确定创建这种模型的方法是通过缩放过去的模型还是一些新方法。一些 AI 专家认为，缩放不会提供 AI 世界正在努力实现的急需的通用智能。

OpenAI 在文本生成方面展示了 GPT-4 的巨大优势，但我们是否曾经想过生成文本与一些标准考试的生成文本相比如何？尽管 GPT-4 在某些考试中表现相当不错，但在需要更高推理能力的考试中表现不佳。Open AI 发布的技术报告显示，GPT-4 在两个版本的 GRE 写作考试中一直处于第 54 百分位¹。这门考试是许多考试中考验研究生推理和写作能力的考试之一。可以说，GPT-4 生成的文本几乎与大学毕业生一样好，这对于一台“计算机”来说并不差。我们还可以说，这种语言模型不喜欢数学，或者更确切地说，在微积分方面表现不佳。它在 AP 微积分 BC 考试中的表现处于第 43 至 59 百分位，与同一考试委员会的生物学、历史学、英语、化学、心理学和统计学对应学科的高百分位得分相比显得较低。随着难度的增加，该模型表现出了困难。目前人类仍然处于思维的顶端。

有没有想过这些语言模型在编码方面的表现如何？GPT-4 在一些 Leetcode 任务上进行了编码能力检验。它在简单任务上的表现相当不错，但随着任务难度的增加，其表现却在持续下降。值得注意的是，GPT-4 在 Leetcode 任务的总体得分几乎与 GPT-3 相似。OpenAI 这次的表现并不比以前好，或者说他们可能没有试图将 GPT 模型打造成下一个 Github Copilot。想象一台计算机在面试编码问题上表现优于一般程序员，真是太疯狂了！

尽管某些功能与前代模型相比并未见到许多改进，值得注意的是模型在其他任务上的表现如何。

## 结论

GPT 的第四个版本展示了语言模型的范围没有限制，因为这些模型并不是多模态的，能够接受文本以外的输入。这可以被视为未来版本更高级功能的先兆。我们可能会看到一个语言模型在图像识别任务中表现得和计算机视觉模型一样好，甚至更好，这得益于 GPT-4 的图像理解能力。我们正逐步朝着通用人工智能迈进。虽然还有很长的路要走，但我们显然有一个方向，并且知道我们要去哪里。

[1]: OpenAI. (2023 年 3 月 16 日). *GPT-4 技术报告* [`cdn.openai.com/papers/gpt-4.pdf`](https://cdn.openai.com/papers/gpt-4.pdf)

## 谢谢！

*如果你喜欢我的文章，请* [*关注我*](https://medium.com/@doziesixtus) *，这样你就会在我发布故事时收到通知。我将在这个领域发布更多文章。祝你一切顺利。*
