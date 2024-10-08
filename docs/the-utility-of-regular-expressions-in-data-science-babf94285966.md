# 正则表达式在数据科学中的实用性

> 原文：[`towardsdatascience.com/the-utility-of-regular-expressions-in-data-science-babf94285966`](https://towardsdatascience.com/the-utility-of-regular-expressions-in-data-science-babf94285966)

## Python 中常见应用的示例

[](https://thomasdorfer.medium.com/?source=post_page-----babf94285966--------------------------------)![Thomas A Dorfer](https://thomasdorfer.medium.com/?source=post_page-----babf94285966--------------------------------)[](https://towardsdatascience.com/?source=post_page-----babf94285966--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----babf94285966--------------------------------) [Thomas A Dorfer](https://thomasdorfer.medium.com/?source=post_page-----babf94285966--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----babf94285966--------------------------------) ·7 分钟阅读·2023 年 1 月 5 日

--

![](img/5966cb182cb7e1ef7df827d76ab910c0.png)

图片由 [Kevin Ku](https://unsplash.com/@ikukevk) 提供，来源于 [Unsplash](https://unsplash.com/photos/w7ZyuGYNpRQ)

# 介绍

数据科学家经常发现自己需要确定数据中的特定字段是否符合所需的文本格式，或特定字符串是否存在。在其他情况下，他们可能需要将数据中的特定字符串替换为另一个。为实现这一点，他们使用已成为这类问题常规的**正则表达式**。

本文将简要介绍什么是正则表达式，介绍一些形成相应搜索模式所需的基本字符，展示一些 Python 中常用的函数，并最后讨论一些数据科学家日常生活中经常遇到的实际使用案例。

# 什么是正则表达式？

正则表达式，或称 Regex，是一组字符，用于启用特定文本模式的搜索和——如果需要——替换。这是一种极其方便的技术，数据科学家可以利用它来避免繁琐的手动搜索任务。

为了定义这些搜索模式，必须熟悉相应的语法。[Dataquest](https://www.dataquest.io/wp-content/uploads/2019/03/python-regular-expressions-cheat-sheet.pdf) 提供了一个全面且简洁的 Python 正则表达式语法备忘单。虽然本文不会涵盖所有使用案例，但仍值得强调一些用于定义搜索模式的基本字符（见表 1）。

![](img/8db7a7ccc22f9d90ac07be15ae8099ce.png)

表 1：定义正则表达式搜索模式的基本字符。作者提供的图示，灵感来源于 [Dataquest](https://www.dataquest.io/wp-content/uploads/2019/03/python-regular-expressions-cheat-sheet.pdf)。

# Python 中的正则表达式

Python 提供了一个名为[**re**](https://docs.python.org/3/library/re.html)的模块，该模块提供了一整套丰富的正则表达式匹配操作。在这里，我们将重点介绍四个常用的模块函数：

1.  **搜索：** 匹配表达式的第一个实例。

1.  **查找所有：** 匹配表达式的所有实例。

1.  **替换：** 用另一个字符串替换指定的字符串。

1.  **拆分：** 根据指定的分隔符拆分字符串。

让我们通过一些具体的例子来进一步说明这些概念：

```py
import re

# Defining the string upon which we'll run some regex operations
s = "Paris is the capital of France. Also, Paris is a beautiful city."

# Search
re.search("Paris", s)
>>> <re.Match object; span=(0, 5), match='Paris'>

# Find All
re.findall("Paris", s)
>>> ['Paris', 'Paris']

# Sub
re.sub("Paris", "Rome", s)
>>> 'Rome is the capital of France. Also, Rome is a beautiful city.'

# Split
re.split("\. ", s)
>>> ['Paris is the capital of France', 'Also, Paris is a beautiful city.']
```

正如预期的那样，**search** 函数只返回表达式的第一个实例作为 *re.Match* 对象。另一方面，**findall** 函数匹配表达式的所有实例并将它们以列表形式返回。使用 **sub** 函数，可以将特定字符串替换为另一个字符串——在这种情况下，*Paris* 被替换为 *Rome*。最后，可以使用 **split** 函数并指定特定的分隔符来拆分字符串——在这种情况下，是句点符号。请注意，我们必须在句点前使用转义字符 `\` 来转义它在正则表达式中的特殊含义。

# 数据科学中的实用性

基于模式的搜索是数据科学家经常遇到的一个概念。让我们通过一些例子来突显将正则表达式作为工具放入工具箱中的实用性。

## 验证文本格式

一个相当常见的场景是表格数据的分析，其任务是检查某一列中的字段是否符合所需的格式。假设我们在一个名为 `sample.csv` 的文件中有一些虚构数据，该文件包含以下列：电子邮件地址、城市、州和邮政编码（表 2）。

![](img/6f0fefd9957b87db5a92f4ebe0db8d77.png)

表 2：包含电子邮件和地理信息的表格化的虚构数据。

现在，我们希望验证所有电子邮件地址是否符合 `firstname.lastname@gmail.com` 的格式。利用正则表达式，我们可以进行以下操作：

```py
import re
import pandas as pd

df = pd.read_csv("sample.csv")

# Define pattern that matches the email format firstname.lastname@gmail.com
pattern = "^[a-z]+\.+[a-z]+@gmail.com$"

# Iterate through rows and print those that do not match
for index, row in df.iterrows():
    match = re.search(pattern, row.Email)
    if match == None:
        print(index, *row)

# Output
>>> 0 johnsmith@gmail.com Pasadena California 91103
>>> 3 matt.hawkins@hotmail.com Kailua Hawaii 96734
```

如果模式与字符串输入不匹配，**search** 函数将返回 `None`。如果我们打印出这些场景，我们可以立即发现第 0 行的电子邮件地址的名字和姓氏没有用句点分隔，而第 3 行的电子邮件地址使用了 Hotmail 而不是 Gmail。

以类似的方式，我们可以验证数据中的邮政编码是否符合标准的五位数字格式。

```py
# Define pattern that matches 5-character strings comprised of all digits
pattern = "^\d{5}$" 

for index, row in df.iterrows():
    match = re.search(pattern, row.ZIP)
    if match == None:
        print(index, *row)

# Output
>>> 2 Kirkland Washington 9560
```

我们可以看到在第 2 行，邮政编码是错误的，因为它只有四位数字。

相同的原理可以应用于各种格式特定的字段输入，如日期、网址、电话号码、IP 地址、社会安全号码等。

## 检测模糊词

可以对文本进行分析，以查找某些短语、单词或其模糊形式。例如，垃圾邮件发送者倾向于模糊特定的单词，希望绕过垃圾邮件过滤器。例如，可以使用*j@ckpot, jackp0t,* 或 *j@ackp0t*来代替*jackpot*。正则表达式可以确保不仅捕获感兴趣的单词，还能捕获一些其模糊形式：

```py
import re

# Define pattern capable of handling some obfuscations of "jackpot"
pattern = "^j[a@]ckp[o0]t$"

re.search(pattern, "j@ckpot")
re.search(pattern, "jackp0t")
re.search(pattern, "j@ckp0t")

# Outputs
>>> <re.Match object; span=(0, 7), match='j@ckpot'>
>>> <re.Match object; span=(0, 7), match='jackp0t'>
>>> <re.Match object; span=(0, 7), match='j@ckp0t'>
```

在这里，我们定义了一组字符，用于在特定位置进行匹配，例如位置 1 的`[a@]`和位置 5 的`[o0]`。因此，我们上面提到的所有*jackpot*的模糊匹配都将被识别。

## 正确的格式或拼写

假设我们有一些文本数据，其中包含以美国格式表示的日期，即`MM/DD/YYYY`。然而，我们希望将这些数据中的所有日期转换为更广泛使用的格式`DD/MM/YYYY`。同样，可以应用正则表达式来解决这个问题：

```py
import re

string = "I have a dentist appointment on 04/06/2023 at 10:00 AM."

# Define pattern capable of capturing data format
pattern = "(\d{1,2}/)(\d{1,2}/)(\d{4})"

# Substitute days and months
re.sub(pattern, r"\2\1\3", string)

# Output
>>> 'I have a dentist appointment on 06/04/2023 at 10:00 AM.'
```

我们成功地将*04/06/2023*替换为*06/04/2023*。具体来说，我们使用`( )`对对应于月份（组 1）、日期（组 2）和年份（组 3）的表达式进行分组。为了交换月份和日期，我们只需在替换参数中指定所需的组顺序——在我们的例子中是`r"\2\1\3"`。请注意，我们在这里使用原始字符串表示法，以避免在没有`r`前缀的字符串字面量中出现的反斜杠特殊处理。

类似的方法可以用于纠正文本中的拼写错误，例如将*gray*替换为*grey*，或反之。

## 计算生物学中的应用

另一个广泛使用正则表达式的领域是计算生物学。蛋白质由一串氨基酸组成，这些氨基酸在折叠成最终的三维结构后，通常在我们体内发挥特定的功能。检测特定结构模式的存在或缺失——一种与特定功能相关的短氨基酸模式——对于更好地理解其最终功能可能至关重要。

假设我们有一组短蛋白质，我们希望找到那些包含以下模式的蛋白质：`xC[DA]GG{Y}`。这是一种生物学符号，解释如下：任何氨基酸（x），后跟半胱氨酸（C），后跟天冬氨酸（D）或丙氨酸（A），后跟两个连续的甘氨酸（G），后跟任何氨基酸但不包括酪氨酸（Y）。使用 Python 中的正则表达式，我们可以检测到包含此模式的蛋白质，如下所示：

```py
import re

# Define pattern capable of matching the above-mentioned motif
pattern = ".C[DA]GG[^Y]"

proteins = ['AARKYL', 'LELCDGGPG', 'RAAANCDD', 'LYYRCAGGEGPGG', 'CAEELR']  

for prot in proteins:
    match = re.search(p, prot)
    if match:
        print(prot, match)

# Output
>>> LELCDGGPG <re.Match object; span=(2, 8), match='LCDGGP'>
>>> LYYRCAGGEGPGG <re.Match object; span=(3, 9), match='RCAGGE'>
```

这种方法可以正确识别出含有我们寻找的特定结构的两个蛋白质。

# 结论

正如我们所看到的，正则表达式在各种学科中都非常有用，从电子邮件垃圾过滤器到计算生物学。本文仅展示了正则表达式常见应用的几个场景，但其适用范围实际上非常广泛。尽管正则表达式有其局限性，并且肯定不是解决所有文本处理问题的万灵药，但它们应该被视为每个数据科学家工具箱中的基本工具。

# 参考文献

[1] *数据科学备忘单*。Dataquest。检索日期：2023 年 1 月 4 日，网址：[`www.dataquest.io/wp-content/uploads/2019/03/python-regular-expressions-cheat-sheet.pdf`](https://www.dataquest.io/wp-content/uploads/2019/03/python-regular-expressions-cheat-sheet.pdf)

[2] *正则表达式操作*。Python。检索日期：2023 年 1 月 4 日，网址：[`docs.python.org/3/library/re.html`](https://docs.python.org/3/library/re.html)
