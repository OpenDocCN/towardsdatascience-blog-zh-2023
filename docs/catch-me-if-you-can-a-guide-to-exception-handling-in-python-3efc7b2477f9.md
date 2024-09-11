# 《抓住我，如果你能：Python 异常处理指南》

> 原文：[`towardsdatascience.com/catch-me-if-you-can-a-guide-to-exception-handling-in-python-3efc7b2477f9?source=collection_archive---------9-----------------------#2023-05-08`](https://towardsdatascience.com/catch-me-if-you-can-a-guide-to-exception-handling-in-python-3efc7b2477f9?source=collection_archive---------9-----------------------#2023-05-08)

## 通过智能异常管理，释放 Python 的全部潜力

[](https://medium.com/@orjwan.zaafarani?source=post_page-----3efc7b2477f9--------------------------------)![Orjuwan Zaafarani](https://medium.com/@orjwan.zaafarani?source=post_page-----3efc7b2477f9--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3efc7b2477f9--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----3efc7b2477f9--------------------------------) [Orjuwan Zaafarani](https://medium.com/@orjwan.zaafarani?source=post_page-----3efc7b2477f9--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F155bd470ac4c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcatch-me-if-you-can-a-guide-to-exception-handling-in-python-3efc7b2477f9&user=Orjuwan+Zaafarani&userId=155bd470ac4c&source=post_page-155bd470ac4c----3efc7b2477f9---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----3efc7b2477f9--------------------------------) ·6 分钟阅读·2023 年 5 月 8 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F3efc7b2477f9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcatch-me-if-you-can-a-guide-to-exception-handling-in-python-3efc7b2477f9&user=Orjuwan+Zaafarani&userId=155bd470ac4c&source=-----3efc7b2477f9---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F3efc7b2477f9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcatch-me-if-you-can-a-guide-to-exception-handling-in-python-3efc7b2477f9&source=-----3efc7b2477f9---------------------bookmark_footer-----------)![](img/bf4a79f5d2abc3d82f44902159d3cfd3.png)

图片来源：[Cookie the Pom](https://unsplash.com/@cookiethepom?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

作为软件开发者，处理异常通常被视为一种必要的恶行。然而，掌握 Python 的异常处理系统可以让你成为更高效、更有效的程序员。

在这篇博客文章中，我将对以下内容进行深入解释：

+   什么是异常处理？

+   `if` 语句与异常处理的区别

+   使用 `else` 和 `finally` 子句进行正确的错误管理

+   自定义异常的定义

+   异常处理的最佳实践

# 什么是异常处理？

异常处理是编写代码以捕获和处理可能在程序执行期间发生的错误或异常的过程。这使得开发者能够编写即使在面对意外事件或错误时也能继续运行的健壮代码，而不是完全崩溃。

当发生异常时，Python 会搜索匹配的异常处理程序。处理程序代码将执行并采取适当的行动，如记录错误、显示错误信息或尝试从错误中恢复。总体而言，异常处理有助于使 Python 应用程序更加可靠、可维护，并且更易于调试。

# `if` 语句和异常处理之间的区别

`if` 语句和 Python 中异常处理的主要区别在于它们各自的目标和使用场景。

`if` 语句作为结构化编程的基本构建块。它评估一个条件，并根据条件是否为真执行不同的代码块。以下是一个示例：

```py
temperature = int(input("Please enter temperature in Fahrenheit: "))
if temperature > 100:
    print("Hot weather alert! Temperature exceeded 100°F.")
elif temperature >= 70:
    print("Warm day ahead, enjoy sunny skies.")
else:
    print("Bundle up for chilly temperatures.")
```

异常处理在编写健壮且有弹性的程序中扮演着重要角色，它通过处理在运行时可能出现的意外事件和错误来实现这一点。

异常用于信号问题，并指出代码中需要改进、调试或额外错误检查措施的区域。它们允许 Python 优雅地处理错误情况，并继续执行脚本，而不是突然终止。

下面是一个如何实现异常处理以更好地管理与除零相关的潜在失败的示例：

```py
# Define a function that tries to divide a number by zero
def divide(x, y):
    result = x / y
    return result
# Call the divide function with x=5 and y=0
result = divide(5, 0)
print(f"Result of dividing {x} by {y}: {result}")
```

输出：

```py
Traceback (most recent call last):
  File "<stdin>", line 8, in <module>
ZeroDivisionError: division by zero attempted
```

由于引发了异常，程序在到达 print 语句之前立即停止执行。

我们可以通过将对“divide”函数的调用放入 `try-except` 块中来处理上述异常，如下所示：

```py
# Define a function that tries to divide a number by zero
def divide(x, y):
    result = x / y
    return result
# Call the divide function with x=5 and y=0
try:
    result = divide(5, 0)
    print(f"Result of dividing {x} by {y}: {result}")
except ZeroDivisionError:
    print("Cannot divide by zero.")
```

输出：

```py
Cannot divide by zero.
```

通过这样做，我们优雅地处理了 `ZeroDivisionError` 异常，而不会因为未处理的异常使脚本的其余部分失败。

有关 Python 内置异常的更多信息，请参见 [[2]](https://www.geeksforgeeks.org/python-exception-handling/)。

# 使用 Else 和 Finally 子句进行正确的错误管理

在处理 Python 中的异常时，建议在 `try-except` 块中同时包含 `else` 和 `finally` 子句。`else` 子句允许你指定如果没有引发异常时应该发生的情况，而 `finally` 子句确保无论是否发生异常，某些清理操作总是会执行 [[1]](https://docs.python.org/3/tutorial/errors.html)[[2]](https://www.geeksforgeeks.org/python-exception-handling/)。

例如，考虑一个场景，你想从文件中读取数据并对数据进行一些操作。如果在读取文件时发生异常，你可能想记录错误并停止进一步处理，但仍然想正确关闭文件。

使用 `else` 和 `finally` 子句可以让你做到这一点——如果没有发生异常，则正常处理数据；或者在处理任何异常时仍能适当地关闭文件。如果没有这些子句，你的代码可能会遭遇资源泄漏或不完整的错误处理。因此，它们在创建健壮和可靠的程序中扮演着至关重要的角色。

```py
try:
    # Open the file in read mode
    file = open("file.txt", "r")
    print("Successful opened the file")
except FileNotFoundError:
    # Handle missing files
    print("File Not Found Error: No such file or directory")
    exit()
except PermissionError:
    # Handle permission issues
    print("Permission Denied Error: Access is denied")
else:
    # All good, do something with the file data
    content = file.read().decode('utf-8')
    processed_data = process_content(content)

# Cleanup after ourselves even if an exception occurred above
finally:
    file.close()
```

在这个例子中，我们首先尝试使用 `with` 语句打开“file.txt”文件进行读取，这保证了文件对象在执行完成后自动正确关闭。如果在文件 I/O 操作期间发生 `FileNotFoundError` 或 `PermissionError`，相应的 except 语句将被执行。**为了简单起见**，如果找不到文件，我们只是打印错误信息并退出程序。

否则，当 `try` 块中没有异常发生时，我们在 `else` 分支中继续处理文件内容。最后，`finally` 块保证了文件的关闭，无论之前是否抛出了异常 [[1]](https://docs.python.org/3/tutorial/errors.html)。

通过采用这样的结构化方法，你的代码保持组织良好，易于跟随，同时考虑到可能由于与外部系统或输入交互而出现的潜在错误。

# 自定义异常定义

在 Python 中，你可以通过从内置异常如 `Exception` 或任何直接继承自 `Exception` 的类创建子类来定义自定义异常。

为此，你需要创建一个继承自这些基本异常之一的新类，并添加特定于你需求的属性。然后，你可以在代码中像使用其他内置异常类一样使用你新定义的异常类。

下面是定义一个名为 `InvalidEmailAddress` 的自定义异常的示例：

```py
class InvalidEmailAddress(ValueError):
    def __init__(self, message):
        super().__init__(message)
        self.msgfmt = message
```

这个自定义异常是从 `ValueError` 派生的，它的构造函数接受一个可选的消息参数（默认为`"invalid email address"`）。

当你遇到无效的电子邮件地址格式时，可以抛出这个异常：

```py
def send_email(address):
    if isinstance(address, str) == False:
        raise InvalidEmailAddress("Invalid email address")
# Send email
```

现在，如果你将一个无效的字符串传递给 `send_email()` 函数，你将看到一个自定义的错误消息，而不是普通的 `TypeError`，它清楚地指示了问题所在。例如，抛出异常的代码可能如下所示：

```py
>>> send_email(None)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "/path/to/project/main.py", line 8, in send_email
    raise InvalidEmailAddress("Invalid email address")
InvalidEmailAddress: Invalid email address
```

# 异常处理的最佳实践

以下是一些与 Python 中错误处理相关的最佳实践：

1.  **设计以应对失败**：提前规划，考虑可能的失败情况并设计你的程序以优雅地处理这些失败。这意味着要预见边界情况并实施适当的错误处理程序。

1.  **使用描述性的错误信息**：提供详细的错误信息或日志，帮助用户理解出了什么问题以及为什么。避免使用诸如“发生错误”或“发生了不好的事情”的通用错误信息。相反，显示一个友好的消息，建议解决方案或提供文档链接。务必在提供详细说明和避免 UI 过于繁杂之间取得平衡。

1.  **最小化副作用**：通过使用 try-finally 或 try-with-resources 块隔离问题代码段，最小化失败操作的后果。确保清理任务在成功或失败的结果下都能始终执行。

1.  **彻底测试**：确保你的异常处理程序在各种情况下表现正确，通过运行全面的测试来实现。

1.  **定期重构**：重构易出错的代码段，以提高其可靠性和性能。保持代码库的模块化和松散耦合，使独立部分可以独立演进而不会对其他部分产生负面影响。

1.  **记录重要事件**：通过将有趣的事件记录到文件或控制台输出中，跟踪应用程序中的发生情况。这有助于你快速诊断问题，而无需在大量未结构化的日志中筛选。

# 结论

编写错误处理代码是软件开发中不可或缺的一部分，特别是在使用 Python 时，它使开发者能够构建更可靠和健壮的应用程序。通过遵循行业标准和最佳实践，开发者可以减少调试时间，确保代码质量，并提供更好的用户体验。

# 资源

[1] [`docs.python.org/3/tutorial/errors.html`](https://docs.python.org/3/tutorial/errors.html)

[2] [`www.geeksforgeeks.org/python-exception-handling/`](https://www.geeksforgeeks.org/python-exception-handling/)
