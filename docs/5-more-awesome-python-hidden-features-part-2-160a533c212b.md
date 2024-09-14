# 5 个更多超棒的 Python 隐藏功能 — 第二部分

> 原文：[`towardsdatascience.com/5-more-awesome-python-hidden-features-part-2-160a533c212b`](https://towardsdatascience.com/5-more-awesome-python-hidden-features-part-2-160a533c212b)

## PYTHON | PROGRAMMING | FEATURES

## 探索一些强大的功能，以释放 Python 的全部潜力

[](https://david-farrugia.medium.com/?source=post_page-----160a533c212b--------------------------------)![David Farrugia](https://david-farrugia.medium.com/?source=post_page-----160a533c212b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----160a533c212b--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----160a533c212b--------------------------------) [David Farrugia](https://david-farrugia.medium.com/?source=post_page-----160a533c212b--------------------------------)

·发布在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----160a533c212b--------------------------------) ·5 分钟阅读·2023 年 3 月 22 日

--

![](img/25e6d165f55c74ea200f659a5606e136.png)

图片由 [Joshua Reddekopp](https://unsplash.com/@joshuaryanphoto?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

Python 是一种强大且稳健的语言 —— 使 Python 脱颖而出的一个原因是它的多功能性和动态性。Python 以其酷炫的‘一行代码’而闻名。毫无疑问，每当我们看到一行优美的 Pythonic 代码时，都会感到兴奋或好奇。

在我们之前的帖子中，我们讨论了 Python 编程语言中的 5 个超棒的隐藏功能。你可以在这里找到那篇文章：

[](/5-awesome-python-hidden-features-a0172e0bd98e?source=post_page-----160a533c212b--------------------------------) ## 5 个超棒的 Python 隐藏功能 — 第一部分

### 利用这些酷炫的隐藏 Python 功能，将你的编码技能提升到一个新的水平

towardsdatascience.com

我收到了很多积极的反馈，有些人甚至联系我请求更多这样的强大 Python 技巧。

那么，来看看这 5 个酷炫的隐藏功能，它们可以让你在用 Python 编程时变得更加厉害！

# 隐藏功能 1: 列表步进

列表步进用于选择列表的不同部分，甚至可以选择间隔不同的项。语法如下：

```py
list[start:end:step]
```

+   start: **包括**的第一个元素的索引

+   end: **排除**的第一个元素的索引

+   step: 每次迭代之间我们跳过的元素数量

假设我们有一个包含数字 0–9 的列表，我们想要返回只有偶数的部分。我们可以这样做：

```py
my_list: list = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
even_numbers: list = my_list[::2]  # [0, 2, 4, 6, 8]
```

在这里，我们不指定起始和结束索引，因此，Python 将起始索引视为第一个元素，结束索引视为最后一个元素（即整个列表）。然后我们指定步长为 2。因此，Python 将从第一个元素开始并返回它（即 0）。Python 然后移动 2 步（移动到 1，然后移动到 2），并返回结果（即 2）。这个过程会重复，直到遍历完整个列表。

使用列表步进的另一个强大技巧是能够使用负索引反转列表。

```py
my_list: list = [1, 2, 3, 4, 5]
reversed_list: list = my_list[::-1]  # [5, 4, 3, 2, 1]
```

# 隐藏功能 2：链式比较操作符

在编程中，我们经常需要作为逻辑流程的一部分评估多个比较。

假设我们有一个变量 `x`，我们想确保 `x` 大于 1 但小于 10。我们通常会这样做：

```py
x: int = 5

condition1: bool = x > 1  # check if x is greater than 1
condition2: bool = x < 10 # check if x is smaller than 10 

print(condition1 and condition2) # True
```

在 Python 中，我们可以按如下方式链式比较：

```py
x: int = 5

print(1 < x < 10)   # True
print(10 < x < 20)  # False
```

我们也可以这样做：

```py
x: int = 5

print(5 == x > 4)  # True
print(x < 10 < x*10 < 100)  # True
```

# 隐藏功能 3：复数/虚数

如果你学习过数学，你对复数的概念应该很熟悉。对于那些没有学习过且感兴趣的人，你可以阅读以下文章作为这个话题的良好介绍。

[](https://www.mathsisfun.com/numbers/complex-numbers.html?source=post_page-----160a533c212b--------------------------------) [## 复数

### 复数 复数是实数和虚数的组合 实数是数字…

[www.mathsisfun.com](https://www.mathsisfun.com/numbers/complex-numbers.html?source=post_page-----160a533c212b--------------------------------)

Python 中一个有趣的功能是大多数人不知道的，它完全支持复数。在数学中，我们通常使用符号 `i` 来表示复数。在 Python 中，我们使用 `j` 或调用 `complex()` 函数。

```py
# Creating complex numbers
z1 = 2 + 3j
z2 = complex(4, -2)  # (4 -2j)

# Accessing real and imaginary parts
print(z1.real)  # 2.0
print(z1.imag)  # 3.0

# Arithmetic with complex numbers
z3 = z1 + z2  # (6+1j)
z4 = z1 * z2  # (14+8j)
z5 = z1 / z2  # (0.1+0.8j)

# Conjugate of a complex number
z6 = z1.conjugate()  # (2-3j)
```

# 隐藏功能 4：使用 _ 访问最后一个结果

你可能已经看到大多数程序员将 `_` 作为一个占位符，用于在执行过程中未使用或不需要的变量。

然而，大多数人不知道的是，默认情况下，Python 将你最后一次执行的结果赋值给变量 `_`。

```py
x: int = 5
y: int = 99

x + y  # 104
print(_)  # 104
```

# 隐藏功能 5：参数解包

假设我们有一个任意的函数：

```py
def my_sum(a, b, c):
    return a + b + c
```

我们有一个包含 3 个数字的列表，我们希望将其传递给我们的函数。我们通常写：

```py
my_list = [1, 2, 3]

result = my_sum(my_list[0], my_list[1], my_list[2])
print(result)  # 6
```

在 Python 中，我们可以这样做：

```py
result = my_sum(*my_list)
print(result)  # 6
```

`*` 将解包整个列表，并将每个项作为参数传递给函数。随后，我们也可以使用 `**` 来解包字典。

```py
# Example of dictionary argument unpacking
def my_func(a, b, c):
    print(f"a={a}, b={b}, c={c}")

my_dict = {'a': 1, 'b': 2, 'c': 3}

my_func(**my_dict)
```

# 额外功能：import antigravity

Python 中散布着几个隐藏的彩蛋。尝试 `import antigravity` 😁

**你喜欢这篇文章吗？每月 $5，你可以成为会员，解锁对 Medium 的无限访问权限。你将直接支持我和你在 Medium 上的所有其他喜爱作者。非常感谢！**

[](https://david-farrugia.medium.com/membership?source=post_page-----160a533c212b--------------------------------) [## 使用我的推荐链接加入 Medium - David Farrugia

### 获得对我所有⚡优质⚡内容的独家访问权限，并在 Medium 上无限制浏览。通过请我喝杯咖啡来支持我的工作…

[david-farrugia.medium.com](https://david-farrugia.medium.com/membership?source=post_page-----160a533c212b--------------------------------)

**也许你还可以考虑订阅我的邮件列表，以便在我发布新内容时收到通知。这是免费的 :)**

[](https://david-farrugia.medium.com/subscribe?source=post_page-----160a533c212b--------------------------------) [## 当 David Farrugia 发布新内容时，获取电子邮件。

### 当 David Farrugia 发布新内容时，获取电子邮件。通过注册，如果你还没有 Medium 账户，你将创建一个…

[david-farrugia.medium.com](https://david-farrugia.medium.com/subscribe?source=post_page-----160a533c212b--------------------------------)

# 想要联系我吗？

我很想听听你对这个话题或任何有关 AI 和数据的想法。

如果你希望联系我，可以通过 ***davidfarrugia53@gmail.com*** 给我发邮件。

[Linkedin](https://www.linkedin.com/in/david-farrugia/)
