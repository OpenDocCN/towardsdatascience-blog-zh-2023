# 生成性人工智能的法律与伦理视角

> 原文：[`towardsdatascience.com/legal-and-ethical-perspectives-on-generative-ai-5e7ba2308469`](https://towardsdatascience.com/legal-and-ethical-perspectives-on-generative-ai-5e7ba2308469)

![](img/19ee75231d984d0fe6c26de8a3515a74.png)

照片由 [charlesdeluvio](https://unsplash.com/@charlesdeluvio?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 探索 AI 生成内容的法律和伦理影响

[](https://tanuwidjajaolivia.medium.com/?source=post_page-----5e7ba2308469--------------------------------)![Olivia Tanuwidjaja](https://tanuwidjajaolivia.medium.com/?source=post_page-----5e7ba2308469--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5e7ba2308469--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----5e7ba2308469--------------------------------) [Olivia Tanuwidjaja](https://tanuwidjajaolivia.medium.com/?source=post_page-----5e7ba2308469--------------------------------)

·发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5e7ba2308469--------------------------------) ·阅读时长 7 分钟·2023 年 8 月 25 日

--

2023 年是人工智能特别是生成性人工智能的崛起之年。该技术本身并不新颖；许多科技公司已经开发并推出了具有某些“生成性”组件的桌面/网页/移动应用程序——也许你还记得曾经玩过 [SimSimi 聊天机器人](https://simsimi.com/) 或使用过 [Snapseed](https://play.google.com/store/apps/details?id=com.niksoftware.snapseed&hl=en&gl=US) 的背景去除功能？

GPT-4 的推出于 2023 年 3 月引发了该技术的崛起和公众（非科技）兴趣。ChatGPT 用户界面提供了便捷的使用方式，而 GPT-4 的高质量内容生成能力引起了轰动，人们开始看到这项技术的利用潜力。

这种生成性人工智能的民主化——来自公众的快速采用和使用——带来了一些风险和影响，包括法律和伦理方面。从法律/监管角度来看，这仍然是未开发的领域，但了解这项技术的潜在影响仍然是一个好主意。

# 生成性人工智能的法律与伦理视角

生成性人工智能，通过人类的创造性使用，可以（有意或无意地）成为法律威胁的来源。到 2023 年 7 月，生成性人工智能的监管仍然是一个相对开放和有争议的领域，但风险是迫在眉睫的。

## 版权和所有权

引用维基百科，[版权](https://en.wikipedia.org/wiki/Copyright)是一种知识产权，赋予其***所有者***对创作作品的复制、分发、改编、展示和表演的独占权，通常为期有限。在生成性 AI 中，这种版权的“所有者”方面变得**不明确**，因为在内容创作中涉及**多个参与方**，包括：

+   编写提示的**人员**

+   建立该 AI 模型的**公司/组织**

+   在训练模型中使用其作品的**艺术家**

+   生成艺术/内容的**AI**

每个参与方在内容创作中都扮演着重要角色，没有每个参与方的参与，内容就无法生成。具体来说，对于撰写提示的人，美国版权局明确表示**AI 生成的内容（图像、文字、视频等）不能获得版权**，考虑到**缺乏人类创作**和**AI 生成结果的可预测性**。

[US 版权申请中的 Kristina Kashtanova](https://www.copyright.gov/docs/zarya-of-the-dawn.pdf)明确说明了这一点，她创作了一部图画小说，其中书中的插图是使用 AI 图像生成工具 Midjourney 创建的。版权局提出的论点是“*提示更像是建议而非命令，类似于雇佣艺术家创作图像时给出的通用方向*”。简单来说，作者创建的提示类似于委托视觉艺术家时提供的项目简介，在这种情况下，项目简介的创建者不会是艺术作品的所有者。

随着技术的不断发展，这种情况可能会发生变化，更多的争论也会被提出。无论如何，在生成性 AI 创作过程中记录一些**文档，展示“*人类创作*”组件**以应对任何版权问题，总是一个好主意。

![](img/71d6f6d0b927af8815e7c9a5cc7b0845.png)

图片由[Markus Winkler](https://unsplash.com/@markuswinkler?utm_source=medium&utm_medium=referral)拍摄，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 2\. 隐私和数据保护

2023 年 4 月，[ChatGPT 在意大利曾被短暂封锁](https://techcrunch.com/2023/04/28/chatgpt-resumes-in-italy/)，原因是怀疑违反了欧盟的《通用数据保护条例》（GDPR）。意大利政府强调的主要问题之一与数据保护和隐私有关。ChatGPT 已经更新了其隐私政策和产品以应对这些问题，但仍然不是每个用户都会认真阅读和评估这些内容。

生成性 AI 中存在的隐私数据风险包括（1）数据收集的同意，（2）数据保留，以及（3）可用性风险。

+   **数据收集同意**：在初始模型创建过程中，数据从各种来源收集，可能包括个人信息，而信息的所有者并不知道这些数据被用于模型训练。一些平台提供用户选择退出的选项，以便将他们的内容从未来的 AI 模型改进中排除，但默认情况下此选项是关闭的，需要用户手动提交。

+   **数据保留**：GDPR 规定了“被遗忘权”部分，允许用户要求公司纠正其个人信息或将其完全删除。尽管公司可以尝试促进这一点，但考虑到大语言模型训练中使用的数据复杂性，关于其技术可行性的讨论仍在继续。

+   **可用性风险**：默认情况下，用户与如 ChatGPT 等 LLM 模型的交互记录会被公司收集并用于重新训练 AI 模型。在 LLM 模型的各种使用案例中，如将机器人用作治疗师或医生，用户可能无意中提供敏感信息。

作为生成 AI 的用户，我们需要**了解工具的隐私政策**——数据如何存储和使用，并且**小心我们输入的任何数据**，因为这些数据会被收集并可能由提供组织进行人工审查。

![](img/564e1ba873e8b2c6f2d183762961b809.png)

照片由[Dan Nelson](https://unsplash.com/@danny144?utm_source=medium&utm_medium=referral)拍摄，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 3\. 错误信息

使用生成 AI 时可能会发生无意（或甚至故意）的错误信息。由于其高质量内容生成能力，这些模型能够生成出色的内容，类似于真实或人类生成的内容，使得**识别内容的真实性和准确性**变得**困难**。

AI 模型容易出现“[**幻觉**](https://en.wikipedia.org/wiki/Hallucination_(artificial_intelligence))”，这是一个用来描述 AI 模型生成的事实不准确或无关输出的术语。这可能由于训练数据不足、序列/理解限制以及训练过程中固有的偏见而发生。在 AI 模型生成的内容中，常常会发现听起来合理的随机虚假信息。我们作为实际用户，需要**批判性地分析和验证模型提供的信息**，在进一步使用或分发之前。

其他生成 AI 被恶意用于错误信息的方式包括创建[***深度伪造***](https://en.wikipedia.org/wiki/Deepfake)。深度伪造是经过数字操控的合成媒体，能够令人信服地将一个人的形象替换成另一个人，主要利用深度学习技术。随着强大的生成 AI 的出现，创建此类欺骗性媒体变得越来越容易。

存在因生成内容不准确而导致的相关方受到损害的可能性，这可能构成**强有力的诽谤案件**。一位澳大利亚市长已经[威胁起诉 OpenAI 诽谤](https://www.reuters.com/technology/australian-mayor-readies-worlds-first-defamation-lawsuit-over-chatgpt-content-2023-04-05/)，因为 ChatGPT 错误地声称他因贿赂入狱。

## 4. 伦理影响

生成 AI 模型的非凡能力为各行各业的多个过程开启了新用例。然而，随着模型被用于行业中的决策过程，可能会出现伦理问题。

比如，假设像 ChatGPT 这样的 LLM 模型被用于分析候选人的简历或推荐信，在招聘或大学申请过程中。由于模型训练中的[**固有偏见**](https://medium.com/towards-data-science/beware-of-biases-in-data-analysis-accf0cb9b3a)，模型可能会无意中突出来自特定背景的候选人，违背了应给予所有候选人的平等机会。

另一个图像生成的例子是，由于训练数据的限制，生成的图像可能会偏向某种文化或人群。模型只能基于其所接受的训练生成输出，如果只训练于特定人群的数据，它将仅生成该人群的输出——最终可能无法为用户提供**代表性图像**。

一种处理方法是**在*人工监督*中使用生成 AI**。将人工评估作为 AI 使用生命周期的一部分非常重要，以确保输出的可解释性，并避免 AI 决策过程中的偏见。

![](img/f020b18957bdb94b862bfdf235d5aa8c.png)

图片由[José Martín Ramírez Carrasco](https://unsplash.com/@martinirc?utm_source=medium&utm_medium=referral)提供，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 结束语

随着生成 AI 使用的增加，用户也需关注法律和伦理问题，并能够在这一领域保护自己。尽管目前尚未明确建立相关法规，我们可以开始采取措施解决上述法律和伦理问题，防止出现不必要的问题。

+   **保护你的个人信息**。了解工具的隐私政策，注意输入到系统中的每一条信息，考虑其保密性。

+   指定并**记录 AI 生成内容的人类创作元素**。尽管目前 AI 生成内容的所有权不在提示创建者身上，但通过充分的文档记录，可能能够主张人类的创意和创作权。

+   **警惕技术的输出**。针对 AI 生成内容的幻觉问题和可能的诽谤，我们可以采取措施在分享内容之前验证信息的准确性，并在内容上添加明确的水印或说明，指明内容是 AI 生成的。

生成式 AI 是一个不断发展的领域，**随着技术的额外使用和趋势，法规将会被制定和更新**。看看法律领域如何动态地响应这一技术将会很有趣。
