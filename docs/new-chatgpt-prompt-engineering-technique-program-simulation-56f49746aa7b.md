# 新的 ChatGPT 提示工程技术：程序模拟

> 原文：[https://towardsdatascience.com/new-chatgpt-prompt-engineering-technique-program-simulation-56f49746aa7b?source=collection_archive---------0-----------------------#2023-09-03](https://towardsdatascience.com/new-chatgpt-prompt-engineering-technique-program-simulation-56f49746aa7b?source=collection_archive---------0-----------------------#2023-09-03)

[](https://medium.com/@hominum_universalis?source=post_page-----56f49746aa7b--------------------------------)[![Giuseppe Scalamogna](../Images/ff7b3bec7c26e5684fba26211b6f027a.png)](https://medium.com/@hominum_universalis?source=post_page-----56f49746aa7b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----56f49746aa7b--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----56f49746aa7b--------------------------------) [Giuseppe Scalamogna](https://medium.com/@hominum_universalis?source=post_page-----56f49746aa7b--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe039aa8b7221&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnew-chatgpt-prompt-engineering-technique-program-simulation-56f49746aa7b&user=Giuseppe+Scalamogna&userId=e039aa8b7221&source=post_page-e039aa8b7221----56f49746aa7b---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----56f49746aa7b--------------------------------) · 9分钟阅读 · 2023年9月3日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F56f49746aa7b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnew-chatgpt-prompt-engineering-technique-program-simulation-56f49746aa7b&user=Giuseppe+Scalamogna&userId=e039aa8b7221&source=-----56f49746aa7b---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F56f49746aa7b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnew-chatgpt-prompt-engineering-technique-program-simulation-56f49746aa7b&source=-----56f49746aa7b---------------------bookmark_footer-----------)![](../Images/5639210e80645d1130cefa920300f7d3.png)

来源：作者提供的图像，使用 MidJourney 生成

提示工程的世界在各个层面上都非常迷人，并且有很多巧妙的方法可以引导像 ChatGPT 这样的代理生成特定类型的响应。诸如链式思维（CoT）、基于指令、N-shot、Few-shot 甚至像奉承/角色分配这样的技巧都是灵感的来源，激发了许多满足各种需求的提示库。

在这篇文章中，我将深入探讨一种技术，根据我的研究，这种技术可能尚未被充分探索。虽然我会暂时将其标记为“新”，但我会避免称其为“创新”。鉴于提示工程中的创新速度之快以及新方法的易于开发，这种技术可能已经以某种形式存在。

该技术的本质在于使 ChatGPT 以模拟程序的方式运行。我们知道，程序由一系列指令组成，这些指令通常打包成函数以执行特定任务。从某种程度上说，这种技术是基于指令和基于角色的提示技术的结合。但与这些方法不同的是，它寻求利用一个可重复且静态的指令框架，使得一个函数的输出可以影响另一个函数，并且整个交互保持在程序的范围内。这种模式应该与像 ChatGPT 这样的代理中的提示-完成机制很好地契合。

![](../Images/8f76c7112e2bff921353f1bf273237d6.png)

来源：作者提供的图像

为了说明这项技术，让我们在 ChatGPT4 中指定一个迷你应用程序的参数，该应用程序旨在作为互动创新工作坊。我们的迷你应用程序将包含以下功能和特点：

1.  处理新想法

1.  扩展想法

1.  总结想法

1.  检索想法

1.  继续处理先前的想法

1.  Token/“记忆”使用统计

需要明确的是，我们不会要求 ChatGPT 用任何特定编程语言编写迷你应用程序，我们将在我们的程序参数中反映这一点。

根据这个程序大纲，让我们开始编写启动提示，以在 ChatGPT 中实例化我们的互动创新工作坊迷你应用程序。

**程序模拟启动提示**

```py
Innovator’s Interactive Workshop Program

I want you to simulate an Innovator’s Interactive Workshop application whose core features are defined as follows:

1\. Work on New Idea: Prompt user to work on new idea. At any point when a user is ready to work through a new idea the program will suggest that a date or some time reference be provided. Here is additional detail on the options:
  a. Start from Scratch: Asks the user for the idea they would like to work on.
  b. Get Inspired: The program assists user interactively to come up with an idea to work on. The program will ask if the user has a general sense of an area to focus on or whether the program should present options. At all times the user is given the option to go directly to working on an idea.
2\. Expand on Idea: Program interactively helps user expand  on an idea.
3\. Summarize Idea: Program proposes a summary of the idea regardless of whether or not it has been expanded upon and proposes a title. The user may choose to rewrite or edit the summary. Once the user is satisfied with the summary, the program will "save" the idea summary.
4\. Retrieve Ideas: Program retrieves the titles of the idea summaries that were generated during the session. User is given the option to show a summary of one of the ideas or Continue Working on a Previous Idea.
5\. Continue Working on Previous Idea: Program retrieves the titles of the idea summaries that were generated during the session. User is asked to choose an idea to continue working on.
6\. Token/Memory Usage: Program displays the current token count and its percentage relative to the token limit of 32,000 tokens.

Other program parameters and considerations:

1\. All output should be presented in the form of text and embedded windows with code or markdown should not be used.
2\. The user flow and user experience should emulate that of a real program but nevertheless be conversational just like ChatGPT is.
3\. The Program should use emojis in helping convey context around the output. But this should be employed sparingly and without getting too carried away. The menu should however always have emojis and they should remain consistent throughout the conversation.

Once this prompt is received, the program will start with Main Menu and a short inspirational welcome message the program devises. Functions are selected by typing the number corresponding to the function or text that approximates to the function in question.  "Help" or "Menu" can be typed  at any time to return to this menu. 
```

如果你想以更互动的方式跟随并自己测试，可以随意将提示加载到 ChatGPT4 中。

这是 ChatGPT 对提示的完成结果。

![](../Images/5de6729285841a681d7a75ca867063e4.png)

到目前为止，一切顺利。我们已经启动了我们的“迷你应用程序”，收到了振奋人心的欢迎消息，并且展示了一个与我们的程序参数一致的功能菜单。让我们通过提交“1”来测试我们的迷你应用程序的功能，以启动“处理新想法”功能。

![](../Images/4e02140ce29b9e81907b1c205b38dda3.png)

对话继续很好地遵循我们设定的“程序”结构，适当地提供了符合参数的完成。让我们继续从零开始构建一个想法，并让程序与我们合作，开发一种用于生长建筑物而非建造它们的技术。

![](../Images/23b11d7363e1dd550a47c899f18286ff.png)

有趣的是，我们注意到“程序”在没有明确指示的情况下自动调用“Expand on Idea”功能。鉴于程序的目标，这种行为并不不当，可能受到我们最初上下文设置的影响，这些设置引导聊天代理像程序一样运行。让我们继续深入探讨一下增长建筑所需的技术。

![](../Images/14ac811a46722df7e87c7d1fe755efa9.png)

现在让我们检查一下用于增长建筑的材料。

![](../Images/afb9aecb941a3ab9cac99119d9a3e64a.png)

我继续沿着这些思路前进，现在，让我们看看是否可以返回到菜单。

![](../Images/8e8d0947adcb59c8b6219c8ae42d3f61.png)

菜单仍然完整。让我们尝试让程序执行Summarize Idea功能。

![](../Images/ffbf12cc3f76670330d78b717f0dfe8f.png)

我对这个标题和摘要暂时满意，所以让我们“保存”它。

![](../Images/70b2a80f80d2ef35c3743629b5a760ef.png)

很快，我们将测试检索我们“保存”的想法，以检查我们在实现数据持久性方面的努力是否成功。另一方面，调整我们的“迷你应用”以省略保存后的重复摘要可能会有所帮助。

角色启动作为程序的结果是在输出中包含主菜单——这种行为在程序的背景下是合理的，即使它在我们的程序定义中没有被明确配置。

接下来，让我们测试我们的令牌计数功能。

![](../Images/e85a97dcba2b2f53328f565c6637fff5.png)

为了核实准确性，我转向OpenAI的分词器工具。

![](../Images/791e2c68c44d9212d6cdd62a8a556272.png)

令牌计数不准确，证据在于显著的差异——我们的程序报告大约有1,200个令牌，而分词器工具显示为2,730。鉴于这种不匹配，明智的做法是从程序中移除此功能。我不会深入讨论为何这种任务通常对语言模型来说是个问题，以及功能损失相对较小。最终，我预计这样的功能将会原生集成到ChatGPT中，特别是考虑到令牌计数信息在后台不断传递。

接下来，让我们深入研究“Get Inspired”功能以生成新想法。为了简洁起见，我将进一步展示对话。正如你所见，我选择深入探讨一个我们的程序建议的废物转化为能源的无人机概念，概括了这个想法，并让我们的程序“保存”了它。

![](../Images/3d4d55b455483b8b8849fe8f907bfd91.png)

一切看起来都很好，系统甚至擅自给我们的想法命名为“SolarSky”。为了更有效地实现这一点，我们可能会在程序定义中为此任务加入一个独立的函数，或者在“工作在新想法”或“扩展新想法”函数中提供更具体的指示。同样，我们在完成中看到菜单，这从程序流的角度看是合乎逻辑的。

现在让我们看看是否可以“检索想法”。

![](../Images/a4ae29e9fe3471683a32b828973c3140.png)

这似乎符合我们的原始指示，仅提供了所请求的标题。它还提示我们继续工作一个想法，即使这并没有明确地编程到迷你应用程序中。接下来，让我们评估它是否保持了根菜单索引。为此，我将输入“5”，对应于“继续工作在之前的想法”功能，看看是否有效。

![](../Images/98ccf59448e8d89deac0c79c93041c8d.png)

显然，索引在对话上下文中被维护，并且相应地调用了函数。这一观察值得注意，特别是在考虑到多个索引可能处于活动状态的情况下。这引发了有关“程序”在这种条件下如何表现的有趣问题。你可能错过了，但在我们互动的早期，程序在征求用户对想法扩展选择的输入时实际上采用了索引技术。

让我们继续工作在我们的建筑构想上。

![](../Images/3ce770c0fc332866d358c3eb565bbbe9.png)

再次看起来不错。“程序”的行为如预期，并且也跟踪了我们在想法扩展过程中暂停的确切点。

让我们在这里停止测试我们的提示，看看通过这种技术我们学到了什么。

**结论与观察**

坦白说，这次练习虽然在范围和功能上都有限，但超出了我的预期。我们本可以让ChatGPT用Python等语言编写这个迷你应用程序，然后利用代码解释器（现在称为高级数据分析）在持续的Python会话中运行它。然而，这种方法会引入一种刚性，使得启用我们迷你应用程序中固有的对话功能变得困难。更不用说，特别是在具有多个重叠功能的程序中，我们立即面临着代码无法正常工作的风险。

ChatGPT的表现尤其令人印象深刻，因为它以高度逼真的方式模拟了程序行为。提示完成保持在程序定义的边界内，即使在函数行为没有明确规定的情况下，完成也在迷你应用程序目的的上下文中有逻辑性。

这种程序模拟技术可能与ChatGPT的“自定义指令”功能配合良好，尽管值得一提的是，这样做会将程序的行为应用于所有后续的互动中。

我的下一步包括对这种技术进行更深入的研究，以评估是否可以通过一个全面的测试框架来了解这种方法相对于其他提示工程技术的表现。这种练习也可能帮助确定这种技术最适合哪些特定任务（或任务类别）。敬请关注更多信息。

与此同时，希望你在互动中发现这种技术和提示有帮助。如果你想进一步讨论这种技术，请随时通过[LinkedIn](https://www.linkedin.com/in/giuseppe-scalamogna-8b389145/)与我联系。

除非另有说明，本文中的所有图片均由作者提供。
