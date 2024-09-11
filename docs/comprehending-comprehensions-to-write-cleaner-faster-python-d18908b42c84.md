# 理解列表推导式以编写更简洁、更快速的 Python 代码

> 原文：[`towardsdatascience.com/comprehending-comprehensions-to-write-cleaner-faster-python-d18908b42c84?source=collection_archive---------19-----------------------#2023-03-20`](https://towardsdatascience.com/comprehending-comprehensions-to-write-cleaner-faster-python-d18908b42c84?source=collection_archive---------19-----------------------#2023-03-20)

[](https://medium.com/@Carobert?source=post_page-----d18908b42c84--------------------------------)![Charles Mendelson](https://medium.com/@Carobert?source=post_page-----d18908b42c84--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d18908b42c84--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----d18908b42c84--------------------------------) [Charles Mendelson](https://medium.com/@Carobert?source=post_page-----d18908b42c84--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fa6f4d278f87e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcomprehending-comprehensions-to-write-cleaner-faster-python-d18908b42c84&user=Charles+Mendelson&userId=a6f4d278f87e&source=post_page-a6f4d278f87e----d18908b42c84---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d18908b42c84--------------------------------) ·5 分钟阅读·2023 年 3 月 20 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fd18908b42c84&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcomprehending-comprehensions-to-write-cleaner-faster-python-d18908b42c84&user=Charles+Mendelson&userId=a6f4d278f87e&source=-----d18908b42c84---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd18908b42c84&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcomprehending-comprehensions-to-write-cleaner-faster-python-d18908b42c84&source=-----d18908b42c84---------------------bookmark_footer-----------)![](img/858aa3ba9b38a9ad03cd5fee90cae041.png)

图片由 [Jonathan Cooper](https://unsplash.com/@theshuttervision?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/BuPQp8BST4I?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 使用函数和海象运算符编写更好的推导式

# TLDR

1.  将转换逻辑封装在函数中。

1.  在列表推导式中调用该函数。

1.  使用海象运算符在列表推导式命名空间内赋值

# 问题：

列表和字典推导是中级到高级 Python 的基石。不幸的是，它们也很容易被误解为`for`循环的一对一替代品，当你想要生成一个可迭代对象时。

推导式的语法使得将`for`循环的内容直接转移到推导式中时，编写清晰的代码变得具有挑战性。

# 入门：

让我们从一个复杂度适中的问题开始。

对于由[随机名称生成器](https://www.fantasynamegenerators.com/island-names.php)生成的岛屿列表：

1.  移除空白

1.  替换单词“the”

1.  将新单词的首字母变为小写

一个初学者使用`for`循环可能会这样解决问题：

```py
island_names = ['The Neglected Holm', 'The Barnacle Key', 'The Dancing Enclave',
                'The Peaceful Island', 'Allerport Reef', 'Cresstead Archipelago',
                'Petromeny Islet', 'Esterisle Peninsula', 'Traygami Cay',
                'Savaside Peninsula']

new_island_names_loop = []

for island in island_names:
    new_island = island.replace(' ', '')
    new_island = new_island.replace('The', '')
    # turns first letter into lowercase
    new_island = new_island[0].lower() + new_island[1:]
    new_island_names_loop.append(new_island)
```

这段代码有效，并且逻辑清晰，但性能和 Pythonic 风格上并不是特别出色。

# 难以理解的推导式：

仅用列表推导来替换它会产生这段难以理解的乱码：

```py
island_names = ['The Neglected Holm', 'The Barnacle Key', 'The Dancing Enclave',
                'The Peaceful Island', 'Allerport Reef', 'Cresstead Archipelago',
                'Petromeny Islet', 'Esterisle Peninsula', 'Traygami Cay',
                'Savaside Peninsula']

# This is incomprehensible and no one sane would approach the problem this way

new_island_incomprehensible_comprehenson = [
    island.replace(' ', '').replace('The', '')[0].lower() +
    island.replace(' ', '').replace('The', '')[1:]
    for island in island_names]
```

要了解如何修复这个问题，我们必须理解列表推导的起源。Python 从 Haskell 中借鉴了列表和字典推导，Haskell 是一种非常有主张的函数式语言。要有效地使用它们，你需要以函数式思维来考虑。将可迭代对象中的每一项传递给一个函数来执行转换。

# 函数式推导

在这段代码中，我将转换逻辑封装在一个函数中，并在推导式中调用该函数，将逻辑与实现分开：

```py
island_names = ['The Neglected Holm', 'The Barnacle Key', 'The Dancing Enclave',
                'The Peaceful Island', 'Allerport Reef', 'Cresstead Archipelago',
                'Petromeny Islet', 'Esterisle Peninsula', 'Traygami Cay',
                'Savaside Peninsula']

# Understanding that list comprehensions come from functional languages
# helped me realize you're supposed to define a function for complex # transformations
# This also helps improve code quality by putting the transformation logic inside
# a function

def prep_island_name(island):
    new_island = island.replace(' ', '')
    new_island = new_island.replace('The', '')
    return new_island[0].lower() + new_island[1:]

new_island_comprehension = [prep_island_name(island) for island in island_names]
```

在这里，我们保留了原始循环的清晰度，但能够利用推导式的性能提升。

使用`for`循环也是一种好的实践。将逻辑与实现分开使代码更具模块化。

# 如果我们需要在推导式中使用相同的函数怎么办？

如果我们的问题需要保留多个转换步骤，并多次使用相同的函数怎么办？

让我们稍微修改一下提示。

现在我们想对我们的岛屿列表进行操作，并：

1.  移除空白

1.  替换单词“the”

1.  将清理后的字符串作为字典中的键

1.  将相同的字符串（首字母小写）作为值附加。

初学者可能会像以前一样使用`for`循环解决问题：

```py
island_names = ['The Neglected Holm', 'The Barnacle Key', 'The Dancing Enclave', 'The Peaceful Island',
                'Allerport Reef', 'Cresstead Archipelago', 'Petromeny Islet', 'Esterisle Peninsula', 'Traygami Cay',
                'Savaside Peninsula']

new_island_dict = {}
for island in island_names:
    new_island = island.replace(' ', '')
    new_island = new_island.replace('The', '')
    # turns first letter into lowercase
    new_island_dict[new_island] = new_island[0].lower() + new_island[1:]
```

与我们之前的循环类似，这很容易理解，逻辑清晰，但繁琐。

# 使用函数式推导

我们将对之前的函数做几个改动。我们将把逻辑分成两个函数，一个用于替换不需要的字符，另一个用于将首字母转换为小写：

```py
island_names = ['The Neglected Holm', 'The Barnacle Key', 'The Dancing Enclave',
                'The Peaceful Island', 'Allerport Reef', 'Cresstead Archipelago',
                'Petromeny Islet', 'Esterisle Peninsula', 'Traygami Cay',
                'Savaside Peninsula']

def prep_island_name(island):
    new_island = island.replace(' ', '')
    new_island = new_island.replace('The', '')
    return new_island

def turn_first_letter_lowercase(string):
    return string[0].lower() + string[1:]

# One challenge of using a dictionary comprehension is that we can end up making
# the same function call multiple times.
new_island_comprehension = {prep_island_name(island): 
turn_first_letter_lowercase(prep_island_name(island))
                            for island in island_names}
```

这个逻辑是合理的，但我们需要调用`prep_island_name()`两次，这样显得繁琐且效率低下。

# 海象操作符来救援

海象操作符`:=`（也称为赋值操作符），在 Python 3.8 中引入，允许你在传统上不能使用`=`的命名空间中定义变量名。

现在，除了两次调用 prep_island_name()函数外，我们可以使用赋值运算符在列表推导内部定义一个变量，并将该变量传递给 turn_first_letter_lowercase()。请特别注意我们列表推导中包围关键定义的括号：

```py
island_names = ['The Neglected Holm', 'The Barnacle Key', 'The Dancing Enclave',
                'The Peaceful Island', 'Allerport Reef', 'Cresstead Archipelago',
                'Petromeny Islet', 'Esterisle Peninsula', 'Traygami Cay',
                'Savaside Peninsula']

def prep_island_name(island):
    new_island = island.replace(' ', '')
    new_island = new_island.replace('The', '')
    return new_island

def turn_first_letter_lowercase(string):
    return string[0].lower() + string[1:]

# by using a walrus operator we can call the function once and then assign it a
# variable name in the comprehension name space, avoiding redundant function
# calls
new_island_comprehension_walrus = {
    (new_island := prep_island_name(island)): turn_first_letter_lowercase(new_island)
    for island in island_names}
```

# 结论

我花了令人尴尬的长时间才弄明白如何有效地使用列表推导，因为我一直把它们认为是 for 循环。功能性编程帮助我打破了对 for 循环的依赖，并编写了更简洁、更快速的代码。

学习海象运算符去除了 for 循环的最后一个优势——轻松的变量赋值。

# 关于

查尔斯·门德尔森是一位总部位于西雅图的数据工程师，同时也是华盛顿大学继续教育学院的教学助理，在那里他教授 Python 证书课程。

他即将从哈佛延伸学院获得心理学硕士学位。如果你想与他联系，最好的方式是通过[LinkedIn](https://www.linkedin.com/in/charles-mendelson-carobert/)，他曾被 Databand.ai 评选为 2022 年 25 位数据工程影响者之一。

*最初发表于* [*https://charlesmendelson.com*](https://charlesmendelson.com/tds/effective-comprehensons-in-python/) *2023 年 3 月 20 日。*
