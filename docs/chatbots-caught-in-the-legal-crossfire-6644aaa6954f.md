# 聊天机器人陷入（法律）交火中

> 原文：[`towardsdatascience.com/chatbots-caught-in-the-legal-crossfire-6644aaa6954f?source=collection_archive---------10-----------------------#2023-12-22`](https://towardsdatascience.com/chatbots-caught-in-the-legal-crossfire-6644aaa6954f?source=collection_archive---------10-----------------------#2023-12-22)

## GDPR、人工智能法、DSA 和公共体面之间

[](https://medium.com/@tea.mustac?source=post_page-----6644aaa6954f--------------------------------)![Tea Mustać](https://medium.com/@tea.mustac?source=post_page-----6644aaa6954f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6644aaa6954f--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----6644aaa6954f--------------------------------) [Tea Mustać](https://medium.com/@tea.mustac?source=post_page-----6644aaa6954f--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F109d4928877a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fchatbots-caught-in-the-legal-crossfire-6644aaa6954f&user=Tea+Musta%C4%87&userId=109d4928877a&source=post_page-109d4928877a----6644aaa6954f---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6644aaa6954f--------------------------------) ·9 分钟阅读·2023 年 12 月 22 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F6644aaa6954f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fchatbots-caught-in-the-legal-crossfire-6644aaa6954f&user=Tea+Musta%C4%87&userId=109d4928877a&source=-----6644aaa6954f---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F6644aaa6954f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fchatbots-caught-in-the-legal-crossfire-6644aaa6954f&source=-----6644aaa6954f---------------------bookmark_footer-----------)![](img/b50e7a6a7081c9804243f1dff4510ed3.png)

图片由 [Ant Rozetsky](https://unsplash.com/@rozetsky) 提供，来源于 [Unsplash](https://unsplash.com/)

最近，我一直在尝试提供一些法律建议，关于如何在不踩欧洲监管雷区的情况下实施聊天机器人。在这个过程中，我总是喜欢查看“别人是怎么做的”，然而，当涉及到这个特定问题时，这是一条相当令人失望的途径。似乎对于目前在每第三个网站上实现的技术，提供从法律到烹饪等各种建议的技术，关于如何实施这些机器人并没有太多的书面资料。当我说应该如何做时，我并不是指最佳的网络安全和安全建议或最佳的微调或训练建议。我是指如何、何时以及为哪些用途可以使用机器人，我可以使用哪些数据来训练它，以及我需要与用户分享哪些信息，以确保我的机器人至少在一定程度上符合大多数欧洲法规？（欧洲，仅仅是为了相对简单，但我提出的大多数建议也应被视为对用户的基本体面。）

无论如何，我坐下来做了我通常在面对法律麻烦时会做的事情。我写下了这些内容。

# 实施 ChatBot 101

## 1. 选择聊天机器人

尽管这听起来很简单，但这远非一个微不足道的问题。选项众多，包括使用开源代码自行构建聊天机器人。[1] 使用市场上提供的众多聊天机器人 API 中的一个，允许你进行最简单、最快速的准备设置。[2] 基于这些 API 之一对自己的聊天机器人进行微调。[3] 使用各种聊天机器人工具对聊天机器人进行微调。[4] 或者通过选择聊天机器人即服务（Chatbot as a Service）来支付别人代劳。[5]

选择这些选项中的任何一个都不无波及效应。这些波及效应当然包括设置机器人的性能和灵活性，但也包括遵守法律义务的特定要求。例如，从头开发自己的机器人或完全依赖开源代码，数据保护方面绝对是最安全的选项，因为你掌控所有的训练数据，并且数据不会流向其他地方。然而，这也有其缺点，只有在拥有足够的专家资源来确保设置和运行的同时保证一定的性能水平时，才应当考虑这个选项。相反，依赖 API 总是涉及一定程度的数据泄露风险。更不用说你依赖于别人的表现，并且至少在第一时间对他们的错误负责（GDPR 共同控制警告）。当使用其他工具进行微调时，情况当然会变得更加复杂。

最简单的选择可能是将麻烦留给别人，直接购买产品或服务。然而，除了这是最昂贵的方式（尤其是如果你想要一个高度个性化的机器人），这个选项也有其陷阱，因此在选择要雇佣的具体机器人时需要非常小心，同时考虑所有公开共享的数据处理实践、使用的训练数据等信息。否则，你可能会再次陷入麻烦，因为未能遵守应尽的尽职调查义务。

## 2\. 细调聊天机器人

一旦你选择了你的机器人，并且假设你选择了一个包括一些细调的选项，恭喜你！你刚刚从火坑跳入了火炉。不论你是使用自动细调工具还是利用开源代码，卷起袖子自己动手，喂入模型的数据与你选择模型一样重要。

我们都已经熟悉了垃圾进垃圾出的整个议程，但还有另一个可能更重要的议程需要考虑。那就是法律问题的风险。我们已经通过艺术家和报纸对大型语言模型提供者提起的诉讼对这一概念有了一定的了解。而且，很可能的情况是，一旦法律情况在那里得到澄清，这些诉讼可能会扩展到任何 1\. 使用他们的产品或服务的人以及 2\. 做类似事情的人。关键的启示当然是，跟踪该领域的法律发展，不要用（可能）违法的数据来训练你的模型。我们还可以增加一个额外的建议，避免在任何情况下将个人数据喂给你的模型。暂且不谈版权问题，在非绝对必要的情况下使用个人数据总会让你陷入麻烦。

最后一个需要考虑的可能性和潜在问题是，如今你甚至不需要对模型进行细调。你可以通过进一步的 API 调用或网站调用来不断进行所谓的细调，从中获取机器人响应的数据。如果是这种情况，确保遵守原始网站提供者对数据使用的任何限制。这些限制可以以 robots.txt 文件的形式出现，也可能在其条款和条件中说明。是的，即使是爬取和链接也有其限制。

## 3\. 免责声明

如果有一件法律专家永远不嫌多的事情，那就是‘免责声明’。因此，确保在你的聊天机器人中实现足够数量的免责声明。两个绝对不可妥协的要点是，与 AI 系统互动的人需要在开始互动之前了解这一事实，以及了解输出可能不准确且不应被依赖。这两个点可以很好地打包成一个弹出窗口，但也应在网站上的某个地方持续可见，或者用户可以被反复提醒它们的存在。这里“宁可过于透明，也不要后悔”是适用的。

隐私通知也是如此，整个通知本身就是一种免责声明。虽然大型语言模型的工作原理需要计算机科学学位才能略微理解，但你仍然需要尽量在隐私通知的有限范围内使其易于理解。想象一下如何向你的六岁孩子或可能是你的祖父母解释模型的工作原理，然后从那里开始。图片、视频和图形都非常受欢迎。另一方面，如果你使用了第 1 步中提到的任何 API 或自动化工具，你当然可以链接相关服务提供商的隐私通知，但这并不意味着你就此免责。在这种特定情况下，你是提供服务的人，是处理问题和投诉的首要联系人。因此，你有责任解释用户数据的流向、为什么这样做是必要的以及如何停止处理。这再次需要一些技能和创造力，以便做到透明和适当。祝你好运解决这个难题！

## 4\. 输出

现在我们终于到了输出部分，所以我们肯定要接近尾声了。如果你这么想，那你是对的！至少在某种程度上是如此。这个问题仍然是一座需要单独攀登的山峰。除了已经提到的免责声明，说明结果可能不准确之外，还有一些其他需要考虑的因素，因为可能出现不准确性的原因有很多。第一个原因当然是大语言模型臭名昭著的幻觉，由于它们对我们如此慷慨喂给它们的数据本质上缺乏理解。此外，除了祈祷一些非常聪明的人能找出解决方法，我们别无他法，除了实施我们的免责声明。

然而，另一方面，我们面临的情况有所不同，这适用于所有爬取其他网站以查找和输出信息的聊天机器人。因此，你现在需要问自己，如果抓取的信息是虚假的或甚至是非法的，会发生什么。对于这种情况，最好依赖于所谓的“托管例外”，即现已陈旧的电子商务指令第 14 条中的内容。例如，这一例外也适用于搜索引擎，保证了主机和中介不对他们仅提供访问的内容承担责任。不过，这仅在内容不是明显违法的情况下适用。因此，为了最大程度地简化这一点。首先，只爬取和抓取你事先检查过的可信信息来源（不要试图充当谷歌）。其次，确保在所有聊天机器人的输出中集成参考，以便所有信息的原始来源都能立即可见。

还有一件值得考虑的事，并值得投入额外的编码时间，即集成跟进问题以应对用户初始输入非常广泛或不明确的情况。通过这种方式，你的机器人可以*重新提示*用户，以便用户提供更好的提示。这将反过来使模型产生更好的输出，无论是准确性还是性能方面。

## 5\. 质量重于速度

为了再次强调这一点，因为它似乎总是归结于此。特别注意你机器人输出的质量，因为这是它们最显著和最明显的问题之一。在意大利 ChatGPT 临时禁令的争议中，不准确的输出旨在证明训练数据的不准确性。[6] 幻觉，作为一种输出缺陷，一直以来都是主要关注点，也仍然阻碍着聊天机器人进入搜索引擎领域。[7] 我们甚至不会讨论算法偏见/垃圾进垃圾出的争论。[8]

除了幻觉，这些仍然是单独的难题，输出的准确性和质量可以通过特别注意训练数据的准确性和质量大大提高。以及这些数据的相关性。此外，如果你通过 API 调用或其他方式主动获取数据，你所获取的数据也应该进行准确性、代表性以及适当性的双重检查。最后，你应该有适当的机制来识别任何必要的更新或需要更新的数据集的变化，当然，也要有一些机制来适当地回应这些识别出的事件。

质量是一个持续关注的问题，而不是一次性完成的检查项目。这一切都需要成本，主要是时间上的，这使得开发过程变得更慢。然而，质量应始终优先于速度，因为并不是所有人都能承受‘快速行动和破坏’的代价。 [9 ] 至少在他们试图开发一个可持续和负责任的商业模式时不能如此。

# 最后的思考

尽管我一般提倡一种更具深思熟虑和负责任的创新方法，但似乎行业正在倾向于‘快速行动和破坏’的口号，跑步机转动得越来越快。没有人愿意在我们这个时代最重要的比赛中落后。然而，当 OpenAI 忙于开发 AGI，律师们争相定义什么是 AI 系统时，很多初创企业和商业人士心中似乎存在着一些相关性较小的问题。然而，一旦我们考虑到这些问题的规模及其影响的参与者数量，这些问题的琐碎性就会消失。其中一个问题是如何在尽量少踩法律泥潭的情况下开发和实施聊天机器人？希望这篇文章能帮助那些试图正确行事的人们走上正确的道路。

![](img/128ed8f79d392b99e2033d003d76916f.png)

图片由 [William Navarro](https://unsplash.com/@williamnavarro_) 提供，来源于 [Unsplash](https://unsplash.com/)

 [1 ] 2023 年最佳开源聊天机器人平台, botpress, 2022 年 7 月 14 日, [`botpress.com/blog/open-source-chatbots`](https://botpress.com/blog/open-source-chatbots)。

 [2 ] Jesse Sumrak, 2023 年最佳聊天机器人 API, twilio BLOG, 2022 年 12 月 27 日, [`www.twilio.com/blog/best-chatbot-apis`](https://www.twilio.com/blog/best-chatbot-apis)。

 [3 ] Olasimbo Arigbabu, 微调 OpenAI GPT-3 以构建定制聊天机器人, Medium, 2023 年 1 月 25 日, [`medium.com/@olahsymbo/fine-tuning-openai-gpt-3-to-build-custom-chatbot-fe2dea524561`](https://medium.com/@olahsymbo/fine-tuning-openai-gpt-3-to-build-custom-chatbot-fe2dea524561)。

 [4 ] Ali Mahdi, 8 款聊天机器人替代品：哪个工具适合你？, Chatling, 2023 年 11 月 11 日, [`chatling.ai/blog/chatbot-alternatives`](https://chatling.ai/blog/chatbot-alternatives)。

 [5 ] Allen Bernard, 2023 年你应该了解的 10 大聊天机器人提供商, CMS WIRE, 2023 年 3 月 10 日, [`www.cmswire.com/digital-experience/10-chatbot-providers-you-should-know-about/`](https://www.cmswire.com/digital-experience/10-chatbot-providers-you-should-know-about/)。

 [6 ] GPDP, 人工智能：监管机构阻止 ChatGPT。非法收集个人数据。缺乏未成年年龄验证系统, 2023 年 3 月 31 日, [`www.gpdp.it/web/guest/home/docweb/-/docweb-display/docweb/9870847#english`](https://www.gpdp.it/web/guest/home/docweb/-/docweb-display/docweb/9870847#english)。

[7] 威尔·道格拉斯·赫文，聊天机器人有一天可能会取代搜索引擎。这就是为什么这是个糟糕主意的原因，《麻省理工科技评论》，2022 年 3 月 29 日，[`www.technologyreview.com/2022/03/29/1048439/chatbots-replace-search-engine-terrible-idea/`](https://www.technologyreview.com/2022/03/29/1048439/chatbots-replace-search-engine-terrible-idea/)。

[8] 伊莎贝尔·布斯凯特，人工智能的崛起将焦点集中在算法的偏见上，《华尔街日报》，2023 年 3 月 9 日，[`www.wsj.com/articles/rise-of-ai-puts-spotlight-on-bias-in-algorithms-26ee6cc9`](https://www.wsj.com/articles/rise-of-ai-puts-spotlight-on-bias-in-algorithms-26ee6cc9)；拉胡尔·阿瓦蒂，垃圾进，垃圾出（GIGO），TechTarget，[`www.techtarget.com/searchsoftwarequality/definition/garbage-in-garbage-out`](https://www.techtarget.com/searchsoftwarequality/definition/garbage-in-garbage-out)。

[9] 比阿特丽斯·诺兰，硅谷有了一个新的版本的“快速行动和打破常规”口号，商业内幕，2023 年 12 月 6 日，[`www.businessinsider.com/silicon-valley-move-fast-and-break-things-sam-altman-openai-2023-12`](https://www.businessinsider.com/silicon-valley-move-fast-and-break-things-sam-altman-openai-2023-12)。
