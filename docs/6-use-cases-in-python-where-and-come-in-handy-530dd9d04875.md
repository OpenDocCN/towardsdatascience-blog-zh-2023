# Python 中 * 和 ** 的 6 种用例

> 原文：[`towardsdatascience.com/6-use-cases-in-python-where-and-come-in-handy-530dd9d04875`](https://towardsdatascience.com/6-use-cases-in-python-where-and-come-in-handy-530dd9d04875)

## 通过示例进行解释

[](https://sonery.medium.com/?source=post_page-----530dd9d04875--------------------------------)![Soner Yıldırım](https://sonery.medium.com/?source=post_page-----530dd9d04875--------------------------------)[](https://towardsdatascience.com/?source=post_page-----530dd9d04875--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----530dd9d04875--------------------------------) [Soner Yıldırım](https://sonery.medium.com/?source=post_page-----530dd9d04875--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----530dd9d04875--------------------------------) ·阅读时间 5 分钟·2023 年 6 月 19 日

--

![](img/bee7c0a318f34a43becb83b38cfb4d44.png)

图片由[Szabolcs Toth](https://unsplash.com/@szabolcs_taking_pictures?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)拍摄，发布在[Unsplash](https://unsplash.com/photos/FYt8CIOosOw?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)上

如果你曾经浏览过 Python 库的文档，你会不可避免地注意到在多个地方使用了*或**。

那么，*和**到底有什么作用？

我们将学习 * 和 ** 有用的 7 种用例，包括你在文档中看到的情况。

用例最好通过示例进行说明，所以我们直接进入主题。

## 1. 使用可迭代对象调用函数

假设你定义了一个接受 5 个位置参数的函数，将它们相加并返回总和。

```py
# define the function
def calculate_sum(a, b, c, d, e):
    return a + b + c + d + e

# call the function
result = calculate_sum(1, 4, 3, 2, 6)
print(result)
16
```

如果传递给函数的参数存储在一个可迭代对象中（例如列表），你可以直接使用列表调用函数，但你需要使用*，如下所示：

```py
# call the function
numbers = [1, 4, 3, 2, 6]

calculate_sum(*numbers)
print(result)
16
```

如果你尝试调用函数`calculate_sum(numbers)`，Python 会报错，抛出`TypeError`。*的作用是解包可迭代对象中的值，有时被称为参数解包。

需要注意的是，`numbers` 列表中的值的数量必须等于函数所需的参数数量，这里是 5。如果不匹配，我们会得到一个`TypeError`。

*如果你也想了解更多关于 Pandas 的信息，可以访问我的课程* [*500 Exercises to Master Python Pandas*](https://www.udemy.com/course/500-exercises-to-master-python-pandas/learn/lecture/37842166#overview)*。*

## 2. 使用字典调用函数

这个用例类似于之前的示例。在之前的示例中，函数只有位置参数。在具有关键字参数的函数中，我们仍然可以进行参数解包，但使用的是**而不是*。

```py
# define the function
def calculate_mass(density, length=1, width=1, height=1):
    return density * length * width * height

# call the function
density = 20
mass = calculate_mass(density, 2, 3, 5)
print(mass)
600
```

这个函数使用物体的密度、长度、宽度和高度来计算质量。假设这些测量值存储在一个 Python 字典中。我们可以直接使用字典来调用函数，如下所示：

```py
# call the function
density = 20

measures = {
    "length": 2,
    "width": 3,
    "height": 5
}

calculate_mass(density, **measures)
600
```

类似于第一个例子，如果我们尝试以 `calculate_mass(density, measures)` 的形式调用它，我们会得到一个 `TypeError`。这是另一个参数解包的例子。

## 3\. 定义一个可以接受任意数量位置参数的函数

你定义一个函数来汇总给定的值，但你不想对传递的值数量施加约束。相反，你希望它具有动态性，能够汇总任意数量的给定值。

下面是我们如何定义这个函数：

```py
# define the function
def calculate_sum(*args):
    result = 0
    for i in args:
        result += i
    return result
```

`*args` 表达式将传递给函数的参数打包，使我们可以使用任意数量的参数或可迭代对象来调用函数。

下面是调用这个函数的不同方式：

```py
# calling with 2 values
calculate_sum(3, 4)
7

# calling with a list
my_list = [1, 4, 5, 10, 20]
calculate_sum(*my_list)
40

# calling with a value and a list
my_list = [10, 20, 30]
calculate_sum(5, *my_list)
65
```

在使用可迭代对象来调用函数的情况下，我们仍然需要在调用函数时加上 `*`。

> 关于位置参数和关键字参数的说明：
> 
> 位置参数仅通过名称声明。当函数被调用时，必须提供位置参数的值。否则，我们会得到一个错误。
> 
> 关键字参数通过名称和默认值来声明。如果我们没有为关键字参数指定值，它将采用默认值。

## 4\. 定义一个可以接受任意数量关键字参数的函数

这一个与之前的用例类似，但我们将创建一个接受关键字参数的函数。

```py
# define the function
def greet(name, **kwargs):
    greeting = f"Hello, {name}.\n"

    if kwargs:
        greeting += "Here are the things I know about you:\n"
        for key, value in kwargs.items():
            greeting += f" {key.title()}: {value}\n"

    return greeting
```

这个函数根据名字向人们打招呼。如果它被调用时附带了关于这些人的其他关键信息，函数也会打印这些信息。

`**kwargs` 表达式允许我们在调用函数时传递任意数量的关键字参数。

下面是调用这个函数的不同方式：

```py
# calling with the name (positional argument) only
print(greet("John"))
# output
Hello, John.

# calling with adding some keyword arguments
print(greet("Jane", age=34, job="doctor", hobby="chess"))
# output
Hello, Jane.
Here are the things I know about you:
 Age: 34
 Job: doctor
 Hobby: chess

# calling with adding some keyword arguments as a dictionary
ashley = {
    "age": 27,
    "profession": "athlete",
    "hobby": "reading"
}

print(greet("Ashley", **ashley))
# output
Hello, Ashley.
Here are the things I know about you:
 Age: 27
 Profession: athlete
 Hobby: reading
```

在将关键字参数作为字典传递时，我们需要在调用函数时加上 `**`。

## 5\. 合并字典

我们可以使用 `**` 来合并字典。

```py
ages = {
    "John": 34,
    "Jane": 36
}

new_items = {
    "Matt": 28,
    "Ashley": 24
}

ages = {**ages, **new_items}

print(ages)
{'John': 34, 'Jane': 36, 'Matt': 28, 'Ashley': 24}
```

在这种情况下，使用 `ages = {ages, new_items}` 也会引发 `TypeError`。

## 6\. 将值打包成可迭代对象

假设我们有一个包含多个值的列表。我们想要从这个列表中将一个值赋给另一个变量，并将其余的值赋给一个不同的变量。通过下面的例子会更清楚：

```py
first, *others = [3, 5, 1, 10, 24]

print(first)
print(others)
# output
3
[5, 1, 10, 24]
```

第一个值被分配给一个名为 `first` 的变量，其余的值被打包到另一个名为 `others` 的列表中。

我们还可以按照下面的例子来使用它：

```py
first, *others, last = [3, 5, 1, 10, 24]

print(first)
print(others)
print(last)
# output
3
[5, 1, 10]
24
```

这个用例在处理返回多个值的函数时可能会有所帮助。这里是一个例子：

```py
# define the function
def my_func(a, b, c, d, e):
    sum_1 = a + b
    sum_2 = a + b + c
    sum_3 = a + b + c + d
    sum_4 = a + b + c + d + e
    return sum_1, sum_2, sum_3, sum_4

# call the function
first_sum, *other_sums = my_func(1, 3, 5, 2, 6)

print(first_sum)
print(other_sums)
# output
4
[9, 11, 17]
```

`my_func` 函数返回一个包含 4 个值的元组。第一个值分配给一个名为 `first_one` 的变量，其他值分配给一个名为 `other_sums` 的列表。

## 结束语

正如我们在例子中演示的，`*` 和 `**` 在 Python 中非常有用。我们可以用它们来进行参数的打包和解包。

这些也在定义和调用函数时使用，以增加更多的灵活性和多功能性。

*你可以成为* [*Medium 会员*](https://sonery.medium.com/membership) *以解锁对我所有写作内容的完全访问权限，以及 Medium 上的其他内容。如果你已经是会员，请不要忘记* [*订阅*](https://sonery.medium.com/subscribe) *，如果你希望在我发布新文章时收到电子邮件的话。*

感谢你的阅读。如果你有任何反馈，请告诉我。
