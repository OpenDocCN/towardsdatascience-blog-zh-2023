# 使用 ChatGPT 进行高效调试

> 原文：[https://towardsdatascience.com/using-chatgpt-for-efficient-debugging-fc9e065b7856?source=collection_archive---------6-----------------------#2023-06-22](https://towardsdatascience.com/using-chatgpt-for-efficient-debugging-fc9e065b7856?source=collection_archive---------6-----------------------#2023-06-22)

## 利用大型语言模型提升你的调试体验，快速学习

[](https://medium.com/@andimid?source=post_page-----fc9e065b7856--------------------------------)[![Dmytro Nikolaiev (Dimid)](../Images/4121156b9c08ed20e7aa620712a391d9.png)](https://medium.com/@andimid?source=post_page-----fc9e065b7856--------------------------------)[](https://towardsdatascience.com/?source=post_page-----fc9e065b7856--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----fc9e065b7856--------------------------------) [Dmytro Nikolaiev (Dimid)](https://medium.com/@andimid?source=post_page-----fc9e065b7856--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F97b5279dad26&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fusing-chatgpt-for-efficient-debugging-fc9e065b7856&user=Dmytro+Nikolaiev+%28Dimid%29&userId=97b5279dad26&source=post_page-97b5279dad26----fc9e065b7856---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----fc9e065b7856--------------------------------) · 15分钟阅读 · 2023年6月22日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ffc9e065b7856&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fusing-chatgpt-for-efficient-debugging-fc9e065b7856&user=Dmytro+Nikolaiev+%28Dimid%29&userId=97b5279dad26&source=-----fc9e065b7856---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ffc9e065b7856&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fusing-chatgpt-for-efficient-debugging-fc9e065b7856&source=-----fc9e065b7856---------------------bookmark_footer-----------)![](../Images/eaccf4d8ab5127f53e8f34b436af3116.png)

图片来源：[Pavel Danilyuk](https://www.pexels.com/photo/elderly-man-thinking-while-looking-at-a-chessboard-8438918/) 于 [Pexels](https://www.pexels.com/)

难以否认，大型语言模型（LLMs）正在对各行各业和应用产生深远的影响，彻底改变了我们的工作和互动方式。尽管自从约六个月前（2022年11月）发布以来，围绕 ChatGPT 的最初炒作已平息，但它的影响仍然显著。看来自回归 LLMs 将在不久的将来继续成为我们生活的一部分，并且 **值得培养与它们互动的技能**，无论作为开发者还是用户。

正如[Chip Huyen 在她的博客文章中所述](https://huyenchip.com/2023/04/11/llm-engineering.html)，*使用 LLMs 达成令人印象深刻的成果相对容易，但考虑到[LLMs 目前存在的限制和潜在问题](https://www.honeycomb.io/blog/hard-stuff-nobody-talks-about-llm)，构建生产就绪的系统却相当具有挑战性*。然而，尽管研究和工程界正积极努力解决这些挑战，值得承认的是，**个人已经能够从 LLMs 中获益**，至少可以将它们作为个人助手用于日常非关键任务，或作为头脑风暴的合作伙伴。

在我之前的文章中，我讨论了[提示工程的最佳实践](/summarising-best-practices-for-prompt-engineering-c5e86c483af4)，提供了帮助你开发基于本地 LLMs 的应用程序的见解。在这篇文章中，我将分享一系列技巧，帮助你利用如 ChatGPT 这样的模型进行**有效的代码调试**和**加速编程学习**。我们还将看看编写和解释代码的示例提示。这些技巧不仅在与 ChatGPT 互动时有用，也能帮助你从同事那里寻求帮助或独立解决编程挑战。

本文主要面向初学者，因此我尝试提供了说明性的例子和解释。我希望这些技巧能帮助你更高效地理解和排除代码中的问题。

# 代码调试的一般框架

实际上，ChatGPT 对调试过程没有做出**重大**改变。好在现在你可以轻松地与*虚拟同事*联系，而不用担心打扰别人或担心问蠢问题！但我们将考虑的这些技巧**只要软件工程存在就会存在**，因此不仅在与 LLMs 互动时有用，也有助于更好地理解过程和更有效地与同事互动。

要找到代码中的错误，你只需两个基本步骤（实际上有三个）：

1.  *隔离错误并用最少量的代码展示它*；

1.  *对你的错误做出假设*并进行测试；

1.  *不断迭代*，提出更多假设，直到找到解决办法。

虽然你可以立即开始使用 ChatGPT，但实际上从重现错误开始会更好，这有几个原因。首先，在语言模型的上下文中，可能很难包括所有相关点并准确解释你想要实现的目标。其次，这将使你更好地理解问题，并可能自己找到错误。让我们来看看。

> 顺便说一下，在这篇文章中，我使用的是原版 ChatGPT（GPT-3.5），但对于编码任务，GPT-4 通常更为高效。

## 第一步：使用最少量的代码隔离并重现问题

第一步是重现问题。众所周知，大多数问题仍然可以通过经典的 **“重启”** 来解决。可能你已经在 Jupyter Notebook 中与代码执行顺序纠缠不清。

如果可能（通常是），建议 **编写新的代码以抛出相同的错误，并尽可能保持简单**。

让我们考虑一个 `TypeError: ‘int' object is not iterable` 的例子，这种错误发生在你尝试迭代 `some_integer` 而不是使用 `range(some_integer)` 构造时。

糟糕的例子：一个函数调用另一个函数，该函数又调用一个类的方法。乍一看，可能需要一些时间来确定实际计算发生的位置，尽管这是一个相对简单的例子。类似地，对于模型而言，在无关细节中 *定位相关信息* 变得更加具有挑战性。

更好的例子：通过将 `do_some_work()` 函数（引发错误的函数）的功能直接移到我们调用的函数中，来去掉类。

除了我们在变量命名约定上仍然做得很糟糕（记住，**变量名应该具有描述性和意义！**），这段代码仍然更容易调试和理解。

更好的例子：我们还可以去掉 `some_function()`。

总体而言，我们将代码缩短了超过一半。比较一下找出其中错误的难度。

在 pandas 的背景下，这一原则可能意味着不使用原始的数据框。假设我们想计算每个职位的平均工资，并遇到 `KeyError`。这是一个糟糕的例子：

首先，我们不能确定数据框中是否包含注释中提供的数据。实际上，我们只需要其中的两列，如果我们创建一个类似的迷你版本，会更容易理解我们只是拼写错误了工资列（`Salary` vs `salary`）。

> 顺便说一下，ChatGPT 在 **生成虚拟数据** 方面相当出色，所以它在这里也可能有帮助！

错误类型繁多，当然不可能一一列举。总体而言，尝试以 **产生相同错误** 的方式修改代码，但 **尽可能让其容易理解**。

由于所谓的 “[橡皮鸭调试](https://en.wikipedia.org/wiki/Rubber_duck_debugging)”，这一步通常能帮助你自己理解问题的原因，而无需寻求外部帮助。例如，如果你的迷你代码没有产生相同的错误，你已经找到了解决方案的一半。然而，即使它产生了相同的错误，这也是一个积极的结果。 :)

## 第2步：*做出假设、测试并迭代*

如果你仍然找不到修复错误的方法，值得寻求帮助。但对可能出错的地方有自己的假设会很有帮助。

## 查找确切的行

首先，**找到导致问题的表达式和确切的代码行**。你可能已经知道它在你之前编写的迷你代码的最后一行里面。

记住，[Python追溯](https://realpython.com/python-traceback/#how-do-you-read-a-python-traceback)显示的**错误信息**在*底部*，而对应的**执行代码**在*顶部*，中间是内部函数的调用。

![](../Images/c1c119d27a2e584f80b68ceffa6a422e.png)

Python追溯。图像由作者提供

对于直接的错误，这可能相对简单，但对于*逻辑错误*，这些错误不会生成任何错误信息，但由于逻辑错误导致意外的输出，处理起来可能更具挑战性。在这种情况下，使用*调试器*或简单的`print()`语句逐步观察值，并确定不符合预期的代码行是很有帮助的。

如果错误是由复杂的表达式引起的，例如`df.groupby(‘Occupation’)[‘Address’].apply(lambda x: ‘, ‘.join(x))`，你可以首先将其拆分为几个部分，并逐步探索输出，例如，先运行`df.groupby(‘Occupation’)`，然后`df.groupby(‘Occupation’)[‘Address’]`，等等。

## 思考常见原因

之后，考虑一些常见的错误原因：

+   是否可能是所需的**库未安装**或安装了错误的版本？

+   也许某处有一个简单的**拼写错误**或**语法错误**？

+   错误是否可能与**数据类型**有关，例如你将字符串和数字相加？

+   等等。

## 请询问ChatGPT

如果想不到任何解决办法，可以寻求ChatGPT的帮助。简单的问题通常可以通过直接粘贴代码并询问*哪里出错了*来解决。然而，对于更复杂的问题，你可能需要提供额外的相关信息。例如，如果你遇到系统错误，提供你使用的Python版本可能会有帮助。总的来说，尽量总是包括**错误信息**和**描述你想要实现的目标**。你可能需要尝试几种不同的措辞，所以不要害怕进行实验。

情况可能有很大变化，所以我们继续一些示例。首先，让我们看一下之前遇到的pandas `KeyError`。

示例提示：

```py
This Python code results in <error> in <this line>.

```

在此处插入你的代码

```py

Tell me how to debug the code to solve the given error.
```

示例：

```py
This Python code results in KeyError: 'Column not found: salary' 
in the last line.

```

import pandas as pd

df = pd.DataFrame({

    'Occupation': ['Engineer', 'Doctor', 'Engineer'],

    'Salary': [56056, 61304, 86850],

})

average_salary_per_occupation = df.groupby('Occupation')['salary'].mean()

```py

Tell me how to debug the code to solve the given error.
```

![](../Images/ebce750f4572403e8da6a1d57978f203.png)

调试pandas示例的输出。图像由作者提供，使用[ChatGPT](https://chat.openai.com/chat)创建

看起来不错！让我们来看一个具有逻辑错误的更具挑战性的例子。

示例提示：

```py
This Python code <does this, but I want it to do this>.

```

在此处插入你的代码

```py

Tell me how to fix the code to solve the problem.
```

示例：

```py
This Python code trains a random forest classifier on the Iris dataset.
As far as I know, the dataset is relatively simple and I expect 
the classifier to make the perfect predictions,
however, I am getting about 95% accuracy even with 100 trees in the forest.
Are these results reasonable for this situation?

```

from sklearn.datasets import load_iris

from sklearn.ensemble import RandomForestClassifier

from sklearn.metrics import accuracy_score

from sklearn.model_selection import train_test_split, GridSearchCV

# 加载 Iris 数据集

iris = load_iris()

X = iris.data

y = iris.target

# 切分数据

X_test, X_train, y_test, y_train = train_test_split(

    X, y, test_size=0.2, random_state=42)

X_train, X_val, y_train, y_val = train_test_split(

    X_train, y_train, test_size=0.2, random_state=42)

param_grid = {

    'n_estimators': [3, 7, 15, 25, 50, 100]

}

# 创建一个随机森林分类器并

# 使用网格搜索来找到最佳的树木数量

rf_classifier = RandomForestClassifier()

grid_search = GridSearchCV(estimator=rf_classifier,

                        param_grid=param_grid, cv=5)

grid_search.fit(X_train, y_train)

# 从网格搜索中获取最佳的估算器并进行测试

best_rf = grid_search.best_estimator_

y_pred = best_rf.predict(X_test)

test_accuracy = accuracy_score(y_test, y_pred)

print("Test accuracy:", test_accuracy)

# 从网格搜索中获取最佳的树木数量

best_n_estimators = grid_search.best_params_['n_estimators']

print("Best number of trees:", best_n_estimators)

```py

Tell me how to fix the code to solve the problem if there is a problem.
```

![](../Images/94e0c1edf0a1d00ac00e37b8b9c29ee5.png)

调试 sklearn 示例的输出。图像由 [ChatGPT](https://chat.openai.com/chat) 创建。

模型能够找到隐藏的问题并修复它。太棒了！

由于 ChatGPT 能够记住你之前的消息，*这里的可能性是无限的*。你可以让它解释一些你难以理解的概念，建议替代解决方案，将代码从一种语言翻译到另一种语言，等等。

此外，由于 ChatGPT 能够理解代码，它也可以编写代码。

# 使用 ChatGPT 编写和解释代码

在这一部分，我们将*探讨*一些在与 ChatGPT 编码时可以使用的技巧。但首先，我认为重要的是要记住，在 ChatGPT 出现之前，**Google** 是软件开发人员的主要工具。

我认为重要的是*不要忘记如何使用 Google*，出于各种原因。最终，使用 Google 你可以做 ChatGPT 能做的所有事情（在编码环境中），只是可能会更慢。然而，当涉及到像用 numpy 创建对角矩阵这样的特定任务时，我可能会使用 Google 更快地完成。

> 我认为适合的类比是学习外语的过程（尽管可以说你通过学习**编程**语言实际上就是在做这件事 :)）。使用 Google 就像用词汇翻译**单个单词**，而使用 ChatGPT 则类似于**翻译整句话和段落**的在线翻译器。虽然 ChatGPT 可以非常强大，但你可能会遇到在识别隐藏错误或理解某些代码块时的挑战，尤其是作为初学者。
> 
> 鉴于 ChatGPT 的巨大能力，它可以以各种方式被使用。我建议你用它来**生成简短的代码片段**——充当习语或稳定表达的词典。确保你对生成的代码有一个扎实的理解是很重要的，因为这有助于防止未来的问题和复杂性。
> 
> 所以，使用ChatGPT，但也记得去谷歌查找信息！
> 
> [这篇文章](https://codeeasy.io/blog/how-to-effectively-google-as-a-software-devel)提供了一些关于如何有效谷歌搜索的软件开发者的宝贵建议。

## 写

在这种情况下，一个有用的技巧叫做[角色提示](https://learnprompting.org/docs/basics/roles)。你可以让模型以*初级开发者*的角色来编写代码，而不是仅仅让它生成代码。通过假设这个角色，模型更有可能生成对初学者易于理解的代码，避免过于复杂的构造。

示例提示：

```py
Act like a junior Python developer.
Write code with comments explaining what is going on.
When providing the solution, ensure the output is well-formatted
and the code is well-documented.
Include example usage and explanations.

<Describe your problem here>
```

示例：

```py
Act like a junior Python developer.
Write code with comments explaining what is going on.
When providing the solution, ensure the output is well-formatted
and the code is well-documented.
Include example usage and explanations.

Write a program to find common values between two numpy arrays.
It is a function that takes two numpy arrays as inputs and
output a numpy array.
```

![](../Images/774bdc72f6c19dfe22ee95db4751b57e.png)

针对代码生成提示的输出。图片由作者创建，使用[ChatGPT](https://chat.openai.com/chat)

在这个例子中，ChatGPT很好地遵循了我们的所有指示，包括示例和详细解释。

别忘了将代码与最初的假设进行比较，并得出结论，这将帮助你在未来避免做出类似的请求。

总的来说，请记住，LLMs对你或你所面临的具体问题没有先验知识，因此你提供的信息越多，得到的输出就会越好：

+   **描述你的任务**；

+   **定义代码结构**：例如，它是完整的脚本、类还是函数；

+   **指定输入和输出**：例如，函数接受两个整数参数并输出浮点数；

+   **提及你想使用的工具/库**，如numpy或pandas，以及编程语言；

+   如果可行，**添加你的建议**，例如解决方案可能是什么样的：例如，我建议使用`pandas.DataFrame.groupby`函数按职位计算平均薪资。

## 优化

如果我们之前让模型表现得像一个初级开发者，我们希望它在编程任务，如代码优化方面更强。

> 顺便说一下，你也可以用相同的角色提示技巧来调试代码。

示例提示：

```py
Act like an experienced Python developer mentoring a junior developer.
Provide explanations and comments about the concepts that can be
hard for beginners. Use Python best practices and write docstrings.

Rewrite the above Python code and optimize it.

```

在这里插入你的代码

```py
```

示例：

```py
Act like an experienced Python developer mentoring a junior developer.
Provide explanations and comments about the concepts that can be
hard for beginners. Use Python best practices and write docstrings.

Rewrite the above Python code and optimize it.

```

def unique_list(l):

    # 从给定的列表中获取独特元素的列表

    x = []

    for a in l:

        if a not in x:

            x.append(a)

    return x

```py
```

![](../Images/12f0aabb7685d6f0c5264a868e9daf1a.png)

针对代码优化提示的输出。图片由作者创建，使用[ChatGPT](https://chat.openai.com/chat)

在这里，为了从列表中提取独特元素，ChatGPT建议使用`set`数据类型（因为其定义上不允许重复值），而不是使用for循环。这是一个不错的选择，因为它基本上是一个一行解决方案。

## 解释

我们可以使用相同的标记请求评论和代码解释。通常，这会输出冗余的注释，如`import numpy as np # importing numpy library`。然而，这在你开始学习时仍然有用，并且帮助模型*表达其内部思想*，正如在[链式思维推理](https://learnprompting.org/docs/intermediate/chain_of_thought)部分和[我的上一篇文章](/summarising-best-practices-for-prompt-engineering-c5e86c483af4)中讨论的那样。

样例提示：

```py
Act like an experienced Python developer mentoring a junior developer.
Provide explanations and comments about the concepts that can be
hard for beginners. 

Explain and comment the following code.

```

在此处插入你的代码

```py
```

示例：

```py
Act like an experienced Python developer mentoring a junior developer.
Provide explanations and comments about the concepts that can be
hard for beginners. 

Explain and comment the following code.

```

def func(n):

    trow = [1]

    y = [0]

    for x in range(max(n, 0)):

        print(trow)

        trow = [l + r for l, r in zip(trow + y, y + trow)]

    return n >= 1

func(6)

```py
```

![](../Images/9d5b8a36c34fbe2dad3d1c274aab16c9.png)

代码解释提示的输出。图片由作者使用[ChatGPT](https://chat.openai.com/chat)创建

在这里，模型能够识别这段代码背后的任务：生成[帕斯卡三角形](https://en.wikipedia.org/wiki/Pascal%27s_triangle)的行。做得好！

虽然你可以想出许多有用的 ChatGPT 应用，但重要的是要注意，它可能并不总是能够解决你遇到的每一个问题。值得进一步讨论这一方面。

# 关于潜在陷阱的一些注意事项

虽然 ChatGPT 可以非常有用并执行各种任务，但重要的是要记住潜在的缺点，这些缺点可能比较棘手。

## 作为自回归LLM，ChatGPT 不是确定性的……

ChatGPT 是一个大型语言模型的例子，当前的大型语言模型是**自回归的**，这意味着*它们被训练来预测序列中的下一个标记*。模型的输出是所有可能标记的概率分布，我们从这个分布中逐个标记地采样最终文本。因此，采样过程是**非确定性的**，这意味着*由于概率原因，你对相同的输入可能会有不同的输出*。

为了说明这一点，我们可以将采样过程想象成一棵树。这里，初始句子用蓝色显示，选中的标记用绿色显示，未选中的标记用红色表示（不包括它们的进一步演变）。概率是随机选择的，仅用于说明。

![](../Images/b691129872ca6693fa841de166d63d2e.png)

LLM采样过程的简化可视化。图片由作者提供

输入序列是：“*我的名字是*”，而 ChatGPT 通过“*ChatGPT，很高兴为您服务！*”来完成它。这是一个来自[我的上一篇文章](/behind-the-millions-estimating-the-scale-of-large-language-models-97bd7287fb6b)的例子，我在其中讨论了LLM的基本知识。

## … 这就是为什么 ChatGPT 可能会出错的原因

在实际操作中，这意味着你可能因为一开始得到了某些稀有不太可能的标记而得到次优的输出。因此，**你可能需要对相同的输入进行多次运行**以检查不同的输出并选择最合适的一个，甚至*结合来自不同输出的不同部分*。

此外，重要的是要强调现代语言模型，特别是GPT-4，具有令人印象深刻的*自我纠正能力*。如果生成的代码有错误，你可以简单地返回它并指出它的功能不正确。GPT-4擅长**调试自己的代码**并提供相关建议。你通常可以在几次迭代后获得正确的代码。

## 过度自信

尽管LLMs有时可能提供不正确的输出，但它们经过训练优先考虑准确性。这可能使它们的**输出看起来非常令人信服，即使它是错误的**。因此，识别隐藏的错误可能具有挑战性，因为模型通常不能明确地说*“我需要更多的信息”*，尽管正在进行的研究正积极探索解决这一限制的方法。

从这个意义上讲，使用ChatGPT生成**小段代码**以解决特定任务，如我之前提到的谷歌搜索，可以更安全。确保你对所收到的代码有扎实的理解，这样可以有效避免潜在的陷阱。

# 结论

在这篇文章中，我们探讨了可以帮助你调试的方法，不仅仅是与ChatGPT一起，还有你自己。

通过**隔离**问题和**用最少的代码行重新编写**，你很可能会洞察到潜在的问题。或者，让ChatGPT帮助你，提供**关于发生了什么的完整信息**，进行假设并进行实验。

你还可以利用ChatGPT进行任务，例如编写、优化或解释代码，就像我们在角色提示示例中讨论的那样。其他与代码相关的应用几乎无穷无尽，包括创建虚拟数据、编写测试、生成文档等。

但要记住LLMs的局限性，因为它们可能引入隐藏的问题。由于其自回归性质，LLMs**可能在表现自信的同时犯错**，这可能需要更多的问题或多次迭代以选择最佳输出。

祝你学习顺利！

# 资源

> 查看[这篇文章](https://realpython.com/chatgpt-coding-mentor-python/)以获取将ChatGPT作为个人编码导师的更广泛指南。

这里是我关于LLMs的其他文章，可能对你有用。我已经涵盖了：

+   [估计大型语言模型的规模](/behind-the-millions-estimating-the-scale-of-large-language-models-97bd7287fb6b)：LLMs是什么，它们是如何训练的，需要多少数据和计算资源；

+   [提示工程的最佳实践](/summarising-best-practices-for-prompt-engineering-c5e86c483af4)：如何应用提示工程技术与LLMs有效互动，以及如何使用OpenAI API和Streamlit构建本地LLM应用程序。

你可能还感兴趣：

+   免费的[Learn Prompting课程](https://learnprompting.org/)帮助你深入了解提示工程及其相关技术；

+   最近发布了[DeepLearning.AI的短课程](https://www.deeplearning.ai/short-courses/)，用于构建OpenAI API应用程序。

# 感谢你的阅读！

+   希望这些材料对你有帮助。[关注我在Medium上的更新](https://medium.com/@andimid)以获取更多类似的文章。

+   如果你有任何问题或意见，我很乐意收到任何反馈。可以在评论中问我，或者通过[LinkedIn](https://www.linkedin.com/in/andimid/)或[Twitter](https://twitter.com/dimid_ml)与我联系。

+   为了支持我作为作家，并获得其他数千篇Medium文章的访问权限，可以使用[我的推荐链接](https://medium.com/@andimid/membership)来获取Medium会员（对你没有额外费用）。
