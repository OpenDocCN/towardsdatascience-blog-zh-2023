# *args, **kwargs 和一切介于两者之间

> 原文：[`towardsdatascience.com/args-kwargs-and-everything-in-between-ff7d9b536494`](https://towardsdatascience.com/args-kwargs-and-everything-in-between-ff7d9b536494)

## Python 中的函数参数和实参基础

[](https://philip-wilkinson.medium.com/?source=post_page-----ff7d9b536494--------------------------------)![Philip Wilkinson, Ph.D.](https://philip-wilkinson.medium.com/?source=post_page-----ff7d9b536494--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ff7d9b536494--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----ff7d9b536494--------------------------------) [Philip Wilkinson, Ph.D.](https://philip-wilkinson.medium.com/?source=post_page-----ff7d9b536494--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----ff7d9b536494--------------------------------) ·阅读时间 6 分钟·2023 年 7 月 18 日

--

![](img/f4942f955c3c59bada72dac594277ddb.png)

图片由[Sigmund](https://unsplash.com/@sigmund?utm_source=medium&utm_medium=referral)提供，[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

Python 因其多功能性、简洁性和强大的库，已成为数据科学的首选语言。函数凭借其封装可重用代码的能力，在 Python 的数据科学工作流程中发挥着关键作用。理解函数参数和实参的细微差别对于在数据科学背景下充分发挥 Python 函数的真正潜力至关重要。

## 参数与实参

在 Python 中处理函数时，首先要理解参数和实参之间的区别。参数是函数定义中的变量，而实参是你在调用函数时传入函数参数的值。例如：

```py
def my_func(param1, param2):
    print(f"{param1} {param2}")

my_func("Arg1", "Arg2")

# Out:
# Arg1 Arg2
```

`param1` 和 `param2` 是函数参数，而`"Arg1"`和`"Arg2"`是实参。

## 位置参数与关键字参数

在这个示例中，“Arg1”和“Arg2”作为位置参数传入。这是因为每个参数相关的参数在函数调用中没有被指定。这意味着由于它们的顺序，“Arg1”占据了`param1`的位置，而“Arg2”占据了`param2`的位置。

我们可以通过利用关键字参数来改变顺序。这是因为每个参数相关的参数是通过正确的关键字明确定义的。

```py
def my_func(param1, param2):
    print(f"{param1} {param2}")

my_func(param2 = "Arg2", param1 = "Arg1")

# Out:
# Arg1 Arg2
```

这个示例产生的输出与第一次函数调用相同，即使参数的位置被交换，因为每个参数相关的参数是使用相应的关键字定义的。

## 默认参数

你经常会看到的第二种情况是默认参数。这些参数通常具有一个常见的值或“默认”值，当调用函数时，通常可以忽略这些值。它们通过给参数赋予默认值来在函数定义中设置：

```py
def statement(sentence, print = False):
    if print:
       print(sentence)
    return sentence
```

在上面的函数中，我们将是否打印的默认值设置为`False`。调用函数时可以覆盖此设置：

```py
text = statement(sentence = "Hello there!", print=True)

# Out:
# Hello there!
```

在处理默认参数时，重要的是要注意在函数定义中，非默认参数不能跟随默认参数。这确保了你最常用和变化的参数在函数定义的开头，并且首先被赋值。

## *Args

Python 函数的一个精彩之处在于它们可以接受任意数量的位置参数。这种语法以参数名前加星号的形式出现。按照惯例，这个参数被定义为`*args`：

```py
def volumne(*args):
    vol= 0
    for arg in args:
        vol *= arg
    return vol
```

`*args`通常作为元组传递给函数，这样我们可以利用迭代。例如，在上面的体积函数中，我们假设传递了任意数量的数字来计算一个多维物体的体积。

然而，一种更安全的实现方式是确保至少传递一个数字开始，同时有一个变量数量的其他维度长度：

```py
def sum(length, *lengths):
    v = length
    for item in lengths:
        v *= length
    return v
```

使用`*args`参数允许你将任意数量的位置参数传递给函数，使函数更加灵活，能够处理不同数量的输入。

需要注意的是，在处理`*args`时：

+   `*args`必须在函数定义中的所有其他位置参数之后出现。

+   每个参数列表中只能有一个`*args`。

+   `*args`仅收集位置参数，不包括关键字参数。

+   任何在`*args`之后的参数都被视为常规位置参数。

+   在`*args`之后传递的任何参数必须作为强制关键字参数传递。

## `**kwargs`

另一种选择是接受任意数量的关键字参数。这可以通过在函数定义中用`**`前缀参数来完成。按照惯例，这个参数称为`**kwargs`。这允许你将键值对作为参数传递给函数，提供了一种灵活的方式来处理函数中的命名参数。

一个例子是创建具有键和值的 HTML 标签：

```py
def tag(name, **attributes):
    result = "<" + name
    for key, value in attributes.items():
        result += f' {key}="{str(value)}"'
    result += ">"

    return result
```

要利用`**kwargs`，你必须确保：

+   `*args`必须始终在`**kwargs`之前出现在参数列表中。

+   `**kwargs`必须总是放在参数列表的最后。

## 仅位置参数

在 Python 3.8 之后，你还可以指定仅位置参数。这通过在参数列表的末尾添加`/`来完成，表示该参数只能是位置参数。

```py
def number_length(x, /):
    return len(str(x))
```

在这个例子中，你不能将值作为关键字传递给 x 参数，x 必须是一个位置参数。

这也可以与常规参数结合使用，以确保某些参数是位置参数，而其他参数可以是位置参数或关键字参数。

```py
def greet(name, /, greeting="Hello")
    return f"{greeting}, {name}"
```

在这个例子中，`greeting` 可以通过位置或关键字传递给函数，而 `name` 只能通过位置传递。

当参数有自然顺序但很难给出良好的描述性名称时，这可能很有用。它还允许你重构代码而不必过于担心依赖这些名称的代码。

## 仅关键字参数

另一种方式是指定仅关键字参数。这通过在函数定义中的参数列表前添加一个`*`来完成。在`*`后的参数必须是关键字：

```py
def to_fahrenheiht(*, celsius):
    return 32 + celsius * 9 / 5
```

在这个例子中，`celsius` 是一个仅关键字参数，因此如果你尝试基于位置而不使用关键字来指定它，Python 会抛出错误。

这在你想确保正确的值被传递到函数中时非常有用。在这种情况下，我们确保传递给函数的是摄氏度，而不是华氏度或开尔文。

## 类型提示

最后，从 Python 3.5 起，你现在可以提示参数应该期望什么类型。这遵循以下语法：

```py
<paramater_name>: <paramater_type> = <paramater_value>
```

例如，提示你希望在函数中将两个数字相加：

```py
def add_numbers(num1: float, num2: float):
    return num1 + num2
```

需要注意的是，这并不强制类型，而只是用来建议和提示应该传递给函数的类型。重要的是，现代 IDE 现在可以识别类型提示，并在传递错误类型时提供警告。

## 总结

掌握 Python 中的函数参数对于任何寻求优化工作流的数据科学家来说都是一项关键技能。通过对函数参数的深入理解，你可以编写更简洁、清晰的代码，促进可重用性、模块化和可维护性。因此，拥抱 Python 函数参数的多样性和复杂性，继续探索 Python 函数的深度，并尝试不同的参数模式，以改善你的编码工作流。

如果你喜欢你所读的内容，还不是 Medium 会员，可以通过下面的推荐链接注册 Medium，以支持我和平台上的其他优秀作者！提前感谢。

[## 使用我的推荐链接加入 Medium — Philip Wilkinson](https://philip-wilkinson.medium.com/membership?source=post_page-----ff7d9b536494--------------------------------)

### 作为 Medium 会员，你的会员费的一部分将分配给你阅读的作者，你可以完全访问每个故事……

[philip-wilkinson.medium.com](https://philip-wilkinson.medium.com/membership?source=post_page-----ff7d9b536494--------------------------------)

或者随时查看我在 Medium 上的其他文章：

[## 每个数据科学家都应该知道的八种数据结构](https://philip-wilkinson.medium.com/membership?source=post_page-----ff7d9b536494--------------------------------)

### 从 Python 中的基本数据结构到抽象数据类型

[初学者的数据科学完整课程](https://towardsdatascience.com/a-complete-data-science-curriculum-for-beginners-825a39915b54?source=post_page-----ff7d9b536494--------------------------------)

### UCL 数据科学社团: Python 介绍，数据科学家工具包，使用 Python 进行数据科学

[随机森林分类器的实用介绍](https://python.plainenglish.io/a-practical-introduction-to-random-forest-classifiers-from-scikit-learn-536e305d8d87?source=post_page-----ff7d9b536494--------------------------------)

### UCL 数据科学社团研讨会 14: 随机森林分类器是什么，实施，评估和改进

[八种每个数据科学家都应该知道的数据结构](https://towardsdatascience.com/eight-data-structures-every-data-scientist-should-know-d178159df252?source=post_page-----ff7d9b536494--------------------------------)
