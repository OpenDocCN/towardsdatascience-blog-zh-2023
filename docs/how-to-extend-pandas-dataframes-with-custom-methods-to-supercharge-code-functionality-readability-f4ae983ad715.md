# 如何通过自定义方法扩展 Pandas 数据框，以增强代码的功能性和可读性

> 原文：[`towardsdatascience.com/how-to-extend-pandas-dataframes-with-custom-methods-to-supercharge-code-functionality-readability-f4ae983ad715`](https://towardsdatascience.com/how-to-extend-pandas-dataframes-with-custom-methods-to-supercharge-code-functionality-readability-f4ae983ad715)

## **逐步指南：如何通过自定义方法扩展 pandas 数据框，包括实现条件概率和期望值扩展的完整示例**

[](https://grahamharrison-86487.medium.com/?source=post_page-----f4ae983ad715--------------------------------)![Graham Harrison](https://grahamharrison-86487.medium.com/?source=post_page-----f4ae983ad715--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f4ae983ad715--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----f4ae983ad715--------------------------------) [Graham Harrison](https://grahamharrison-86487.medium.com/?source=post_page-----f4ae983ad715--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f4ae983ad715--------------------------------) ·6 分钟阅读·2023 年 10 月 10 日

--

![](img/a15437590c86b85c8418a6056b45fb95.png)

照片由 [Markus Spiske](https://unsplash.com/@markusspiske?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄，来自 [Unsplash](https://unsplash.com/photos/hvSr_CVecVI?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 介绍

## 问题

Pandas 数据框提供了广泛的内置函数，但 Python 程序员总是希望以新的方式查询和转换数据。

## 机会

一种常见的方法是编写函数并将数据框作为参数传递，但这可能会变得难以处理，而最佳的方法是直接向 pandas 类添加新的方法和属性。

## 前进的方向

扩展 pandas 或任何其他库中的类功能非常简单，只需使用自定义方法和属性即可生成易于阅读、理解和重用的代码。

# 背景

我在为一个因果推断库编写一些代码，这涉及到一些复杂的数据操作，而频繁调用函数使得代码难以阅读和理解。

我想到一个主意，如果 pandas 数据框原生实现了我所编写的函数，那么代码将变得干净、简单且易于理解，从而很容易找到如何扩展 pandas 的方法……

# 解决方案

我想实现的第一个函数是概率和条件概率。让我们考虑以下数据集……

![](img/61346ad3797b5150c68255667910507b.png)

作者提供的图像

这是一个合成数据集，捕捉了训练、技能和收入之间的关系，每一行代表一个个体，该个体要么接受了训练（或没有），获得了技能（或没有），收入增加了（或没有）。

了解数据后，知道训练是否接受过将会非常有用。

这可以表示为一个数学公式如下：**ℙ(𝑡𝑟𝑎𝑖𝑛𝑖𝑛𝑔=1)**，意思是“训练=1 的概率是多少？”。

传统的做法是编写特定的代码，将 `DataFrame` 筛选到仅包含 `training==1` 的行，并计算过滤后的行数占整体数据的比例…

这相当直接，但每次需要计算概率时必须编写特定代码，而且这段代码与概率的关系并不立即显现。如果我们能简单地写 `df_training.probability("p(training=1)")` 不是更好吗？

好吧，事实证明，这可以用 7 行程序代码完成，无论是任何概率还是条件概率！

## 概率的完整源代码

以下是完整代码及详细解释…

## 理解正则表达式

通过使用正则表达式来实现字符串解析，代码已经大大压缩，尽管这不是关于正则表达式的文章，但花点时间理解它们会很有用…

使用像 [`regex101.com/`](https://regex101.com/) 这样的免费解析器来理解正则表达式会更容易。有了 regex101，解析和解释使用的正则表达式变得简单…

+   `VARIABLE_REGEX` - `[a-zA-Z0-9_ "\']+` 匹配变量名，例如 "SurnameField", "ABC123", X, training 等等。

+   `OPERATOR_REGEX` - `=|<|<=|>|>=|!=` 匹配常见的数学运算符，即 =, <, <=, >, >=, !=

+   `VALUE_REGEX` - `\'[^\']*\'|\"[^\"]*\"|-?\d+\.?\d*` 匹配一个值，例如 48.52, 5, "01/01/2023", 'Hello' 等等。

`ASSIGNMENT_REGEX` 然后将这些表达式连接起来以识别模式 VARIABLE, OPERATOR, VALUE。

例如…

`re.findall(ASSIGNMENT_REGEX, "training=1, income=0")` 将 `"training=1, income=0"` 解析为 `[('training', '=', '1'), ('income', '=', '0')]`

最终我们有 `PROB_REGEX` - `(?i)^p\(([^|]+)(?:\s*\|\s*)?(.*)?\)$`

这用于解析概率表达式和条件概率表达式如下…

`re.findall(PROB_REGEX, "p(A=5, B=6)")` 返回 `[('A=5, B=6', '')]` 和 `re.findall(PROB_REGEX, "p(A=5, B=6 | X=1, Y=2)")` 返回 `[('A=5, B=6 ', 'X=1, Y=2')]`

## 概率和条件概率

在这一点上，值得稍微偏离一下，探索条件概率语句。

我们已经确定 **ℙ(𝑡𝑟𝑎𝑖𝑛𝑖𝑛𝑔=1)** 可以理解为“训练=1 的概率是多少？”

下一个表达式是一个条件概率：**ℙ(𝑖𝑛𝑐𝑜𝑚𝑒=1∣𝑡𝑟𝑎𝑖𝑛𝑖𝑛𝑔=1)。**

它可以理解为“在 training = 1 的情况下，income = 1 的概率是多少？” 即在我们的数据示例中，“在接受培训的情况下，收入增加的概率是多少？”

表达式中“|”左侧的部分被称为“结果”，右侧的部分称为“事件”，整个表达式 **ℙ(𝑜𝑢𝑡𝑐𝑜𝑚𝑒∣𝑒𝑣𝑒𝑛𝑡𝑠)** 可以理解为“在事件已经发生的情况下，结果的概率是多少？”

## 理解函数

所以为了分解我们的代码，让我们从 `probability` 函数开始...

`parse_probability_expr = re.findall(PROB_REGEX, expression)` 解析传入的表达式，所以 `"p(income=1 | training=1, skills=1)"` 被解析为 `[('income=1 ', 'training=1, skills=1')]`。

…因此 `parse_probability_expr[0][1].strip()` 包含 `'training=1, skills=1'` 和 `parse_probability_expr[0][0].strip()` 包含 `'income=1'`

为了理解函数的其余部分，让我们看看 `_filter_dataset` 函数...

下一步是解包 `_filter_dataset` 函数...

`_filter_dataset` 处理部分概率表达式如 `'training=1, skills=1'` 并将其转换为 `DataFrame` 过滤器。

例如 `'training=1, skills=1'` 被转换为 `"(data['training'] == 1) & (data['skills'] == 1)"`，然后应用到 `DataFrame` 中以进行过滤。

剩下的代码使用 `_filter_dataset` 从表达式中计算概率...

还要注意的是，如果没有条件，例如在表达式为 `p(training=1)` 的情况下，第 1 步仅返回完整数据集，因此第 2 步将正确计算（无条件）概率。

## 扩展 Pandas `DataFrame` 类

剩下要做的就是使用 Python “monkey patching” 语法扩展 `DataFrame` 类，添加一个新方法...

`pd.DataFrame.probability = probability`

这实际上为 pandas `DataFrame` 类添加了一个新方法，任何你创建的 `DataFrame` 都可以调用新方法来计算数据上的概率和条件概率。

此外，如果完整的源代码存储在一个单独的 Python 模块中，例如 `dataframe_extensions.py`，那么只需一行导入即可包含这些扩展方法和你编写的任何其他代码...

`import dataframe_extensions`

## 测试解决方案

一旦代码编写完成并且 `DataFrame` 类已被扩展，调用它就变得非常简单和直观...

`0.5`

`0.05`

`0.1`

# 额外部分 — 期望值

期望值或期望表示随机变量的平均值或长期值。

当我们在 `DataFrame` 列中有一组连续数据时，这将简单地是均值 - `df_training["training"].mean()`，这似乎不值得扩展功能。

然而，期望值可以是条件性的，例如 **𝔼[𝑖𝑛𝑐𝑜𝑚𝑒|𝑡𝑟𝑎𝑖𝑛𝑖𝑛𝑔=1]** 可以理解为“在 training = 1 的情况下，收入的期望值是多少？”

通过重用上面开发的编码模式来扩展 `DataFrame` 以计算概率，期望值可以在仅 3 行代码中实现为方法扩展…

## 期望值的完整源代码

这里是一个合成数据集，包含连续数据，并附带一些使用上面代码计算期望值的示例…

![](img/20a2e5e4f1057dd869a638477edd1179.png)

图片由作者提供

… 这里是一些测试，展示了期望值扩展的实际效果 …

`4.412406593060421`

`4.429821998698432`

`3.7051574107849214`

# 结论

通过添加新方法或属性来扩展库类，可以生成易于理解、简洁、清晰且可维护的源代码。

Python 的“猴子补丁”约定可以很容易地用于实现扩展，即使这些类存在于像 pandas 这样的标准外部库中。

通过为扩展类创建一个单独的模块，并将其导入到未来的项目中，Python 开发者可以建立一个扩展库，以帮助提高未来项目的效率和有效性。

# 连接并保持联系 …

如果你喜欢这篇文章，你可以通过每月仅需$5 成为 Medium 会员，获得对更多文章的无限访问权限，方法是 [点击我的推荐链接](https://grahamharrison-86487.medium.com/membership)（如果你通过这个链接注册，我将从费用中获得一部分，但你无需额外付费）。

[](https://grahamharrison-86487.medium.com/membership?source=post_page-----f4ae983ad715--------------------------------) [## 通过我的推荐链接加入 Medium - 格雷厄姆·哈里森

### 作为 Medium 会员，你的会员费用的一部分会分配给你阅读的作者，同时你可以完全访问每个故事…

grahamharrison-86487.medium.com](https://grahamharrison-86487.medium.com/membership?source=post_page-----f4ae983ad715--------------------------------)

… 或通过 … 进行连接

[订阅免费电子邮件，以便每当我发布新故事时获得通知](https://grahamharrison-86487.medium.com/subscribe)。

[快速查看我之前的文章](https://grahamharrison-86487.medium.com/)。

[下载我的免费战略数据驱动决策框架](https://relentless-originator-3199.ck.page/5f4857fd12)。

访问我的数据科学网站 — [数据博客](https://www.the-data-blog.co.uk/)。
