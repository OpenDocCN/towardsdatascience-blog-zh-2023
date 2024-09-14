# 使用 GPT-3 精简你的文档

> 原文：[`towardsdatascience.com/streamline-your-documentation-with-gpt-3-5d9f2bbf217c`](https://towardsdatascience.com/streamline-your-documentation-with-gpt-3-5d9f2bbf217c)

## 使用人工智能生成 Python 文档字符串

[](https://mikhailklassen.medium.com/?source=post_page-----5d9f2bbf217c--------------------------------)![Mikhail Klassen](https://mikhailklassen.medium.com/?source=post_page-----5d9f2bbf217c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5d9f2bbf217c--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----5d9f2bbf217c--------------------------------) [Mikhail Klassen](https://mikhailklassen.medium.com/?source=post_page-----5d9f2bbf217c--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5d9f2bbf217c--------------------------------) ·6 分钟阅读·2023 年 1 月 16 日

--

![](img/a6660b6f88943c3032a35dca7d0ba80a.png)

图像由 DALL-E 生成，提示语为：“一个机器人在计算机终端检查代码。赛博朋克风格，逼真。”

GPT-3，OpenAI 开发的最新语言模型，具有生成类人文本的能力，使其成为各种自然语言处理任务的强大工具。

该模型还进行了编程语言的训练。一个应用场景是生成 Python 函数的文档字符串（docstrings）。

# 什么是文档字符串？

文档字符串是出现在 Python 函数中的第一个语句。它提供了函数及其输入和输出的简要描述，使其他开发人员更容易理解和使用代码。编写清晰且信息丰富的文档字符串可能耗时且乏味，尤其是在拥有许多函数的大型项目中。

```py
# A docstring example
def square(n):
    """Takes in a number n and returns the square of n"""
    return n**2
```

通过在一个有用的提示中提供 Python 函数的源代码，可以使用 GPT-3 来生成文档字符串。

## 示例：计算一组数字的平均值

假设你编写了一个计算数字列表平均值的函数。你希望 GPT-3 创建文档字符串。

**以下是提示的示例：**

```py
# Python 3.7

def mean_of_arr(arr):
    return sum(arr)/len(arr)

# An elaborate, high quality docstring for the above function:
"""
```

制定正确的提示非常重要。

注意我们如何通过以下方式帮助 GPT-3：

+   告诉它编程语言（Python 3.7），

+   提供关于如何显式要求生成详细且高质量的函数文档字符串的评论，

+   然后添加一行带有三个双引号的内容，这也是文档字符串的标准起始模式。

GPT-3 生成了以下响应

```py
 This function takes an array of numbers and returns the mean of the array.
    The mean is the sum of the numbers divided by the length of the array.
```

GPT-3 甚至知道如何缩进文本块。

需要注意的是，生成的文档字符串可能不完美，可能需要一些编辑。然而，GPT-3 可以在编写文档字符串的过程中节省大量时间和精力。

# 使用文档字符串生成器自动创建文档字符串

将你的代码复制并粘贴到 OpenAI 的 playground 中，仅仅让它为你生成文档字符串有点乏味。

我们已经看到 AI 工具直接嵌入我们的开发环境中，如 [GitHub Copilot](https://github.com/features/copilot) 或 [Amazon CodeWhisperer](https://techcrunch.com/2022/06/23/amazon-launches-codewhisperer-its-ai-pair-programming-tool/?guccounter=1&guce_referrer=aHR0cHM6Ly9zZWFyY2guYnJhdmUuY29tLw&guce_referrer_sig=AQAAAE_zMP6X9JPGw2-oobAzWmPyrnvxZKCz3fzevCOsNsJwZiwYfDv2y8qMHW5JdEDrNCnFF_1nF3a_MoELF1UVXnvZMT8SmSszuBnIFieQkGk21a1Q913Z0fN4EtJNSmVtzeVzGfWAfdshlhMMsED1xwqBifbRqb8V-ZPP_UGN0OH-).

未来的程序员将是人类和 AI 的混合体。让我们自己开始玩转这一能力。

假设你在使用 Jupyter notebook，如在 [Colab](https://colab.research.google.com/)，并且你想快速为一个新函数生成文档字符串。

假设你已经有一个 OpenAI 账户，[生成你的 API 密钥](https://beta.openai.com/account/api-keys) 并将其粘贴到下面的示例中，复制到你的 notebook。

```py
import openai # Install via "pip install openai"
import inspect

openai.api_key = "YOUR_OPENAI_API_KEY_GOES_HERE"

def generate_docstring(my_func, python_version=3.7):
    # Grab the source code from the function as a string
    source_code = inspect.getsource(my_func)

    # Remove any existing docstring
    if my_func.__doc__ is not None:
        source_code = source_code.replace(my_func.__doc__,"")\
                                 .replace("\"\"\"","")

    # Wrap the source code with hints for GPT-3
    prompt = f"# Python {python_version}\n\n" \
           + source_code \
           + "\n# An elaborate, high quality docstring for the above function:\n\"\"\""

    # Send prompt to GPT-3
    response = openai.Completion.create(
                   model="code-davinci-002",
                   prompt=prompt,
                   temperature=0,
                   max_tokens=150,
                   top_p=1.0,
                   frequency_penalty=0.0,
                   presence_penalty=0.0,
                   stop=["#", "\"\"\""]
               )

    # Return the generated docstring
    docstring = response["choices"][0]["text"]
    return docstring
```

`inspect` 库的 `getsource` 函数从你传递给它的任何函数中获取源代码，并将其作为字符串返回。

如果函数已经有了文档字符串，我不想让 GPT-3 产生偏见，因此我包括了一些代码来剥离任何现有的文档字符串。

我假设代码是 Python，但我将 Python 版本留作可选变量。

# 实验

现在进入有趣的部分。让我们看看它在几个示例中的表现。

## 选择排序

这是一种时间复杂度为 O(n²) 的排序算法，意味着它在大列表上效率低下，但它的优点是简单。

```py
def sort(arr):
    for i in range(len(arr)):
        min_idx = i
        for j in range(i+1, len(arr)):
            if arr[min_idx] > arr[j]:
                min_idx = j
        arr[i], arr[min_idx] = arr[min_idx], arr[i]
    return arr
```

我明确选择不将函数命名为 `selection_sort`，因为我不希望 GPT-3 仅仅从它在训练语料库中见过的选择排序示例中获取文档字符串。

有趣的是，GPT-3 推断出这里使用的是选择排序算法。很可能在它的语料库中，选择排序算法就是以这种方式编写的。以下是 GPT-3 返回的结果：

```py
 This function sorts an array of integers in ascending order.
    It uses the selection sort algorithm.

    Parameters
    ----------
    arr : list
        The array to be sorted.

    Returns
    -------
    list
        The sorted array.
```

那是一个相当适当的文档字符串。

## 将数据拆分为训练集和测试集

在对数据进行机器学习模型训练之前，常见的一步是将数据拆分为训练集和测试集。

假设你有一个函数来完成这个任务，并将结果写入两个文件：

```py
import pandas as pd
from sklearn.model_selection import train_test_split

def randomly_split_dataset(folder, filename, split_ratio=[0.8, 0.2]):
    df = pd.read_json(folder + filename, lines=True)
    train_name, test_name = "train.jsonl", "test.jsonl"
    df_train, df_test = train_test_split(df, test_size=split_ratio[1], 
                                         random_state=42)
    df_train.to_json(folder + train_name, orient='records', lines=True)
    df_test.to_json(folder + test_name, orient='records', lines=True)
    return
```

仅将 `randomly_split_dataset` 函数传递给我们的文档字符串生成器：

```py
my_docstring = generate_docstring(randomly_split_dataset)

print(my_docstring)
```

GPT-3 返回：

```py
 This function takes in a folder and filename, and splits the dataset into
    two files, train.jsonl and test.jsonl, with the split ratio specified by
    the user.

    Parameters
    ----------
    folder : str
        The folder where the dataset is located.
    filename : str
        The name of the dataset file.
    split_ratio : list
        The ratio of the split, with the first element being the training set
        and the second element being the test set.

    Returns
    -------
    None

    Examples
    --------
    >>> randomly_split_dataset("data/", "train
```

不错。它在示例中突然停止，但我发现可以通过增加 `max_tokens` 的数量来改进这一点。

现在进行最后的测试：记录它自己。

## 为文档字符串生成器编写文档字符串

也许最令人满意的结果是让文档字符串生成器为自己编写文档字符串。

```py
my_docstring = generate_docstring(generate_docstring)

print(my_docstring)
```

结果是一个完全可以接受的文档字符串，我们现在可以将其复制并粘贴到函数中。

```py
 This function takes a function as input and returns a docstring for that function.
    It uses the OpenAI API to generate the docstring.

    Parameters
    ----------
    my_func : function
        The function for which you want a docstring.
    python_version : float
        The version of Python you are using.

    Returns
    -------
    docstring : str
        The generated docstring.
```

# 结论

你的体验可能会有所不同。在我自己的实验中，结果有时会显得怪异或与我测试的功能无关。其他时候，我被明显的洞察力震惊。调整 `temperature` 变量可能值得尝试，以探索更多“创造性”的输出。

需要注意的是，GPT-3 实际上并不*理解*代码。它不能对你的代码进行推理。GPT-3 是在数十亿行代码上进行训练的，其中包括许多高质量的 docstrings。这个拥有 1750 亿参数的神经网络非常擅长预测在给定提示之后*最有可能出现*的 token（单词等）序列。

除了生成 docstrings，GPT-3 还可以用于许多其他自然语言处理任务，如文本总结、问答和语言翻译。GPT-3 的潜在应用广泛，未来它的使用情况将非常有趣。

总结来说，GPT-3 可以成为生成 Python 函数 docstrings 的有用工具，帮助开发者以最小的努力编写清晰且有信息量的文档。由于该模型仍在开发中，我们可以期待它在软件开发的其他方面如何改进。

如果你喜欢阅读这些故事并希望支持我作为作者，请考虑注册成为 Medium 会员。每月 $5，可访问我所有的写作以及成千上万其他作家的作品。如果你通过 [我的链接](https://mikhailklassen.medium.com/membership) 注册，我将获得一小部分佣金，对你没有额外费用。

[## 使用我的推荐链接加入 Medium — Mikhail Klassen](https://mikhailklassen.medium.com/membership?source=post_page-----5d9f2bbf217c--------------------------------)

### 阅读 Mikhail Klassen（以及 Medium 上成千上万其他作者）的每个故事。你的会员费用直接支持…

[mikhailklassen.medium.com](https://mikhailklassen.medium.com/membership?source=post_page-----5d9f2bbf217c--------------------------------)
