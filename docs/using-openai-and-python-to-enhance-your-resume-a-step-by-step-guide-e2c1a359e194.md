# 使用 OpenAI 和 Python 提升你的简历：一步一步的指南

> 原文：[https://towardsdatascience.com/using-openai-and-python-to-enhance-your-resume-a-step-by-step-guide-e2c1a359e194?source=collection_archive---------0-----------------------#2023-02-24](https://towardsdatascience.com/using-openai-and-python-to-enhance-your-resume-a-step-by-step-guide-e2c1a359e194?source=collection_archive---------0-----------------------#2023-02-24)

## 这是一个成功故事的经历，只需几个小时的工作

[](https://piero-paialunga.medium.com/?source=post_page-----e2c1a359e194--------------------------------)[![Piero Paialunga](../Images/de2185596a49484698733e85114dd1ff.png)](https://piero-paialunga.medium.com/?source=post_page-----e2c1a359e194--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e2c1a359e194--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----e2c1a359e194--------------------------------) [Piero Paialunga](https://piero-paialunga.medium.com/?source=post_page-----e2c1a359e194--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F254e653181d2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fusing-openai-and-python-to-enhance-your-resume-a-step-by-step-guide-e2c1a359e194&user=Piero+Paialunga&userId=254e653181d2&source=post_page-254e653181d2----e2c1a359e194---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e2c1a359e194--------------------------------) ·10 min read·2023年2月24日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe2c1a359e194&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fusing-openai-and-python-to-enhance-your-resume-a-step-by-step-guide-e2c1a359e194&user=Piero+Paialunga&userId=254e653181d2&source=-----e2c1a359e194---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe2c1a359e194&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fusing-openai-and-python-to-enhance-your-resume-a-step-by-step-guide-e2c1a359e194&source=-----e2c1a359e194---------------------bookmark_footer-----------)![](../Images/e0bc7fdecb48039eadb745a3b4c0592c.png)

图片由作者使用 Midjourney 制作

几天前，我下班回家，开始玩视频游戏。

玩视频游戏对我来说是一个很好的消遣，因为我非常糟糕，我根本不专注于游戏；我只是开始思考。我让我的思绪随意飘荡。通常，我会想到我所喜欢的东西，比如**人工智能**。

所以，当我在 PS4 上进行任务时（当然，我又输了），我在想**许多** NLP 任务，比如标记分类、下一个词预测（文本生成）、情感分析、文本分类以及其他许多任务，现在基本上可以在几秒钟内解决。

比如说，ChatGPT 是一个非常出色的聊天机器人，经过训练可以以对话的方式回答任何领域的问题。它可以总结文本、回答问题、写代码、做模仿、写歌曲、写食谱……

当前的趋势是构建更小、更具可扩展性且开源的代码，这些代码可以用来替代 ChatGPT 并且拥有免费的 API，所以重点并不仅仅是**ChatGPT**。重点在于这些所谓的“大型语言模型”，它们在大量文本上进行训练，凭借如此强大的计算能力，它们可能超越了你在笔记本电脑上可以建立的所有小型方法。

我的意思是，如果你需要总结一段文本（那不是医学上未知的、超级困难的文本）或理解一条评论是好是坏，没有理由自己开发模型，因为 OpenAI 的免费模型可能在几秒钟内就能做到这一点。

所以我在想，“什么任务需要大量工作，但现在我们可以在几秒钟内完成？”例如，**写简历**。

当然，没有人可以从零开始为我们写简历，因为他们不了解我们的职业生涯，但在线上有一些服务，你可以利用 AI 来改进你的简历。这些服务通常不是免费的，我认为现在它们已经过时了，因为 OpenAI 确实是免费的，而且可能比所有其他模型做得更好，除非那些模型是 Meta、Google 或微软的模型。😅

所以我决定使用 Streamlit 构建一个网页应用，大家可以上传自己的简历，人工智能（正如我们将看到的，更具体地说是 OpenAI）将在几秒钟内改进他们的简历。

在我看来，这款应用的工作方式如下：

![](../Images/3c362e165cbb02954370b45f31a78cba.png)

作者提供的图片

很简单，对吧？

几个小时的工作，这就是结果：

![](../Images/1c3fd7b80dff75ed2bd2693b7c6c4fb5.png)![](../Images/f29df0cd5bc6957416359a85d8ceae87.png)![](../Images/96d46579594de7a88d2893b3e4c01260.png)![](../Images/e5b10b3e0b8d0bab40d6961bd446fe74.png)

现在，这看起来像是一个已经运营了几个月的初创公司，但事实上，现在只需要我花费几个小时，一些基本的 Python 技巧，加上 Open AI 的 GPT-3 魔力，就能完成这些！

我是一个极客，如果有人在没有代码的情况下向我展示这些，我是不会相信的，所以让我在下一章中详细解释一下我做了什么！

# 0\. 一些考虑事项

在我们开始之前，有一些事情需要考虑，以使讨论更加全面。

***一个公平的假设：***

首先，HR和招聘人员的世界因多种原因而充满挑战。例如，求职市场是动态的，并且始终在不断发展。AI使用**训练**模型。这意味着一旦模型被训练，其获得的信息和性能与可能已经过时的数据集有关。

因此，我会说这个模型更多地作为**语法检查**和**文本美化器**工作，而不是一个真正审查你的简历并找到改进方法的专家。

***一个安全问题：***

提交个人数据到软件中是否安全？

嗯……是的，也不是。

在这篇文章中，我并没有邀请你提交除工作经验之外的任何东西，这些内容并不是什么秘密，因为任何拥有LinkedIn页面的人都能看到。如果你对此仍然感到不安，记住始终可以选择在本地运行我的代码，这样就不会使用任何网络应用程序，你可以将简历的所有信息保留给自己。

我不推荐在AI简历改进工具中添加个人信息，如地址、电话号码或电子邮件。

***一个伦理问题：***

在使用AI来改进你的简历时，有一些一般性的伦理考虑需要记住。

+   构建一个没有偏见的数据集是一个非常棘手的概念，因为我们所有人都可能在某种程度上**有偏见**：我们能做的唯一事情就是尽量构建一个尽可能少偏见的数据集。这也适用于这种情况。人工智能在招聘过程中的盲目和不受控制的使用，无论是招聘还是构建简历，都极具风险，因为机器学习决策算法在所有决策阶段和简历的各个部分都可能会出现偏见错误。（阅读更多 [这里](https://hbr.org/2019/05/all-the-ways-hiring-algorithms-can-introduce-bias)）

+   重要的是要对AI在简历中的使用保持透明。**如果你使用AI生成内容或优化格式，我们需要确保在你的简历中披露这一点**。这是我们在工作中应该做的事情。有时由于这些技术与我们的生活如此紧密，容易忘记正确披露，但这仍然需要提醒。还有一些工具可以用来判断文本是否由AI编写（阅读更多 [这里](http://gltr.io/)）

+   最后，记住你只是在使用**语言模型**。模型真正做的只是以一种花哨的方式基于全球数十亿文本预测“下一个词”。你对自己了解胜过计算机，因此深入挖掘，提升你的优点，给自己一些肯定，然后**再**使用语言模型来改进你的简历😊

既然我们都达成一致了，那就开始代码吧 💻

# 1\. Github！

首先，这里没有什么秘密！一切都是公开的，且在Github上！ 👇

我现在将描述所有必要的内容，以生成结果。

[](https://github.com/PieroPaialungaAI/AI_CV_improver?source=post_page-----e2c1a359e194--------------------------------) [## GitHub - PieroPaialungaAI/AI_CV_improver: 使用人工智能改进你的 CV]

### 你好 😃 感谢你来到这里！这是一个 Python 项目，将帮助你使用人工智能改进 CV…

github.com](https://github.com/PieroPaialungaAI/AI_CV_improver?source=post_page-----e2c1a359e194--------------------------------)

## 1.1\. Constants.py

让我们从简单的事情开始

**constants.py** 是一个包含……常量的文件。

它获取我们模板简历的关键，OpenAI 模型的温度，以及我们用来改进简历的提示。这就是他们所谓的“提示工程”。

> 注意！！！你需要使用你的 OpenAI 密钥来更改 OPENAIKEY。它不是公开的，你不应该共享，因此我将其命名为 fake_key。你可以在这里获取密钥 [https://openai.com/api/](https://openai.com/api/)

你可以通过更改 constants.py 文件来调整提示工程。难道这不是很简单吗？🙃

## 1.2 utils.py

**utils.py** 是一个帮助我们从 .txt 文件中提取内容并提取总结部分的文件。只是一个诚实的家伙，做他被支付的工作。

## 1.3 cv_parser.py

**cv_parser.py** 做的事情确实类似于 utils.py，我实际上不确定是否应该将代码拆分为两个 .py 文件。它只是一个工具箱；它处理模板和结果文件（分别是过程的开始和结束），并将工作经验解析成文本部分。它有点像工具所做的工作，但更相关于任务……我可能应该将它们放在同一个文件中，但我喜欢有序 😂

## 1.4 ai_improver.py

**ai_improver.py** 处理实际的 AI 部分。它通过使用 OpenAI 密钥连接到 OpenAI 来改进简历总结和列出的每一项工作经验。它还使用了我们在 constants.py 文件中构建并放置的所有提示。

## 1.5 app.py

这是我们用来运行应用程序的内容。我们用以下命令运行它

```py
streamlit run app.py
```

它做的就是……一切。

这里是所有路径连接的地方。脚本执行以下步骤：

+   它从上传的文件中获取数据，并使用 **utils.py** 和 **cv_parser.py** 解析这些数据。

+   它使用**ai_improver.py**来处理 AI 部分。**ai_improver.py** 还使用 **constants.py** 文件。

+   它允许你通过下载按钮下载 AI 改进的简历。

## 1.6 main.py

**main.py** 的功能与 **app.py** 完全一样，但在本地运行（没有流媒体）。

使用以下命令运行它

```py
python main.py
```

# **2\. 结果！**

让我们好好看看我得到的结果。

**我的输入总结**如下：

> “我是一名物理学家，已在数据科学和深度学习方面发展了强大的技能。拥有复杂系统物理学和大数据的硕士学位（成绩：110/110 荣誉）。目前在辛辛那提大学航空航天工程与工程力学系担任研究助理，同时攻读博士学位。我的研究目标是寻找使用机器学习和数据科学优化工程实验的方法。为《Towards Data Science》撰写关于流行机器学习应用和技术解决现实问题的技术文章（每月超过6万次浏览）。《青少年人工智能》课程的创作者和教师。图像处理 I 和信号处理 I 课程的开发者。”

**人工智能模型**将其输出为一个对招聘人员更具吸引力和趣味性的总结：

> 我拥有**广泛的物理学背景**，拥有复杂系统物理学和大数据的硕士学位（成绩：110/110 荣誉）。我还在数据科学和深度学习方面发展了强大的技能。我目前在辛辛那提大学航空航天工程与工程力学系担任研究助理，同时攻读博士学位。我的研究重点是寻找使用机器学习和数据科学优化工程实验的方法。**此外**，我还是《Towards Data Science》的技术写作者，撰写关于流行的机器学习应用和解决现实世界问题的技术文章（每月超过6万次浏览）。我还是《青少年人工智能》课程的创作者和教师，并且是《图像处理 I》和《信号处理 I》课程的开发者。我在这些领域的经验和知识使我培养了一套在行业中非常受欢迎的独特技能。

我不打算撒谎……这对我来说确实听起来更好。

这无疑更流畅（这很可预测，因为我不是以英语为母语的人），听起来也更激动人心。不过，它略长一些，这可能是个问题，因为我们希望在简历中节省空间。

**让我们来看一下某段工作经历的回顾**。例如，其中之一是：

> 使用机器学习创建代理模型并提高机械工程实验的效率。实践传统和深度机器学习的研究。

**而人工智能将其改正为**：

> 利用机器学习创建代理模型并优化机械工程实验。拥有传统和深度机器学习的经验，并且有成功研究的可靠记录。

…有时我感觉这有点过于大胆了。😂

我仍然会说它确实提高了文本的质量。在这种情况下，它也保持了简洁，这也是一个优点！

# 3\. 一些要点！

在这篇文章中，我分享了使用 OpenAI 的 GPT-3 模型来创建简历改进网络应用程序的经验。以下是我的经验的关键要点：

1.  Open AI 模型是一个可以执行各种 NLP 任务的工具，包括令牌分类、文本生成、情感分析和文本分类。它非常有用，以至于许多以前的模型现在基本上已经过时。

1.  这个应用的概念是**下载模板，** **填写**你的简历，**上传回去，**然后让AI为你分析。

1.  创建一个网页应用来提升你的简历非常简单。只需几个小时的代码和一些基础的 Python 编程。

1.  这让我想到，AI 驱动应用的可能性是巨大的，随着像 GPT-3 和 Streamlit 这样的工具变得更加普及，即使是不懂编程（或编程能力不强）的人员，也可以在短短几小时内创造出令人印象深刻的结果。

最后，这次经历再次展示了AI的巨大力量及其改变我们工作和生活方式的潜力。但也许我们早已知道这一点🙃

# 4\. 结论

如果你喜欢这篇文章，想了解更多机器学习的内容，或者只是想问我一些问题，你可以：

在 [**Linkedin**](https://www.linkedin.com/in/pieropaialunga/) 上关注我，我会在那里发布我的所有故事。

订阅我的 [**通讯**](https://piero-paialunga.medium.com/subscribe)。它会让你了解最新故事，并给你机会发信息给我，获取所有的修正或疑问。

成为 [**推荐会员**](https://piero-paialunga.medium.com/membership)，这样你就不会有“每月最大故事数量”的限制，你可以阅读我（以及数千位机器学习和数据科学顶级作者）关于最新技术的所有内容。
