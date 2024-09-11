# 将线性回归应用于 GPT 的七个步骤

> 原文：[https://towardsdatascience.com/linear-regression-to-gpt-in-seven-steps-cb3ab3173a14?source=collection_archive---------7-----------------------#2023-04-25](https://towardsdatascience.com/linear-regression-to-gpt-in-seven-steps-cb3ab3173a14?source=collection_archive---------7-----------------------#2023-04-25)

## 这项朴素的预测方法如何指引我们进入生成式 AI 的世界

[](https://deveshrajadhyax.medium.com/?source=post_page-----cb3ab3173a14--------------------------------)[![Devesh Rajadhyax](../Images/35a81a5f148f83aea8b9353e030f79c7.png)](https://deveshrajadhyax.medium.com/?source=post_page-----cb3ab3173a14--------------------------------)[](https://towardsdatascience.com/?source=post_page-----cb3ab3173a14--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----cb3ab3173a14--------------------------------) [Devesh Rajadhyax](https://deveshrajadhyax.medium.com/?source=post_page-----cb3ab3173a14--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fff46afc9b9c2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flinear-regression-to-gpt-in-seven-steps-cb3ab3173a14&user=Devesh+Rajadhyax&userId=ff46afc9b9c2&source=post_page-ff46afc9b9c2----cb3ab3173a14---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----cb3ab3173a14--------------------------------) · 9 分钟阅读 · 2023 年 4 月 25 日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fcb3ab3173a14&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flinear-regression-to-gpt-in-seven-steps-cb3ab3173a14&source=-----cb3ab3173a14---------------------bookmark_footer-----------)

关于生成式 AI 的写作非常多。有专门讨论其应用、伦理道德问题以及对人类社会风险的文章。如果你想了解技术本身，有从原始研究论文到入门文章和视频的各种材料可供选择。根据你当前的水平和兴趣，你可以找到合适的学习资源。

这篇文章是为特定读者群体写的。这些读者已经学习了机器学习，但不是作为主修科目。他们知道预测和分类是涵盖大多数应用的 ML 的两个主要用例。他们还学习了常见的预测和分类机器学习算法，如线性回归、逻辑回归、支持向量机、决策树以及一些神经网络。他们可能用 Python 和 scikit-learn 等库编写过一些小项目，甚至使用过一些预训练的 TensorFlow 模型，如 ResNet。我认为许多学生和专业人士能够与这种描述产生共鸣。

对这些读者来说，生成式 AI 是一种新的 ML 用例吗？它似乎确实不同于预测和分类。流行的术语如变换器、多头注意力、大型语言模型、基础模型、序列到序列和提示工程，容易让人觉得这与我们熟悉的预测-分类世界截然不同。

本文的核心观点是生成式 AI 只是预测的一个特例。如果你符合我之前描述的 ML 爱好者的特点，那么你可以在七个简单的步骤中理解生成式 AI 的基本工作原理。我从线性回归（LinReg）开始，这是每个人都知道的 ML 技术。在本文中，我探讨了生成式 AI 的一个特定分支——大型语言模型（LLM），主要是因为广受欢迎的 ChatGPT 就属于这个分支。

![](../Images/bb968188821ce9ae6de24665eb49c2b8.png)

图片来自 Rajashree Rajadhyax

# 第 1 步：通过线性回归进行预测

LinReg 确定代表给定数据点的最佳直线。一旦找到这条直线，就用它来预测新输入的输出。

![](../Images/bf77e70265bbc669331743fe3dffd3dc.png)

图片来自作者

我们可以将 LinReg 模型写成一个数学函数。以易于理解的方式写出来，它看起来像：

```py
new output = Line Function (new input)
```

我们也可以为其绘制一个示意图：

![](../Images/cc5f44b34cb1b8544c9f88151afc84d1.png)

这是最基本的预测。LinReg 模型‘学习’最佳直线并用它进行预测。

# 第 2 步：通过神经网络进行预测

只有在知道数据适合直线时，你才可以使用 LinReg。这通常对单输入单输出问题很容易做到。我们可以简单地绘制图表并直观检查。但在大多数现实问题中，有多个输入。我们无法可视化这样的图表。

此外，现实世界的数据并不总是遵循线性路径。许多时候，最佳拟合形状是非线性的。见下图：

![](../Images/ab7cf3aa9ddecf853a771a22d706ffbd.png)

图片来自作者

在多维数据中学习这样一个函数不可能通过简单的方法如LinReg来实现。这就是神经网络（NN）发挥作用的地方。NN不需要我们决定它们应该学习哪个函数。它们自己找到并学习函数，无论它多么复杂。一旦NN学习了复杂的、多输入的函数，它们就会使用这个函数进行预测。

我们可以再次写出类似的方程，但有所变化。我们的输入现在是多个，因此我们必须用一个向量来表示它们。实际上，输出也可以是多个，我们也会为它们使用一个向量。

```py
output vector = NN Function (input vector)
```

我们将绘制这种新的、更强大预测的示意图：

![](../Images/c8ea3d4bee71906a1eb97dee5bb3afe2.png)

作者的图片

# 步骤 3：预测一个单词

现在考虑我们有一个问题，输入到神经网络中的是某种语言中的一个单词。神经网络只能接受数字和向量。为了适应这一点，单词被转换为向量。你可以把它们想象成是多维空间中的居民，相关的单词彼此靠近。例如，‘Java’的向量将接近其他编程技术的向量；但它也会接近远东地区的地方，如苏门答腊的向量。

![](../Images/3c2a8a1c641f3e005a2f215d86c610d0.png)

作者的（非常虚构的）单词嵌入

这样一组与语言中单词对应的向量称为‘嵌入’。有许多方法可以创建这些嵌入；Word2Vec 和 GloVe 是两个流行的例子。这些嵌入的典型大小为256、512或1024。

一旦我们有了单词的向量，我们可以在它们上使用NN进行预测。但是通过对单词进行预测，我们能实现什么呢？我们可以做很多事情。我们可以将一个单词翻译成另一种语言，找到该单词的同义词或找到它的过去式。这个预测的方程式和示意图看起来与步骤 3 非常相似。

```py
output word embedding = NN Function (input word embedding)
```

![](../Images/0dab89793568a814fc8408f37b47e359.png)

作者的图片

# 步骤 4：简单翻译的预测

在翻译问题中，输入是一种语言的句子，输出是另一种语言的句子。我们如何利用对单词预测的已有知识来实现翻译呢？这里我们采用一种简单的翻译方法。我们将输入句子的每个单词转换为另一种语言中的等效单词。当然，真正的翻译不会像这样工作；但在这一步骤中，假装它会。

这次我们先画出示意图：

![](../Images/20818d988257c4b43d22f9706eb20506.png)

作者的图片

第一个单词的方程式将是：

```py
NN Translation Function (Embeddings for word. no 1 in input sentence) 
= Embedding for word no.1 in output sentence
```

我们可以类似地为其他单词写出方程式。

此处使用的神经网络通过查看许多单词对的示例来学习翻译功能。我们为每个单词使用一个这样的神经网络。

因此，我们有了一个使用预测的翻译系统。我已经承认这是一种幼稚的翻译方法。有哪些改进可以使其在现实世界中有效？我们将在接下来的两步中看到。

# 步骤 5: 使用上下文的预测

幼稚方法的第一个问题是，一个词的翻译依赖于句子中的其他词。例如，考虑下面的英语到印地语的翻译：

> 输入（英语）: ‘Ashok sent a *letter* to Sagar’
> 
> 输出（印地语）: ’Ashok ne Sagar ko *khat* bheja’。

词‘sent’在输出中被翻译为‘bheja’。然而，如果输入句子是：

> 输入（英语）: ‘Ashok sent *sweets* to Sagar’

然后，相同的词被翻译为‘bheji’。

> 输出（印地语）: ’Ashok ne Sagar ko mithai bheji’。

因此，在预测输出时，必须添加句子中其他词的上下文。我们只为一个词绘制示意图：

![](../Images/21b37b734d502deb965f9b7b5e46d1b4.png)

图片由作者提供

生成上下文的方法有很多种。最强大、最先进的方法称为‘注意力’。使用注意力进行上下文生成的神经网络称为‘变换器’。Bert 和 GPT 就是变换器的例子。

我们现在有了一种使用上下文的预测方法。我们可以将方程式写成：

```py
NN Translation Function (Embeddings for word. no 1 in input sentence 
+ context from other words in input sentence) 
= Embedding for word no.1 in output sentence
```

# 步骤 6: 下一个词的预测

我们现在将处理幼稚翻译方法中的第二个问题。翻译不是单词的一对一映射。请参见前一步的例子：

> 输入（英语）: ‘Ashok sent a letter to Sagar’
> 
> 输出（印地语）: ’Ashok ne Sagar ko khat bheja’。

你会注意到词的顺序不同，输入中没有‘a’的对应词，也没有输出中的‘ne’。我们每个词一个神经网络的方法在这种情况下不起作用。事实上，在大多数情况下它都不起作用。

幸运的是，已经有了更好的方法。在将输入句子提供给神经网络（NN）之后，我们要求它仅预测一个词，即将作为输出句子第一个词的词。我们可以将其表示为：

![](../Images/d777acbdf5b1d53dab0771ebb7b1baf3.png)

图片由作者提供

在我们发送信件的例子中，我们可以将其写为：

```py
NN Translation Function (Embeddings for 'Ashok sent a letter to Sagar' 
+ context from input sentence) 
= Embedding for 'Ashok'
```

为了获取输出中的第二个词，我们将输入更改为：

```py
Input = Embeddings for input sentence + Embedding for first word in output
```

我们还必须在上下文中包含这一新的输入：

```py
Context = Context from input sentence + context from first word in output
```

神经网络将预测句子的下一个（第二个）词：

![](../Images/dcca414b204059a6613ed6a713233cdf.png)

这可以写成：

```py
NN Translation Function 
(Embeddings for 'Ashok send a letter to Sagar + Embedding for 'Ashok', 
Context for input sentence + context for 'Ashok') 
= Embedding for 'ne'
```

我们可以继续这个过程，直到神经网络预测出‘.’的嵌入，换句话说，它会发出输出已经结束的信号。

我们因此将翻译问题简化为‘预测下一个词’的问题。在下一步中，我们将看到这种翻译方法如何引导我们走向更通用的生成 AI 能力。

# 步骤 7: 生成 AI 的预测

‘下一个词的预测’方法不限于翻译。神经网络可以被训练来预测下一个词，使输出成为对问题的回答，或是你指定主题的文章。想象一下，输入到这样的生成型神经网络中的句子是：

‘写一篇关于全球变暖对喜马拉雅冰川影响的文章’。

生成模型的输入被称为‘提示’。生成型神经网络预测文章的第一个词，然后继续预测后续的词，直到生成整篇漂亮的文章。这就是大型语言模型如 ChatGPT 所做的。正如你可以想象的那样，这些模型的内部机制远比我在这里描述的要复杂。但它们包含了我们看到的相同基本组件：嵌入、注意力和下一个词预测神经网络。

![](../Images/e46a217c32951d76eb1348afffca73b0.png)

作者提供的图片

除了翻译和内容生成，LLMs 还可以回答问题、规划旅行和做许多其他奇妙的事情。所有这些任务中使用的基本方法仍然是预测下一个词。

# 摘要

我们从使用 LinReg 的基本预测技术开始。我们通过添加向量和词嵌入使预测问题变得更加复杂。我们通过解决简单的翻译问题学会了如何将预测应用于语言。在增强简单方法以处理真实翻译的过程中，我们熟悉了 LLMs 的基本元素：上下文和下一个词预测。我们意识到大型语言模型的核心是对文本序列的预测。我们熟悉了如提示、嵌入、注意力和变换器等重要术语。

序列不一定非得是文本。我们可以使用任何序列，例如图片或声音。本文的信息涵盖了生成型人工智能的全部内容，即：

> 生成型人工智能是利用神经网络对序列进行预测。
