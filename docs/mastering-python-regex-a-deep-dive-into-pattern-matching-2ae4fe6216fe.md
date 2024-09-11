# 掌握 Python RegEx：深入探讨模式匹配

> 原文：[https://towardsdatascience.com/mastering-python-regex-a-deep-dive-into-pattern-matching-2ae4fe6216fe?source=collection_archive---------2-----------------------#2023-07-24](https://towardsdatascience.com/mastering-python-regex-a-deep-dive-into-pattern-matching-2ae4fe6216fe?source=collection_archive---------2-----------------------#2023-07-24)

## *Python 正则表达式揭秘：解读使用 Python 的 re 模块进行模式匹配的艺术*

[](https://nathanrosidi.medium.com/?source=post_page-----2ae4fe6216fe--------------------------------)[![Nathan Rosidi](../Images/f500246a4d2fb080a73f6ef740c226d2.png)](https://nathanrosidi.medium.com/?source=post_page-----2ae4fe6216fe--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2ae4fe6216fe--------------------------------)[![数据科学前沿](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----2ae4fe6216fe--------------------------------) [Nathan Rosidi](https://nathanrosidi.medium.com/?source=post_page-----2ae4fe6216fe--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fab636cbf3611&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmastering-python-regex-a-deep-dive-into-pattern-matching-2ae4fe6216fe&user=Nathan+Rosidi&userId=ab636cbf3611&source=post_page-ab636cbf3611----2ae4fe6216fe---------------------post_header-----------) 发表在 [数据科学前沿](https://towardsdatascience.com/?source=post_page-----2ae4fe6216fe--------------------------------) ·16 min read·2023年7月24日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F2ae4fe6216fe&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmastering-python-regex-a-deep-dive-into-pattern-matching-2ae4fe6216fe&user=Nathan+Rosidi&userId=ab636cbf3611&source=-----2ae4fe6216fe---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F2ae4fe6216fe&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmastering-python-regex-a-deep-dive-into-pattern-matching-2ae4fe6216fe&source=-----2ae4fe6216fe---------------------bookmark_footer-----------)![](../Images/d5211f787d3686937ee70256f8039e72.png)

图片由作者在 [Canva](https://www.canva.com/) 上创建

# 什么是 Python RegEx 或正则表达式？

正则表达式，通常缩写为 regex，是处理文本的强大工具。它们本质上是一系列字符，用于建立搜索模式。这个模式可以用于各种字符串操作，包括匹配模式、替换文本和分割字符串。

# 正则表达式的历史

![](../Images/355706d0bcfe6d04d89723fe03d035e7.png)

*图像来源:* [*http://serge.mehl.free.fr/chrono/Kleene.html*](http://serge.mehl.free.fr/chrono/Kleene.html)

数学家斯蒂芬·科尔·克利尼在1950年代首次引入了正则表达式，作为描述正规集合或正规语言的记号。

今天，正则表达式已成为程序员、数据科学家和 IT 专业人员的必备技能。

# Python 正则表达式或正则表达式的意义及应用

在深入了解这些正则表达式如何使用 Python 之前，让我们看看它的不同应用范围，以激励自己。

+   **数据验证**：正则表达式在验证不同类型的数据时非常有用。（电子邮件地址、电话号码）

+   **网页抓取**：在通过网页抓取数据时，正则表达式可以用来解析 HTML 并提取必要的信息。

+   **搜索和替换**：正则表达式擅长识别符合特定模式的字符串并用替代品替换它们。这一能力在文本编辑器、数据库和编程中尤其有价值。

+   **语法高亮**：许多文本编辑器使用正则表达式来进行语法高亮。

+   **自然语言处理 (NLP)**：在 NLP 中，正则表达式可以用于标记化、词干提取以及其他各种文本处理功能。

+   **日志分析**：在处理日志文件时，正则表达式在提取特定日志条目或分析一段时间内的模式方面非常有效。

现在我希望你已经有足够的动力了！

让我们开始使用 re 模块，它专注于正则表达式。

# Python 的 re 模块入门

很好，让我们从 Python 的 re 模块基础知识开始。在接下来的部分，我们将涵盖更多高级主题。

# re 模块简介

Python 通过 re 模块提供了对正则表达式的原生支持。

这个模块是 Python 的标准库，这意味着你不需要外部安装，它会随每个 Python 安装包一起提供。

re 模块包含了用于处理正则表达式的各种函数和类。一些函数用于匹配文本，一些用于拆分文本，还有一些用于替换文本。

它包括了广泛的函数和类，专门用于处理正则表达式。其中某些函数用于文本匹配，其余的用于文本拆分或文本替换。

# 导入 re 模块

正如我们已经提到的，它与安装一起提供，因此无需担心安装问题。

这就是为什么，为了开始在 Python 中使用正则表达式，你需要首先导入 re 库。你可以使用如下的 import 语句来完成这一步。

```py
import re
```

在导入库之后，你可以开始使用 re 模块提供的功能，如函数和类。

让我们从一个简单的例子开始。

假设你想在一个字符串中找到所有“Python”一词的出现。

我们可以使用来自 re 模块的 findall() 函数。

下面是代码。

```py
import re 
# Sample text 
text = "Python is an amazing programming language. Python is widely used in various fields." 
# Find all occurrences of 'Python' 
matches = re.findall("Python", text) 
# Output the matches 
print(matches)
```

下面是输出。

![](../Images/06fbbbbf6dd2e2d0b4bb8738226a45cd.png)

在 `re` 模块中还有许多其他函数，我们可以用它们来构建更复杂的模式。

首先，让我们来看一下 `re` 模块中的常见函数。

# `re` 模块中的常见函数

在向你介绍 Python RegEx 的基本知识之前，我们先来了解常见的函数，以便更好地掌握剩下的概念。`re` 模块包括许多不同的函数。通过使用它们，我们可以执行不同的操作。

在接下来的部分中，我们将发现其中的一些函数。

![](../Images/316381be82d9bd520f1841735655d0c0.png)

图片由作者在 [Canva](https://www.canva.com/) 创建

# a. `re.match()` 函数

`re.match()` 用于捕捉正则表达式是否以特定字符串开始。

如果有匹配，函数返回一个匹配对象；如果没有匹配，则返回 None。

接下来，我们将使用 `re.match()` 函数。在这里，我们将检查字符串文本是否以“Python”这个词开始。然后我们将结果打印到控制台。

下面是代码。

```py
import re
pattern = "Python"
text = "Python is amazing."
# Check if the text starts with 'Python'
match = re.match(pattern, text)
# Output the result
if match:
    print("Match found:", match.group())
else:
    print("No match found")
```

下面是输出。

![](../Images/13953395f653add262f3c05dc0f0facc.png)

输出显示模式“Python”匹配文本的开头。

# b. `re.search()` 函数

与 `re.match()` 相对的是，`re.search()` 函数扫描整个字符串以查找匹配项，如果发现匹配项，则返回一个匹配对象。

在以下代码中，我们使用 `re.search()` 函数在字符串文本中查找“amazing”这个词。如果找到这个词，我们将其打印出来；否则，打印“未找到匹配项”。

下面是代码。

```py
pattern = "amazing"
text = "Python is amazing."
# Search for the pattern in the text
match = re.search(pattern, text)
# Output the result
if match:
    print("Match found:", match.group())
else:
    print("No match found")
```

下面是输出。

![](../Images/88ce4b491c7aec745a428317c75614f3.png)

输出显示我们的代码从给定文本中捕捉到了“amazing”。

# c. `re.findall()` 函数

`re.findall()` 函数用于收集字符串中所有非重叠的模式匹配项。它将这些匹配项作为字符串列表返回。

在以下示例中，我们使用 `re.findall()` 函数来查找字符串中的所有“a”。匹配项作为列表返回，然后我们将其打印到控制台。

下面是代码。

```py
pattern = "a"
text = "This is an example text."
# Find all occurrences of 'a' in the text
matches = re.findall(pattern, text)
# Output the matches
print(matches)
```

下面是输出。

![](../Images/38797c77d84525c72ee82185f20b457e.png)

输出展示了文本中所有非重叠的字母“a”出现的情况。

# d. `re.finditer()` 函数

`re.finditer()` 函数类似于 `re.findall()`，但它返回一个迭代器，迭代器生成匹配对象。

在以下代码中，使用 `re.finditer()` 函数来查找字符串文本中所有字母“a”的出现情况。它返回一个匹配对象的迭代器，我们打印每个匹配的索引和值。

下面是代码。

```py
pattern = "a"
text = "This is an example text."
# Find all occurrences of 'a' in the text
matches = re.finditer(pattern, text)
# Output the matches
for match in matches:
    print(f"Match found at index {match.start()}: {match.group()}")
```

下面是输出。

![](../Images/3963aa4048a0e9c8b6c9c48e94102261.png)

输出显示了模式“a”在文本中的索引。

# e. `re.sub()` 函数

`re.sub()` 函数用于将一个字符串替换为另一个字符串。

接下来，我们将使用 `re.sub()` 函数将“Python”替换为“Java”。

然后我们打印修改后的字符串。

下面是代码。

```py
pattern = "Python"
replacement = "Java"
text = "I love Python. Python is amazing."
# Replace 'Python' with 'Java'
new_text = re.sub(pattern, replacement, text)
# Output the new text
print(new_text)  # Output: "I love Java. Java is amazing."
```

这是输出结果。

![](../Images/8486f32dba380145eae49f69b5511d89.png)

输出显示我们可以成功将“Python”替换为“Java”。

在下一节中，我们将深入探讨可以在正则表达式中用于匹配各种文本模式的基本模式。

# Python 正则表达式中的基本模式

让我们从基本模式开始。

正则表达式是通过字面字符、元字符和量词的组合构建的。因此，掌握这些基本组件对于创建有效的正则表达式至关重要。

让我们从字面字符开始。

# a. 字面字符

字面字符是正则表达式中最简单的模式匹配形式。

它们本身完全匹配，没有特殊含义。

例如，正则表达式 python 将精确匹配字符串 python。

```py
import re 
pattern = "python"
text = "I love programming in python!" 
# Find all occurrences of 'Python' 
matches = re.findall(pattern, text) 
# Output the matches 
print(matches)
```

这是输出结果。

![](../Images/98d006028b6a64feb2e1ff645130f419.png)

输出显示我们的 re.findall() 函数找到了所有“python”模式的实例。

# b. 元字符

元字符如“.”、“^”、 “$”。这些字符在处理字符串时可能非常重要。让我们看看。

## i. 点（.）

点号 . 就像一张万能牌。它可以代替任何单个字符，除了换行符。

在下面的代码中，我们将使用正则表达式模式“p.t”。

这是代码。

```py
import re 
pattern = "p.t"
text = "pat, pet, p5t, but not pt." 
# Find all occurrences of 'Python' 
matches = re.findall(pattern, text) 
# Output the matches 
print(matches)
```

这是输出结果。

![](../Images/c44c2a62673674d4ba8391eccf7cd5ba.png)

输出显示我们的代码找到了所有以“p”开头并以“t”结尾的三个字符实例。

## ii. 插入符号 (^)

插入符号 ^ 用于检查字符串是否以某个字符开头。

让我们看一个例子。

以下代码检查文本是否以 Hello 开头（匹配找到：“匹配”）或没有（未找到匹配）。

这是代码。

```py
import re
pattern = "^Hello"
text = "Hello, world!"
# Use re.match() because it checks for a match only at the beginning of the string
match = re.match(pattern, text)
# Output the match
if match:
    print("Match found:", match.group())
else:
    print("No match found")
```

这是输出结果。

![](../Images/0ea91ccd449a91fec82c5065ed0d4e9a.png)

输出显示我们的代码捕捉到了文本开头的 hello 模式。

## iii. 美元符号（$）

美元符号 $ 用于检查字符串是否以某个字符结尾。

以下代码检查文本是否以 world$ 结尾（如果是，则打印“匹配找到：‘匹配’”），否则（打印“未找到匹配”）。

这是代码。

```py
import re
pattern = "world$"
text = "Hello, world"
# Use re.search() to search the entire string
match = re.search(pattern, text)
# Output the match
if match:
    print("Match found:", match.group())  # Output: Match found: world
else:
    print("No match found")
```

这是输出结果。

![](../Images/38c8fc07aee1d66759269dbcc02dd749.png)

输出显示 re.search() 函数找到了以“world”结尾的文本。

# c. 量词

量词用于定义在你试图匹配的模式中，字符（或字符）出现的次数。

在本小节中，我们将查看关于星号（*）、加号（+）和问号（?）的示例，并以大括号（{}）结束。

让我们从星号开始。

## i. 星号 (*)

正则表达式中的星号（*）表示前一个字符可以出现零次或多次。

让我们看看代码。在以下代码中，我们首先定义了模式（“py”），然后我们将使用 findall() 函数。

这是代码。

```py
import re

pattern = "py*"
text = "p py pyy pyyy pyyyy"

matches = re.findall(pattern, text)

print(matches)
```

这是输出结果。

![](../Images/080282d827bd7bc7b39b86b2566ac7e2.png)

输出显示所有，因为星号允许“y”出现零次或多次。

## ii. 加号（+）

加号 + 匹配前一个字符的 1 次或更多次重复。

在这里我们再次使用 `findall()` 函数配合 py 模式，但这次我们会使用加号（+）。

这是代码。

```py
import re

pattern = "py+"
text = "p py pyy pyyy pyyyy"

matches = re.findall(pattern, text)

print(matches)  # Output: ['py', 'pyy', 'pyyy', 'pyyy']
```

这是输出。

![](../Images/cf5c1b1d5cc687bada74b5b0da584ac3.png)

从输出中可以看到，加号要求“p”之后至少有一个或多个“y”字符。

## iii. 问号（?）

问号 ? 匹配前一个字符的 0 次或 1 次重复。它使前一个字符成为可选的。

这是代码。

```py
import re

pattern = "py?"

text = "p py pyy pyyy pyyyy"

matches = re.findall(pattern, text)

print(matches)  # Output: ['p', 'py', 'p', 'p', 'p']
```

这是输出。

![](../Images/e46075bfa16b69f5e0a23ff4a1397bf1.png)

在输出中，你可以看到它只匹配了“p”和“py”，因为问号允许“y”出现一次或零次。

## iv. 花括号（{}）

花括号 {} 允许你匹配特定次数的重复。

```py
import re

pattern = "py{2,3}"
text = "py, pyy, pyyy, pyyyy"

matches = re.findall(pattern, text)

print(matches)  # Output: ['pyy', 'pyyy', 'pyy']
```

这是输出。

![](../Images/c1e3852f2a26e8e72cdae20e85c39e85.png)

在这个例子中，模式匹配了“pyy”和“pyyy”，但没有匹配“py”或“pyyyy”，因为我们指定了要匹配“p”之后正好有 2 或 3 个“y”字符。

# Python 正则表达式中的特殊字符

特殊字符可以用来构建更复杂的模式。

![](../Images/dd2fbe5fc6b93a6d4252bbd3800c0e27.png)

图片由作者在 [Canva](https://www.canva.com/) 上创建

# a. 字符类

让我们先看看字符类。

在接下来的例子中，我们将看到其中的 3 个。

让我们从 \d, \D 开始。

## i. \d, \D

“\d”用于查找数字（从 0 到 9），而“\D”用于查找不是数字的元素。

在下面的代码中，“\d”扫描文本字符串并提取文本中的数字。

```py
import re
pattern = "\d"
text = "My phone number is 123-456-7890."
# Find all digits in the text
matches = re.findall(pattern, text)
# Output the matches
print(matches)
```

这是输出。

![](../Images/6b85b1d7d069608beabe2c54938e1b56.png)

输出显示我们找到了文本中的所有数字（0–9）。

## ii. \s, \S

“\s”可以用来查找空白字符，而“\S”可以用来查找不是空白的字符。

在下方，正则表达式 “\s” 识别了给定文本中的所有空格和制表符。

这是代码。

```py
import re
pattern = "\s"
text = "This is a text with spaces and\ttabs."
# Find all whitespace characters in the text
matches = re.findall(pattern, text)
# Output the matches
print(matches)  # Output: [' ', ' ', ' ', ' ', ' ', ' ', '\t']
```

这是输出。

![](../Images/e812a7ec8a6577267b96398574bbd7f2.png)

从输出中可以看出，我们可以识别所有的空白字符。

## iii. \w, \W

“\w”可以用来查找单词（字母、数字和下划线字符），而“\W”则是其相反的。

在下面的代码中，“\w”从文本中提取所有字母和数字。

这是代码。

```py
import re
pattern = "\w"
text = "This is an example with words and numbers 123!"
# Find all word characters in the text
matches = re.findall(pattern, text)
# Output the matches
print(matches)
```

这是输出。

![](../Images/7dc9f617a83454f0068210b047dd7b2b.png)

# b. 预定义字符类

预定义字符类提供了常见类的快捷方式。例如，“\d”是一个预定义字符类，表示数字。

在这种情况下，“\d”模式提取了给定文本中的所有数字。

```py
import re
pattern = "\d"
text = "The year is 2023."
# Find all digits in the text
matches = re.findall(pattern, text)
# Output the matches
print(matches)
```

这是输出。

![](../Images/caf0ef71691f62e07213c4de164e3daf.png)

输出显示我们的代码在文本中找到了所有的预定义字符类“\d”（代表所有数字）实例。

# c. 自定义字符类

自定义字符类允许你使用方括号 [] 定义自己的字符集。

在下面的示例中，自定义字符类 “[aeiou]” 用于查找文本中的所有元音字母。

这是代码。

```py
import re
pattern = "[aeiou]"
text = "This is an example text."
# Find all vowels in the text
matches = re.findall(pattern, text)
# Output the matches
print(matches)
```

这是输出结果。

![](../Images/de8ec38f248f91d0205c26f8d01a120b.png)

输出结果显示了文本中所有我们定义的元音字母的实例。

我们还可以使用“-”来定义字符范围。

这是代码。

```py
pattern = "[A-Z]"
text = "This is an Example Text With Uppercase Letters."
# Find all uppercase letters in the text
matches = re.findall(pattern, text)
# Output the matches
print(matches)
```

这是输出结果。

![](../Images/d99317e04c37175596423be53e5b9f89.png)

这里我们可以看到输出由文本中的大写字母组成。

# 编译 Python 正则表达式

当你在脚本中多次使用相同的正则表达式时，首先将其编译成模式对象可以节省时间。这节省了很多时间，因为正则表达式不需要在每次使用时重新解析。

# a. compile() 方法

可以使用 re.compile() 方法将正则表达式模式编译成模式对象。

一旦我们拥有这个模式对象，我们可以调用它的方法（匹配文本、搜索和其他操作。）

这是代码。

```py
import re
# Compile the regular expression pattern
pattern = re.compile(r'\d+')  # Matches one or more digits
# Use the pattern object to search for matches
text = "There are 3 apples and 4 oranges."
matches = pattern.findall(text)
# Output the matches
print(matches)
```

这是输出结果。

![](../Images/6b9918f5ae19099cb8a82b7bb8732f06.png)

输出结果显示数字。

# b. 编译正则表达式的好处

使用正则表达式的一些好处；

+   **性能**：它更快，特别是当正则表达式需要反复使用时。

+   **可重用性**：一旦编译，相同的模式对象可以在代码的不同部分多次重用。

+   **可读性**：使用模式对象可以使你的代码更简洁，特别是当你使用复杂的正则表达式时。

这是一个编译正则表达式的简单示例：

```py
import re
# Compile the regular expression pattern
pattern = re.compile(r'\d+')  # Matches one or more digits
# Use the pattern object to search for matches in different texts
text1 = "There are 3 apples."
text2 = "I have 15 dollars and 30 cents."
# Find matches in text1
matches1 = pattern.findall(text1)
# Find matches in text2
matches2 = pattern.findall(text2)
# Output the matches
print(matches1)
```

这是输出结果。

![](../Images/bdc2aab30fcacd6fc1b3734c7138a6d6.png)

现在让我们检查第二个文本。

这是代码。

```py
print(matches2)
```

这是输出结果。

![](../Images/d8c8b8899230ae1a78b6d41a92a162bf.png)

上述示例相对简单，帮助你理解可重用性、性能和可读性的重要性，尤其是当我们的模式计划重复使用时。

# 实际示例：提取电话号码

在本节中，让我们通过编写一个 Python 脚本来提取文本中的电话号码，以测试我们所发现的内容。

这是正则表达式的一个常见用法，特别是在数据清理过程中。

# a. 定义正则表达式模式

电话号码可以有不同的格式，尤其是在不同的国家，因此你可以根据自己的需要调整这些数字，对于这个示例，让我们考虑格式为 XXX-XXX-XXXX，其中 X 是数字。

以下代码定义了一个匹配上述格式的模式，并将其编译为正则表达式。

让我们看看代码。

```py
import re
# Define the regular expression pattern for phone numbers
phone_number_pattern = re.compile(r'\d{3}-\d{3}-\d{4}')
```

# b. 使用 findall() 方法

在这个示例中，我们将使用 findall() 方法来提取匹配我们模式的电话号码。

以下代码使用正则表达式模式来查找和提取所有

```py
import re
# Define the regular expression pattern for phone numbers
phone_number_pattern = re.compile(r'\d{3}-\d{3}-\d{4}')

# Sample text with phone numbers
text = """
John Doe: 123-456-7890
Jane Doe: 234-567-8901
Office: 555-555-5555
"""
# Find all phone numbers in the text
phone_numbers = phone_number_pattern.findall(text)
```

# c. 打印结果

最后，让我们把提取的电话号码打印到控制台。

这里是代码。

```py
# Output the phone numbers
print("Phone numbers found:")
for phone_number in phone_numbers:
    print(phone_number)
```

这是输出结果。

![](../Images/3fa87b85a078a729e0a216f2db40a2e4.png)

# d. 完整示例代码

下面是结合上述所有步骤的完整 Python 脚本：

```py
import re
# Define the regular expression pattern for phone numbers
phone_number_pattern = re.compile(r'\d{3}-\d{3}-\d{4}')
# Sample text with phone numbers
text = """
John Doe: 123-456-7890
Jane Doe: 234-567-8901
Office: 555-555-5555
"""
# Find all phone numbers in the text
phone_numbers = phone_number_pattern.findall(text)
# Output the phone numbers
print("Phone numbers found:")
for phone_number in phone_numbers:
    print(phone_number)
```

这是输出结果。

![](../Images/611a3a89982a2d5c60f71fd670ace7b7.png)

# 最佳实践

在继续使用正则表达式时，请记住以下几个最佳实践：

+   **保持简单**：简洁是关键。通常建议使用更简单的模式，因为正则表达式可以瞬间变得复杂。

+   **注释你的模式**：在为你的项目开发正则表达式时，不要忘记在注释中包含说明，因为我们提到过它可能会很复杂，但一旦你这样做了，当你回头看时，你的代码将变得可重用。

+   **彻底测试**：反复测试你的代码，因为正则表达式由于其复杂的性质有时会产生意外的结果，这就是为什么要严格测试，以确保你的工作按预期运行。

+   **使用原始字符串**：在 Python 中处理文本时，有时你会使用具有不同含义的特殊字符（如反斜杠 \ 或 \n 表示换行）。为了避免这种混淆，Python 允许你使用所谓的“原始字符串”。你可以通过在字符串的第一个引号前加上字母“r”来使字符串变成“原始”。这样，Python 就会理解该字符串中的反斜杠应被视为普通字符，而不是特殊字符。

# 结论

在本指南中，我们探讨了 Python 正则表达式的领域。我们从常见函数和基本知识开始，深入了解了更高级的概念和实际示例。但请记住，做实际项目将作为你职业生涯中的一个例子，以加深你对这一领域的理解。通过这样做，你将获得知识，并避免在处理 Python 正则表达式时进行 Google 搜索。

查看这个 [高级 Python 概念综合指南](https://www.stratascratch.com/blog/a-comprehensive-guide-to-advanced-python-concepts/?utm_source=blog&utm_medium=click&utm_campaign=kdn+python+regex) 来了解这些概念的概述。

我希望你通过阅读这篇文章也获得了关于 Python 正则表达式的有价值的信息。

感谢阅读！

*最初发布于* [*https://www.stratascratch.com*](https://www.stratascratch.com/blog/mastering-python-regex-a-deep-dive-into-pattern-matching/?utm_source=blog&utm_medium=click&utm_campaign=kdn+python+regex)*。*
